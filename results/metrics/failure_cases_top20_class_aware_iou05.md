# GroundingDINO 失败样例清单（class-aware, IoU=0.5）

- 预测目录: /home/wangzhe/DroneP_VG/project_records/results/predictions/best_groundingdino_preds
- 口径: 按单图 recall 从低到高排序

| rank | stem | gt_boxes | matched | missed | recall | pred_boxes | image_path |
| --- | --- | ---: | ---: | ---: | ---: | ---: | --- |
| 1 | 033_VisDrone2019-DET-train_9999962_00000_d_0000142 | 37 | 0 | 37 | 0.0000 | 41 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/033_VisDrone2019-DET-train_9999962_00000_d_0000142.jpg |
| 2 | 010_VisDrone2019-DET-train_9999962_00000_d_0000150 | 28 | 0 | 28 | 0.0000 | 30 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/010_VisDrone2019-DET-train_9999962_00000_d_0000150.jpg |
| 3 | 015_VisDrone2019-DET-test_9999938_00000_d_0000209 | 212 | 6 | 206 | 0.0283 | 18 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/015_VisDrone2019-DET-test_9999938_00000_d_0000209.jpg |
| 4 | 009_VisDrone2019-DET-test_0000073_01275_d_0000002 | 115 | 5 | 110 | 0.0435 | 35 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/009_VisDrone2019-DET-test_0000073_01275_d_0000002.jpg |
| 5 | 091_VisDrone2019-DET-train_0000013_00465_d_0000067 | 92 | 6 | 86 | 0.0652 | 40 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/091_VisDrone2019-DET-train_0000013_00465_d_0000067.jpg |
| 6 | 058_VisDrone2019-DET-test_9999941_00000_d_0000006 | 14 | 1 | 13 | 0.0714 | 15 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/058_VisDrone2019-DET-test_9999941_00000_d_0000006.jpg |
| 7 | 098_VisDrone2019-DET-train_0000016_01352_d_0000069 | 61 | 5 | 56 | 0.0820 | 32 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/098_VisDrone2019-DET-train_0000016_01352_d_0000069.jpg |
| 8 | 100_VisDrone2019-DET-train_0000030_00754_d_0000036 | 131 | 12 | 119 | 0.0916 | 36 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/100_VisDrone2019-DET-train_0000030_00754_d_0000036.jpg |
| 9 | 089_VisDrone2019-DET-train_9999937_00000_d_0000182 | 109 | 11 | 98 | 0.1009 | 38 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/089_VisDrone2019-DET-train_9999937_00000_d_0000182.jpg |
| 10 | 070_VisDrone2019-DET-train_9999953_00000_d_0000227 | 9 | 1 | 8 | 0.1111 | 4 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/070_VisDrone2019-DET-train_9999953_00000_d_0000227.jpg |
| 11 | 099_VisDrone2019-DET-train_0000084_00281_d_0000002 | 81 | 11 | 70 | 0.1358 | 61 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/099_VisDrone2019-DET-train_0000084_00281_d_0000002.jpg |
| 12 | 093_VisDrone2019-DET-train_0000016_00420_d_0000068 | 46 | 7 | 39 | 0.1522 | 17 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/093_VisDrone2019-DET-train_0000016_00420_d_0000068.jpg |
| 13 | 056_VisDrone2019-DET-train_9999982_00000_d_0000060 | 65 | 11 | 54 | 0.1692 | 37 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/056_VisDrone2019-DET-train_9999982_00000_d_0000060.jpg |
| 14 | 071_VisDrone2019-DET-train_0000010_05149_d_0000057 | 57 | 10 | 47 | 0.1754 | 30 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/071_VisDrone2019-DET-train_0000010_05149_d_0000057.jpg |
| 15 | 021_VisDrone2019-DET-train_0000002_00448_d_0000015 | 85 | 15 | 70 | 0.1765 | 28 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/021_VisDrone2019-DET-train_0000002_00448_d_0000015.jpg |
| 16 | 002_VisDrone2019-DET-train_0000279_03601_d_0000600 | 164 | 32 | 132 | 0.1951 | 75 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/002_VisDrone2019-DET-train_0000279_03601_d_0000600.jpg |
| 17 | 085_VisDrone2019-DET-train_9999966_00000_d_0000054 | 97 | 19 | 78 | 0.1959 | 42 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/085_VisDrone2019-DET-train_9999966_00000_d_0000054.jpg |
| 18 | 036_VisDrone2019-DET-test_9999952_00000_d_0000232 | 66 | 13 | 53 | 0.1970 | 28 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/036_VisDrone2019-DET-test_9999952_00000_d_0000232.jpg |
| 19 | 084_VisDrone2019-DET-train_9999998_00031_d_0000024 | 25 | 5 | 20 | 0.2000 | 31 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/084_VisDrone2019-DET-train_9999998_00031_d_0000024.jpg |
| 20 | 019_VisDrone2019-DET-train_0000334_01177_d_0000027 | 54 | 11 | 43 | 0.2037 | 34 | /home/wangzhe/DroneP_VG/visdrone_test_100/images/019_VisDrone2019-DET-train_0000334_01177_d_0000027.jpg |
