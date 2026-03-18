# Feedback Stage-B Summary (2026-03-18)

## Setup
- Method: epsilon-greedy bandit over sliding-window configs
- Reward: 0.5*Acc@0.5 + 0.3*Acc@0.75 + 0.2*mAP@0.5
- Source metrics: project_records/results/metrics/sw_grid_summary_20260318.tsv

## Selected Policy
- Best arm: cfg02 (window=896,896, overlap=0.20)
- True reward: 0.306870
- Metrics: Acc@0.5=0.3817, Acc@0.75=0.2840, mAP@0.5=0.1541

## Interpretation
- Feedback policy converges to a high-reward arm under the weighted objective.
- For this reward definition, cfg02 is favored due to strong Acc metrics with competitive mAP.
- If report focus shifts to mAP-first, reward weights should be adjusted (e.g., higher weight on mAP).
