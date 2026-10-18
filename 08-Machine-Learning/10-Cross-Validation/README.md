# 10 - Cross-Validation

Learn how to estimate machine-learning model performance more reliably by evaluating models across multiple train/validation splits.

## Learning objectives

By completing this module, you will be able to:

- Explain why a single train/test split can give a misleading estimate.
- Describe the mechanics of K-Fold and Stratified K-Fold cross-validation.
- Use `cross_val_score` and `cross_validate` with appropriate scoring metrics.
- Choose splitters for grouped observations and time-ordered data.
- Build preprocessing and modeling workflows with `Pipeline` to avoid data leakage.
- Use cross-validation for model comparison and hyperparameter tuning.
- Interpret the mean and standard deviation of cross-validation scores.

## Prerequisites

- Python basics
- NumPy and pandas fundamentals
- Basic supervised machine learning
- Familiarity with train/test splitting and classification/regression metrics

## What is cross-validation?

Cross-validation repeatedly partitions available training data into training and validation folds. Each fold is used for validation once, while the remaining folds are used to fit the model. The resulting scores give a more robust view of how performance varies across splits.

In **5-fold cross-validation**, the data is divided into five folds. The model is trained five times; each run uses four folds for training and one fold for validation.

> Cross-validation is for model selection and performance estimation. Keep a final holdout test set untouched until the modeling decisions are complete.

## Main strategies

| Strategy | Best suited for | Key point |
|---|---|---|
| `KFold` | Regression and general IID data | Splits samples into K folds |
| `StratifiedKFold` | Classification | Approximately preserves class proportions in each fold |
| `RepeatedKFold` | Checking split sensitivity | Repeats K-Fold with different splits |
| `GroupKFold` | Repeated observations per person, device, site, or customer | A group never appears in both train and validation in the same fold |
| `StratifiedGroupKFold` | Classification with grouped data | Attempts to preserve class proportions while keeping groups separate |
| `TimeSeriesSplit` | Time-ordered data | Validates on later observations, not randomly shuffled future/past samples |
| `LeaveOneOut` | Very small datasets (with caution) | One sample is held out per split; can be computationally expensive |

Choose the splitter based on how data was collected—not simply on which gives the highest score.

## Essential workflow

1. Define features `X` and target `y`.
2. Reserve a final test set if the project needs an unbiased final evaluation.
3. Select a splitter that matches the data structure.
4. Put preprocessing and the estimator in a `Pipeline`.
5. Calculate one or more suitable metrics across folds.
6. Report the mean and standard deviation, plus individual fold scores.
7. Select the model/hyperparameters using training data only.
8. Evaluate the chosen workflow once on the untouched test set.

## Quick start

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler

X, y = load_breast_cancer(return_X_y=True)

model = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=2000, random_state=42)
)
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)

scores = cross_val_score(model, X, y, cv=cv, scoring="f1")
print("Fold scores:", scores)
print(f"Mean F1: {scores.mean():.3f} ± {scores.std():.3f}")
```

## Metrics

Choose metrics based on the problem and business objective.

- **Classification:** accuracy, precision, recall, F1, ROC-AUC, average precision.
- **Regression:** negative mean absolute error (`neg_mean_absolute_error`), negative mean squared error, or R².
- **Imbalanced classes:** do not rely on accuracy alone; consider recall, F1, balanced accuracy, ROC-AUC, or average precision.
- In scikit-learn, loss metrics are often represented with a negative sign as scorers so that higher values remain better. Convert them back to positive losses when reporting.

## Avoid data leakage

- Fit scaling, imputation, encoding, feature selection, and dimensionality reduction *inside* each fold by using a `Pipeline` or `ColumnTransformer`.
- Do not preprocess the complete dataset before cross-validation if the preprocessing learns from the data.
- Do not use the final test set to choose features, thresholds, hyperparameters, or models.
- For multiple records per person/customer/device, use group-aware splitting when the intended use is generalization to unseen groups.
- For forecasting, use time-aware splitting and ensure features would genuinely be available at prediction time.
- If feature engineering uses future information or target-derived information, a pipeline alone cannot fix that leakage; redesign the features.

## Common mistakes

1. Using `KFold(shuffle=True)` for imbalanced classification without checking class distributions.
2. Randomly splitting time-series observations.
3. Letting records from the same entity occur in both train and validation folds.
4. Scaling or imputing before cross-validation.
5. Selecting the best model by repeatedly checking the final test set.
6. Reporting only the best fold instead of the distribution of scores.
7. Treating cross-validation scores as independent measurements or assuming their standard deviation is a formal confidence interval.

## Exercises

Work through [`practice.md`](practice.md), then compare your work with [`solution.md`](solution.md). Runnable demonstrations are in [`examples.ipynb`](examples.ipynb).

## References

- [scikit-learn: Cross-validation: evaluating estimator performance](https://scikit-learn.org/stable/modules/cross_validation.html)
- [scikit-learn: `cross_val_score`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_val_score.html)
- [scikit-learn: `cross_validate`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.cross_validate.html)
- [scikit-learn: `StratifiedKFold`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.StratifiedKFold.html)
- [scikit-learn: `GroupKFold`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.GroupKFold.html)
- [scikit-learn: `TimeSeriesSplit`](https://scikit-learn.org/stable/modules/generated/sklearn.model_selection.TimeSeriesSplit.html)
