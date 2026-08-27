# HealthConnect Clinic - No-Show Prediction (Week 4)

**Track:** AnalystLab Africa Experience Lab - Data Science Track

**Author:** Ange Maxime

**Milestone:** Week 4 Machine Learning Problem Definition & Data Assessment

## Project Overview

HealthConnect Clinic (a fictional outpatient provider) wants to reduce lost
appointment capacity caused by patients who fail to attend a booked slot
without notice ("no-shows"). This project defines the machine learning
problem — a supervised binary classification task predicting, from
information known before the appointment, whether a scheduled appointment
will end in attendance or a no-show — and assesses whether the available
data can support it.

Week 4 focuses on problem definition and data assessment only. No predictive
model has been trained yet; that begins in Week 5.

## Files in This Folder

| File | Description |
|---|---|
| `HealthConnect_NoShow_Problem_Definition.ipynb` | **Main deliverable.** Executed Jupyter notebook containing the full problem definition, data assessment, data-quality checks, exploratory analysis (with charts), proposed target variable, candidate features, cancellation-handling approach, initial modelling approach, and key modelling considerations. |
| `HealthConnect_Week4_Project_Summary.docx` | Concise Week 4 summary covering the problem addressed, resources used, key observations, proposed approach, key considerations, and proposed focus for Week 5. Complements the notebook rather than repeating it. |
| `HealthConnect_Appointment_Data.csv` | The raw dataset: 5,000 synthetic appointment records, 18 columns. Included alongside the notebook so it re-runs standalone. |
| `HealthConnect_Data_Dictionary.xlsx` | Variable definitions, data types, examples, and notes for every column in the dataset. |
| `HealthConnect_Clinic_Knowledge_Base.docx` | Clinic operating rules (hours, booking, cancellation, late-arrival policy) — the source used to sanity-check the dataset against real business rules. *(Original file supplied for the project; keep it in the same folder if you want to re-verify the business-rule cross-checks.)* |

## How to Run the Notebook

1. Requirements: Python 3.10+, `pandas`, `numpy`, `matplotlib`, `seaborn`,
   `scikit-learn`, `openpyxl`, and Jupyter (Notebook, JupyterLab, or Google
   Colab all work).
2. Keep `HealthConnect_NoShow_Problem_Definition.ipynb`,
   `HealthConnect_Appointment_Data.csv`, and
   `HealthConnect_Data_Dictionary.xlsx` in the **same folder** — the
   notebook loads them by relative path.
3. Open the notebook and run all cells top to bottom (`Run All`).

```bash
pip install pandas numpy matplotlib seaborn scikit-learn openpyxl jupyter
jupyter notebook HealthConnect_NoShow_Problem_Definition.ipynb
```

## Key Findings at a Glance

- **Data quality:** No duplicate rows or appointment IDs. Missing values
  are minor and mostly explainable (`reminder_channel` is only null when
  `reminder_sent` is "No"; `distance_to_clinic_km` and
  `waiting_time_minutes` each have ~1–2% missing).
- **Target distribution:** 48.5% No-Show, 46.3% Attended, 5.3% Cancelled.
  Proposed modelling target is binary (No-Show vs. Attended), with
  Cancelled handled separately.
- **Strongest candidate predictors:** `booking_lead_days` (No-Show rate
  climbs from ~28% to ~68% as lead time increases) and
  `previous_no_shows` (a patient's own history is a strong signal).
- **Two data-quality issues flagged for follow-up:**
  - 737 appointments (14.7%) fall on a Sunday, despite the Knowledge Base
    stating the clinic is closed on Sundays.
  - `waiting_time_minutes` is populated even for appointments that were
    never attended, so its pre-/post-visit timing is ambiguous — it has
    been excluded from the initial feature set as a possible leakage risk.
- **Non-independence:** 5,000 appointment rows come from only 1,696 unique
  patients, so training/validation splits must be grouped by patient (or
  time-based), not purely random.

## Proposed Next Steps (Week 5)

1. Resolve the two flagged data-quality questions with the project
   stakeholder.
2. Build the preprocessing pipeline (encoding, imputation, the derived
   historical no-show rate feature).
3. Implement a patient-grouped train/validation/test split and train
   baseline models (Logistic Regression, then a tree-based ensemble).
4. Evaluate on recall/precision for the No-Show class and ROC-AUC/PR-AUC.
5. Begin a lightweight fairness check across gender and age groups.
