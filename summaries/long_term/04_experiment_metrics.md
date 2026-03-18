# 长期总结-实验指标与结果

## 1. 100 张评估子集默认基线

目的：建立可复现的零样本整图推理基线。

默认配置：

- box_threshold = 0.25
- text_threshold = 0.20
- nms_threshold = 0.50

结果文件：

- 预测目录：[baseline_groundingdino_preds](/home/wangzhe/DroneP_VG/project_records/results/predictions/baseline_groundingdino_preds)
- class-aware 指标：[evaluation_summary_groundingdino_baseline.md](/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_baseline.md)
- class-unaware 指标：[evaluation_summary_groundingdino_baseline_class_unaware.md](/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_baseline_class_unaware.md)

默认基线完整指标：

- 图像处理完成：100/100
- 预测文件数：100
- 非空预测文件数：100
- 总预测框数：2786
- class-aware Acc@0.5 = 0.3077 (1562/5077)
- class-aware Acc@0.75 = 0.2444 (1241/5077)
- class-aware mAP@0.5 = 0.1367
- Empty-image false positive rate = 0.0000 (0/0)
- class-unaware Acc@0.5 = 0.4359
- class-unaware Acc@0.75 = 0.3319
- class-unaware mAP@0.5 = 0.3367

## 2. 阈值搜索与当前最佳基线

目的：在不改模型结构前提下，提高 class-aware 基线。

搜索信息：

- 搜索范围：6 组配置
- 搜索汇总：[search_summary.md](/home/wangzhe/DroneP_VG/project_records/results/search/search_summary.md)
- 搜索表格：[search_summary.tsv](/home/wangzhe/DroneP_VG/project_records/results/search/search_summary.tsv)
- 完整运行记录：[full_runs](/home/wangzhe/DroneP_VG/project_records/results/search/full_runs)

最佳配置：

- box_threshold = 0.20
- text_threshold = 0.20
- nms_threshold = 0.40

最佳基线结果：

- 预测目录：[best_groundingdino_preds](/home/wangzhe/DroneP_VG/project_records/results/predictions/best_groundingdino_preds)
- 指标文件：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_best_class_aware.md)
- 总预测框数：3648
- class-aware Acc@0.5 = 0.3443 (1748/5077)
- class-aware Acc@0.75 = 0.2624 (1332/5077)
- class-aware mAP@0.5 = 0.1451
- Empty-image false positive rate = 0.0000 (0/0)

相对默认基线提升：

- Acc@0.5：+0.0366
- Acc@0.75：+0.0180
- mAP@0.5：+0.0084
- 预测框数：2786 -> 3648

## 3. 2026-03-18 维护更新

- 已核对当前对照线指标与结果文件路径一致。
- 本次新增两组结果并完成归档：
	- sliding-window bestcfg（class-aware）：Acc@0.5 = 0.3760 (1909/5077), Acc@0.75 = 0.2813 (1428/5077), mAP@0.5 = 0.1517
	- Stage A 外接模块长训练（class-aware）：Acc@0.5 = 0.4099 (2081/5077), Acc@0.75 = 0.3118 (1583/5077), mAP@0.5 = 0.1322
- 对照说明：
	- 当前 best 整图基线：Acc@0.5 = 0.3443, Acc@0.75 = 0.2624, mAP@0.5 = 0.1451
	- Stage A 长训练在 Acc 指标上显著高于整图基线，但 mAP@0.5 低于滑窗与整图最佳配置。
