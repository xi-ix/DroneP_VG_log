# DroneP_VG 当前工作进度总结

## 1. 当前阶段目标

目前项目已经完成了中期答辩前最关键的三步准备：

- 完成了基于滑动窗口的高分辨率航拍图像切片脚本。
- 从 VisDrone 数据中抽取了 100 张图片作为快速测试子集。
- 完成了一个可独立运行的评估脚本，可以自动计算 Acc@0.5、Acc@0.75、mAP@0.5 和无目标误报率，并输出简短的 Markdown 结果总结。

当前工作重点已经从“想法阶段”进入“可运行原型阶段”，已经具备中期答辩展示所需的基础工程链路：

原始图像/标注 -> 数据抽样 -> 滑窗切片 -> 指标评估 -> Markdown 结果汇总

## 2. 已完成内容

### 2.1 数据准备

- 已定位 VisDrone 数据路径，位于 RefDrone 子目录下。
- 已从 VisDrone train/val/test 中随机抽取 100 张图片作为测试集。
- 已生成测试集目录：visdrone_test_100。
- 已同步复制可用标注文件，并生成采样日志。

### 2.2 预处理脚本

- 已完成滑动窗口切片脚本 import_cv2.py。
- 支持单图处理和目录批处理。
- 支持是否保留空目标切片。
- 支持两种输出标签格式：xyxy 像素坐标和 YOLO 归一化格式。
- 支持输出 metadata.csv 和 summary.csv，便于后续统计分析。

### 2.3 评估脚本

- 已完成 evaluate_detection_metrics.py。
- 兼容两种标注格式：
  - 空格分隔格式：class x_min y_min x_max y_max 或带 score
  - VisDrone 原生逗号格式：x,y,w,h,score,class,...
- 可自动生成简洁的 Markdown 评估摘要。

### 2.4 GroundingDINO 零样本基线已打通

- 已新增批量推理脚本：/home/wangzhe/DroneP_VG/scripts/run_groundingdino_baseline.py
- 已在 100 张正样本测试子集上完成 GroundingDINO CPU 基线推理
- 已完成 6 组阈值搜索，并确定当前最佳 class-aware 配置：
  - box_threshold = 0.20
  - text_threshold = 0.20
  - nms_threshold = 0.40
- 当前最佳 class-aware 指标：
  - Acc@0.5 = 0.3443
  - Acc@0.75 = 0.2624
  - mAP@0.5 = 0.1451
- 已生成 120 张报告子集的数据画像资产与综合报告

## 3. 当前产出文件

### 3.1 测试子集目录

- visdrone_test_100/images: 抽样得到的 100 张测试图片
- visdrone_test_100/annotations: 对应标注文件
- visdrone_test_100/manifest.tsv: 抽样清单
- visdrone_test_100/records.tsv: 详细记录
- visdrone_test_100/sample_log.txt: 抽样日志
- visdrone_test_100/evaluation_summary.md: 评估摘要示例

### 3.2 当前统计结果

基于已有抽样结果：

- VisDrone 候选图片总数：10786
- 随机抽样数量：100
- 其中具备标注的样本数：80
- 当前评估示例使用的是“真值对真值”自检，因此得到满分结果，仅用于验证评估脚本可用，不代表真实模型性能。

补充最新基线结果：

- 100 张 GroundingDINO 最佳基线预测框数：3648
- class-aware Acc@0.5：0.3443
- class-aware Acc@0.75：0.2624
- class-aware mAP@0.5：0.1451
- 120 张报告子集组成：100 正样本 + 20 负样本
- 120 张子集总有效框数：5077
- 小目标占比：61.32%

## 4. 脚本说明

### 4.1 import_cv2.py

功能：

- 对高分辨率图像进行滑动窗口切片。
- 将原图 bbox 映射到切片局部坐标。
- 保存切片图像与对应标签。
- 支持批量处理整个目录。

输入：

- single 模式：
  - image_path: 单张图片路径
  - label_path: 对应标注文件路径，当前读取格式为 class x_min y_min x_max y_max
  - save_dir: 输出目录
- batch 模式：
  - image_dir: 图片目录
  - label_dir: 标注目录
  - save_dir: 输出目录
- 可选参数：
  - window_size: 滑窗大小，如 1024,1024
  - overlap_ratio: 重叠比例
  - min_bbox_area_ratio: 最小保留面积比例
  - save_empty: 是否保留无目标切片
  - label_format: xyxy_pixel 或 yolo_norm

输出：

- 每张切片图像：jpg/png 等
- 每张切片标签：txt
- 每张原图一个 metadata.csv
- 批处理汇总 summary.csv

使用方法：

单图：

```bash
python /home/wangzhe/DroneP_VG/scripts/import_cv2.py single \
  --image_path /path/to/image.jpg \
  --label_path /path/to/image.txt \
  --save_dir /path/to/output \
  --window_size 1024,1024 \
  --overlap_ratio 0.2 \
  --min_bbox_area_ratio 0.5 \
  --save_empty \
  --label_format xyxy_pixel
```

批处理：

```bash
python /home/wangzhe/DroneP_VG/scripts/import_cv2.py batch \
  --image_dir /path/to/images \
  --label_dir /path/to/labels \
  --save_dir /path/to/output \
  --window_size 1024,1024 \
  --overlap_ratio 0.2 \
  --min_bbox_area_ratio 0.5 \
  --save_empty \
  --label_format xyxy_pixel
```

### 4.2 evaluate_detection_metrics.py

