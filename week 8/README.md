# HealthConnect Clinic — No-Show Prediction

**Track:** AnalystLab Africa Experience Lab — Data Science Track
**Author:** Ange Maxime
**Status:** Week 8 (final) complete — Final Model, Documentation & Presentation

## Project Overview

HealthConnect Clinic (a fictional outpatient provider) wants to reduce lost
appointment capacity caused by patients who fail to attend a booked slot
without notice ("no-shows"). This project frames that as a supervised
binary classification problem and carries it from problem definition
through to a final, tested, documented model over the eight-week
AnalystLab Africa Experience Lab programme.

| Week | Stage | Main output |
|---|---|---|
| 4 | Problem Understanding → Solution Planning | Defined the ML problem, assessed data quality, proposed target/features/approach. No model trained. |
| 5 | Analysis → Initial Implementation | Data preparation, feature engineering, patient-grouped train/test strategy, first Logistic Regression + Random Forest baselines (ROC-AUC 0.678). |
| 6 | Integration → Validation *(not run in this workspace — see note below)* | Sunday-appointment data-quality fix, `gender` dropped on fairness grounds, 5-fold CV adopted (ROC-AUC 0.685). |
| 7 | Testing → Refinement | Stability (10-split), overfitting, and full-sample fairness testing; `age` dropped on fairness evidence; first serialised model artefact produced and tested. |
| 8 | Final Integration → Presentation | Resolved a multicollinearity finding, finalised the model, produced a fresh hand-off artefact, business interpretation, non-technical summary, and presentation materials. |

**Note on Weeks 6–7:** the Week 6 notebook was produced outside this
workspace and is not included here; the Week 7 notebook (supplied by the
user) is included for reference, and this README/Week 8 work treats its
documented Week 6 summary as given, consistent with the assignment's
"continue from previous work" instruction.

## Files in This Folder

| File | Week | Description |
|---|---|---|
| `HealthConnect_NoShow_Problem_Definition.ipynb` | 4 | Problem definition, data assessment, EDA, proposed target/features/approach. |
| `HealthConnect_Week4_Project_Summary.docx` | 4 | Concise Week 4 summary. |
| `HealthConnect_Week5_Baseline_Modelling.ipynb` | 5 | Data preparation, feature engineering, patient-grouped split, baseline models, evaluation. |
| `HealthConnect_Week5_Project_Summary.docx` | 5 | Concise Week 5 summary. |
| `HealthConnect_Week7_ModelTesting_Refinement.ipynb` | 7 | *(User-supplied, included for reference.)* Stability/overfitting/fairness testing of the Week 6 candidate model; `age` dropped; first serialised artefact. |
| `HealthConnect_Week8_Final_Model_DataScience.ipynb` | 8 | **Main Week 8 deliverable.** Week 7 review, multicollinearity fix (before/after test), final candidate model, Week 5→8 baseline comparison, full evaluation, error analysis, five-week decision log, business interpretation, use/non-use boundaries, final limitations register, hand-off documentation, mandatory HC-POD integration section, end-to-end walkthrough, non-technical summary, Week 8 project summary. |
| `HealthConnect_Week8_NonTechnical_Model_Summary.docx` | 8 | One-page plain-language model summary for clinic staff / non-technical stakeholders. |
| `HealthConnect_Week8_Presentation_Script.docx` | 8 | Talking-point script/outline for the required individual 5–10 minute final video (6 sections: intro, track contribution, cross-track collaboration, testing & outcome, overall solution, closing). |
| `healthconnect_noshow_model_final.joblib` | 8 | Final serialised model pipeline (preprocessing + Logistic Regression), reload-tested. See the interface specification in the Week 8 notebook §13 before using it. |
| `HealthConnect_Appointment_Data.csv` | — | Original dataset, 5,000 records. **Never modified** by any notebook. |
| `HealthConnect_Data_Dictionary.xlsx` | — | Variable definitions for the dataset. |
| `HealthConnect_Clinic_Knowledge_Base.docx` | — | Clinic operating rules, used throughout to cross-check the data (e.g. the Sunday-closure anomaly). |
| `LinkedIn_Post_Week5.md` | 5 | Draft LinkedIn post sharing Week 5 progress (professional-development requirement). |

## How to Run the Notebooks

1. Requirements: Python 3.10+, `pandas`, `numpy`, `matplotlib`, `seaborn`,
   `scikit-learn`, `joblib`, `openpyxl`, and Jupyter.
2. Keep all `.ipynb` files, `HealthConnect_Appointment_Data.csv`, and
   `HealthConnect_Data_Dictionary.xlsx` in the **same folder**.
3. Run notebooks in order (4 → 5 → 7 → 8) for full context, or open
   `HealthConnect_Week8_Final_Model_DataScience.ipynb` directly — it
   rebuilds everything it needs from the raw CSV and is self-contained.

```bash
pip install pandas numpy matplotlib seaborn scikit-learn joblib openpyxl jupyter
jupyter notebook HealthConnect_Week8_Final_Model_DataScience.ipynb
```

### Using the final model artefact

```python
import joblib
model = joblib.load('healthconnect_noshow_model_final.joblib')
risk_scores = model.predict_proba(new_appointments_df)[:, 1]  # see notebook §13 for the required input schema
```

## Final Model at a Glance

| Property | Value |
|---|---|
| Algorithm | Logistic Regression |
| Final features (12) | `booking_lead_days`, `previous_appointments`, `previous_no_shows`, `distance_to_clinic_km`, `is_new_patient`, `is_weekend`, `appointment_type`, `appointment_time`, `appointment_day`, `reminder_sent`, `reminder_channel`, `lead_time_bucket` |
| Excluded | `gender` (Wk6, fairness), `age` (Wk7, fairness), `historical_no_show_rate` (Wk8, multicollinearity), `waiting_time_minutes` (Wk4–5, leakage risk) |
| CV ROC-AUC | 0.684 (± 0.011), consistent with Week 7's 0.688 ± 0.012 stability estimate |
| Operating threshold | 0.44 (statistically stable; **not yet clinic-stakeholder-confirmed**) |
| Suitable for | Risk-based triage — prioritising reminder calls/reschedule offers |
| Not suitable for | Cancelled appointments, fully automated decisions, individual-level certainty, production use without re-validation on real data |

**The project's key finding, across all eight weeks:** ceiling performance
(~0.68 ROC-AUC) was reached early and never moved materially — every later
refinement (Sunday exclusion, dropping `gender`/`age`/`historical_no_show_rate`)
was about data quality, fairness, or interpretability, not chasing a
higher score. That is the story told in the Week 8 notebook's decision
log and is the main message for the final presentation.

## Outstanding Items (Not Yet Resolved)

1. `waiting_time_minutes` timing — needs a business-owner decision.
2. The 0.44 operating threshold needs a clinic stakeholder's sign-off
   against real intervention costs.
3. A residual (much-reduced) age-related fairness gap needs continued
   monitoring, not treatment as fully solved.
4. Everything is built on **synthetic data** — must be re-validated
   against real HealthConnect operational data before any production use.
5. This internship iteration has a single Data Science-track contributor;
   cross-track sections in Week 7–8 are documented, evidenced good-faith
   stand-ins rather than genuine live intern-to-intern exchanges.
