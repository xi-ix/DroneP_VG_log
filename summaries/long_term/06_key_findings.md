# 长期总结-关键结论

本文件只保留对后续实验设计有直接价值的结论。

## 1. 可复现实验闭环已完成

- 子集准备、预测生成、指标评估、报告汇总均已打通。
- 当前结果可稳定复现，并可作为后续对照线。

## 2. 当前 best class-aware 基线

- Acc@0.5 = 0.3443
- Acc@0.75 = 0.2624
- mAP@0.5 = 0.1451
- 结果文件：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_best_class_aware.md)

## 3. 数据难点已明确

- 100 张评估子集与 120 张报告子集的小目标占比均为 61.32%。
- 这对整图零样本检测构成主要难度。

## 4. 误差结构已明确

- class-unaware 指标明显高于 class-aware。
- 类别语义错配是当前误差的重要来源之一。

## 5. 当前稳定对照基线入口

- 最优指标：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_best_class_aware.md)
- 搜索汇总：[search_summary.md](/home/wangzhe/DroneP_VG/project_records/results/search/search_summary.md)
- 综合报告：[report_summary_20260315.md](/home/wangzhe/DroneP_VG/project_records/results/report_summary_20260315.md)

## 6. 2026-03-18 维护更新

- 已确认关键结论在当前阶段仍成立，无需替换。
- 本次未出现可覆盖旧结论的新实验证据。
