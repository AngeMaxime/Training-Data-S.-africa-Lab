# HealthConnect Clinic — No-Show Prediction
### Data Science Track · AnalystLab Africa Experience Lab

**Intern:** Ange Maxime TCHOUTANG
**Track:** Data Science
**Programme:** AnalystLab Africa — Experience Lab Internship

---

## 1. Project Overview

HealthConnect Clinic is a fictional healthcare provider losing capacity to missed appointments. This repository contains the Data Science track's contribution to the shared HealthConnect Experience Lab project:

> **How can HealthConnect Clinic use data and AI to reduce missed appointments and improve the patient support experience?**

The Data Science track's specific job is to turn that question into a well-posed, validated machine learning problem: predict, from information known at or before booking, whether a scheduled patient will attend or no-show — so the clinic can target reminders and outreach at the appointments most at risk.

This is a training exercise. The dataset, patients, and clinic are fictional and synthetic; no real medical or clinic information is used or invented.

---

## 2. Repository Structure

```
HealthConnect-DataScience/
├── data/
│   ├── HealthConnect_Appointment_Data.csv        # Raw appointment dataset (5,000 rows, 18 columns)
│   └── HealthConnect_Data_Dictionary.xlsx        # Variable definitions
├── resources/
│   └── HealthConnect_Clinic_Knowledge_Base.docx  # Approved clinic operations reference (GenAI track resource)
├── notebooks/
│   ├── HealthConnect_Week4_DataScience_Analysis.ipynb          # Week 4 — data profiling & feasibility
│   ├── HealthConnect_Week5_Baseline_Modelling.ipynb             # Week 5 — baseline models
│   └── HealthConnect_Week6_ModelImprovement_Validation.ipynb    # Week 6 — error analysis, refinement, validation
├── reports/
│   ├── HealthConnect_Week4_DataScience_MLProblemDefinition.docx # Week 4 — ML problem definition document
│   └── HealthConnect_Week6_DataScience_Report.docx               # Week 6 — integration & validation report
└── README.md
```

> Adjust the paths above if your local/GitHub layout differs — rename folders to match your actual repo and update the links in this file accordingly.

---

## 3. Project Progress by Week

| Week | Focus | Key Output | Status |
|---|---|---|---|
| [Week 4](notebooks/HealthConnect_Week4_DataScience_Analysis.ipynb) | Problem understanding & data feasibility | ML problem definition, initial data profiling | ✅ Complete |
| [Week 5](notebooks/HealthConnect_Week5_Baseline_Modelling.ipynb) | Baseline modelling | Logistic Regression & Random Forest baselines | ✅ Complete |
| [Week 6](notebooks/HealthConnect_Week6_ModelImprovement_Validation.ipynb) | Integration, improvement & validation | Refined, cross-validated candidate model | ✅ Complete |
| Week 7 | Testing & refinement | End-to-end model testing | ⏳ Upcoming |
| Week 8 | Final integration & presentation | Final deliverable | ⏳ Upcoming |

### Week 4 — Problem Understanding & Feasibility
Defined the no-show problem as binary classification (Attended vs. No-Show), profiled the real dataset (5,000 appointments, 1,696 patients), confirmed the data was clean with only two genuinely-missing numeric columns, and found the target close to balanced (51.2% / 48.8%) once cancellations are excluded. Identified `previous_no_shows` and `booking_lead_days` as the earliest strong signals.

### Week 5 — Baseline Modelling
Built a patient-grouped train/test split (to prevent the same patient leaking across both sets), engineered features (`historical_no_show_rate`, `is_new_patient`, `is_weekend`, `lead_time_bucket`), and trained Logistic Regression and Random Forest baselines. Logistic Regression was preferred for its interpretability at parity with Random Forest (ROC-AUC 0.678 vs. 0.682). Flagged six open items for Week 6, including an unresolved data-quality anomaly around Sunday-dated appointments.

