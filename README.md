# Project Records 文档说明（精简版）

本目录只保留一套清晰规则：

1. 实验阶段报告只维护一个文件：`logs/experiment_report.md`
2. `summaries/` 仅保留 `long_term/` 下长期文档（不做阶段汇总）
3. 其他实验证据统一放在 `results/`

## 目录与文档功能

- `assets/`
  - 功能：静态资料与开题材料（如 `plan.pdf`）。

- `protocols/`
  - 功能：流程规范文档。
  - 关键文件：`protocols/progress_save_protocol.md`

- `logs/`
  - 功能：过程日志与唯一实验报告。
  - `logs/experiment_report.md`：唯一阶段实验报告（每次仅追加“目的/方法/结果”）。
  - `logs/session_log_YYYYMMDD.md`：会话记录。
  - `logs/session_handoff_YYYYMMDD.md`：交接记录。

- `summaries/long_term/`
  - 功能：长期稳定知识库（目录、脚本、数据、指标、环境、结论）。
  - 说明：该路径下文件保持不动。

- `results/`
  - 功能：实验原始产物与结果证据。
  - `results/metrics/`：评估与分析文档（按子目录分组）。
  - `results/predictions/`：预测文件目录。
  - `results/search/`：参数搜索产物。
  - `results/report_subset_120_assets/`：汇报图与数据画像资产。

## 当前阅读建议

1. 先看：`logs/experiment_report.md`
2. 再看：`results/metrics/` 对应指标文件
3. 需要背景时看：`summaries/long_term/`

## 维护规则

- 新实验完成后，只更新 `logs/experiment_report.md`（目的/方法/结果三段）。
- 不再新增阶段性总结文件到 `summaries/`。
- 结果文件按现有路径分类存放，确保可复现。

