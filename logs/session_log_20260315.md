# DroneP_VG 工作日志

日期：2026-03-15

## 1. 当前阶段已完成工作

### 1.1 数据子集与画像资产已完成

- 已完成 100 张评估子集修复，当前为 100 张图像 + 100 个标注文件
- 已完成 100 张评估子集的数据画像生成
- 已完成 120 张报告子集构建，当前为 100 张正样本 + 20 张负样本
- 已完成 120 张报告子集的数据画像生成

关键统计：

- 100 张评估子集总有效框数：5077
- 100 张评估子集每图平均目标数：50.77
- 120 张报告子集每图平均目标数：42.31
- 小目标占比：61.32%
- 小目标阈值：bbox_area / image_area < 0.0010

### 1.2 工具链已打通

- [import_cv2.py](/home/wangzhe/DroneP_VG/import_cv2.py)：支持高分辨率图像滑窗切片与标签映射
- [analyze_extracted_dataset.py](/home/wangzhe/DroneP_VG/analyze_extracted_dataset.py)：可生成数据画像日志、分布图和切片展示样例
- [evaluate_detection_metrics.py](/home/wangzhe/DroneP_VG/evaluate_detection_metrics.py)：可计算 Acc@0.5、Acc@0.75、mAP@0.5、空图误报率
- [run_groundingdino_baseline.py](/home/wangzhe/DroneP_VG/run_groundingdino_baseline.py)：可批量运行 GroundingDINO 并输出评估脚本可读预测文件
- [search_groundingdino_thresholds.py](/home/wangzhe/DroneP_VG/search_groundingdino_thresholds.py)：可自动完成多组阈值搜索并汇总结果

### 1.3 GroundingDINO 基线实验已完成

- 已确认权重文件完整可用：[/home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth](/home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth)
- 已解决 `transformers` 版本不兼容问题：当前使用 `transformers==4.33.2`、`tokenizers==0.13.3`
- 已定位 CUDA 自定义扩展 `_C` 未编译问题
- 当前 GroundingDINO 基线在 CPU fallback 模式下可稳定运行

### 1.4 100 张评估子集的基线与最优基线已落地

默认基线结果：

- 预测目录：[baseline_groundingdino_preds](/home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds)
- class-aware 指标文件：[evaluation_summary_groundingdino_baseline.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline.md)
- class-unaware 指标文件：[evaluation_summary_groundingdino_baseline_class_unaware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline_class_unaware.md)

默认基线指标：

- Acc@0.5 = 0.3077 (1562/5077)
- Acc@0.75 = 0.2444 (1241/5077)
- mAP@0.5 = 0.1367
- Empty-image false positive rate = 0.0000 (0/0)
- class-unaware Acc@0.5 = 0.4359
- class-unaware Acc@0.75 = 0.3319
- class-unaware mAP@0.5 = 0.3367

### 1.5 阈值搜索已完成并确定当前最佳 class-aware 基线

- 搜索目录：[groundingdino_threshold_search](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search)
- 搜索汇总：[search_summary.md](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.md)
- 搜索规模：6 组配置

当前最佳配置：

- box_threshold = 0.20
- text_threshold = 0.20
- nms_threshold = 0.40

当前最佳 class-aware 指标：

- Acc@0.5 = 0.3443 (1748/5077)
- Acc@0.75 = 0.2624 (1332/5077)
- mAP@0.5 = 0.1451
- Empty-image false positive rate = 0.0000 (0/0)

相较默认基线提升：

- Acc@0.5：+0.0366
- Acc@0.75：+0.0180
- mAP@0.5：+0.0084

最优结果产物：

- 最优预测目录：[best_groundingdino_preds](/home/wangzhe/DroneP_VG/visdrone_test_100/best_groundingdino_preds)
- 最优指标文件：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md)

### 1.6 120 张报告子集综合报告已生成

- 综合报告：[report_summary_20260315.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_summary_20260315.md)
- 画像日志：[data_processing_log.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/data_processing_log.md)
- 分布图：[data_distribution.png](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/data_distribution.png)
- 切片展示：[slice_showcase.png](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/slice_showcase.png)

## 2. 当前有效结论

- 当前已经具备完整的“子集准备 -> 预测生成 -> 指标评估 -> 报告汇总”闭环
- 当前 best GroundingDINO 基线可以稳定复现，但在 VisDrone 密集小目标场景上仍然偏弱
- 当前限制主要不在评估链路，而在零样本整图推理本身对小目标和类别对齐的适配不足
- 后续所有改进实验，都可以以当前 best class-aware 基线作为固定对照线