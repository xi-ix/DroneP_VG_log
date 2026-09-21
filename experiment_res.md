# 历次实验关键指标总表

说明：
- 本文件统一保存所有实验的三个关键指标：`Acc@0.5`、`Acc@0.75`、`mAP@0.5`。
- 默认记录 `full_metrics`（全量口径）。
- 若某实验暂无三项指标，则以 `N/A` 标注并给出原因。
- 最近核验：2026-05-09 已完成 Exp21 小数据与 1000 数据两条线重跑（`run_exp21_preprocess_exp14_20260509.py`、`run_exp21_preprocess_exp14_large_20260509.py`）；Exp14/Exp16/Exp20/Exp21 指标已逐项核对。

| 实验 | 数据口径 | Acc@0.5 | Acc@0.75 | mAP@0.5 | 指标来源 | 备注 |
| --- | --- | ---: | ---: | ---: | --- | --- |
| GroundingDINO baseline（RefDrone100） | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.1038 | `baseline/groundingdino_base_refdrone100_recovered_20260421/log/groundingdino_base_refdrone100_recovered_20260421_summary.json` | 100 图口径 baseline |
| Exp13 全量重构 | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.1038 | `exp13_full_rebuild_20260421/log/exp13_full_rebuild_20260421_summary.json` | 当前产物与 100 图 baseline 数值一致 |
| Exp14 全量重构 | visdrone_test_100（100 图） | 0.3081 | 0.2405 | 0.1069 | `exp14_full_rebuild_20260421/log/exp14_full_rebuild_20260421_summary.json` | 以可复现实验产物为准 |
| Exp20 小数据集奖励重构 | visdrone_test_100（100 图） | 0.3081 | 0.2405 | 0.1062 | `exp20_small_reward_20260507/log/exp20_small_reward_20260507_summary.json` | 在 Exp14 基础上仅修改 RL reward |
| Exp15 语言注意力层 | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.0993 | `exp15_lang_attention_20260422/log/exp15_lang_attention_20260422_summary.json` | 结构变体 |
| Exp16 注意力+大MLP | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.1011 | `exp16_attn_bigmlp_20260422/log/exp16_attn_bigmlp_20260422_summary.json` | 2026-05-09 架构更新后重跑，mAP 较旧版回升 |
| Exp21 Exp14+双输入预处理 | visdrone_test_100（100 图） | 0.3069 | 0.2400 | 0.1049 | `exp21_preprocess_exp14_20260509/log/exp21_preprocess_exp14_20260509_summary.json` | 独立新脚本，图像/语言预处理后 mAP 高于 Exp16，低于 Exp14 |
| Exp21 Exp14+双输入预处理（1000图） | VisDroneSplit1000Guarded（全量） | 0.4394 | 0.2930 | 0.0934 | `exp21_preprocess_exp14_large_20260509/log/exp21_preprocess_exp14_large_20260509_summary.json` | 1000 图扩容版，较 Exp18 mAP 小幅下降 |
| Exp17 更大数据集守卫 | VisDroneSplit1000Guarded（数据构建） | N/A | N/A | N/A | `exp17_large_dataset_guard_20260422/log/` | 数据集构建实验，无检测评估三指标 |
| Exp18 大数据版 Exp14 方法 | VisDroneSplit1000Guarded（全量） | 0.4395 | 0.2930 | 0.0962 | `exp18_exp14_method_large_data_20260423/log/exp18_exp14_method_large_data_20260423_summary.json` | 候选覆盖修复后结果 |
| Exp19 召回增强版 | VisDroneSplit1000Guarded（全量） | 0.4635 | 0.3032 | 0.0813 | `exp19_recall_union_20260506/log/exp19_recall_union_20260506_summary.json` | 多阈值并集 + 高 top-K + 统一 NMS |
| Exp23 真实指标驱动融合（非单模型） | VisDroneSplit1000Guarded（全量） | 0.4650 | 0.3102 | 0.1295 | `exp23_metric_driven_fusion_20260512/log/exp23_metric_driven_fusion_20260512_summary.json` | Exp18 + GroundingDINO baseline 预测融合；后处理上限参考，不代表单模型能力 |
| Exp25 Ranking Loss 蒸馏消融 | VisDroneSplit1000Guarded（全量） | 0.4661 | 0.3110 | 0.0945 | `exp25_ranked_teacher_distill_20260515/log/exp25_ranked_teacher_distill_20260515_summary.json` | 本表 mAP 记 continuous AP；VOC2007 11-point mAP=0.1288。排序损失作为消融保留 |
| Exp26 A/B 双区域上下文细化 | VisDroneSplit1000Guarded（全量） | 0.4670 | 0.3124 | 0.1015 | `exp26_context_refine_dual_region_20260515/log/exp26_context_refine_dual_region_20260515_summary.json` | A=context 2.0x，B=refine 0.7x；VOC2007 mAP=0.1314，说明双区域方向有弱收益 |
| Exp27 语言增强 A/B 双区域 | VisDroneSplit1000Guarded（全量） | 0.4677 | 0.3121 | 0.1001 | `exp27_lang_context_refine_dual_region_20260515/log/exp27_lang_context_refine_dual_region_20260515_summary.json` | 加类别 prompt hash 与语义先验；VOC2007 mAP=0.1341，但 continuous AP 略降 |
| Exp28 真 query 输入 Grounding Scorer | VisDroneSplit1000Guarded（全量） | 0.4676 | 0.3121 | 0.1004 | `exp28_text_query_grounding_scorer_20260515/log/exp28_text_query_grounding_scorer_20260515_summary.json` | 首版 `image + text query + candidates -> query-box score`；VOC2007 mAP=0.1295，链路跑通但 hash text 较弱 |
| Exp29 词表文本编码 + Grounding TopK | VisDroneSplit1000Guarded（全量） | 0.4677 | 0.3123 | 0.1025 | `exp29_vocab_text_grounding_eval_20260515/log/exp29_vocab_text_grounding_eval_20260515_summary.json` | learnable vocab text encoder；VOC2007 mAP=0.1242；Grounding R@1=0.2979、R@5=0.4825、R@10=0.5791 |
| GroundingDINO baseline（Split1000 全量） | VisDroneSplit1000Guarded（全量） | 0.3391 | 0.2600 | 0.1099 | `baseline/groundingdino_base_visdrone_split1000guarded_20260429/log/groundingdino_base_visdrone_split1000guarded_20260429_summary.json` | 扩容数据口径 baseline |
| GroundingDINO baseline（Split1000 test-only） | VisDroneSplit1000Guarded（仅 test） | 0.3607 | 0.2779 | 0.1255 | `baseline/groundingdino_base_visdrone_split1000guarded_test_only_20260429/log/groundingdino_test_only_20260429_summary.json` | 仅 test split，不可与 full 直接等价对比 |

