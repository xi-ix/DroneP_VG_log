# VisDrone 120 张报告子集综合报告

日期：2026-03-15

## 1. 目的

本报告用于汇总当前阶段在 VisDrone 子集上的两类结果：

- 100 张正样本测试子集上的 GroundingDINO 零样本基线指标
- 120 张报告子集（100 正样本 + 20 负样本）的数据画像统计

当前目标不是追求最终最优精度，而是形成一条可复现的基线实验闭环，用于中期汇报与后续调参。

## 2. 最佳 GroundingDINO class-aware 基线

本轮在 100 张测试子集上完成了 6 组阈值搜索，最佳配置为：

- box_threshold = 0.20
- text_threshold = 0.20
- nms_threshold = 0.40
- 搜索汇总：[search_summary.md](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.md)

最佳 class-aware 指标如下：

- Acc@0.5 = 0.3443 (1748/5077)
- Acc@0.75 = 0.2624 (1332/5077)
- mAP@0.5 = 0.1451
- Empty-image false positive rate = 0.0000 (0/0)
- 指标文件：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md)

相对于上一版默认配置基线（0.25 / 0.20 / 0.50）：

- Acc@0.5：0.3077 -> 0.3443，提升 0.0366
- Acc@0.75：0.2444 -> 0.2624，提升 0.0180
- mAP@0.5：0.1367 -> 0.1451，提升 0.0084

说明：

- 最优配置提高了召回与总体匹配数，但提升幅度有限。
- 这与当前方法属于零样本开放词汇检测、而数据中小目标比例较高有关。
- 目前结果可作为后续切片策略、提示词优化或专用检测器对比的基准线。

## 3. 120 张报告子集数据画像

报告子集构成：

- 图像总数：120
- 正样本：100（83.33%）
- 负样本：20（16.67%）
- 总有效框数：5077
- 每图平均目标数：42.31
- 每张正样本图平均目标数：50.77
- 小目标占比：61.32%
- 小目标阈值：bbox_area / image_area < 0.0010
- 平均图像尺寸：1495.1 x 924.9

主要类别分布（按框数）：

- Class 4: 1914
- Class 1: 1312
- Class 10: 421
- Class 2: 394
- Class 5: 393
- Class 6: 384

画像产物：

- 数据日志：[data_processing_log.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/data_processing_log.md)
- 分布图：[data_distribution.png](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/data_distribution.png)
- 切片展示：[slice_showcase.png](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/slice_showcase.png)

## 4. 当前阶段结论

当前已经完成一条可复现的实验路径：

- 子集抽样完成
- GroundingDINO 基线推理可运行
- 评估脚本可输出 class-aware 指标
- 120 张报告子集画像已生成

从结果看，当前基线可以稳定检出一部分目标，但 class-aware 指标仍偏低。结合数据画像，可以初步判断瓶颈主要来自：

- 小目标比例高
- 零样本类别语义与 VisDrone 标签不完全对齐
- 当前仍在整图级别推理，尚未结合滑窗切片检测

因此，当前最合理的下一步不是继续微调少量阈值，而是进入更高收益的改进方向，例如：

- 在 100 张子集上引入滑窗切片推理
- 优化类别提示词与同义词映射
- 将 GroundingDINO 结果作为伪标注或候选框，与专用检测器做对比

## 5. 可直接用于汇报的表述

当前已在 VisDrone 子集上完成零样本 GroundingDINO 基线实验。通过 6 组阈值搜索，最佳 class-aware 基线达到 Acc@0.5 = 0.3443、Acc@0.75 = 0.2624、mAP@0.5 = 0.1451。与此同时，120 张报告子集的数据画像显示，小目标占比达到 61.32%，说明当前任务难点主要集中在密集小目标场景。下一阶段将重点转向滑窗切片检测与提示词优化，以提升航拍场景下的检测效果。