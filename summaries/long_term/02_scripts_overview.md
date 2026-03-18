# 长期总结-脚本能力与用途

本文件记录可复用脚本、目的和当前可确认结果。

## 1. 数据处理脚本

1. [import_cv2.py](/home/wangzhe/DroneP_VG/scripts/import_cv2.py)
目的：对高分辨率航拍图像执行滑动窗口切片，并将原图 bbox 映射到切片局部坐标。
结果：支持单图/批处理、空切片保留、两种标签格式输出、metadata.csv 和 summary.csv；已兼容 VisDrone 原生逗号标注格式。

2. [analyze_extracted_dataset.py](/home/wangzhe/DroneP_VG/scripts/analyze_extracted_dataset.py)
目的：为抽取后的数据子集生成可直接用于汇报的数据画像。
结果：可生成数据日志、分布图、切片展示和典型窗口样例。

3. [evaluate_detection_metrics.py](/home/wangzhe/DroneP_VG/scripts/evaluate_detection_metrics.py)
目的：对检测结果进行标准化评估，形成统一口径指标。
结果：支持 class-aware / class-unaware 评估，兼容空格格式和 VisDrone 原生逗号格式，可输出 Markdown 摘要。

## 2. GroundingDINO 相关脚本

1. [run_groundingdino_baseline.py](/home/wangzhe/DroneP_VG/scripts/run_groundingdino_baseline.py)
目的：批量运行 GroundingDINO 零样本检测，直接输出评估脚本可读的预测文件。
结果：支持 box_threshold、text_threshold、nms_threshold、max_detections、device 参数；已修复 class_id=None 导致推理中断的问题。

2. [search_groundingdino_thresholds.py](/home/wangzhe/DroneP_VG/scripts/search_groundingdino_thresholds.py)
目的：自动完成多组 GroundingDINO 参数搜索并汇总 class-aware 指标。
结果：可顺序完成多组推理 + 评估，并产出统一的 TSV / Markdown 搜索汇总表。

3. [install_groundingdino_compat.sh](/home/wangzhe/DroneP_VG/scripts/install_groundingdino_compat.sh)
目的：快速安装 GroundingDINO 兼容版本依赖。
结果：可统一安装 transformers==4.33.2 与 tokenizers==0.13.3 并执行版本验证。

## 3. 2026-03-18 维护更新

- 已核对脚本能力描述与当前仓库脚本路径一致。
- 本次未新增脚本功能；现有脚本结论继续有效。