## 使用说明
- 跨实验对比时请优先在同一数据口径内比较（100 图 vs 1000 图不要混比）。
- 任何历史“高分”若无法由当前 `summary.json` 或评估报告复现，不纳入本表主结论。

## 2026-05-07 当日复跑补充（Exp20）
- 运行命令：`cd /home/wangzhe/DroneP_VG && RL_EPOCHS=20 MATCH_IOU=0.5 python experiment/exp20_reward_4amp/scripts/run_exp20_small_reward_20260507.py`
- 关键训练信号：RL reward 非零（约 `0.33~0.36`），梯度范数非零（日志可见），说明奖励链路生效。
- 当前记录口径仍以 `exp20_small_reward_20260507_summary.json` 的 `full_metrics` 为准：Acc@0.5=`0.3081`、Acc@0.75=`0.2405`、mAP@0.5=`0.1062`。

## 2026-05-15/16 语言定位链路补充（Exp25-Exp29）

### Exp25 Ranking Loss 蒸馏消融
- 目录：`experiment/exp25_ranked_teacher_distill_20260515`
- 目的：在 Exp24 teacher distillation 基础上加入同图正负 pair ranking loss，验证排序约束是否改善 mAP。
- full 指标：Acc@0.5=`0.4661`、Acc@0.75=`0.3110`、continuous mAP@0.5=`0.0945`、VOC2007 11-point mAP@0.5=`0.1288`。
- 结论：VOC2007 口径略高于恢复版 Exp24，但 continuous AP 和 Acc 下降；作为消融保留，不替代主线。

