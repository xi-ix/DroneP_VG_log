# DroneP_VG 会话交接报告

日期：2026-03-12

## 1. 当前已经完成的内容

### 1.1 100 张评估子集已修复为全带标注版本

- 目录：/home/wangzhe/DroneP_VG/visdrone_test_100
- 当前状态：100 张图像，100 个标注文件
- 原先缺标注的 20 个 train 样本已用 20 个新的带标注 train 样本替换
- 替换映射：/home/wangzhe/DroneP_VG/visdrone_test_100/replaced_missing_backup_20260312/meta/replacement_map.tsv

### 1.2 100 张子集的数据画像已生成

- 目录：/home/wangzhe/DroneP_VG/visdrone_test_100/data_profile_assets
- 已生成：
  - data_processing_log.md
  - data_distribution.png
  - slice_showcase.png
  - slice_examples/

100 张子集的关键统计：

- 正样本图像：100
- 负样本图像：0
- 总有效框数：5077
- 每图平均目标数：50.77
- 小目标占比：61.32%
- 小目标阈值：bbox_area / image_area < 0.001

### 1.3 120 张报告子集已生成

- 目录：/home/wangzhe/DroneP_VG/visdrone_report_subset_120
- 当前状态：120 张图像，120 个标注文件
- 其中：
  - 正样本：100 张
  - 负样本：20 张
  - 空标注文件：20 个
- 负样本清单：/home/wangzhe/DroneP_VG/visdrone_report_subset_120/report_subset_manifest.tsv

### 1.4 脚本能力已补齐

- /home/wangzhe/DroneP_VG/scripts/import_cv2.py
  - 现在已兼容 VisDrone 原生逗号标注格式
- /home/wangzhe/DroneP_VG/scripts/analyze_extracted_dataset.py
  - 可自动生成数据画像日志、分布图和切片展示样例

### 1.5 VS Code Remote 代理已配置

- 脚本位置：/home/wangzhe/.vscode-server/server-env-setup
- 逻辑：每次通过 VS Code Remote SSH 连接服务器时，自动读取当前 SSH 客户端 IP，并把代理指向 当前客户端IP:7897
- 已测试通过一次 GitHub 连通性
- 说明：如果本地电脑重启后 IP 变化，只要重新连一次 VS Code Remote，脚本会自动切换到新的客户端 IP，不需要手改配置

### 1.6 GroundingDINO Python 依赖已基本就位

当前 qwen_vl 环境中已确认存在：

- supervision
- addict
- yapf
- timm
- pycocotools

## 2. 当前未完成的内容

下面两项还没有真正跑完：

1. 用 GroundingDINO 对当前 100 张子集跑一版基线预测
2. 生成第一版指标表，并与 120 张报告子集画像整合为完整报告

## 3. 当前阻塞点

### 3.1 GroundingDINO 权重文件疑似未完整下载

当前文件：

- /home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth

当前大小：3.3M

这个大小明显不像完整官方权重，大概率是中途中断后的残留文件，因此虽然 GroundingDINO 导入链路已经基本打通，但还不能安全进入真实推理阶段。

下次继续前，第一件事应当是确认这个文件是否为完整权重。如果不是，需要重新下载。

建议先执行：

```bash
ls -lh /home/wangzhe/GroundingDINO/weights/groundingdino_swint_ogc.pth
```

如果大小仍然只有几 MB，就删除后重新下载。

## 4. 下次继续时的优先顺序

### 步骤 1：确认权重文件可用

```bash
cd /home/wangzhe/GroundingDINO
ls -lh weights/groundingdino_swint_ogc.pth
```

如果文件异常，重新下载：

```bash
cd /home/wangzhe/GroundingDINO
rm -f weights/groundingdino_swint_ogc.pth
wget -O weights/groundingdino_swint_ogc.pth https://github.com/IDEA-Research/GroundingDINO/releases/download/v0.1.0-alpha/groundingdino_swint_ogc.pth
```

### 步骤 2：做一次最小导入验证

```bash
cd /home/wangzhe/GroundingDINO
/home/wangzhe/miniconda3/envs/qwen_vl/bin/python -c "import sys; sys.path.insert(0, '/home/wangzhe/GroundingDINO'); from groundingdino.util.inference import Model; print('ok')"
```

### 步骤 3：跑 100 张基线预测

目标输入目录：

- /home/wangzhe/DroneP_VG/visdrone_test_100/images

目标输出目录建议：

- /home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds

### 步骤 4：生成第一版指标表

使用：

- /home/wangzhe/DroneP_VG/scripts/evaluate_detection_metrics.py

真值目录：

- /home/wangzhe/DroneP_VG/visdrone_test_100/annotations

预测目录：

- /home/wangzhe/DroneP_VG/visdrone_test_100/baseline_groundingdino_preds

### 步骤 5：更新 120 张报告子集的画像与总报告

报告子集目录：

- /home/wangzhe/DroneP_VG/visdrone_report_subset_120

建议新生成：

- 一份 120 张子集的数据画像日志
- 一份把“100 张基线指标 + 120 张画像统计 + 好坏样例”整合起来的汇总 Markdown

## 5. 与下次对话时可直接引用的话

下次可以直接对我说：

"继续从 session_handoff_20260312.md 往下做，先检查 GroundingDINO 权重是否完整；如果完整，就跑 100 张基线预测、生成第一版指标表，再更新 120 张报告子集的完整画像报告。"