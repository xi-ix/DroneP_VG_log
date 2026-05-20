# Exp23 Teacher Distillation To Exp22 思路记录

## 背景

Exp23 证明 `Exp18 predictions + GroundingDINO baseline predictions` 存在互补性。融合后 full 指标达到：

- `Acc@0.5 = 0.4650`
- `Acc@0.75 = 0.3102`
- `mAP@0.5 = 0.1295`

但 Exp23 是预测融合/后处理上限实验，不是单模型结果。后续目标是把这个融合策略蒸馏到一个外接 scorer 中，使结果更接近“模型能力提升”。

## 核心目标

在 Exp22 外接预处理头基础上做 Exp24：

```text
候选源：Exp18 predictions/full + GroundingDINO baseline predictions
teacher：Exp23 predictions/full
student：Exp22 风格的候选/图像/语言预处理头 + integration layer + score head
选择标准：真实 val 指标 0.5 * Acc@0.5 + 0.5 * mAP@0.5
```

## 训练信号

### 1. GT 硬标签

候选框与同类别 GT 匹配：

- IoU >= 0.5：正样本
- 否则：负样本

用途：保证 student 不只模仿 teacher，也保留真实目标约束。

### 2. Teacher 软标签

候选框与 Exp23 teacher 预测框匹配：

- 同类别 teacher IoU >= 0.7：强正样本，target 接近 teacher score
- IoU 0.5~0.7：软正样本，target 按 IoU 衰减
- 无匹配：负样本

用途：学习 Exp23 融合后的保留策略和 score 排序。

### 3. Ranking loss

同一张图中构造正负候选对：

```text
positive score > negative score + margin
```

正样本可来自 GT 命中或 teacher 高分匹配；负样本来自低 IoU 或 teacher 不保留的候选。

用途：直接改善 score 排序，服务 `mAP@0.5`。

## 损失函数建议

```text
loss = BCE(student_logit, gt_label)
     + alpha * BCE(student_logit, teacher_soft_label)
     + beta * ranking_loss
```

建议初始权重：

- `alpha = 0.7`
- `beta = 0.2`

如果出现 Acc 提升但 mAP 下降，优先提高 ranking loss 或引入 per-class calibration。

## Checkpoint 选择

不要再只看候选级 `val_acc` 或 proxy reward。每轮训练后直接在 val split 上评估：

```text
objective = 0.5 * Acc@0.5 + 0.5 * mAP@0.5
```

保存真实 val objective 最好的 checkpoint。

## 预期收益

如果 student 成功学习 Exp23 的融合策略，应至少比 Exp22 large 更好，并争取接近：

```text
Exp18 large: Acc@0.5=0.4395, mAP@0.5=0.0962
Exp23 fusion upper bound: Acc@0.5=0.4650, mAP@0.5=0.1295
```

Exp24 的合理目标是先超过 Exp18/Exp21 large，再逐步接近 Exp23。

## 风险

- Exp23 teacher 本身是融合结果，student 不一定能完全拟合。
- 候选量较大，训练和 val 真实评估会更慢。
- 如果只优化 BCE，可能仍然出现 score calibration 变差和 mAP 下降。
- 需要把 Exp24 记录为 student/distillation 实验，不要和 Exp23 fusion upper bound 混淆。

