# 长期总结-数据子集结果

## 1. 100 张评估子集

目的：构建可快速验证模型与评估链路的小规模正样本测试集。

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

## 2. 120 张报告子集

目的：构建更适合汇报展示的数据子集，同时保留负样本场景。

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

- [data_processing_log.md](/home/wangzhe/DroneP_VG/project_records/results/report_subset_120_assets/data_processing_log.md)
- [data_distribution.png](/home/wangzhe/DroneP_VG/project_records/results/report_subset_120_assets/data_distribution.png)
- [slice_showcase.png](/home/wangzhe/DroneP_VG/project_records/results/report_subset_120_assets/slice_showcase.png)
- 综合报告：[report_summary_20260315.md](/home/wangzhe/DroneP_VG/project_records/results/report_summary_20260315.md)

## 3. 2026-03-18 维护更新

- 已完成数据子集统计口径复核：100 张评估子集与 120 张报告子集统计值维持不变。
- 本次未新增抽样或替换操作，现有数据子集结果继续作为后续实验固定输入。
