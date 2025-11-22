# COMP9417 Air Quality Forecasting

This repo contains the workflow for exploring, preprocessing, feature engineering, anomaly detection, and modeling on the UCI Air Quality dataset.

## Structure
- `1. EDA.ipynb` — exploratory data analysis.
- `2. Data Preprocessing.ipynb` — clean raw data, fix sentinel values, build `preprocessed_air_quality.csv`.
- `3. Anomaly and Event Detection.ipynb` — residual + Isolation Forest detectors, saves anomaly CSVs/plots.
- `4. Feature Engineering.ipynb` — lags/rolling stats, saves `artifacts/feature_engineered.csv`.
- `5. Temporal Data Splitting.ipynb` — chronological train/val/test splits, saves split CSVs to `artifacts/`.
- `6. Regression Model Development.ipynb` — horizon regression with/without IF anomaly flags; metrics to `artifacts/regression_metrics.csv`; plots to `figures/`.
- `7. Classification Model Development.ipynb` — CO class classification (current + horizons) with/without IF flags; metrics to `artifacts/classification_*`; plots to `figures/`.

Outputs:
- `artifacts/` — engineered data, splits, metrics, anomaly CSVs.
- `figures/` — saved plots from anomaly, regression, and classification notebooks.
- `preprocessed_air_quality.csv` — cleaned base dataset.

## Environment
- Python 3.10+
- Install deps once:
  ```bash
  pip install pandas numpy matplotlib seaborn scikit-learn xgboost nbclient
  ```

## Repro steps (order matters)
1. **EDA**: run `1. EDA.ipynb` (optional, no outputs needed downstream).
2. **Preprocess**: run `2. Data Preprocessing.ipynb` → produces `preprocessed_air_quality.csv`.
3. **Anomalies**: run `3. Anomaly and Event Detection.ipynb` → produces `artifacts/residual_anomalies.csv`, `artifacts/isolation_forest_anomalies.csv`, and plots in `figures/`.
4. **Feature Engineering**: run `4. Feature Engineering.ipynb` → produces `artifacts/feature_engineered.csv`.
5. **Temporal Splits**: run `5. Temporal Data Splitting.ipynb` → produces train/val/test feature/target CSVs in `artifacts/`.
6. **Regression**: run `6. Regression Model Development.ipynb` → evaluates persistence + Linear Regression + Random Forest + Gradient Boosting across horizons with/without IF flags; metrics to `artifacts/regression_metrics.csv`; plots in `figures/`.
7. **Classification**: run `7. Classification Model Development.ipynb` → evaluates persistence + RF/GB/SVM for current and horizon classes with/without IF flags; metrics to `artifacts/classification_metrics.csv`, `classification_horizon_metrics.csv`, `classification_pr_h1.csv`; plots in `figures/`.

## Notes
- Chronological split: train/val from 2004 (val = last 15% of train), test = 2005.
- Persistence baselines: previous-hour class for classification; previous value for regression.
- Anomaly flags: only Isolation Forest flags are used in “with anomalies” variants; “no anomalies” excludes them entirely.
- Plots are displayed in notebooks and saved under `figures/`.

## How to rerun notebooks programmatically
```bash
python -m nbclient --execute "X. Notebook Name.ipynb"
```
(or open in Jupyter/VS Code and Run All).

## Deliverables
- Metrics CSVs in `artifacts/` for report tables.
- Plots in `figures/` for inclusion in the report.
- Notebooks with inline comments and markdown explaining each step.
