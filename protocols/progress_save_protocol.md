# 进度保存执行规范（给 Copilot 的固定指令）

当用户说“按照 progress_save_protocol.md 保存进度”时，按以下流程执行，不跳步。

注意目录是project_records/，不是project_records/summaries/
## 0. 执行目标

在一次任务结束后，完成以下动作：

1. 更新本次会话日志（session_log_YYYYMMDD.md）
2. 更新长期总结（experiment_long_term_summary.md 索引 + long_term/ 分文件）
3. 更新阶段总结（progress_summary.md）
4. 生成/更新当日交接（session_handoff_YYYYMMDD.md）
5. 归档日志与结果到统一目录（project_records/）
6. 完成 Git 提交与推送（用户未明确禁止时执行）

## 1. 文档更新规则

### 1.1 session_log_YYYYMMDD.md

必须包含：

- 本次做了什么
- 这么做的目的
- 产出结果（文件+路径）
- 关键指标（写完整）

### 1.2 长期总结（分文件）

必须追加/更新：

- [experiment_long_term_summary.md](/home/wangzhe/DroneP_VG/project_records/summaries/experiment_long_term_summary.md) 作为索引入口
- [long_term/01_directory_overview.md](/home/wangzhe/DroneP_VG/project_records/summaries/long_term/01_directory_overview.md)
- [long_term/02_scripts_overview.md](/home/wangzhe/DroneP_VG/project_records/summaries/long_term/02_scripts_overview.md)
- [long_term/03_dataset_summary.md](/home/wangzhe/DroneP_VG/project_records/summaries/long_term/03_dataset_summary.md)
- [long_term/04_experiment_metrics.md](/home/wangzhe/DroneP_VG/project_records/summaries/long_term/04_experiment_metrics.md)
- [long_term/05_environment_and_constraints.md](/home/wangzhe/DroneP_VG/project_records/summaries/long_term/05_environment_and_constraints.md)
- [long_term/06_key_findings.md](/home/wangzhe/DroneP_VG/project_records/summaries/long_term/06_key_findings.md)

注意：

- 不写“接下来做什么”
- 删除过时或已被新结果替代的结论

### 1.3 progress_summary.md

同步当前阶段状态：

- 已完成内容（新增项）
- 当前有效结果（更新数字）
- 当前存在问题（真实阻塞）

### 1.4 session_handoff_YYYYMMDD.md

用于下次继续：

- 本次新增完成
- 当前阶段判断
- 当前未完成项
- 下次建议顺序
- 可直接引用的话

## 2. 归档规则

创建目录：

- /home/wangzhe/DroneP_VG/project_records/

至少包含：

- logs/: session_handoff_*.md, session_log_*.md
- summaries/: progress_summary.md, experiment_long_term_summary.md, long_term/*.md
- results/: 本次新增指标文件、报告文件、图表、关键预测结果目录

要求：

- 采用复制（cp），不移动原文件
- 写一个 README.md 说明目录结构和用途

## 3. Git 操作规则

当前环境约束：

- 当前仓库位于服务器侧。
- 推送到 GitHub 时需要走本机代理。
- 本机代理端口为 `7897`。
- 若直接 `git push` 失败，优先检查/使用代理配置，而不是反复长时间重试。

在用户未明确要求“只整理不提交”时，执行：

```bash
cd /home/wangzhe/DroneP_VG/project_records
git status
git add -A
git commit -m "docs: update progress logs and experiment summaries (YYYY-MM-DD)"
git push
```

如推送异常，优先做短时检查：

```bash
cd /home/wangzhe/DroneP_VG/project_records
git config --global http.proxy socks5://127.0.0.1:7897
git config --global https.proxy socks5://127.0.0.1:7897
git push
```

若 push 失败：

- 记录失败原因
- 给出下一步可执行命令
- 不做 destructive 操作

## 4. 交付输出格式

完成后给用户回报：

1. 更新了哪些文件（路径）
2. 新增了哪些结果文件（路径）
3. 关键指标（完整数值）
4. Git 状态（是否已 commit、是否已 push）

## 5. 快速触发短语

用户可直接说：

- “按照 progress_save_protocol.md 保存进度”
- “按规范做一次收尾并提交 git”
- “按进度协议更新日志并归档”
