# Detection Evaluation Summary

## Config
- GT dir: /home/wangzhe/DroneP_VG/visdrone_test_100/annotations
- Prediction dir: /home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/bt_0p30_tt_0p20_nms_0p50/preds
- Class aware matching: True
- mAP IoU threshold: 0.5

## Dataset
- Label files considered: 100
- GT boxes: 5077
- Prediction boxes: 2040
- Empty GT images: 0
- Skipped GT lines: 196
- Skipped prediction lines: 0

## Metrics
- Acc@0.5: 0.2496 (1267/5077)
- Acc@0.75: 0.2080 (1056/5077)
- mAP@0.5: 0.1139
- Empty-image false positive rate: 0.0000 (0/0)

## Top AP Classes
- Class 4: 0.4731
- Class 6: 0.1520
- Class 3: 0.1470
- Class 1: 0.0909
- Class 9: 0.0909
