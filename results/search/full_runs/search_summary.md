# GroundingDINO Threshold Search Summary

## Best Config
- box_threshold: 0.20
- text_threshold: 0.20
- nms_threshold: 0.40
- Acc@0.5: 0.3443
- Acc@0.75: 0.2624
- mAP@0.5: 0.1451
- Prediction dir: /home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/bt_0p20_tt_0p20_nms_0p40/preds
- Evaluation markdown: /home/wangzhe/DroneP_VG/visdrone_test_100/groundingdino_threshold_search/bt_0p20_tt_0p20_nms_0p40/evaluation_class_aware.md

## All Runs
| run_name | box_threshold | text_threshold | nms_threshold | Acc@0.5 | Acc@0.75 | mAP@0.5 |
| --- | ---: | ---: | ---: | ---: | ---: | ---: |
| bt_0p20_tt_0p20_nms_0p40 | 0.20 | 0.20 | 0.40 | 0.3443 | 0.2624 | 0.1451 |
| bt_0p25_tt_0p25_nms_0p50 | 0.25 | 0.25 | 0.50 | 0.2970 | 0.2405 | 0.1381 |
| bt_0p20_tt_0p15_nms_0p40 | 0.20 | 0.15 | 0.40 | 0.3614 | 0.2720 | 0.1370 |
| bt_0p20_tt_0p15_nms_0p50 | 0.20 | 0.15 | 0.50 | 0.3644 | 0.2744 | 0.1368 |
| bt_0p25_tt_0p20_nms_0p50 | 0.25 | 0.20 | 0.50 | 0.3077 | 0.2444 | 0.1367 |
| bt_0p30_tt_0p20_nms_0p50 | 0.30 | 0.20 | 0.50 | 0.2496 | 0.2080 | 0.1139 |
