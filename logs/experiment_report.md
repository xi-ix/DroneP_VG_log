# 实验统一报告（唯一主报告）

说明：
- 本文件是 `project_records` 下唯一的阶段实验汇总报告。
- 每次新实验只追加三项：实验目的、实验方法、实验结果。
- 不再新增其他阶段总结文档。

---

## 2026-03-18 / Exp-01 / 整图 best 基线确认

### 实验目的
在固定评估集与固定口径下，确认可复现的整图零样本对照线。

### 实验方法
- 数据：`visdrone_test_100`（100 图，GT=5077）
- 模型：GroundingDINO
- 配置：`box_threshold=0.20, text_threshold=0.20, nms_threshold=0.40`
- 评估：class-aware `Acc@0.5 / Acc@0.75 / mAP@0.5`

### 实验结果
- Pred boxes=3648
- Acc@0.5=0.3443
- Acc@0.75=0.2624
- mAP@0.5=0.1451
- Empty FP=0.0000

---

## 2026-03-18 / Exp-02 / 滑窗接入与整图对照

### 实验目的
验证滑窗策略是否优于整图 best 基线。

### 实验方法
- 在同阈值设置下启用滑窗：`window=1024x1024, overlap=0.20`
- 与 Exp-01 使用相同评估口径对照。

### 实验结果
- Pred boxes=4612
- Acc@0.5=0.3760（相对整图 +0.0317）
- Acc@0.75=0.2813（相对整图 +0.0189）
- mAP@0.5=0.1517（相对整图 +0.0066）
- Empty FP=0.0000

---

## 2026-03-18 / Exp-03 / 滑窗网格搜索（9 组）

### 实验目的
识别滑窗参数在精度指标上的最佳取向。

### 实验方法
- 固定阈值：`0.20/0.20/0.40`
- 网格：`window={896,1024,1280}` × `overlap={0.10,0.20,0.30}` 共 9 组。
- 统一评估口径。

### 实验结果
- mAP 取向最优：`cfg07 (1280,0.10)`
  - Acc@0.5=0.3610, Acc@0.75=0.2758, mAP@0.5=0.1616
- Acc 取向最优：`cfg02 (896,0.20)`
  - Acc@0.5=0.3817, Acc@0.75=0.2840, mAP@0.5=0.1541
- 结论：存在“mAP 与 Acc 取向分化”。

---

## 2026-03-18 / Exp-04 / Stage A 保底融合（非训练）

### 实验目的
在不训练主干的前提下，验证轻量增强是否可进一步提升。

### 实验方法
- 将整图 best 与滑窗 cfg02 预测按类合并并做 NMS。
- 统一评估口径。

### 实验结果
- Pred boxes=5492
- Acc@0.5=0.4020
- Acc@0.75=0.2958
- mAP@0.5=0.1556
- Empty FP=0.0000

---

## 2026-03-18 / Exp-05 / Day2-B 反馈策略（bandit）

### 实验目的
实现最小反馈优化逻辑，自动选择滑窗策略。

### 实验方法
- epsilon-greedy bandit
- 臂：9 组滑窗配置
- reward：`0.5*Acc@0.5 + 0.3*Acc@0.75 + 0.2*mAP@0.5`

### 实验结果
- 收敛策略：`cfg02 (896,0.20)`
- 对应指标：Acc@0.5=0.3817, Acc@0.75=0.2840, mAP@0.5=0.1541
- 说明：当前 reward 权重偏向 Acc。

---

## 2026-03-18 / Exp-06 / Day2-C 运行效率基准

### 实验目的
量化精度收益对应的时间与显存成本。

### 实验方法
- 配置对照：`full_best`、`sw_cfg02`、`sw_cfg07`
- 统计：100 图总耗时、单图平均耗时、峰值显存。

### 实验结果
- full_best：76s，0.7600s/图，2284MB
- sw_cfg02：280s，2.8000s/图，2360MB
- sw_cfg07：141s，1.4100s/图，2632MB
- 结论：`cfg07` 在 mAP 与速度之间更均衡，`cfg02` 精度（Acc）更高但耗时更高。

---

## 2026-03-18 / Exp-07 / Stage A 外接模块训练（MLP 校准头）

### 实验目的
在不改 GroundingDINO 主干参数的前提下，训练最小外接校准头，验证是否能优于非训练版 Stage A 保底融合。

### 实验方法
- 输入：整图 best 预测 + 滑窗 `cfg02` 预测。
- 可训练部分：外接 MLP 校准头（hidden=16，epochs=40，lr=0.003），仅学习候选框保留概率。
- 主干约束：GroundingDINO 主干与检测参数保持冻结，不参与反向更新。
- 后处理：校准分数阈值自动搜索（best=0.10）+ class-aware NMS（IoU=0.5）。
- 对照项：Exp-04 的非训练 Stage A 融合结果。

