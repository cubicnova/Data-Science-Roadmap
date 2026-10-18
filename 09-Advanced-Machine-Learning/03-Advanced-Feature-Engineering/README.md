# Advanced Feature Engineering

Feature engineering transforms raw data into useful signals that help machine-learning models learn patterns. This module covers nonlinear transformations, interaction and polynomial features, date/time features, binning, categorical encoding, feature selection, and leakage-safe pipelines.

## Learning Objectives

- Explain why feature engineering matters.
- Create interaction, polynomial, ratio, date/time, and binned features.
- Handle missing values and categorical variables.
- Select useful features using filter and model-based methods.
- Build pipelines with `Pipeline` and `ColumnTransformer`.
- Detect and prevent data leakage.
- Compare a baseline model with an engineered-feature workflow.

## Contents

- [`examples.ipynb`](examples.ipynb) — guided, runnable examples.
- [`practice.md`](practice.md) — hands-on exercises.
- [`solution.md`](solution.md) — reference solutions.

## 1. What Is Feature Engineering?

Feature engineering creates, transforms, selects, or represents input variables so models can learn useful patterns. Examples include deriving purchase totals, extracting weekday from timestamps, applying `log1p` to skewed positive data, encoding categories, and creating interactions such as `income × credit_score`.

A feature must be valid and available at prediction time. A feature computed using information recorded after the outcome is leakage-prone.

## 2. Common Techniques

| Technique | Example | Considerations |
|---|---|---|
| Imputation | Median for numeric, most frequent for categories | Fit on training data only |
| Scaling | `StandardScaler`, `RobustScaler` | Useful for many linear and distance-based models |
| Categorical encoding | `OneHotEncoder` | Avoid implying false order |
| Nonlinear transforms | `log1p`, power transforms | Check value domain |
| Binning | Age bands | Can discard information |
| Interactions | `income * credit_score` | Validate to avoid overfitting |
| Polynomial features | `x²`, `x*y` | Can expand feature count quickly |
| Date/time | Month, weekday, elapsed time | Respect time zones and temporal order |
| Feature selection | Mutual information, `SelectFromModel` | Select inside cross-validation |
| Domain features | Ratios, counts, recency | Ensure valid at inference time |

## 3. Leakage-Safe Workflow

1. Define the target and the moment at which prediction is made.
2. Split data using a strategy appropriate to the task.
3. Fit imputers, scalers, encoders, and selectors on training data only.
4. Apply learned transformations to validation/test data.
5. Combine preprocessing and the estimator in a pipeline.
6. Select models using cross-validation on training data.
7. Evaluate the final choice once on held-out test data.

Data leakage occurs when information unavailable at prediction time influences feature creation, training, or evaluation. Examples include scaling the full dataset before splitting, using future values in a feature, or selecting features before cross-validation.

## 4. Feature Selection

- **Filter methods:** variance threshold, correlation screening, univariate tests, mutual information.
- **Wrapper methods:** recursive feature elimination.
- **Embedded methods:** L1-regularized models and tree-based importance.

Selection is not always beneficial. Compare selected and unselected workflows using cross-validation and assess stability.

## 5. Best Practices

- Start with a simple baseline.
- Keep feature generation reproducible and testable.
- Avoid target-derived features unless legitimately available at prediction time.
- Use time-based splits for temporal problems.
- Do not interpret correlation or feature importance as causation.
- Watch for cardinality explosion from one-hot and polynomial features.
- Document feature definitions, units, missingness, and provenance.

## Setup

```bash
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook examples.ipynb
```

## References

- [scikit-learn: Preprocessing data](https://scikit-learn.org/stable/modules/preprocessing.html)
- [scikit-learn: Pipelines and composite estimators](https://scikit-learn.org/stable/modules/compose.html)
- [scikit-learn: Feature selection](https://scikit-learn.org/stable/modules/feature_selection.html)
- [scikit-learn: Dataset transformations](https://scikit-learn.org/stable/data_transforms.html)
