# DroneP_VG 长期实验总结

本文档用于长期更新，只记录对后续实验真正有帮助的事实、结果、文件与指标，不写后续任务。

## 1. 项目基础能力

### 1.1 数据处理脚本

1. [import_cv2.py](/home/wangzhe/DroneP_VG/import_cv2.py)
目的：对高分辨率航拍图像执行滑动窗口切片，并将原图 bbox 映射到切片局部坐标。
结果：支持单图/批处理、空切片保留、两种标签格式输出、metadata.csv 和 summary.csv 统计输出；已兼容 VisDrone 原生逗号标注格式。

2. [analyze_extracted_dataset.py](/home/wangzhe/DroneP_VG/analyze_extracted_dataset.py)
目的：为抽取后的数据子集生成可直接用于汇报的数据画像。
结果：可生成数据日志、分布图、切片展示和典型窗口样例。

3. [evaluate_detection_metrics.py](/home/wangzhe/DroneP_VG/evaluate_detection_metrics.py)
目的：对检测结果进行标准化评估，形成统一口径指标。
结果：支持 class-aware / class-unaware 评估，兼容空格格式和 VisDrone 原生逗号格式，可输出 Markdown 摘要。

### 1.2 GroundingDINO 相关脚本

1. [run_groundingdino_baseline.py](/home/wangzhe/DroneP_VG/run_groundingdino_baseline.py)
目的：批量运行 GroundingDINO 零样本检测，直接输出评估脚本可读的预测文件。
结果：支持 `box_threshold`、`text_threshold`、`nms_threshold`、`max_detections`、`device` 参数；已修复 `class_id=None` 导致推理中断的问题。

2. [search_groundingdino_thresholds.py](/home/wangzhe/DroneP_VG/search_groundingdino_thresholds.py)
目的：自动完成多组 GroundingDINO 参数搜索并汇总 class-aware 指标。
结果：可顺序完成多组推理 + 评估，并产出统一的 TSV / Markdown 搜索汇总表。

## 2. 数据子集整理结果

### 2.1 100 张评估子集

目的：构建一个可快速验证模型与评估链路的小规模正样本测试集。

结果：

- 目录：[visdrone_test_100](/home/wangzhe/DroneP_VG/visdrone_test_100)
- 状态：100 张图像，100 个标注文件
- 修复：原先缺标注的 20 个 train 样本已替换为 20 个新的带标注样本
- 替换映射：[replacement_map.tsv](/home/wangzhe/DroneP_VG/visdrone_test_100/replaced_missing_backup_20260312/meta/replacement_map.tsv)

关键统计：

- 正样本图像：100
- 负样本图像：0
- 总有效框数：5077
- 每图平均目标数：50.77
- 小目标占比：61.32%
- 小目标阈值：bbox_area / image_area < 0.0010

画像产物：

- [data_processing_log.md](/home/wangzhe/DroneP_VG/visdrone_test_100/data_profile_assets/data_processing_log.md)
- [data_distribution.png](/home/wangzhe/DroneP_VG/visdrone_test_100/data_profile_assets/data_distribution.png)
- [slice_showcase.png](/home/wangzhe/DroneP_VG/visdrone_test_100/data_profile_assets/slice_showcase.png)

### 2.2 120 张报告子集

目的：构建一份更适合汇报展示的数据子集，同时保留负样本场景。

结果：

- 目录：[visdrone_report_subset_120](/home/wangzhe/DroneP_VG/visdrone_report_subset_120)
- 状态：120 张图像，120 个标注文件
- 构成：100 张正样本 + 20 张负样本
- 空标注文件：20 个
- 负样本清单：[report_subset_manifest.tsv](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_subset_manifest.tsv)

关键统计：

- 图像总数：120
- 正样本：100（83.33%）
- 负样本：20（16.67%）
- 总有效框数：5077
- 每图平均目标数：42.31
- 每张正样本图平均目标数：50.77
- 小目标占比：61.32%
- 平均图像尺寸：1495.1 x 924.9

画像产物：

