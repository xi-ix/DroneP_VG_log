# 长期总结-目录与产物总览

本文件记录项目目录层面的稳定信息与核心产物位置。

## 1. 项目主目录结构

- [AerialVG](/home/wangzhe/DroneP_VG/AerialVG)
- [RefDrone](/home/wangzhe/DroneP_VG/RefDrone)
- [scripts](/home/wangzhe/DroneP_VG/scripts)
- [visdrone_test_100](/home/wangzhe/DroneP_VG/visdrone_test_100)
- [visdrone_report_subset_120](/home/wangzhe/DroneP_VG/visdrone_report_subset_120)
- [project_records](/home/wangzhe/DroneP_VG/project_records)

## 2. 统一记录目录

- 日志目录：[project_records/logs](/home/wangzhe/DroneP_VG/project_records/logs)
- 总结目录：[project_records/summaries](/home/wangzhe/DroneP_VG/project_records/summaries)
- 结果目录：[project_records/results](/home/wangzhe/DroneP_VG/project_records/results)
- 协议目录：[project_records/protocols](/home/wangzhe/DroneP_VG/project_records/protocols)

## 3. 核心实验结果入口

- 最佳 class-aware 指标：[evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_best_class_aware.md)
- 阈值搜索汇总：[search_summary.md](/home/wangzhe/DroneP_VG/project_records/results/search/search_summary.md)
- 120 张综合报告：[report_summary_20260315.md](/home/wangzhe/DroneP_VG/project_records/results/report_summary_20260315.md)

## 4. 2026-03-18 维护更新

- 已确认 `project_records/` 目录结构与用途保持稳定。
- 本次新增关键产物已纳入统一目录：
	- 训练脚本：`/home/wangzhe/DroneP_VG/scripts/train_external_calibrator_stageA.py`
	- 长训练评估：`/home/wangzhe/DroneP_VG/project_records/results/metrics/lightweight_optimization/evaluation_summary_fusion_stageA_extcal_longrun_20260318_class_aware.md`
	- 滑窗评估：`/home/wangzhe/DroneP_VG/project_records/results/metrics/sliding_window_experiments/evaluation_summary_groundingdino_sliding_window_bestcfg_20260318_class_aware.md`
	- 指标说明：`/home/wangzhe/DroneP_VG/project_records/protocols/note.md`
