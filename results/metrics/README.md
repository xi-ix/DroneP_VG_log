# Metrics 文档说明

本目录只放“指标与分析文档”，不放原始预测数据。

## 子目录功能

- baseline_full_image/
  - 整图推理基线指标（含 class-aware 与 class-unaware）。

- sliding_window_experiments/
  - 滑窗相关实验指标。
  - 包含：单次滑窗评估、9 组网格评估、网格汇总（md/tsv）。

- lightweight_optimization/
  - 轻量优化与反馈逻辑文档。
  - 包含：融合保底实验、反馈策略日志、反馈阶段总结。

- runtime_benchmark/
  - 运行效率基准。
  - 包含：总耗时、单图耗时、峰值显存（md/tsv）。

- case_analysis/
  - 成功/失败案例分析清单与失败案例统计文档。

## 命名建议

- 评估总结：`evaluation_summary_<method>_<date>.md`
- 汇总表：`<topic>_summary_<date>.md/.tsv`
- 运行基准：`runtime_profile_<date>.md/.tsv`
- 案例清单：`showcase_<type>_<date>.md`
