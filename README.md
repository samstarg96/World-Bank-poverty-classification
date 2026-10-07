# World Bank Poverty Classification (case study)

This project predicts a binary `poor` indicator from seven anonymised features. It was developed as a modelling exercise framed around a World Bank use case. The anonymised column names prevent substantive interpretation of individual predictors; this is a demonstration of the modelling workflow.

## Approach

1. [`01_cleaning_and_EDA.ipynb`](02%20Scripts/01_cleaning_and_EDA.ipynb) inspects the supplied train and test files, removes apparent row identifiers, treats two implausible numeric values as missing, drops one of two nearly perfectly correlated features, and produces descriptive tables and charts.
2. [`02_select_model_via_cross_validation.ipynb`](02%20Scripts/02_select_model_via_cross_validation.ipynb) compares unregularised logistic regression, elastic net logistic regression, random forest, and XGBoost using stratified five-fold cross validation. Imputation, scaling, and encoding are fitted within each training fold. The notebook evaluates logistic regression and XGBoost on the held out test set, including ROC curves, calibration, Brier scores, and logistic regression confusion matrices at several thresholds.

The included anonymised split has 6,562 training observations and 1,641 test observations. About 45% of training observations have `poor=True`; average feature missingness is about 3%.

## Results

| Model | Mean cross validation ROC AUC | Test ROC AUC |
| --- | ---: | ---: |
| XGBoost | 0.844 | 0.841 |
| Random forest | 0.837 | — |
| Elastic net logistic regression | 0.834 | — |
| Unregularised logistic regression | 0.834 | 0.822 |

XGBoost scored highest. The notebook also examines logistic regression as a simpler, more interpretable alternative; its test ROC AUC was lower by about 0.019. Test Brier scores were 0.163 for XGBoost and 0.172 for logistic regression. These are results for this supplied split, not evidence of performance on other populations or periods.

### Test set charts

![ROC curves for logistic regression and XGBoost](05%20Charts/test_roc_logit_xgboost.png)

![Calibration curves for logistic regression and XGBoost](05%20Charts/calibration_logit_xgboost.png)

## Run locally

The project was run with Python 3.14.3. From the project root:

```powershell
python -m venv .venv
.venv\Scripts\Activate.ps1
python -m pip install -r requirements.txt
jupyter lab
```

On macOS or Linux, activate the environment with `source .venv/bin/activate` instead. The anonymised `Train.csv` and `Test.csv` files are included in `01 Input/`. Run the two notebooks in numerical order. The notebooks work when launched from the project root or from `02 Scripts/`. They create `03 Temp/`, `04 Tables/`, and `05 Charts/` if needed. The second notebook runs multiple cross validation searches and can take several minutes.

**Data availability:** The anonymised input CSVs are included. They were supplied for this modelling exercise; their original source is not documented in the project files. Saved notebook outputs are cleared, while the charts above are committed for easy review.

## Repository contents

- `01 Input/`: anonymised training and test CSVs.
- `02 Scripts/`: analysis notebooks. Local `.html` exports are excluded from Git.
- `05 Charts/`: charts from the latest local run.
- `requirements.txt`: direct Python dependencies used by the notebooks.
- `03 Temp/` and `04 Tables/`: generated files, excluded from Git and recreated when the notebooks run.
