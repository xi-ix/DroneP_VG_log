# Detection Evaluation Summary

## Config
- GT dir: /home/wangzhe/DroneP_VG/visdrone_test_100/annotations
- Prediction dir: /home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/bt_0p20_tt_0p15_nms_0p50/preds
- Class aware matching: True
- mAP IoU threshold: 0.5

## Dataset
- Label files considered: 100
- GT boxes: 5077
- Prediction boxes: 3748
- Empty GT images: 0
- Skipped GT lines: 196
- Skipped prediction lines: 0

## Metrics
- Acc@0.5: 0.3644 (1850/5077)
- Acc@0.75: 0.2744 (1393/5077)
- mAP@0.5: 0.1368
- Empty-image false positive rate: 0.0000 (0/0)

## Top AP Classes
- Class 4: 0.5311
- Class 1: 0.2319
- Class 6: 0.1952
- Class 3: 0.1485
- Class 9: 0.0909
