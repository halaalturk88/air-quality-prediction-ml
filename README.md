# Air Quality Prediction Using Machine Learning

**Individual Academic Project · Regression · Python · Environmental Data**

## Overview

This university project estimates **carbon monoxide concentration (`CO(GT)`)** from environmental and gas-sensor measurements in the UCI Air Quality dataset. It compares linear regression, tree-based regressors, and a multilayer perceptron (MLP), and evaluates the effect of hyperparameter tuning.

## Dataset

- **Source:** [UCI Machine Learning Repository — Air Quality](https://archive.ics.uci.edu/dataset/360/air+quality)
- **Original records:** 9,357 hourly observations.
- **Prediction target:** `CO(GT)` (carbon monoxide concentration).
- **Predictors:** Other gas-sensor and environmental variables, plus engineered hour, month, and day-of-week features.
- **Data file:** `AirQualityUCI.xlsx` (download separately; not redistributed here).

## Workflow in the notebook

1. Load and explore the data and visualize missing values and feature correlations.
2. Replace the dataset's `-200` missing-value marker with `NaN`.
3. Drop `NMHC(GT)` (about 90% missing in the original notebook), use forward fill followed by backward fill for remaining gaps, and remove duplicate rows.
4. Build a timestamp from date and time, then derive hour, month, and day-of-week features.
5. Create an 80/20 **random** train/test split and standardize continuous predictors using statistics fitted on the training partition.
6. Fit Linear Regression and Random Forest baselines, followed by XGBoost and MLP regressors. Apply `RandomizedSearchCV` to tune XGBoost and MLP.
7. Compare results using coefficient of determination (**R²**) and mean squared error (**MSE**).

## Saved notebook results

These values are **historical outputs embedded in the submitted notebook**, not results independently reproduced for this GitHub package.

| Model | R² on the notebook test split | MSE |
| --- | ---: | ---: |
| Linear Regression | 0.8024 | Not recorded in printed baseline output |
| Random Forest | 0.9042 | Not recorded in printed baseline output |
| XGBoost (default) | 0.9126 | 0.1868 |
| XGBoost (random search) | **0.9202** | **0.1705** |
| MLP (random search) | 0.9063 | 0.2001 |

The tuned XGBoost model had the highest **reported** R² in this notebook. Its saved search used 20 parameter combinations and three-fold cross-validation.

## Run locally

Requires Python and Jupyter Notebook.

```bash
pip install -r requirements.txt
jupyter notebook air_quality_prediction.ipynb
```

Download `AirQualityUCI.xlsx` from the linked UCI page and save it as `data/AirQualityUCI.xlsx`. Run the notebook cells in order. The file path has been adapted from the original Google Colab path to a relative path for this repository.

> Some hyperparameter-search cells are computationally intensive. The packaged notebook retains historical output cells but **has not been rerun after the path adjustment**.

## Repository structure

```text
air-quality-prediction-ml/
├── air_quality_prediction.ipynb
├── requirements.txt
├── README.md
├── .gitignore
├── data/
│   └── README.md
└── docs/
    └── Air_Quality_Prediction_Presentation.pptx
```

## Limitations and future improvements

- **Time-series evaluation:** The original project used a random 80/20 split. A chronological holdout is more appropriate for measuring future-data performance.
- **Potential data leakage:** Forward/backward imputation and duplicate removal were performed before the split. A more rigorous experiment would split by time first, then fit preprocessing only on training data without using future observations.
- **Missing target values:** The notebook also fills missing values in the `CO(GT)` target. A stricter evaluation would exclude rows without reliable observed targets.
- **Reproducibility:** Dataset files and a fresh end-to-end execution log are not included. Saved results represent the earlier notebook run only.
- **Model claims:** This is an academic comparison, not a production air-quality forecasting system.

## Project ownership

**Individual university coursework project.** The notebook, analysis, and presentation document the author's work as submitted for the course.
