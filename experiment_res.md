# 历次实验关键指标总表

说明：
- 本文件统一保存所有实验的三个关键指标：`Acc@0.5`、`Acc@0.75`、`mAP@0.5`。
- 默认记录 `full_metrics`（全量口径）。
- 若某实验暂无三项指标，则以 `N/A` 标注并给出原因。
- 最近核验：2026-05-06 已重新运行 Exp14（`run_exp14_full_rebuild_20260421.py`），并与本表中 Exp14 指标逐项核对一致。

| 实验 | 数据口径 | Acc@0.5 | Acc@0.75 | mAP@0.5 | 指标来源 | 备注 |
| --- | --- | ---: | ---: | ---: | --- | --- |
| GroundingDINO baseline（RefDrone100） | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.1038 | `baseline/groundingdino_base_refdrone100_recovered_20260421/log/groundingdino_base_refdrone100_recovered_20260421_summary.json` | 100 图口径 baseline |
| Exp13 全量重构 | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.1038 | `exp13_full_rebuild_20260421/log/exp13_full_rebuild_20260421_summary.json` | 当前产物与 100 图 baseline 数值一致 |
| Exp14 全量重构 | visdrone_test_100（100 图） | 0.3081 | 0.2405 | 0.1069 | `exp14_full_rebuild_20260421/log/exp14_full_rebuild_20260421_summary.json` | 以可复现实验产物为准 |
| Exp15 语言注意力层 | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.0993 | `exp15_lang_attention_20260422/log/exp15_lang_attention_20260422_summary.json` | 结构变体 |
| Exp16 注意力+大MLP | visdrone_test_100（100 图） | 0.3083 | 0.2407 | 0.0969 | `exp16_attn_bigmlp_20260422/log/exp16_attn_bigmlp_20260422_summary.json` | 大模型变体 |
| Exp17 更大数据集守卫 | VisDroneSplit1000Guarded（数据构建） | N/A | N/A | N/A | `exp17_large_dataset_guard_20260422/log/` | 数据集构建实验，无检测评估三指标 |
| Exp18 大数据版 Exp14 方法 | VisDroneSplit1000Guarded（全量） | 0.4395 | 0.2930 | 0.0962 | `exp18_exp14_method_large_data_20260423/log/exp18_exp14_method_large_data_20260423_summary.json` | 候选覆盖修复后结果 |
| Exp19 召回增强版 | VisDroneSplit1000Guarded（全量） | 0.4635 | 0.3032 | 0.0813 | `exp19_recall_union_20260506/log/exp19_recall_union_20260506_summary.json` | 多阈值并集 + 高 top-K + 统一 NMS |
| GroundingDINO baseline（Split1000 全量） | VisDroneSplit1000Guarded（全量） | 0.3391 | 0.2600 | 0.1099 | `baseline/groundingdino_base_visdrone_split1000guarded_20260429/log/groundingdino_base_visdrone_split1000guarded_20260429_summary.json` | 扩容数据口径 baseline |
| GroundingDINO baseline（Split1000 test-only） | VisDroneSplit1000Guarded（仅 test） | 0.3607 | 0.2779 | 0.1255 | `baseline/groundingdino_base_visdrone_split1000guarded_test_only_20260429/log/groundingdino_test_only_20260429_summary.json` | 仅 test split，不可与 full 直接等价对比 |

## 使用说明
- 跨实验对比时请优先在同一数据口径内比较（100 图 vs 1000 图不要混比）。
- 任何历史“高分”若无法由当前 `summary.json` 或评估报告复现，不纳入本表主结论。