- [data_processing_log.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/data_processing_log.md)
- [data_distribution.png](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/data_distribution.png)
- [slice_showcase.png](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets/slice_showcase.png)
- 综合报告：[report_summary_20260315.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_summary_20260315.md)

## 3. GroundingDINO 环境与兼容性结论

目的：在当前服务器环境中打通 GroundingDINO 零样本基线推理链路。

结果：

- 权重文件已确认完整可用：[groundingdino_swint_ogc.pth](/home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth)
- 权重大小：662MB
- `transformers 5.0.0` 与当前 GroundingDINO 代码不兼容
- 已切换到兼容版本：`transformers==4.33.2`、`tokenizers==0.13.3`
- 当前 CUDA 自定义扩展 `_C` 未编译成功
- 当前 GroundingDINO 推理是在 CPU fallback 模式下完成的

实验意义：

- 当前环境已经足以稳定复现实验结果
- 现阶段的主要性能瓶颈不在“能不能跑通”，而在模型对密集小目标场景的适配能力

## 4. 100 张评估子集上的 GroundingDINO 基线结果

### 4.1 默认基线

目的：先建立一条可复现的零样本整图推理基线。

预测与结果文件：

- 预测目录：[baseline_groundingdino_preds](/home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds)
- class-aware 指标：[evaluation_summary_groundingdino_baseline.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline.md)
- class-unaware 指标：[evaluation_summary_groundingdino_baseline_class_unaware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline_class_unaware.md)

默认配置：

- box_threshold = 0.25
- text_threshold = 0.20
- nms_threshold = 0.50

默认基线结果：

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

结论：

- class-unaware 结果明显高于 class-aware，说明当前误差来源里类别错配占比较高。
- 默认基线能够提供稳定对照线，但对 VisDrone 的密集小目标场景仍偏弱。

### 4.2 阈值搜索后的当前最佳基线

目的：在不改变模型结构的前提下，通过小规模搜索获得更高的 class-aware 基线。

搜索产物：

- 搜索目录：[groundingdino_threshold_search](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search)
- 搜索汇总：[search_summary.md](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.md)
- 搜索表格：[search_summary.tsv](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.tsv)

搜索范围：6 组配置

最佳配置：

- box_threshold = 0.20
- text_threshold = 0.20
- nms_threshold = 0.40

最佳结果：

- 预测目录：[best_groundingdino_preds](/home/wangzhe/DroneP_VG/visdrone_test_100/best_groundingdino_preds)
- 指标文件：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md)
- 总预测框数：3648
- class-aware Acc@0.5 = 0.3443 (1748/5077)
- class-aware Acc@0.75 = 0.2624 (1332/5077)
- class-aware mAP@0.5 = 0.1451
- Empty-image false positive rate = 0.0000 (0/0)

相对默认基线的提升：

- Acc@0.5：+0.0366
- Acc@0.75：+0.0180
- mAP@0.5：+0.0084
- 预测框数：2786 -> 3648

结论：

- 继续做少量阈值微调仍能带来提升，但幅度有限。
- 当前 best class-aware 基线已经可以作为后续整图推理对照线固定使用。

## 5. 对后续实验最有帮助的结论

1. 当前实验闭环已经完整可复现：子集准备、预测生成、指标评估、报告汇总均已打通。

2. 当前 best GroundingDINO class-aware 基线为：
Acc@0.5 = 0.3443，Acc@0.75 = 0.2624，mAP@0.5 = 0.1451。

3. 当前数据难点已经明确：
100 张评估子集和 120 张报告子集的小目标占比都达到 61.32%，这是影响整图零样本检测效果的核心因素之一。

4. 当前误差结构已经明确：
class-unaware 结果明显高于 class-aware，说明类别语义错配是当前基线的重要误差来源。

5. 当前最稳定的对照基线文件为：
- [evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md)
- [search_summary.md](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.md)
- [report_summary_20260315.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_summary_20260315.md)

6. 当前运行条件的事实约束为：
GroundingDINO 在现环境中可以稳定运行，但当前是 CPU fallback；如果后续需要大规模实验，应优先解决 `_C` 扩展编译与 GPU 推理问题。