功能：

- 自动读取真值目录和预测目录。
- 计算 Acc@0.5。
- 计算 Acc@0.75。
- 计算 mAP@0.5。
- 计算无目标误报率。
- 输出一个简短的 Markdown 总结文件。

输入：

- gt_dir: 真值标注目录
- pred_dir: 预测结果目录
- output_md: 输出 Markdown 文件路径
- class_aware: 是否要求类别匹配
- iou_threshold_for_map: 计算 mAP 时的 IoU 阈值

支持输入格式：

- 空格分隔：class x_min y_min x_max y_max
- 空格分隔带分数：class x_min y_min x_max y_max score
- VisDrone 原生格式：x,y,w,h,score,class,truncation,occlusion

输出：

- 终端打印四项指标
- 一个简短的 Markdown 结果文件

使用方法：

```bash
python /home/wangzhe/DroneP_VG/scripts/evaluate_detection_metrics.py \
  --gt_dir /home/wangzhe/DroneP_VG/visdrone_test_100/annotations \
  --pred_dir /your/prediction_dir \
  --output_md /your/output/evaluation_summary.md \
  --class_aware
```

### 4.3 RefDrone/draw_box.py

功能：

- 读取 VisDrone 风格标注文本。
- 将 bbox 画到原图上。
- 可用于人工检查标注是否正确。

输入：

- image_name: 图片文件名
- dataset_name: 数据集 split，如 train/test
- annotations_list: 标注行列表
- num_lines_to_draw: 绘制框数上限
- text_to_overlay: 可选显示文字

输出：

- 在窗口中显示绘制结果
- 额外保存一张带框图片到 output 目录

适用场景：

- 人工核对原始标注
- 快速排查 bbox 是否越界或错位

### 4.4 RefDrone/draw_from_json.py

功能：

- 读取 RefDrone JSON 文件中的 image、caption 和 bbox 信息。
- 调用 draw_box.py 在图片上把目标框和文本画出来。

输入：

- RefDrone 的 JSON 标注文件
- 指定 image id

输出：

- 终端输出 file_name、caption、bbox
- 保存一张带文本和框的可视化图片

适用场景：

- 检查 RefDrone 文本描述与 bbox 是否一致

### 4.5 AerialVG/annotation/avg_w_h.py

功能：

- 读取 JSONL 文件中的图像宽高信息。
- 统计平均宽度和平均高度。

输入：

- JSONL 文件路径

输出：

- 平均宽高
- 有效图像数量

适用场景：

- 了解数据集分辨率分布
- 为滑窗大小设计提供参考

## 5. 当前存在的问题

- import_cv2.py 当前默认读取的是空格分隔 xyxy 标注，不直接读取 VisDrone 原生逗号格式。
- GroundingDINO 当前在 CPU fallback 模式下可运行，但 CUDA 自定义扩展 `_C` 仍未编译，尚未启用 GPU 加速。

## 6. 2026-03-18 维护性进度更新

### 6.1 已完成内容（新增）

- 按协议完成一次项目收尾：更新日志、长期总结、阶段总结与会话交接文档。
- 将当前有效指标与结果入口进行一致性核对，确认口径与路径统一。

### 6.2 当前有效结果（本次未新增实验）

- 当前 best class-aware 配置：box_threshold=0.20, text_threshold=0.20, nms_threshold=0.40。
- 当前 best class-aware 指标：Acc@0.5=0.3443 (1748/5077), Acc@0.75=0.2624 (1332/5077), mAP@0.5=0.1451。
- 当前默认基线 class-aware 指标：Acc@0.5=0.3077 (1562/5077), Acc@0.75=0.2444 (1241/5077), mAP@0.5=0.1367。
- 当前 120 张报告子集统计：100 正样本 + 20 负样本，总有效框 5077，小目标占比 61.32%。

### 6.3 当前存在问题（真实阻塞）

- GroundingDINO `_C` 扩展未编译，GPU 路径尚未打通。
- 零样本整图推理在密集小目标场景下仍偏弱，类别语义错配仍是主要误差来源。
- 当前最佳基线仍属于零样本整图推理，对 VisDrone 密集小目标场景适配有限。
- 当前尚未把滑窗切片策略接入真实检测模型链路。

## 6. 下一步建议

建议中期答辩前优先完成以下三项：

1. 把滑窗切片策略接入 GroundingDINO 或其他检测器，验证对小目标场景的收益。
2. 按成功案例和失败案例各选 3 到 5 张图，形成中期展示素材。
3. 若需要缩短实验时长，再处理 `_C` 扩展编译与 GPU 推理链路。

## 8. 最新产物

- 最佳 class-aware 指标：[visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md](/home/wangzhe/DroneP_VG/visdrone_test_100/evaluation_summary_groundingdino_best_class_aware.md)
- 阈值搜索汇总：[visdrone_test_100/groundingdino_threshold_search/search_summary.md](/home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/search_summary.md)
- 120 张综合报告：[visdrone_report_subset_120/report_summary_20260315.md](/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_summary_20260315.md)

## 7. 中期答辩可直接汇报的结论

- 已经完成了无人机图像数据预处理与评估基础工具链的搭建。
- 已经从 VisDrone 中抽取了 100 张测试图片构建快速实验子集。
- 已具备生成切片数据、保存日志、统计评估指标和输出 Markdown 报告的能力。
- 下一阶段工作重点是接入真实预测模型，形成完整实验闭环，并输出第一版定量结果。