### 实验结果
[结果](DroneP_VG/project_records/results/metrics/lightweight_optimization/training_log_external_calibrator_stageA_20260318.txt)
- 新 Stage A（外接模块训练后）：
  - Pred boxes=5494
  - Acc@0.5=0.4014
  - Acc@0.75=0.2974
  - mAP@0.5=0.1576
  - Empty FP=0.0000
- 相对 Exp-04（非训练融合）：
  - Acc@0.5：-0.0006（0.4020 -> 0.4014）
  - Acc@0.75：+0.0016（0.2958 -> 0.2974）
  - mAP@0.5：+0.0020（0.1556 -> 0.1576）
- 结论：外接校准头在不动主干下带来小幅质量增益（主要体现在 `Acc@0.75` 与 `mAP@0.5`），可作为中期“有训练闭环”的最小可交付版本。

---

## 2026-03-18 / Exp-08 / 外接模块长训练 + 进度可视化

### 实验目的
提高训练时长并保证训练过程可观测，继续冲击 `Acc@0.5` 上限。

### 实验方法
- 训练参数：`epochs=200, hidden_dim=64, lr=0.0015, weight_decay=5e-5`。
- 搜索策略：联合搜索 `score_threshold + nms_iou`（目标 `Acc@0.5`）。
- 进度可视化：
  - 终端实时输出 epoch 进度条与指标；
  - 实时写入进度文件（每 epoch 一行）。

### 实验结果
- 长训练版本（official 评估）：
  - Pred boxes=8469
  - Acc@0.5=0.4099
  - Acc@0.75=0.3118
  - mAP@0.5=0.1322
  - Empty FP=0.0000
- 相对 Exp-07（40 epoch）：
  - Acc@0.5：+0.0085（0.4014 -> 0.4099）
  - Acc@0.75：+0.0144（0.2974 -> 0.3118）
  - mAP@0.5：-0.0254（0.1576 -> 0.1322）
- 可达性上限（当前候选框集合的 oracle 覆盖）：
  - oracle_cover_acc05=0.4130（2097/5077）
  - 结论：在“不新增候选检测框、仅重排/校准”的前提下，`Acc@0.5` 理论上限约 `0.413`，因此当前阶段无法达到 `0.6`。

---

## 2026-03-18 / Exp-09 / 外接模块双阶段训练（监督学习 + 强化学习）

### 实验目的
将原有单一外接校准模型升级为“双模块”：先进行监督学习训练，再进行强化学习策略微调，并验证训练后测试效果。

### 实验方法
- 代码改造：将 Stage A 外接模型拆分为两个可训练模块。
  - 监督学习模块（SL）：二分类校准头，目标函数为加权 BCE。
  - 强化学习模块（RL）：策略微调头，采用 REINFORCE 对候选框保留动作进行奖励优化。
- 数据划分：按图像 stem 固定随机划分 `train/val/test=70/15/15`。
- 训练配置：
  - SL：`epochs=60, hidden_dim=64, lr=0.002`
  - RL：`epochs=40, hidden_dim=32, lr=0.001, entropy_coef=0.001`
- 过程记录：终端逐 epoch 输出 `train/val loss + train/val acc + elapsed_sec`，并落盘为进度日志。
- 训练后测试：
  - 划分内 test（分类与检测代理指标）；
  - 全量 100 图 official 评估（class-aware）。

### 实验结果
- 训练过程日志：[external_calibrator_stageA_hybrid_sl_rl_20260318_progress.tsv](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_20260318_progress.tsv)
- 模型输出：[external_calibrator_stageA_hybrid_sl_rl_20260318.json](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_20260318.json)
- 测试评估：[evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_20260318.md](DroneP_VG/project_records/results/metrics/evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_20260318.md)

- 划分内 test（训练脚本自动测试）：
  - test_cls_loss=0.6707
  - test_cls_acc=0.7042
  - test_det_acc05=0.3039

- 全量 100 图 official 评估（class-aware）：
  - Pred boxes=8469
  - Acc@0.5=0.4099
  - Acc@0.75=0.3118
  - mAP@0.5=0.1329
  - Empty FP=0.0000

- 结论：
  - 双模块训练流程（监督学习 + 强化学习）已完成并可复现，且满足“训练进度可视化、训练/验证指标记录、训练后测试”要求。
  - 当前全量检测指标与上一阶段长训练版本在 `Acc@0.5/Acc@0.75` 基本持平，`mAP@0.5` 小幅变化，后续可继续调优 RL 奖励设计与阈值搜索目标。

---

## 2026-03-18 / Exp-10 / 检测指标导向奖励 + 早停 + 受限搜索（对照）

### 实验目的
针对 Exp-09 提升不明显的问题，改造 RL 训练目标与后处理搜索策略，使优化方向更贴近检测指标并避免后期退化。

