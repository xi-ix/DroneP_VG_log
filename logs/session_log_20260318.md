# DroneP_VG 工作日志

日期：2026-03-18

## 1. 本次做了什么

- 按 `progress_save_protocol.md` 完成一次维护性进度保存。
- 更新阶段总结、长期总结索引与分文件，统一当前有效指标口径。
- 生成当日会话交接文档，补齐 `project_records` 目录层级记录。
- 将滑窗切片策略接入真实检测推理链路，并完成与当前 best 整图基线的同口径对照实验。

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
- 滑窗对照总结：`/home/wangzhe/DroneP_VG/project_records/summaries/sliding_window_vs_best_groundingdino_20260318.md`
- 滑窗预测目录：`/home/wangzhe/DroneP_VG/project_records/results/predictions/sliding_window_groundingdino_preds_bestcfg_20260318`
- 滑窗指标文件：`/home/wangzhe/DroneP_VG/project_records/results/metrics/evaluation_summary_groundingdino_sliding_window_bestcfg_20260318_class_aware.md`

## 4. 关键指标（完整）

本次为维护性收尾，无新增推理实验。当前有效指标如下：

- best class-aware 配置：box_threshold=0.20, text_threshold=0.20, nms_threshold=0.40
- best class-aware：Acc@0.5=0.3443 (1748/5077), Acc@0.75=0.2624 (1332/5077), mAP@0.5=0.1451
- default class-aware：Acc@0.5=0.3077 (1562/5077), Acc@0.75=0.2444 (1241/5077), mAP@0.5=0.1367
- class-unaware（default）：Acc@0.5=0.4359, Acc@0.75=0.3319, mAP@0.5=0.3367
- 120 张报告子集：100 正样本 + 20 负样本，总有效框数 5077，小目标占比 61.32%
- sliding-window（bestcfg 20260318）：Pred boxes=4612，Acc@0.5=0.3760 (1909/5077)，Acc@0.75=0.2813 (1428/5077)，mAP@0.5=0.1517，Empty FP=0.0000
- 相比当前 best 整图基线增益：Acc@0.5 +0.0317，Acc@0.75 +0.0189，mAP@0.5 +0.0066

## 5. 当前真实阻塞

- 在 VisDrone 密集小目标场景中，整图零样本 class-aware 性能仍偏弱。
- 滑窗策略虽已提升精度，但参数（窗口大小、重叠率）与推理开销的折中仍需系统化评估。

## 6. 2026-03-18 环境复核补充

- 已完成运行时复核：`torch.cuda.is_available()=True`，CUDA 设备数为 6。
- 已确认 `groundingdino._C` 可成功导入，模块路径为 `/home/wangzhe/GroundingDINO/groundingdino/_C.cpython-310-x86_64-linux-gnu.so`。
- 结论：GroundingDINO CUDA 扩展在当前环境已可用，GPU 推理路径已具备启用条件。

## 7. 2026-03-18 晚间补充（外接模块训练与文档归档）

### 本次做了什么
- 完成 Stage A 外接校准头训练（40 epoch）与长训练（200 epoch）。
- 训练脚本新增可视化进度能力：终端实时进度条 + 每 epoch 落盘到 TSV。
- 完成长训练结果评估、候选集合上限核验、指标解释文档整理。

### 这么做的目的
- 在不改 GroundingDINO 主干参数的前提下补齐“有训练环节”的最小可交付链路。
- 提供可观测训练过程，满足训练期间可视化进度需求。
- 明确当前 Acc@0.5 上限可达性，避免无效迭代。

### 产出结果（文件 + 路径）
- 训练脚本：`/home/wangzhe/DroneP_VG/scripts/train_external_calibrator_stageA.py`
- 长训练日志：`/home/wangzhe/DroneP_VG/project_records/results/metrics/lightweight_optimization/training_log_external_calibrator_stageA_longrun_20260318.txt`
- 长训练进度：`/home/wangzhe/DroneP_VG/project_records/results/metrics/lightweight_optimization/training_progress_external_calibrator_stageA_longrun_20260318.tsv`
- 长训练模型信息：`/home/wangzhe/DroneP_VG/project_records/results/metrics/lightweight_optimization/external_calibrator_stageA_longrun_20260318.json`
- 长训练评估：`/home/wangzhe/DroneP_VG/project_records/results/metrics/lightweight_optimization/evaluation_summary_fusion_stageA_extcal_longrun_20260318_class_aware.md`
- 指标解释：`/home/wangzhe/DroneP_VG/project_records/protocols/note.md`

### 关键指标（完整）
- Stage A 外接模块长训练（200 epoch）：
  - Acc@0.5 = 0.4099 (2081/5077)
  - Acc@0.75 = 0.3118 (1583/5077)
  - mAP@0.5 = 0.1322
  - Empty-image false positive rate = 0.0000 (0/0)
- 候选集可达性上限（oracle 覆盖）：Acc@0.5 = 0.4130 (2097/5077)
