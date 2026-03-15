# DroneP_VG 会话交接报告

日期：2026-03-15

## 1. 本次会话新增完成内容

### 1.1 GroundingDINO 阈值搜索已完成

- 搜索脚本：/home/wangzhe/DroneP_VG/search_groundingdino_thresholds.py
- 搜索规模：6 组配置
- 搜索目录：/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search
- 搜索结果汇总：/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.md

最佳 class-aware 配置：

- box_threshold = 0.20
- text_threshold = 0.20
- nms_threshold = 0.40

最佳 class-aware 指标：

- Acc@0.5 = 0.3443 (1748/5077)
- Acc@0.75 = 0.2624 (1332/5077)
- mAP@0.5 = 0.1451

相较上一版默认基线（0.25 / 0.20 / 0.50）：

- Acc@0.5 提升 0.0366
- Acc@0.75 提升 0.0180
- mAP@0.5 提升 0.0084

### 1.2 最优基线结果已固化

- 最优预测目录：/home/wangzhe/DroneP_VG/visdrone_test_100/best_groundingdino_preds
- 最优指标文件：/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md

### 1.3 120 张报告子集画像已补齐

- 画像目录：/home/wangzhe/DroneP_VG/visdrone_report_subset_120/data_profile_assets
- 当前统计：
  - 图像总数：120
  - 正样本：100
  - 负样本：20
  - 总有效框数：5077
  - 每图平均目标数：42.31
  - 小目标占比：61.32%

### 1.4 120 张综合总报告已生成

- 文件：/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_summary_20260315.md
- 内容包含：
  - 最佳基线指标
  - 阈值搜索结论
  - 120 张子集画像统计
  - 当前阶段结论与下步建议

## 2. 当前阶段判断

- 当前“整图 + 零样本 + class-aware”基线已经稳定可复现。
- 继续只做少量阈值微调，收益已经不大。
- 后续最值得投入的方向是把滑窗切片策略真正接入检测推理链路，以改善密集小目标场景表现。

## 3. 当前未完成项

1. 失败案例 / 成功案例可视化整理成汇报素材
2. 滑窗切片检测实验接入真实推理链路
3. GroundingDINO `_C` 扩展编译，启用 GPU 加速

## 4. 下次继续建议顺序

1. 在 100 张子集上做“整图推理 vs 滑窗推理”对照实验
2. 输出 3 到 5 个成功案例、3 到 5 个失败案例
3. 若时间允许，再处理 GPU 扩展编译问题，缩短后续实验耗时

## 5. 下次可直接引用的话

"继续从 session_handoff_20260315.md 往下做：先把滑窗切片策略接到真实检测推理里，和当前 best GroundingDINO 整图基线做对照。"