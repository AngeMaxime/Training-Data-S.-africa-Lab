# Week 2 — Feature Engineering & Data Preprocessing for Machine Learning

**Programme:** AnalystLab Africa — Data Science Internship
**Project:** Heart Disease Prediction — Feature Engineering & Preprocessing
**Role:** Junior Data Scientist, AnalystLab Africa Consulting

## Overview

This project prepares the [Heart Failure Prediction Dataset](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction)
(918 patient records, 11 clinical features) for machine learning. It covers data
inspection, cleaning, feature engineering, encoding, scaling, outlier treatment,
and feature selection, producing a clean dataset and a final machine-learning-ready
dataset.

**Objective:** Prepare the dataset for predicting whether a patient is likely to
develop heart disease.

**Target variable:** `HeartDisease` (1 = disease, 0 = no disease)

## Repository Structure

```
.
├── README.md
├── Week2_Heart_Disease_Feature_Engineering.ipynb   # Full notebook (code + outputs)
├── data/
│   └── heart.csv                                    # Original dataset
├── output/
│   ├── heart_cleaned.csv                             # Cleaned dataset (pre-encoding)
│   └── heart_ml_ready.csv                            # Final machine-learning-ready dataset
├── images/
│   ├── 01_boxplots_before.png
│   ├── 02_boxplots_after.png
│   ├── 03_age_histogram.png
│   ├── 04_sex_countplot.png
│   ├── 05_age_vs_maxhr_scatter.png
│   ├── 06_chestpain_countplot.png
│   ├── 07_correlation_heatmap.png
│   └── 08_feature_importance.png
└── reports/
    ├── Business_Understanding_Report.docx
    └── Data_Preprocessing_Report.docx
```

## Dataset

| | |
|---|---|
| Source | [Kaggle — fedesoriano/heart-failure-prediction](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction) |
| Records | 918 patients |
| Features | 11 clinical features + target |
| Target | `HeartDisease` (55.3% positive, 44.7% negative) |

## Pipeline Summary

| Stage | What was done |
|---|---|
| **Missing values** | `Cholesterol` (172 rows) and `RestingBP` (1 row) had physiologically impossible zeros, treated as missing and median-imputed by diagnosis group |
| **Duplicates** | None found; removal step included for reusability |
| **Data types** | Categorical columns cast to pandas `category` dtype |
| **Feature engineering** | Added `AgeGroup` (binned) and `HR_Reserve` (derived); `HR_Reserve` was later dropped after correlation analysis showed it was redundant with `MaxHR` |
| **Encoding** | Label Encoding (`Sex`, `ExerciseAngina`), ordinal mapping (`ST_Slope`, `AgeGroup`), One-Hot Encoding (`ChestPainType`, `RestingECG`) |
| **Scaling** | `StandardScaler` applied to continuous numeric features |
| **Outliers** | Detected via IQR method, capped (winsorised) rather than removed |
| **Feature selection** | Correlation heatmap + Random Forest feature importance; `HR_Reserve` and `AgeGroup` dropped as redundant with `MaxHR`/`Age` |

Full reasoning for every decision is documented in the notebook and in
`reports/Data_Preprocessing_Report.docx`.

## Key Findings

- `ST_Slope`, `MaxHR`, `Cholesterol`, `ChestPainType`, and `Oldpeak` are the strongest predictors of heart disease.
- No pair of original clinical features showed problematic multicollinearity (`|r| > 0.75`).
- The final dataset (`heart_ml_ready.csv`) has zero missing values, zero non-numeric columns, and passes a stratified train/test split with no errors.

## How to Run

```bash
pip install pandas numpy matplotlib seaborn scikit-learn jupyter
jupyter notebook Week2_Heart_Disease_Feature_Engineering.ipynb
```

Run all cells top to bottom. Outputs (cleaned/ML-ready CSVs and images) will be
written to `output/` and `images/`.

## Reports

- **Business Understanding Report** — business problem, objective, target variable, and expected impact
- **Data Preprocessing Report** — full documentation of every cleaning, encoding, scaling, outlier, and feature-selection decision

## Tools

Python, Jupyter Notebook, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn

## Author

Prepared as part of the AnalystLab Africa Data Science Internship Programme, Week 2 assignment.
