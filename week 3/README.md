# Heart Disease Prediction — AnalystLab Africa Data Science Internship

**Programme:** AnalystLab Africa — Data Science Internship
**Project:** Heart Disease Prediction — Data Preparation, Advanced Analysis & Feature Engineering
**Role:** Junior Data Scientist, AnalystLab Africa Consulting
**Progress:** Week 2 (Feature Engineering & Preprocessing) → Week 3 (Advanced Analysis, Statistical Validation & Feature Engineering) complete. Week 4 (Machine Learning Modelling) next.

## Overview

This project prepares and analyses the [Heart Failure Prediction Dataset](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction)
(918 patient records, 11 clinical features) to predict whether a patient is likely to
develop heart disease.

- **Week 2** covered data inspection, cleaning, feature engineering, encoding, scaling,
  outlier treatment, and feature selection, producing a cleaned dataset and a
  machine-learning-ready dataset.
- **Week 3** built directly on the Week 2 cleaned dataset (no cleaning steps repeated)
  to perform advanced exploratory analysis, six statistical hypothesis tests, four new
  engineered features, a combined feature evaluation, and a refined final modelling
  dataset ready for Week 4.

**Target variable:** `HeartDisease` (1 = disease, 0 = no disease)

## Repository Structure

```
.
├── README.md
├── requirements.txt
├── Week2_Heart_Disease_Feature_Engineering.ipynb     # Week 2 notebook (code + outputs)
├── Week3_Heart_Disease_Advanced_Analysis.ipynb       # Week 3 notebook (code + outputs)
├── data/
│   └── heart_cleaned.csv                              # Original dataset
├── Data saved/
│   └── heart_week3_final_modelling_dataset.csv         # Week 3: final refined modelling dataset
├── Results/
│   └── w3_01-11_*.png                                  # Week 3 visualizations
└── Presentation/
    ├── Project_Continuity_Summary.docx                   # Week 3
    ├── Statistical_Analysis_Summary.docx                 # Week 3
    ├── Feature_Engineering_Documentation.docx            # Week 3
    ├── Feature_Evaluation_and_Selection_Summary.docx     # Week 3
    ├── Business_Insights_and_Recommendations_Report.docx # Week 3
    └── Updated_Data_Dictionary.docx                      # Week 3
```

## Dataset

| | |
|---|---|
| Source | [Kaggle — fedesoriano/heart-failure-prediction](https://www.kaggle.com/datasets/fedesoriano/heart-failure-prediction) |
| Records | 918 patients |
| Original features | 11 clinical features + target |
| Final modelling features | 15 features + target (after Week 3 refinement) |
| Target | `HeartDisease` (55.3% positive, 44.7% negative) |

## Week 2 Summary — Preprocessing Pipeline

| Stage | What was done |
|---|---|
| **Missing values** | `Cholesterol` (172 rows) and `RestingBP` (1 row) had physiologically impossible zeros, treated as missing and median-imputed by diagnosis group |
| **Duplicates** | None found; removal step included for reusability |
| **Data types** | Categorical columns cast to pandas `category` dtype |
| **Feature engineering** | Added `AgeGroup` (binned) and `HR_Reserve` (derived) |
| **Encoding** | Label Encoding (`Sex`, `ExerciseAngina`), ordinal mapping (`ST_Slope`, `AgeGroup`), One-Hot Encoding (`ChestPainType`, `RestingECG`) |
| **Scaling** | `StandardScaler` applied to continuous numeric features |
| **Outliers** | Detected via IQR method, capped (winsorised) rather than removed |
| **Feature selection** | Correlation heatmap + Random Forest importance; `HR_Reserve` dropped as redundant with `MaxHR` |

## Week 3 Summary — Advanced Analysis, Statistics & Refinement

| Stage | What was done |
|---|---|
| **Advanced EDA** | 19 visualizations across distribution, bivariate, multivariate, and target-group comparisons |
| **Statistical testing** | 6 hypothesis tests (Mann-Whitney U, Chi-Square, Kruskal-Wallis, Spearman) — all significant (p < 0.001) |
| **Feature engineering** | Added `Cholesterol_Category`, `BP_Category`, `Oldpeak_Log`, `ClinicalRiskScore` |
| **Feature evaluation** | Correlation + mutual information + Random Forest importance on original and engineered features combined |
| **Dataset refinement** | Removed `AgeGroup` and `HR_Reserve` (confirmed redundant); retained all 4 new features |

Full reasoning for every decision is documented in the notebooks and in the corresponding
reports under `reports/`.

## Key Findings

- **`ClinicalRiskScore`** (a new Week 3 feature combining four risk flags) is the single
  strongest predictor identified so far — disease rate rises from 5.7% (score 0) to 100%
  (score 4).
- `ST_Slope`, `MaxHR`, `Cholesterol`, `ChestPainType`, and `Oldpeak` are the next strongest predictors.
- Asymptomatic chest pain (`ASY`) carries a 79% disease rate — higher than any other chest pain type.
- No pair of original clinical features showed problematic multicollinearity (`|r| > 0.75`);
  only mechanically redundant engineered features (`AgeGroup`, `HR_Reserve`) were removed.
- The final Week 3 modelling dataset (`heart_week3_final_modelling_dataset.csv`) has zero
  missing values and is ready for Week 4 encoding, scaling, and model development.

## How to Run

```bash
pip install -r requirements.txt
jupyter notebook Week2_Heart_Disease_Feature_Engineering.ipynb
jupyter notebook Week3_Heart_Disease_Advanced_Analysis.ipynb
```

Run all cells top to bottom in each notebook. Outputs (CSVs and images) are written to
`output/` and `images/`. The Week 3 notebook expects `heart_cleaned.csv` (produced by
Week 2) to be present in its working directory.

## Reports

**Week 3**
- **Project Continuity Summary** - how Week 3 builds on Weeks 1-2
- **Statistical Analysis Summary** - 6 hypothesis tests with H0/H1, method, results, and business implications
- **Feature Engineering Documentation** - how and why each new feature was created
- **Feature Evaluation and Selection Summary** - retain/remove decisions with evidence
- **Business Insights and Recommendations Report** - executive summary, findings, recommendations, limitations, next steps
- **Updated Data Dictionary** - full column reference for the final modelling dataset

## Tools

Python, Jupyter Notebook, Pandas, NumPy, Matplotlib, Seaborn, Scikit-learn, SciPy

## Author

Prepared as part of the AnalystLab Africa Data Science Internship Programme, Weeks 2-3.
