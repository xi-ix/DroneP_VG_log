# GroundingDINO 失败样例展示清单（2026-03-17）

## 结论

- GroundingDINO 在 100 张子集上推理流程是跑通的（100/100 处理完成），没有出现“脚本中断导致图片未处理”的运行失败。
- 可用于展示的“失败图片”主要有两类：
  1. 检测效果失败（class-aware 召回很低）
  2. 历史数据失败（样本曾标记为 MISSING，后续已替换）

## A. 检测效果失败（建议优先展示）

来源：project_records/results/metrics/failure_cases_top20_class_aware_iou05.md

1. 033_VisDrone2019-DET-train_9999962_00000_d_0000142.jpg
   - recall=0.0000 (0/37)
   - image: /home/wangzhe/DroneP_VG/visdrone_test_100/images/033_VisDrone2019-DET-train_9999962_00000_d_0000142.jpg
2. 010_VisDrone2019-DET-train_9999962_00000_d_0000150.jpg
   - recall=0.0000 (0/28)
   - image: /home/wangzhe/DroneP_VG/visdrone_test_100/images/010_VisDrone2019-DET-train_9999962_00000_d_0000150.jpg
3. 015_VisDrone2019-DET-test_9999938_00000_d_0000209.jpg
   - recall=0.0283 (6/212)
   - image: /home/wangzhe/DroneP_VG/visdrone_test_100/images/015_VisDrone2019-DET-test_9999938_00000_d_0000209.jpg
4. 009_VisDrone2019-DET-test_0000073_01275_d_0000002.jpg
   - recall=0.0435 (5/115)
   - image: /home/wangzhe/DroneP_VG/visdrone_test_100/images/009_VisDrone2019-DET-test_0000073_01275_d_0000002.jpg
5. 091_VisDrone2019-DET-train_0000013_00465_d_0000067.jpg
   - recall=0.0652 (6/92)
   - image: /home/wangzhe/DroneP_VG/visdrone_test_100/images/091_VisDrone2019-DET-train_0000013_00465_d_0000067.jpg
6. 098_VisDrone2019-DET-train_0000016_01352_d_0000069.jpg
   - recall=0.0820 (5/61)
   - image: /home/wangzhe/DroneP_VG/visdrone_test_100/images/098_VisDrone2019-DET-train_0000016_01352_d_0000069.jpg

## B. 历史数据失败（MISSING 样本）

来源：visdrone_test_100/replaced_missing_backup_20260312/meta/records_before.tsv

1. 001_VisDrone2019-DET-train_0000249_01635_d_0000006.jpg
   - status: MISSING
   - image: /home/wangzhe/DroneP_VG/RefDrone/VisDrone2019-DET-train/images/0000249_01635_d_0000006.jpg
2. 021_VisDrone2019-DET-train_0000186_01387_d_0000188.jpg
   - status: MISSING
   - image: /home/wangzhe/DroneP_VG/RefDrone/VisDrone2019-DET-train/images/0000186_01387_d_0000188.jpg
3. 024_VisDrone2019-DET-train_9999973_00000_d_0000022.jpg
   - status: MISSING
   - image: /home/wangzhe/DroneP_VG/RefDrone/VisDrone2019-DET-train/images/9999973_00000_d_0000022.jpg
4. 026_VisDrone2019-DET-train_9999938_00000_d_0000107.jpg
   - status: MISSING
   - image: /home/wangzhe/DroneP_VG/RefDrone/VisDrone2019-DET-train/images/9999938_00000_d_0000107.jpg
5. 032_VisDrone2019-DET-train_9999952_00000_d_0000299.jpg
   - status: MISSING
   - image: /home/wangzhe/DroneP_VG/RefDrone/VisDrone2019-DET-train/images/9999952_00000_d_0000299.jpg
6. 040_VisDrone2019-DET-train_9999938_00000_d_0000021.jpg
   - status: MISSING
   - image: /home/wangzhe/DroneP_VG/RefDrone/VisDrone2019-DET-train/images/9999938_00000_d_0000021.jpg
