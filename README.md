# Project Records Bundle (2026-03-15)

本目录用于集中存放当前阶段可直接上传到 Git 的日志、总结与实验结果产物。

## 目录结构

- logs/
  - session_handoff_20260312.md
  - session_handoff_20260314.md
  - session_handoff_20260315.md
  - session_log_20260315.md
- summaries/
  - progress_summary.md
  - experiment_long_term_summary.md
- results/
  - report_summary_20260315.md
  - metrics/
    - evaluation_summary_groundingdino_baseline.md
    - evaluation_summary_groundingdino_baseline_class_unaware.md
    - evaluation_summary_groundingdino_best_class_aware.md
  - search/
    - search_summary.md
    - search_summary.tsv
    - full_runs/groundingdino_threshold_search/ (完整参数搜索产物)
  - report_subset_120_assets/
    - data_processing_log.md
    - data_distribution.png
    - slice_showcase.png
  - predictions/
    - baseline_groundingdino_preds/ (100 张默认基线预测)
    - best_groundingdino_preds/ (100 张当前最佳基线预测)

## 用途说明

- logs/: 每次会话和阶段整理日志
- summaries/: 可长期维护的阶段总结
- results/: 可复现实验结果与汇报素材

## 维护约定

- 后续有用的日志与总结统一保存在 `project_records/` 下。
- 不再在 `DroneP_VG` 根目录单独保留 markdown 日志文件。
- 本目录是当前 Git 仓库根目录，后续提交与推送都在本目录执行。

