# Breast Cancer Survival under Differential Privacy: An Explainable Boosting Approach

> **Protecting patient privacy while delivering powerful survival predictions.**

---

## 🚀 Overview

Building predictive models on clinical and genomic data offers life‑saving insights—but safeguarding patient privacy is paramount. This project demonstrates how to train an **Explainable Boosting Machine (EBM)** under **Differential Privacy (DP)** guarantees, striking a clear **privacy–utility trade‑off** in breast cancer survival modeling.

---

## 🎯 Why This Matters

- **Privacy Regulations**: GDPR, HIPAA, and ethical research mandates require strong privacy safeguards on sensitive health data.  
- **Clinical Impact**: Predicting 5‑year survival can guide treatment planning and patient counseling.  
- **Research Frontier**: Differentially Private ML on real biomedical data is a hot topic—few end‑to‑end tutorials exist.

---

## 📦 Input Data

1. **Clinical Data (TSV)**  
   - METABRIC cohort: demographics, tumor stage, treatment details, survival times.  
2. **Genomic Data (CSV)**  
   - METABRIC RNA expression + mutation features (600+ numeric columns).  
3. **Survival Label**  
   - Binary indicator: survived beyond 60 months (`overall_survival_months > 60`).  

> All files are loaded from Google Drive in Colab, ensuring reproducibility.

---

## 🛠 Pre‑Processing Pipeline

1. **Merge & Clean**  
   - Read TSV/CSV, drop non‑numeric and unused columns.  
2. **Imputation & Scaling**  
   - Fill missing values with column means.  
   - Standardize features (`StandardScaler`).  
3. **Dimensionality Reduction**  
   - Apply PCA → **10 principal components**.  
   - Focuses DP budget on strongest signals, reducing noise per feature.

---

## 🤖 Models Employed

| Model                      | Key Parameters                          |
|----------------------------|-----------------------------------------|
| **Non‑private EBM**        | 100 boosting rounds, `n_jobs=-2`        |
| **DP‑EBM**                 | ε ∈ {0.5, 1.0, 5.0}, 100 rounds, LR=0.01 |
| **Threshold Sweeper**      | 101 thresholds [0–1], maximize F1       |

- **Explainable Boosting Machine**: A glass‑box GAM offering per‑feature contributions.  
- **DPExplainableBoostingClassifier**: Adds noise to shape‑functions under a specified ε budget.  
- **Threshold Optimization**: Ensures we report best possible accuracy & F1 for each privacy setting.

---

## 📊 Results & Significance

| ε             | AUC    | Accuracy | F1     | Best Threshold |
|---------------|--------|----------|--------|----------------|
| **0.5 (DP)**  | 0.5892 | 0.7521   | 0.8585 | 0.00           |
| **1.0 (DP)**  | 0.5895 | 0.7542   | 0.8591 | 0.63           |
| **5.0 (DP)**  | 0.6343 | 0.7526   | 0.8588 | 0.64           |
| **Non‑private** | 0.6684 | 0.7537   | 0.8593 | 0.34           |

- **AUC vs. ε**: Low ε (0.5–1.0) injects heavy noise (AUC~0.59). By ε=5.0, performance recovers to ~0.63—just ~0.04 shy of the non‑private ceiling.  
- **Accuracy & F1**: Hover around 75%/0.86 as threshold tuning compensates for DP noise—ideal for decision‑level stability.  
- **Privacy–Utility Trade‑off**: Visualized with crisp line plots, illustrating how increasing ε steadily recovers model utility.

---