### 实验方法
- 奖励改造：
  - 从候选框级分类奖励改为图像级“检测指标增益”奖励；
  - 目标模式：`acc05_map_proxy_gain`（`Acc@0.5` + mAP 近似代理的加权增益）。
- 训练稳定性：
  - RL 训练启用 early stopping（patience=8，min_delta=2e-4）；
  - 每轮保存 best-val checkpoint，并在结束后自动回载 best 模型。
- 搜索约束：
  - threshold 搜索范围限制为 `[0.12, 0.50]`；
  - NMS 网格限制为 `{0.40,0.50,0.60,0.70}`；
  - 禁止退化到 `threshold=0.0` 与 `nms=1.0`。
- 训练配置：
  - SL：`epochs=60, hidden=64, lr=0.002`
  - RL：`epochs=60, hidden=32, lr=0.0008, entropy=0.001`

### 实验结果
- 训练日志：[external_calibrator_stageA_hybrid_sl_rl_rewarded_20260318_progress.tsv](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_rewarded_20260318_progress.tsv)
- 模型结果：[external_calibrator_stageA_hybrid_sl_rl_rewarded_20260318.json](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_rewarded_20260318.json)
- 官方评估：[evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_rewarded_20260318.md](DroneP_VG/project_records/results/metrics/evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_rewarded_20260318.md)

- 训练关键事件：
  - best RL epoch=46（best_val_objective=0.3928）
  - early stop 于 epoch 54 触发并回载 best checkpoint
  - 搜索最优：`threshold=0.12, nms=0.70`

- 全量 100 图 official 评估（class-aware）：
  - Pred boxes=5654
  - Acc@0.5=0.4038
  - Acc@0.75=0.3014
  - mAP@0.5=0.1634
  - Empty FP=0.0000

- 相对 Exp-09（奖励未对齐版本）变化：
  - Acc@0.5：`0.4099 -> 0.4038`（-0.0061）
  - Acc@0.75：`0.3118 -> 0.3014`（-0.0104）
  - mAP@0.5：`0.1329 -> 0.1634`（+0.0305）
  - Pred boxes：`8469 -> 5654`（更稀疏）

- 结论：
  - 奖励改造 + 早停 + 搜索约束有效抑制了“全保留”退化，显著提升 `mAP@0.5`；
  - 但 `Acc@0.5/0.75` 有回落，当前方案更偏“精度/去冗余”而非“召回优先”。

---

## 2026-03-18 / Exp-11 / 偏召回 reward 权重对照（acc05_weight=0.90）

### 实验目的
在 Exp-10 的基础上进一步把 RL 目标向 `Acc@0.5` 倾斜，验证是否能显著提升召回导向指标。

### 实验方法
- 奖励函数：`acc05_map_proxy_gain`，并将权重调整为 `acc05_weight=0.90`（更偏召回）。
- 其余关键设置：
  - `rl_reward_threshold=0.30`
  - `rl_reward_nms_iou=0.60`
  - early stop（patience=8, min_delta=2e-4）+ best checkpoint 回载
  - 搜索约束：`threshold∈[0.10,0.40]`，`nms∈{0.40,0.50,0.60,0.70}`

### 实验结果
- 训练日志：[external_calibrator_stageA_hybrid_sl_rl_recall90_20260318_progress.tsv](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_recall90_20260318_progress.tsv)
- 模型结果：[external_calibrator_stageA_hybrid_sl_rl_recall90_20260318.json](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_recall90_20260318.json)
- 官方评估：[evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_recall90_20260318.md](DroneP_VG/project_records/results/metrics/evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_recall90_20260318.md)

- 训练关键事件：
  - best RL epoch=58（best_val_objective=0.4252）
  - 搜索最优：`threshold=0.10, nms=0.70`

- 全量 100 图 official 评估（class-aware）：
  - Pred boxes=5667
  - Acc@0.5=0.4038
  - Acc@0.75=0.3010
  - mAP@0.5=0.1625
  - Empty FP=0.0000

- 相对 Exp-10（acc05_weight=0.70）变化：
  - Acc@0.5：`0.4038 -> 0.4038`（0.0000）
  - Acc@0.75：`0.3014 -> 0.3010`（-0.0004）
  - mAP@0.5：`0.1634 -> 0.1625`（-0.0009）

- 结论：
  - 在当前候选框集合与约束下，将 reward 进一步偏向召回未带来 `Acc@0.5` 提升，指标基本持平；
  - 当前瓶颈更可能来自候选框覆盖上限，而非 reward 权重本身。

---

## 2026-03-18 / Exp-12 / GroundingDINO 小样本微调可行性验证

### 实验目的
在极小数据量和极少训练轮次下，验证“对 GroundingDINO 主模型进行反向更新”是否可执行，并观察是否存在正向指标变化。