### Week 6 — Integration, Improvement & Validation
- **Error analysis:** located the baseline's failure mode — errors cluster at the 0.5 decision boundary rather than being random.
- **Cross-track integration:** consulted the Clinic Knowledge Base (Generative AI track's resource) to confirm the clinic is closed Sundays, resolving Week 5's open question and removing 704 invalid rows from the modelling data.
- **Feature refinement:** tested and dropped `gender` (no predictive value); tested and rejected an interaction feature.
- **Robust validation:** replaced the single train/test split with 5-fold patient-grouped cross-validation (ROC-AUC 0.685 ± 0.011), plus a time-based split for comparison.
- **Model comparison:** tested tuned Random Forest and Gradient Boosting against the refined Logistic Regression — none outperformed it, confirming the signal in this data is close to linear.
- **Threshold tuning, fairness audit, and a decision on Cancelled appointments** — all detailed in the Week 6 notebook and report.

---

## 4. Key Results (Final Candidate Model)

**Model:** Logistic Regression · refined feature set (`gender` excluded) · Sunday-cleaned data · 0.44 operating threshold

| Evaluation | Accuracy | Precision | Recall | F1 | ROC-AUC |
|---|---|---|---|---|---|
| Week 5 baseline (single split) | 0.630 | 0.625 | 0.652 | 0.638 | 0.678 |
| Week 6 refined model (5-fold CV, 0.5 threshold) | 0.632 | 0.638 | 0.641 | 0.639 | **0.685 ± 0.011** |
| Week 6 refined model (time-based split) | 0.646 | 0.653 | 0.640 | 0.647 | 0.696 |
| Week 6 refined model (recommended 0.44 threshold) | ~0.60 | ~0.58 | **~0.75** | ~0.65 | — |

**Strongest predictors:** `historical_no_show_rate`, `previous_no_shows`, `booking_lead_days`.

**Intended use:** a triage / risk-scoring tool to prioritise staff outreach (reminder calls, reschedule offers) toward the highest-risk appointments — not an automated accept/reject decision-maker. See the Week 6 report for the full suitability assessment.

---

## 5. Data

| File | Description |
|---|---|
| `HealthConnect_Appointment_Data.csv` | 5,000 fictional, anonymised appointment records: demographics, booking details, appointment history, reminders, distance to clinic, and outcome. |
| `HealthConnect_Data_Dictionary.xlsx` | Definitions for every variable in the dataset above. |
| `HealthConnect_Clinic_Knowledge_Base.docx` | Approved reference for clinic hours, services, and procedures (primary resource for the Generative AI track; used here in Week 6 to resolve a data-quality question). |

Original resource files are never modified in place; all cleaning and feature engineering happens on in-memory copies within the notebooks.

---

## 6. Setup & Installation

```bash
# Clone the repository
git clone <your-repo-url>
cd HealthConnect-DataScience

# Create and activate a virtual environment (recommended)
python -m venv venv
source venv/bin/activate      # Windows: venv\Scripts\activate

# Install dependencies
pip install pandas numpy scikit-learn matplotlib seaborn openpyxl python-docx jupyter
```

## 7. How to Reproduce

```bash
jupyter notebook
```

Open notebooks in order — each one documents what it reproduces from the previous week before building forward:

1. `notebooks/HealthConnect_Week4_DataScience_Analysis.ipynb`
2. `notebooks/HealthConnect_Week5_Baseline_Modelling.ipynb`
3. `notebooks/HealthConnect_Week6_ModelImprovement_Validation.ipynb`

All notebooks read directly from `data/`, so keep the folder structure above intact, or update the file paths at the top of each notebook to match your local layout.

---

## 8. Key Decisions & Assumptions

- **Cancelled appointments** are excluded from the primary no-show model — a cancellation is a communicated decision, not an unpredicted absence. No predictive signal was found for cancellations themselves, so a fixed ~5.3% operational buffer is recommended instead of a dedicated model.
- **Sunday-dated appointments** (704 rows) were excluded in Week 6 after confirming against the Clinic Knowledge Base that the clinic is closed Sundays — these cannot represent genuine appointments.
- **`waiting_time_minutes`** is excluded from every model as a leakage risk — it is only known once a patient is already at the clinic, after the outcome that matters has effectively been decided.
- **`gender`** is excluded as a model input (no measurable predictive value) but retained for the fairness audit.
- Train/test splitting is **patient-grouped**, not row-random, since 1,696 patients generate 5,000+ appointments and a naive split would leak the same patient into both sets.

## 9. Known Limitations

- Dataset is synthetic; all findings require re-validation on real operational data before any production use.
- ROC-AUC of ~0.685-0.696 is a real but moderate improvement over chance — not strong enough for full automation.
- The age-group fairness audit (Week 6) found a directionally consistent flagging-rate skew toward younger patients, but per-group sample sizes (75-203) are too small for a firm conclusion; needs revisiting with more data.
- `waiting_time_minutes`'s exact collection timing is still unresolved with a business stakeholder.
- This internship iteration involves a single Data Science-track contributor; the Week 6 cross-track integration uses another track's resource (the Knowledge Base) but does not reflect a live intern-to-intern handoff — see the Week 6 report for the full scope note.

## 10. Roadmap

- **Week 7:** serialise the final model artefact (`joblib`) and test it end-to-end; confirm the 0.44 operating threshold with stakeholders; strengthen the fairness audit with a larger evaluation sample; resolve the `waiting_time_minutes` question.
- **Week 8:** final integration and presentation.

---

## 11. Acknowledgements

Built as part of the **AnalystLab Africa Experience Lab Internship Programme**. HealthConnect Clinic, its patients, and its dataset are fictional and used for training purposes only.
