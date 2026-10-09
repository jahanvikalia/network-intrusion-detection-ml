# ML-Based Network Intrusion Detection System (CICIDS2017)

Multi-class network intrusion detection using tree-based machine learning models trained on flow-level traffic features from the CICIDS2017 dataset.

## Overview

The model classifies each network flow into one of 7 classes:

| Class | Flows in sample |
|---|---|
| BENIGN | 22,731 |
| DoS | 19,035 |
| PortScan | 7,946 |
| BruteForce | 2,767 |
| WebAttack | 2,180 |
| Bot | 1,966 |
| Infiltration | 36 |

Sample used: 56,661 flows × 77 flow features (packet counts, packet lengths, flow duration, inter-arrival times, active/idle times, TCP flags, etc.).

## Pipeline

1. **Sampling** – stratified random sampling of majority classes (BENIGN, DoS, PortScan, BruteForce) to keep the dataset manageable
2. **Preprocessing** – min-max normalisation of numeric features, missing values filled with 0, label encoding
3. **Train/test split** – 80/20, stratified
4. **Class imbalance** – SMOTE oversampling of the rarest class (Infiltration: 29 → 1,500 training samples)
5. **Models** – Decision Tree, Random Forest, Extra Trees, XGBoost
6. **Evaluation** – accuracy, weighted precision/recall/F1, per-class report, confusion matrix

## Results (test set, 11,333 flows)

| Model | Accuracy | Weighted F1 |
|---|---|---|
| Decision Tree | 99.55% | 99.56% |
| Random Forest | 99.65% | 99.64% |
| Extra Trees | _fill after re-run_ | |
| XGBoost | _fill after re-run_ | |

**Hardest class:** Infiltration (only 7 test samples) – Random Forest recall 0.71.

## Known limitations

- Normalisation is fitted on the full dataset before the split (minor leakage) – fix: fit scaler on train only
- Very high accuracy is typical for CICIDS2017 and partly reflects how separable the dataset is; performance on real traffic will be lower
- Infiltration class is too small for reliable evaluation

## Run it

```bash
pip install -r requirements.txt
jupyter notebook ML_Based_IDS.ipynb
```

Dataset: [CICIDS2017 – Canadian Institute for Cybersecurity](https://www.unb.ca/cic/datasets/ids-2017.html). Place `CICIDS2017_sample.csv` in the repo root (not committed due to size).

## Credits

Pipeline adapted from the open-source tree-based IDS work by Li Yang et al. (Western-OC2-Lab, *Intrusion-Detection-System-Using-Machine-Learning*). Modifications and analysis in this repo are my own.