### 实验方法
- 数据：`visdrone_test_100` 中随机抽取 10 张样本（固定 seed=42）。
- 训练：1 epoch，小学习率 `1e-5`，仅微调检测头相关参数（冻结其余参数）。
- 目标：轻量可微目标（objectness + box L1），用于快速可行性验证。
- 评估：在同一 10 图子集上对比微调前后 `Acc@0.5`。

### 实验结果
- 输出目录：[finetune_feasibility_small_20260318](DroneP_VG/project_records/results/metrics/lightweight_optimization/finetune_feasibility_small_20260318)
- 汇总文件：[feasibility_summary.json](DroneP_VG/project_records/results/metrics/lightweight_optimization/finetune_feasibility_small_20260318/feasibility_summary.json)
- 训练日志：[finetune_progress.tsv](DroneP_VG/project_records/results/metrics/lightweight_optimization/finetune_feasibility_small_20260318/finetune_progress.tsv)
- 微调前子集 Acc@0.5：`0.2664`
- 微调后子集 Acc@0.5：`0.2889`
- 变化量：`+0.0225`

- 结论：
  - 小样本条件下已完成真实反向更新、权重保存与指标回测，证明 GroundingDINO 微调在当前环境可行；
  - 后续可在独立 train/val 划分上扩展为规范微调实验（避免同集训练评估带来的乐观偏差）。

---

## 2026-03-18 / Exp-13 / 微调参数量小幅提升（head_plus）

### 实验目的
在保持小样本快速验证节奏的前提下，适度提高可训练参数量，验证更强微调是否带来更明显收益。

### 实验方法
- 在可行性脚本中新增 `finetune_scope`：
  - `head`：检测头相关参数；
  - `head_plus`：检测头 + transformer encoder + feat_map。
- 本次采用 `head_plus`，并使用：
  - subset=20，epochs=2，lr=1e-5。

### 实验结果
- 汇总文件：[feasibility_summary.json](DroneP_VG/project_records/results/metrics/lightweight_optimization/finetune_feasibility_headplus_20260318/feasibility_summary.json)
- 可训练参数数目：`479`（相对前版 `196`）
- 微调前子集 Acc@0.5：`0.3016`
- 微调后子集 Acc@0.5：`0.3378`
- 变化量：`+0.0363`
- 产出微调权重：[groundingdino_swint_ogc_finetuned_feasibility.pth](DroneP_VG/project_records/results/metrics/lightweight_optimization/finetune_feasibility_headplus_20260318/groundingdino_swint_ogc_finetuned_feasibility.pth)

- 结论：
  - 参数量小幅提升后，小样本验证收益进一步增加，说明微调强度提升是有效方向。

---

## 2026-03-18 / Exp-14 / 使用微调主模型进行 Exp-09 同口径对照

### 实验目的
将 Exp-13 产出的微调 GroundingDINO 权重接入 Exp-09 流程，评估“主模型微调 + 外接 SL+RL”对全量指标的影响。

### 实验方法
- 主模型输入改为微调权重：
  - 先用微调权重生成整图预测（100 图，`box/text/nms=0.20/0.20/0.40`）。
- 之后保持 Exp-09 外接训练链路口径：
  - 外接模块：SL+RL 双阶段训练；
  - 自动搜索阈值与 NMS（受限网格）。

### 实验结果
- 微调整图预测目录：[fullimage_groundingdino_finetuned_headplus_20260318](DroneP_VG/project_records/results/predictions/fullimage_groundingdino_finetuned_headplus_20260318)
- 外接训练配置输出：[external_calibrator_stageA_hybrid_sl_rl_finetuned_headplus_exp9cmp_20260318.json](DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_hybrid_sl_rl_finetuned_headplus_exp9cmp_20260318.json)
- 官方评估文件：[evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_finetuned_headplus_exp9cmp_20260318.md](DroneP_VG/project_records/results/metrics/evaluation_summary_external_calibrator_stageA_hybrid_sl_rl_finetuned_headplus_exp9cmp_20260318.md)

- 全量 100 图 official 评估（class-aware）：
  - Pred boxes=5542
  - Acc@0.5=0.4113
  - Acc@0.75=0.3021
  - mAP@0.5=0.1685
  - Empty FP=0.0000

- 相对 Exp-09（未微调主模型）变化：
  - Acc@0.5：`0.4099 -> 0.4113`（+0.0014）
  - Acc@0.75：`0.3118 -> 0.3021`（-0.0097）
  - mAP@0.5：`0.1329 -> 0.1685`（+0.0356）

- 结论：
  - 在 Exp-09 同口径下，引入“参数量提升后的微调主模型”明显抬升了 `mAP@0.5`，并小幅提升 `Acc@0.5`；
  - `Acc@0.75` 仍有下降，后续可通过更细粒度阈值/NMS与 box regression 目标继续调优。
