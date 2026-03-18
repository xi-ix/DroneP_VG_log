# DroneP_VG 工作日志

日期：2026-03-18

## 1. 本次做了什么

- 按 `progress_save_protocol.md` 完成一次维护性进度保存。
- 更新阶段总结、长期总结索引与分文件，统一当前有效指标口径。
- 生成当日会话交接文档，补齐 `project_records` 目录层级记录。

## 2. 这么做的目的

- 将当前阶段成果以统一结构固化，便于后续连续实验与答辩材料复用。
- 确保“当前有效结果”在日志、总结、交接三类文档中一致，避免口径漂移。
- 为下一次会话提供可直接续做的起点。

## 3. 产出结果（文件 + 路径）

- 阶段总结：`/home/wangzhe/DroneP_VG/project_records/summaries/progress_summary.md`
- 长期索引：`/home/wangzhe/DroneP_VG/project_records/summaries/experiment_long_term_summary.md`
- 长期分文件：
  - `/home/wangzhe/DroneP_VG/project_records/summaries/long_term/01_directory_overview.md`
  - `/home/wangzhe/DroneP_VG/project_records/summaries/long_term/02_scripts_overview.md`
  - `/home/wangzhe/DroneP_VG/project_records/summaries/long_term/03_dataset_summary.md`
  - `/home/wangzhe/DroneP_VG/project_records/summaries/long_term/04_experiment_metrics.md`
  - `/home/wangzhe/DroneP_VG/project_records/summaries/long_term/05_environment_and_constraints.md`
  - `/home/wangzhe/DroneP_VG/project_records/summaries/long_term/06_key_findings.md`
- 会话交接：`/home/wangzhe/DroneP_VG/project_records/logs/session_handoff_20260318.md`
- 归档说明：`/home/wangzhe/DroneP_VG/project_records/README.md`

## 4. 关键指标（完整）

本次为维护性收尾，无新增推理实验。当前有效指标如下：

- best class-aware 配置：box_threshold=0.20, text_threshold=0.20, nms_threshold=0.40
- best class-aware：Acc@0.5=0.3443 (1748/5077), Acc@0.75=0.2624 (1332/5077), mAP@0.5=0.1451
- default class-aware：Acc@0.5=0.3077 (1562/5077), Acc@0.75=0.2444 (1241/5077), mAP@0.5=0.1367
- class-unaware（default）：Acc@0.5=0.4359, Acc@0.75=0.3319, mAP@0.5=0.3367
- 120 张报告子集：100 正样本 + 20 负样本，总有效框数 5077，小目标占比 61.32%

## 5. 当前真实阻塞

- 在 VisDrone 密集小目标场景中，整图零样本 class-aware 性能仍偏弱。

## 6. 2026-03-18 环境复核补充

- 已完成运行时复核：`torch.cuda.is_available()=True`，CUDA 设备数为 6。
- 已确认 `groundingdino._C` 可成功导入，模块路径为 `/home/wangzhe/GroundingDINO/groundingdino/_C.cpython-310-x86_64-linux-gnu.so`。
- 结论：GroundingDINO CUDA 扩展在当前环境已可用，GPU 推理路径已具备启用条件。
