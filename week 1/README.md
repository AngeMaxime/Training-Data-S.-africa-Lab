# IBM HR Analytics — Employee Attrition & Performance

A data science project analysing the [IBM HR Analytics Employee Attrition & Performance](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset/data) dataset, covering the full workflow from business framing through data inspection, cleaning, exploratory visualisation, and evidence-based HR insights.

## Overview

Employee attrition carries direct financial cost and disrupts organisational continuity. This project uses Python and Pandas to explore a real-world HR dataset of 1,470 employees, identify the workforce segments most affected by attrition, and surface the factors most strongly associated with it — providing an evidence base an HR team could act on.

**Key finding:** employees who work overtime leave at nearly **3x** the rate of those who don't (30.5% vs 10.4%), making it the single strongest attrition signal in the dataset.

## Repository Contents

| File | Description |
|---|---|
| `IBM_HR_Attrition_Analysis.ipynb` | Main Jupyter notebook — data loading, inspection, cleaning, 8 visualisations, and business insights |
| `WA_Fn-UseC_-HR-Employee-Attrition.csv` | Source dataset (1,470 rows × 35 columns) |
| `Business_Understanding_Report.docx` | Business context: attrition's cost impact and how data science supports HR decisions |
| `Dataset_Inspection_Report.docx` | Structured report on dataset shape, types, missing values, duplicates, and descriptive statistics |
| `Reflection_Report.docx` | Reflection on the project's learning experience |

## Dataset

- **Source:** Kaggle — IBM HR Analytics Employee Attrition & Performance (fictional dataset created by IBM data scientists)
- **Size:** 1,470 employees, 35 attributes
- **Target variable:** `Attrition` (Yes / No) — imbalanced at 16.1% Yes / 83.9% No
- **Quality:** no missing values, no duplicate records

## Project Structure

The analysis follows five parts:

1. **Business Understanding** — attrition's business impact and the role of data science in HR decision-making
2. **Data Loading & Inspection** — shape, types, missing values, duplicates, descriptive statistics
3. **Data Cleaning** — removal of non-informative constant columns, type validation
4. **Exploratory Data Analysis** — 3 bar charts, 2 histograms, 2 boxplots, 1 pie chart, each interpreted
5. **Business Insights** — five workforce observations and five evidence-backed attrition factors

## Key Insights

Attrition is most strongly associated with:

- **Overtime** — 30.5% attrition among overtime workers vs. 10.4% among non-overtime workers
- **Job role** — Sales Representatives (39.8%) and Laboratory Technicians (23.9%) far exceed the workforce average
- **Business travel intensity** — attrition rises from 8.0% (no travel) to 24.9% (frequent travel)
- **Compensation** — employees who leave earn ~30% less on average than those who stay
- **Life stage** — single employees leave at 25.5%, more than double the rate of married employees (12.5%)

## Requirements

```
python >= 3.9
pandas
numpy
matplotlib
seaborn
jupyter
```

Install with:

```bash
pip install pandas numpy matplotlib seaborn jupyter
```

## Usage

1. Clone or download this repository.
2. Ensure `WA_Fn-UseC_-HR-Employee-Attrition.csv` is in the same directory as the notebook.
3. Launch and run the notebook:

```bash
jupyter notebook IBM_HR_Attrition_Analysis.ipynb
```

## Author

Prepared as part of [Learn Data Science the Smart Way](https://learndatascience.ai).

## License

This project uses a publicly available Kaggle dataset for educational purposes. Refer to the [original Kaggle listing](https://www.kaggle.com/datasets/pavansubhasht/ibm-hr-analytics-attrition-dataset/data) for dataset licensing terms.
