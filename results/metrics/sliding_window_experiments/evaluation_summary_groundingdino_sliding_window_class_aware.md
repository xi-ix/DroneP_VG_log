# Detection Evaluation Summary

## Config
- GT dir: visdrone_test_100/annotations
- Prediction dir: project_records/results/predictions/sliding_window_groundingdino_preds
- Class aware matching: True
- mAP IoU threshold: 0.5

## Dataset
- Label files considered: 100
- GT boxes: 5077
- Prediction boxes: 3527
- Empty GT images: 0
- Skipped GT lines: 196
- Skipped prediction lines: 0

## Metrics
- Acc@0.5: 0.3437 (1745/5077)
- Acc@0.75: 0.2663 (1352/5077)
- mAP@0.5: 0.1420
- Empty-image false positive rate: 0.0000 (0/0)

## Top AP Classes
- Class 4: 0.5173
- Class 6: 0.1985
- Class 3: 0.1810
- Class 1: 0.1628
- Class 9: 0.1558
