# Detection Evaluation Summary

## Config
- GT dir: /home/wangzhe/DroneP_VG/visdrone_test_100/annotations
- Prediction dir: /home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds
- Class aware matching: True
- mAP IoU threshold: 0.5

## Dataset
- Label files considered: 100
- GT boxes: 5077
- Prediction boxes: 2786
- Empty GT images: 0
- Skipped GT lines: 196
- Skipped prediction lines: 0

## Metrics
- Acc@0.5: 0.3077 (1562/5077)
- Acc@0.75: 0.2444 (1241/5077)
- mAP@0.5: 0.1367
- Empty-image false positive rate: 0.0000 (0/0)

## Top AP Classes
- Class 4: 0.5369
- Class 6: 0.2056
- Class 1: 0.1592
- Class 3: 0.1470
- Class 9: 0.1212



是的，这三个 class-aware 指标偏低，但目前看不像“流程跑错”，更像是“基线能力有限 + 评估设置严格”导致的。

我刚做了两项核验：

对照评估（不要求类别匹配）
在同样预测结果上，class-unaware 指标明显更高：
Acc@0.5: 0.4359
Acc@0.75: 0.3319
mAP@0.5: 0.3367
结果文件在 evaluation_summary_groundingdino_baseline_class_unaware.md
预测文件格式核验
总行数 2786
坏行数 0
坐标均满足 x2>x1, y2>y1
类别 ID 范围正常（1,2,3,4,5,6,7,9,10）
这说明输出格式和评估读取链路是通的，不是文件格式错误。
为什么会低（主要原因）：

你现在是零样本 GroundingDINO baseline，不是针对 VisDrone 专门训练的检测器。
这批数据小目标占比高，零样本方法在小目标和细分类别上天然吃亏。
你用了 class-aware 匹配，类别一旦错配就不算 TP，会明显拉低 mAP/Acc。
当前提示词与类别映射是通用英文词，和 VisDrone 标注语义不一定完全对齐。