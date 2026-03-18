# Sliding Window vs Best Full-Image Baseline (GroundingDINO)

## Experiment Setup
- Dataset: `visdrone_test_100` (100 images, 5077 GT boxes)
- Model: GroundingDINO SwinT OGC
- Device: CUDA
- Metric mode: class-aware (`--class_aware`)

### Sliding-window inference config
- `box_threshold=0.25`
- `text_threshold=0.20`
- `nms_threshold=0.5`
- `max_detections=300`
- `window_size=1024,1024`
- `overlap_ratio=0.2`

## Result Comparison

| Method | Pred boxes | Acc@0.5 | Acc@0.75 | mAP@0.5 | Empty FP rate |
|---|---:|---:|---:|---:|---:|
| Best full-image baseline | 3648 | 0.3443 | 0.2624 | 0.1451 | 0.0000 |
| Sliding-window | 3527 | 0.3437 | 0.2663 | 0.1420 | 0.0000 |

## Delta (Sliding - Best Full-Image)
- Pred boxes: -121
- Acc@0.5: -0.0006
- Acc@0.75: +0.0039
- mAP@0.5: -0.0031
- Empty FP rate: +0.0000

## Takeaways
- Sliding-window is almost tied with best full-image baseline on `Acc@0.5`.
- Sliding-window improves stricter localization (`Acc@0.75`) slightly.
- Overall `mAP@0.5` is lower than best full-image baseline in this setting.
- Current window config does not outperform the best full-image baseline globally.

## Source Files
- Best full-image summary: `project_records/results/metrics/evaluation_summary_groundingdino_best_class_aware.md`
- Sliding-window summary: `project_records/results/metrics/evaluation_summary_groundingdino_sliding_window_class_aware.md`
