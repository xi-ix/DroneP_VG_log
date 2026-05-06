# DroneP_VG 工作日志

日期：2026-03-27

## 1. 本次做了什么

- 按会话请求读取并汇总 `project_records/logs` 与 `project_records/results/metrics` 下的实验日志与指标表。
- 生成中期报告四页版（摘要、方法、结果表、结论），并保存为可直接复用的 Markdown 文档。
- 进一步生成正式答辩展示的精简版（每页 3-5 条），用于快速投屏讲解。
- 按进度协议同步更新统一实验报告与会话交接文档。

## 2. 这么做的目的

- 将分散在多份评估文件中的关键结果统一成答辩可复用结构，降低临场整理成本。
- 固化本阶段“同口径对照”的核心数据，避免后续汇报中出现版本不一致。
- 形成可连续迭代的中期材料底稿（完整版 + 精简版）。

## 3. 产出结果（文件 + 路径）

- 中期报告完整版：`/home/wangzhe/DroneP_VG/project_records/summaries/midterm_report_ppt_4pages_20260327.md`
- 中期报告精简版：`/home/wangzhe/DroneP_VG/project_records/summaries/midterm_report_ppt_4pages_concise_20260327.md`
- 统一实验报告追加：`/home/wangzhe/DroneP_VG/project_records/logs/experiment_report.md`
- 本次会话日志：`/home/wangzhe/DroneP_VG/project_records/logs/session_log_20260327.md`
- 本次交接文档：`/home/wangzhe/DroneP_VG/project_records/logs/session_handoff_20260327.md`

## 4. 关键指标（完整）

本次为文档汇总与排版归档，无新增模型训练/推理。当前有效核心指标如下：

- 默认整图基线（class-aware）：
  - Pred boxes=2786
  - Acc@0.5=0.3077 (1562/5077)
  - Acc@0.75=0.2444 (1241/5077)
  - mAP@0.5=0.1367
- best 整图（class-aware，0.20/0.20/0.40）：
  - Pred boxes=3648
  - Acc@0.5=0.3443 (1748/5077)
  - Acc@0.75=0.2624 (1332/5077)
  - mAP@0.5=0.1451
- 滑窗 bestcfg（class-aware）：
  - Pred boxes=4612
  - Acc@0.5=0.3760 (1909/5077)
  - Acc@0.75=0.2813 (1428/5077)
  - mAP@0.5=0.1517
- Stage A 非训练融合（class-aware）：
  - Pred boxes=5492
  - Acc@0.5=0.4020 (2041/5077)
  - Acc@0.75=0.2958 (1502/5077)
  - mAP@0.5=0.1556
- Exp-14（微调主模型 + SL+RL，class-aware，当前综合最优）：
  - Pred boxes=5542
  - Acc@0.5=0.4113 (2088/5077)
  - Acc@0.75=0.3021 (1534/5077)
  - mAP@0.5=0.1685
- Qwen 对照：
  - Qwen3VL rerun：Acc@0.5=0.0012，Acc@0.75=0.0000，mAP@0.5=0.0053
  - Qwen2.5-VL-3B：Acc@0.5=0.0120，Acc@0.75=0.0055，mAP@0.5=0.0239
- 运行效率（100 图）：
  - full_best：76s，0.7600s/图，2284MB
  - sw_cfg02：280s，2.8000s/图，2360MB
  - sw_cfg07：141s，1.4100s/图，2632MB