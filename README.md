# Hospital Resource Demand Forecasting

An end-to-end machine learning project for forecasting daily hospital admissions using historical operational patterns, time-series feature engineering, and predictive modeling.

> **Note:** This project uses synthetic hospital operations data created for portfolio and educational purposes. It contains no patient records, PHI, employer data, or confidential healthcare information.

## Project Overview

Hospitals need to anticipate changes in patient demand to support decisions around staffing, bed capacity, scheduling, and other operational resources.

This project develops a machine learning workflow to forecast daily hospital admissions using historical demand patterns.

The project covers:

- Exploratory data analysis (EDA)
- Hospital operations analysis
- Seasonality analysis
- Time-series feature engineering
- Lag and rolling-average features
- Chronological train/test splitting
- Baseline forecasting
- Linear Regression
- Random Forest Regression
- Model evaluation using MAE and RMSE
- Feature importance analysis
- Model artifact and prediction storage

## Dataset

The project uses a synthetic hospital operations dataset covering:

**January 1, 2024 – December 31, 2025**

The original dataset contains:

- 2,924 records
- 1 synthetic hospital
- 4 departments:
  - Emergency
  - Medical
  - Pediatrics
  - Surgical

### Main Fields

| Field | Description |
|---|---|
| `date` | Date of observation |
| `hospital_id` | Synthetic hospital identifier |
| `department` | Hospital department |
| `admissions` | Number of patient admissions |
| `discharges` | Number of patient discharges |
| `available_beds` | Total available beds |
| `occupied_beds` | Number of occupied beds |
| `staff_count` | Staff scheduled |
| `procedure_count` | Number of procedures |

## Exploratory Data Analysis

EDA was performed to understand hospital demand and operational patterns.

Key analyses included:

- Daily admissions trends
- Admissions by department
- Bed utilization
- Staffing levels
- Procedure volume
- Admissions/staffing correlation
- Monthly seasonality
- Department-level seasonal patterns

In this synthetic dataset, Emergency had the highest average admissions, and overall admissions showed a clear seasonal pattern with higher demand around winter and lower demand around summer.

## Feature Engineering

Department-level records were aggregated into daily hospital-level observations.

Calendar features:

- `day_of_week`
- `month`
- `day_of_year`

Historical demand features:

- `admissions_lag_1`
- `admissions_lag_7`
- `admissions_lag_14`

Rolling demand features:

- `admissions_rolling_7`
- `admissions_rolling_14`

Rolling averages were shifted by one day before calculation so that the current day's target was not used as an input feature, helping prevent target leakage.

After feature engineering and removal of initial rows without sufficient historical context, the modeling dataset contained **717 daily observations**.

## Train/Test Strategy

Because this is a time-series forecasting problem, the data was split chronologically rather than randomly.

### Training Period

**January 15, 2024 – August 9, 2025**

573 observations.

### Testing Period

**August 10, 2025 – December 31, 2025**

144 observations.

This setup evaluates the models on future observations that were not used during training.

## Models

Three forecasting approaches were evaluated.

### Baseline

The baseline assumes:

**Today's admissions ≈ yesterday's admissions**

This provides a simple benchmark that machine learning models should improve upon.

### Linear Regression

Linear Regression was trained using calendar, lag, and rolling-average features.

### Random Forest

A Random Forest Regressor with 200 trees was trained to capture nonlinear relationships between historical demand features and future admissions.

## Model Performance

| Model | MAE | RMSE |
|---|---:|---:|
| Baseline | 8.23 | 10.53 |
| Linear Regression | 6.16 | 7.59 |
| **Random Forest** | **6.10** | **7.49** |

Among the tested approaches, Random Forest produced the lowest MAE and RMSE on the held-out test period.

### Improvement Over Baseline

Random Forest improved upon the baseline by:

- **25.88% reduction in MAE**
- **28.86% reduction in RMSE**

The Random Forest forecast therefore missed actual daily admissions by approximately **6.10 admissions per day on average** during the test period.

## Feature Importance

Random Forest feature importance showed that recent demand trends were especially useful for forecasting.

The strongest feature was:

**14-day rolling average of admissions**

followed by:

**7-day rolling average of admissions**

`day_of_year` also contributed seasonal information.

Feature importance describes how the trained model used the available predictors; it should not be interpreted as evidence of causal relationships.

## Project Structure

```text
hospital-resource-demand-forecasting/
│
├── data/
│   ├── hospital_operations.csv
│   └── README.md
│
├── models/
│   └── random_forest_admissions_model.pkl
│
├── notebooks/
│   └── 01_exploratory_data_analysis.ipynb
│
├── results/
│   ├── model_comparison.csv
│   └── random_forest_predictions.csv
│
├── .gitignore
└── README.md
```

## Technologies Used

- Python
- Pandas
- NumPy
- Matplotlib
- Scikit-learn
- Jupyter Notebook
- Git
- GitHub
- GitHub Codespaces

## Evaluation Metrics

### Mean Absolute Error (MAE)

MAE measures the average absolute difference between predicted and actual admissions.

Lower MAE indicates better forecasting performance.

### Root Mean Squared Error (RMSE)

RMSE measures prediction error while placing greater weight on larger forecasting mistakes.

Lower RMSE indicates better performance.

## Saved Artifacts

The trained Random Forest model is saved as:

```text
models/random_forest_admissions_model.pkl
```

Model comparison results are stored in:

```text
results/model_comparison.csv
```

Random Forest test predictions are stored in:

```text
results/random_forest_predictions.csv
```

## How to Run the Project

Clone the repository and install the required Python libraries:

```bash
pip install pandas numpy matplotlib scikit-learn joblib jupyter
```

Open:

```text
notebooks/01_exploratory_data_analysis.ipynb
```

and run the notebook cells sequentially.

## Key Takeaways

This project demonstrates an end-to-end forecasting workflow including data exploration, time-series feature engineering, leakage-aware feature construction, chronological model evaluation, baseline comparison, machine learning modeling, model interpretation, and artifact storage.

On the synthetic dataset used in this project, Random Forest achieved the lowest test error among the evaluated approaches with an MAE of **6.10** and RMSE of **7.49**.

## Limitations

This project uses synthetic data and should not be interpreted as a validated clinical or operational forecasting system.

The current model predicts aggregate daily admissions and does not incorporate external factors such as weather, holidays, disease outbreaks, demographic changes, or real-time hospital conditions.

Future work could include additional forecasting algorithms, hyperparameter tuning, time-series cross-validation, external predictors, department-level forecasting, and model deployment.