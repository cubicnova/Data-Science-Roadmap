# 05 — Regression

Regression is a supervised machine learning task used to predict a continuous numerical value, such as house price, delivery duration, energy demand, or sales revenue. This module introduces core regression algorithms, evaluation metrics, assumptions, diagnostics, and reliable modeling workflows using Python and scikit-learn.

## Learning objectives

By the end of this module, you should be able to:

- Explain regression and distinguish it from classification.
- Prepare features and a continuous target for regression.
- Train a baseline and fit linear regression.
- Interpret coefficients, intercepts, and residuals.
- Evaluate predictions using MAE, MSE, RMSE, and R².
- Recognize underfitting, overfitting, multicollinearity, and influential observations.
- Compare linear, regularized, and non-linear regression models.
- Use train/test splits and cross-validation appropriately.
- Build leakage-safe preprocessing and modeling pipelines.
- Communicate model performance, assumptions, and limitations.

## Topics covered

### 1. Regression fundamentals
- Continuous targets and supervised learning.
- Simple and multiple linear regression.
- Prediction vs. explanation.
- Baselines and train/test separation.

### 2. Linear regression
A simple linear regression model represents a relationship as:

\[
\hat{y} = b_0 + b_1x
\]

Here, \(b_0\) is the intercept and \(b_1\) is the coefficient for the feature \(x\). Multiple linear regression uses multiple features. Coefficients describe model associations conditional on the other included features; they do not automatically establish causation.

### 3. Regression metrics
- **MAE:** mean absolute error; average absolute error in target units.
- **MSE:** mean squared error; penalizes large errors more heavily because errors are squared.
- **RMSE:** square root of MSE; expressed in the target's units.
- **R²:** compares the model with a mean-prediction reference; it can be negative on held-out data.
- Use metrics that match the cost of errors and always compare with a sensible baseline.

### 4. Residual analysis
A residual is the observed target minus the predicted target. Examine residuals for systematic patterns, non-constant variance, outliers, and evidence that the model misses non-linear structure. A high R² alone does not guarantee that assumptions or deployment needs are satisfied.

### 5. Regularization and model complexity
- **Ridge regression:** L2 penalty that shrinks coefficients.
- **Lasso regression:** L1 penalty that can set some coefficients to zero.
- **Polynomial features:** allow linear models to represent some non-linear relationships, but can increase overfitting risk.
- Scaling is especially important for regularized linear models.

### 6. Assumptions and limitations
Classical linear regression often relies on assumptions about the functional form, error independence, error variance, and (for some inferential procedures) residual distribution. Prediction can still be useful when assumptions are imperfect, but diagnostics and validation matter. Multicollinearity can make individual coefficients unstable.

### 7. Data preparation and evaluation
- Separate the target from features.
- Split data before fitting learned transformations.
- Use pipelines for imputation, encoding, scaling, and estimation.
- Use cross-validation for model selection.
- Use time-aware splitting for forecasting or time-dependent observations.
- Avoid target leakage and evaluate once on held-out test data after model decisions are complete.

## Setup

Python 3.10+ is recommended.

```bash
python -m venv .venv
# Windows:
.venv\Scripts\activate
# macOS/Linux:
source .venv/bin/activate

python -m pip install --upgrade pip
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Open `examples.ipynb` and run the cells from top to bottom. The notebook generates synthetic regression data locally and does not require external downloads.

## Recommended workflow

1. Define the target, prediction time, and cost of errors.
2. Inspect the dataset, target distribution, missingness, and possible leakage.
3. Establish a baseline, such as predicting the training target mean.
4. Split data using a strategy that reflects how predictions will be used.
5. Build preprocessing and regression models inside pipelines.
6. Compare candidates using cross-validation and appropriate metrics.
7. Inspect residuals and prediction errors.
8. Evaluate the final selected model on held-out test data.
9. Document intended use, performance, limitations, and monitoring signals.

## Repository contents

- `README.md` — concepts, setup, and workflow.
- `examples.ipynb` — runnable examples with synthetic data.
- `practice.md` — progressive exercises and mini-project.
- `solution.md` — reference solutions and explanations.

## Troubleshooting

- **Negative R²:** the model performs worse than predicting the test target mean under the R² definition. Inspect the split, feature signal, target noise, and model fit.
- **Large RMSE compared with MAE:** a small number of large errors may be affecting squared-error metrics.
- **Unstable coefficients:** investigate feature scaling, correlated predictors, limited data, and model specification.
- **Train score much better than test score:** possible overfitting, leakage, distribution shift, or an unsuitable split.
- **Unexpectedly strong test performance:** investigate target-derived features, duplicate records, preprocessing before splitting, and whether the evaluation split matches deployment.
- **Missing values or unseen categories:** use fitted imputers and encoders inside a pipeline.

## Good-practice checklist

- [ ] Use a baseline and justify the chosen metric.
- [ ] Keep the test set out of model selection.
- [ ] Fit preprocessing on training folds only.
- [ ] Inspect residuals and large errors.
- [ ] Check the relevance and availability of each feature.
- [ ] Consider time or group structure in the split.
- [ ] Report limitations and avoid causal claims from coefficients alone.
- [ ] Record code, data version, random seed, metrics, and model settings.

## Further reading

- [scikit-learn: Linear models](https://scikit-learn.org/stable/modules/linear_model.html)
- [scikit-learn: Regression metrics](https://scikit-learn.org/stable/modules/model_evaluation.html#regression-metrics)
- [scikit-learn: Cross-validation](https://scikit-learn.org/stable/modules/cross_validation.html)