### Exp26 A/B 双区域上下文细化
- 目录：`experiment/exp26_context_refine_dual_region_20260515`
- 目的：实现“两步图像区域处理”：A 为候选框外扩上下文区域，B 为候选框中心细化区域，将 A/B 统计和 B-A 差异输入 scorer。
- full 指标：Acc@0.5=`0.4670`、Acc@0.75=`0.3124`、continuous mAP@0.5=`0.1015`、VOC2007 11-point mAP@0.5=`0.1314`。
- 结论：A/B 双区域方向有效但收益有限，后续应换成更强视觉特征而不是继续堆 RGB 统计。

### Exp27 语言增强 A/B 双区域
- 目录：`experiment/exp27_lang_context_refine_dual_region_20260515`
- 目的：在 Exp26 上加入类别 prompt hash、小目标先验和 person/vehicle 语义分组。
- full 指标：Acc@0.5=`0.4677`、Acc@0.75=`0.3121`、continuous mAP@0.5=`0.1001`、VOC2007 11-point mAP@0.5=`0.1341`。
- 结论：历史 VOC2007 口径继续小涨，但 continuous AP 未改善；说明固定伪语言先验不足以解决排序问题。

### Exp28 真 query 输入 Grounding Scorer
- 目录：`experiment/exp28_text_query_grounding_scorer_20260515`
- 目的：第一次把模型形式改成真正 `image + text query + candidate boxes -> query-box matching score`。之前 Exp25-27 的语言信号均以固定伪先验方式注入，Exp28 是首次让文本 query 作为动态输入参与打分。
- 模型架构：4 层 MLP（`input_dim → 96 → 96 → 48 → 1`），带 LayerNorm + ReLU。
- 输入特征拼接：`[box geometry (11)] + [A context RGB stats (8)] + [B refined RGB stats (8)] + [B-A delta (8)] + [area ratios (2)] + [query semantic priors (7)] + [hashed text embedding (24)]`，共约 60 维。
- 文本表示：
  - **Hashed text embedding（24 维）**：分词后对每个 token 计算 bucket hash（`sum(ord(ch)) % 24`）和 sign hash（`sum(ord(ch) * (idx+1)) % 2 → ±1`），归一化得到 24 维向量。
  - **Query semantic priors（7 维）**：`is_small_target`、`is_vehicle`、`is_person`、`compactness_prior`、`contrast_gain × is_small_target`、`context_contrast × is_vehicle`、`refine_contrast × is_person`。
- Query 模板：10 类 × 3 模板 = 30 个 query 训练；推理仅用 canonical query（每类 1 个，如 `pedestrian`、`car`、`motor`）。
- 训练目标：`loss = BCE(gt_label) + 0.7 × BCE(teacher_soft_label)`，仅当 query class == 候选框 class 时计算 label。
- 后处理：val 集搜索 threshold ∈ {0.03, 0.04, 0.05, 0.06, 0.08}、NMS ∈ {0.45, 0.50, 0.55, 0.60}，objective = `0.5 × Acc@0.5 + 0.5 × mAP@0.5`。
- Best checkpoint：epoch 22，val objective=0.2956；best postprocess：threshold=0.03, NMS=0.60。
- full 指标：Acc@0.5=`0.4676`、Acc@0.75=`0.3121`、continuous mAP@0.5=`0.1004`、VOC2007 11-point mAP@0.5=`0.1295`、预测框数=123823。
- 训练观察：val Acc@0.5 几乎不变（~0.481），分类能力主要来自视觉特征；val mAP@0.5 在 0.098~0.110 波动；hash text 区分度有限，不同类别 query hash 可能碰撞。
- 结论：Exp29 是目前最接近最终“语言 + 图片定位目标”目标的一版。下一步优先接入 `RefDrone` / `AerialVG` 真实语言标注或更强预训练文本/视觉语言编码器。
