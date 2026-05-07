这段代码非常清晰地揭示了为什么你的模型会出现 $mAP@0.5$ 上升而 $Acc@0.5$ 下降的现象。核心问题就出在这一行：

```python
reward = 0.7 * recall + 0.3 * precision
```

这解释了模型变“激进”的根本原因。结合你正在使用的大型视觉-语言基础模型（如 GroundingDINO）作为 Backbone 的情况，我们可以从以下几个层面来对这套 RL 逻辑进行“手术”：

### 1. 召回率权重过高的“副作用”
你给了 `recall` 0.7 的权重，而 `precision` 只有 0.3。
在使用多模态大模型做交通目标提取时，这类模型本身的泛化和召回能力就已经很强了。在这个权重下，强化学习 Agent 发现了一条捷径：**只要尽可能多地输出预测框，保证抓到所有的真实车辆（拉满 Recall），即使产生大量误检（拉低 Precision），总的 Reward 依然会非常高。** 这在算理上直接导致了模型通过“狂刷候选框”来换取高分，$mAP$ 会因为潜在正确的框变多而上升，但严格考核精度的 $Acc$ 必然惨跌。

**优化方案：改用调和平均数（F1-Score）或平衡权重**
简单的线性加权（算术平均）掩盖了某一项过低的问题。建议将 Reward 修改为 $F_1$ 分数的变体，或者强制平衡：
```python
# 方案 A：平衡加权
reward = 0.5 * recall + 0.5 * precision

# 方案 B：使用 F1-Score（更推荐，强烈惩罚单项过低）
# 加上一个极小值避免除以 0
f1_score = 2 * (precision * recall) / (precision + recall + 1e-6)
reward = f1_score
```

### 2. 离散奖励转化为连续奖励（Dense Reward）
目前的匹配准则是：只要 IoU $\ge 0.5$ 就算 matched，对应 Recall 和 Precision 直接 $+1$。这是一个**离散的阶跃奖励**。对于无人机航拍这种极小目标的场景，IoU 0.51 和 IoU 0.95 的质量差距巨大，但现在的代码给它们的奖励是一样的。
模型没有动力去精细调整框的边界，只要“大致框住”混及格就行。

**优化方案：引入 IoU 质量惩罚/奖励**
在循环计算匹配时，不仅统计 matched 的数量，顺便把成功匹配的 `IoU` 值累加起来。
```python
# 假设 matched_ious 是一个包含所有成功匹配对 IoU 值的列表
if matched > 0:
    mean_iou = sum(matched_ious) / matched
else:
    mean_iou = 0

# 将框的贴合质量也作为一部分奖励
reward = 0.4 * recall + 0.4 * precision + 0.2 * mean_iou
```

### 3. 加入明确的假阳性（FP）惩罚
无人机视角下的房顶空调外机、垃圾桶等很容易被误认为车辆。虽然目前的 Precision 在分母 `len(chosen)` 变大时会降低，但这属于一种“被动惩罚”。我们可以在 Reward 中加一个主动的负反馈：如果模型输出了高置信度但并未匹配到 GT 的框，给予倒扣分。

```python
# fp_count 就是预测出的总框数减去匹配上的框数
fp_count = len(chosen) - matched
fp_penalty_rate = 0.05 # 每个假框扣除一定分数

# 在原来的基础上扣分
reward_base = 0.5 * recall + 0.5 * precision
reward = max(0, reward_base - fp_penalty_rate * fp_count) 
```

### 4. 熵正则化系数（Entropy Coefficient）的退火
你的代码中有一项 `entropy_coef = 0.001`。这在强化学习初期是非常好的，可以鼓励模型探索不同的边界框输出。
但如果你正在一台 RTX 4060 Laptop 这样显存受限的设备上进行微调，且 Batch Size 可能无法开得很大，到了训练中后期，固定的 exploration noise 会导致模型不断尝试输出无效的边界框，从而降低模型的绝对准确率。

**优化方案：**
引入熵系数的线性衰减或余弦退火。随着 Epoch 的增加，将 `entropy_coef` 从 $0.001$ 逐渐衰减到 $0.0001$ 甚至更低，让模型在后期能够收敛到最确定的预测上，减少随机乱框。

**总结下一步动作：**
你可以先尝试最简单的改动：将 `run_exp14_full_rebuild_20260421.py` 中的 `0.7 * recall + 0.3 * precision` 修改为 `0.5 * recall + 0.5 * precision`，重新跑几轮验证（Validation），看看 $Acc@0.5$ 是否有明显的回升。通常仅这一项改动，就能立竿见影地把过剩的“激进”表现压制回去。