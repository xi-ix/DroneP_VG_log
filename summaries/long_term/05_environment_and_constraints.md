# 长期总结-环境状态与约束

## 1. GroundingDINO 环境结论

目的：在当前服务器环境中打通 GroundingDINO 零样本推理。

当前状态：

- 权重文件可用：[groundingdino_swint_ogc.pth](/home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth)
- 权重大小：662MB
- 依赖兼容版本：transformers==4.33.2，tokenizers==0.13.3
- 当前 CUDA 自定义扩展 `_C` 未编译成功
- 当前实验使用 CPU fallback 模式完成

## 2. 运行侧影响

- 现有环境可稳定复现实验结果
- 大规模实验速度会受 CPU fallback 限制
- 当前主要性能瓶颈是密集小目标与类别语义对齐，不是评估链路

## 3. 记录口径

为保证可比性，后续新增实验建议保持以下口径：

- 评估集：100 张评估子集（GT boxes=5077）
- 主要指标：class-aware Acc@0.5、Acc@0.75、mAP@0.5
- 对照指标：class-unaware 三项指标
- 输出要求：同时保留指标文件路径与预测目录路径
