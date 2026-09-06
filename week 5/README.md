# HealthConnect Clinic, No-Show Prediction

**Track:** AnalystLab Africa Experience Lab - Data Science Track
**Author:** Ange Maxime TCHOUTANG
**Status:** Week 5 complete - Data Preparation, Feature Engineering & Baseline Model

## Project Overview

HealthConnect Clinic (a fictional outpatient provider) wants to reduce lost
appointment capacity caused by patients who fail to attend a booked slot
without notice ("no-shows"). This project frames that as a supervised
binary classification problem, predicting from information known before
the appointment, whether a scheduled appointment will end in attendance or
a no-show and works through problem definition, data preparation, feature
engineering, and an initial baseline model.

- **Week 4**: defined the ML problem, assessed data quality, and proposed a
  target variable, candidate features, and a modelling approach. No model
  was trained.
- **Week 5**: prepared the data for modelling, engineered new features,
  defined a leakage-safe train/test strategy, and trained + evaluated two
  baseline classification models (Logistic Regression and Random Forest).

## Files in This Folder

| File | Week | Description |
|---|---|---|
| `HealthConnect_NoShow_Problem_Definition.ipynb` | 4 | Problem definition, initial data assessment, data-quality checks, exploratory analysis, proposed target/features, initial modelling approach and key considerations. |
| `HealthConnect_Week4_Project_Summary.docx` | 4 | Concise Week 4 summary (problem, resources, observations, approach, considerations, Week 5 focus). |
| `HealthConnect_Week5_Baseline_Modelling.ipynb` | 5 | **Main Week 5 deliverable.** Executed notebook: Week 4 review, data preparation, decision-linked visual evidence, feature engineering, patient-grouped train/test strategy, Logistic Regression + Random Forest baseline models, evaluation (accuracy/precision/recall/F1/confusion matrix/ROC-AUC), interpretation, limitations, updated risk register, cross-track collaboration note, and Week 6 recommendations. |
| `HealthConnect_Week5_Project_Summary.docx` | 5 | Concise Week 5 summary (planned vs. completed, key findings, challenges, decisions, changes to the Week 4 approach, collaboration, remaining work, Week 6 focus). |
| `HealthConnect_Appointment_Data.csv` | — | The original dataset: 5,000 synthetic appointment records, 18 columns. **Never modified** — both notebooks load it read-only and keep all cleaning/derived data in memory. |
| `HealthConnect_Data_Dictionary.xlsx` | — | Variable definitions, data types, examples, and notes for every column in the dataset. |
| `HealthConnect_Clinic_Knowledge_Base.docx` | — | Clinic operating rules (hours, booking, cancellation, late-arrival policy) — used to cross-check the dataset against real business rules (e.g. the Sunday-closure anomaly flagged below). |

## How to Run the Notebooks

1. Requirements: Python 3.10+, `pandas`, `numpy`, `matplotlib`, `seaborn`,
   `scikit-learn`, `openpyxl`, and Jupyter (Notebook, JupyterLab, or Google
   Colab all work).
2. Keep both `.ipynb` files, `HealthConnect_Appointment_Data.csv`, and
   `HealthConnect_Data_Dictionary.xlsx` in the **same folder** - each
   notebook loads the data by relative path.
3. Open a notebook and run all cells top to bottom (`Run All`). Read
   `HealthConnect_NoShow_Problem_Definition.ipynb` (Week 4) first, then
   `HealthConnect_Week5_Baseline_Modelling.ipynb` (Week 5), which builds on it.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
jupyter notebook HealthConnect_Week5_Baseline_Modelling.ipynb
```

## Key Findings at a Glance

**Data quality (confirmed in both weeks).** No duplicate rows or
appointment IDs. Missing values are minor and mostly structural
(`reminder_channel` is only null when `reminder_sent` is "No";
`distance_to_clinic_km` and `waiting_time_minutes` each have ~1–2%
missing). 1,696 unique patients generate 5,000 rows, so train/test splits
must be **patient-grouped**, not random.

**Two open data-quality questions, still unresolved - need a business
decision:**

- 737 appointments (14.7%) fall on a Sunday, despite the Knowledge Base
  stating the clinic is closed Sundays. *Decision taken: rows retained,
  results near Sunday treated as lower-confidence.*

- `waiting_time_minutes` is populated even for appointments that were
  never attended, so its pre-/post-visit timing is ambiguous. *Decision
  taken: excluded from the feature set as a likely leakage risk.*

**Target.** Binary: No-Show vs. Attended (48.5% / 46.3% of appointments);
Cancelled (5.3%) is handled separately, not folded into either class.

**Baseline model results (Week 5, patient-grouped 80/20 split, 966 test
rows):**

| Model | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Majority-class baseline | 0.50 | 0.50 | 1.00 | 0.667* | 0.50 |
| Logistic Regression | 0.63 | 0.625 | 0.652 | 0.638 | 0.678 |
| Random Forest | 0.63 | 0.628 | 0.640 | 0.634 | 0.682 |

\* *The dummy baseline's F1 looks numerically high only because it always
predicts "No-Show" on a near-balanced test set. It has zero real
predictive power (ROC-AUC 0.50). See the notebook's interpretation section
for the full explanation.*

Both real models clearly beat chance (ROC-AUC ~0.68) and perform almost
identically to each other. **Logistic Regression is the preferred baseline
going into Week 6**, since it matches the Random Forest's performance
while remaining interpretable for non-technical clinic staff. The
strongest predictors in both models are `historical_no_show_rate`,
`previous_no_shows`, and `booking_lead_days`.

## Proposed Focus for Week 6

1. Resolve the Sunday-appointment and `waiting_time_minutes` data-quality
   questions with the project/business owner.
2. Cross-validate both models with `GroupKFold` instead of a single split.
3. Tune the classification threshold and Random Forest hyperparameters
   against the clinic's real cost trade-off.
4. Compare the patient-grouped split against a time-based split.
5. Run a fairness check on model error rates across `gender` and
   `age_group`.
6. Design a lightweight approach for the ~5% of appointments that end in
   cancellation, which the current model does not cover.
7. Deploy the model
