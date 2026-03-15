# DroneP_VG 会话交接报告

日期：2026-03-14

## 1. 本次会话新增完成内容

### 1.1 GroundingDINO 权重已确认完整

- 文件：/home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth
- 当前大小：662MB
- 结论：权重文件可用，不再是之前的 3.3MB 异常残留

### 1.2 GroundingDINO 基线推理脚本已落地

- 新增脚本：/home/wangzhe/DroneP_VG/run_groundingdino_baseline.py
- 功能：
  - 批量读取图像目录
  - 使用 GroundingDINO 进行开放词汇检测
  - 输出评估脚本可读格式：`class_id x1 y1 x2 y2 score`
  - 支持阈值与 NMS 参数控制
- 兼容性修复：
  - 已处理 `class_id=None` 的情况，避免推理中断

### 1.3 100 张子集基线预测已跑通

- 输入目录：/home/wangzhe/DroneP_VG/visdrone_test_100/images
- 输出目录：/home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds
- 运行模式：CPU（通过纯 PyTorch fallback）
- 结果：
  - 100/100 图像处理完成
  - 预测文件数：100
  - 非空预测文件数：100
  - 总预测框数：2786

### 1.4 第一版指标表已生成（class-aware）

- 指标文件：/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline.md
- 关键指标：
  - Acc@0.5 = 0.3077 (1562/5077)
  - Acc@0.75 = 0.2444 (1241/5077)
  - mAP@0.5 = 0.1367
  - Empty-image false positive rate = 0.0000 (0/0)

### 1.5 对照指标已补充（class-unaware）

- 对照文件：/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline_class_unaware.md
- 对照结果：
  - Acc@0.5 = 0.4359
  - Acc@0.75 = 0.3319
  - mAP@0.5 = 0.3367
- 结论：
  - 指标偏低主要来自类别错配与小目标难度，不是评估链路格式错误

## 2. 本次会话定位并解决的问题

### 2.1 transformers 版本兼容问题

- 现象：`BertModel` 缺少 `get_head_mask`
- 原因：环境里是 `transformers 5.0.0`，与当前 GroundingDINO 代码不兼容
- 处理：降级到 `transformers==4.33.2`、`tokenizers==0.13.3`

### 2.2 自定义算子 `_C` 未编译

- 现象：CUDA 推理时报 `NameError: _C is not defined`
- 现状：当前通过 CPU fallback 已成功完成 100 张基线
- 说明：若后续要 GPU 加速，仍需完整编译 GroundingDINO 扩展

## 3. 当前可直接复用的命令

### 3.1 跑基线预测（CPU 稳定版）

```bash
cd /home/wangzhe/DroneP_VG
/home/wangzhe/miniconda3/envs/qwen_vl/bin/python run_groundingdino_baseline.py \
  --device cpu \
  --image_dir /home/wangzhe/DroneP_VG/visdrone_test_100/images \
  --output_dir /home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds
```

### 3.2 跑 class-aware 指标

```bash
cd /home/wangzhe/DroneP_VG
/home/wangzhe/miniconda3/envs/qwen_vl/bin/python evaluate_detection_metrics.py \
  --gt_dir /home/wangzhe/DroneP_VG/visdrone_test_100/annotations \
  --pred_dir /home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds \
  --output_md /home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline.md \
  --class_aware
```

### 3.3 跑 class-unaware 对照

```bash
cd /home/wangzhe/DroneP_VG
/home/wangzhe/miniconda3/envs/qwen_vl/bin/python evaluate_detection_metrics.py \
  --gt_dir /home/wangzhe/DroneP_VG/visdrone_test_100/annotations \
  --pred_dir /home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds \
  --output_md /home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_baseline_class_unaware.md
```

## 4. 当前未完成项

1. 120 张报告子集的完整画像更新（结合本次 baseline 指标）
2. 失败案例/成功案例可视化（用于中期汇报展示）
3. （可选）阈值网格搜索，进一步抬升 class-aware 指标
4. （可选）编译 `_C` 扩展以启用 GPU 加速推理

## 5. 下次继续建议顺序

1. 对 `box_threshold / text_threshold / nms_threshold` 做 6-9 组参数搜索
2. 选取最佳配置重跑 100 张并固化“最佳基线”指标表
3. 生成 120 张报告子集的综合报告：
   - 数据分布
   - class-aware / class-unaware 对照
   - 典型成功/失败样例

## 6. 下次可直接引用的话

"继续从 session_handoff_20260314.md 往下做：先做一轮 GroundingDINO 阈值搜索，确定最佳 class-aware 基线，再更新 120 张报告子集总报告。"
