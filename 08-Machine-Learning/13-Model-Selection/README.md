# 12 - Pipelines

Build repeatable, leakage-safe machine-learning workflows by combining preprocessing, feature engineering, and estimators into a single object.

## Learning objectives

By the end of this module, you should be able to:

- Explain why pipelines make machine-learning workflows more reproducible.
- Chain transformations and estimators with `Pipeline` and `make_pipeline`.
- Apply different preprocessing to numeric and categorical columns using `ColumnTransformer`.
- Handle missing values, scaling, and one-hot encoding within a pipeline.
- Tune preprocessing and model parameters together with `GridSearchCV`.
- Evaluate a pipeline with cross-validation without leaking validation data.
- Inspect feature names and persist a fitted pipeline for later inference.
- Recognize pipeline limitations and common mistakes.

## Why pipelines?

A machine-learning model often needs more than a final estimator. Real data may require missing-value imputation, categorical encoding, scaling, feature selection, and then classification or regression. A pipeline packages those operations in the correct order behind a consistent `fit`, `predict`, and (where available) `predict_proba` interface.

Pipelines are especially important during cross-validation: each preprocessing step is fitted only on the training portion of a fold, rather than learning statistics from validation data.

## Main building blocks

| Component | Purpose |
|---|---|
| `Pipeline` / `make_pipeline` | Chain transformations in sequence and finish with an estimator |
| `ColumnTransformer` | Apply different transformations to selected columns |
| `SimpleImputer` | Fill missing numeric or categorical values |
| `StandardScaler` | Standardize numeric features |
| `OneHotEncoder` | Encode nominal categories as indicator columns |
| `FunctionTransformer` | Wrap a simple function as a transformer |
| `GridSearchCV` | Tune parameters of pipeline steps and the final estimator |
| `joblib` | Save and load fitted Python estimators/pipelines |

## Basic example

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import accuracy_score

X, y = load_breast_cancer(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)

model = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=2000, random_state=42)
)
model.fit(X_train, y_train)
print("Accuracy:", accuracy_score(y_test, model.predict(X_test)))
```

## Pipeline design pattern

1. Split data into training and test sets before fitting transformations.
2. Define preprocessing for each feature type.
3. Combine preprocessing with the estimator in a pipeline.
4. Use the full pipeline for cross-validation and parameter tuning.
5. Fit the selected pipeline on training data only.
6. Evaluate once on the untouched test set.
7. Save the fitted pipeline and use the same object for future inference.

## Mixed-type data with `ColumnTransformer`

For a pandas DataFrame with numeric and categorical columns, a common pattern is:

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler

numeric_features = ["age", "income"]
categorical_features = ["city", "plan"]

numeric_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("onehot", OneHotEncoder(handle_unknown="ignore")),
])

preprocessor = ColumnTransformer([
    ("numeric", numeric_pipeline, numeric_features),
    ("categorical", categorical_pipeline, categorical_features),
])
```

Use the preprocessor as the first step in a modeling pipeline. `handle_unknown="ignore"` allows inference to proceed when a category appears that was not seen during fitting; it does not mean every unseen category is automatically handled optimally for the business problem.

## Hyperparameter tuning

Pipeline step names become part of parameter names, separated by `__`:

```python
param_grid = {
    "preprocessor__numeric__scaler__with_mean": [True, False],
    "model__C": [0.1, 1.0, 10.0],
}
```

Prefer searching over a pipeline rather than preprocessing data once before `GridSearchCV`. Every candidate and fold must learn its own preprocessing from the relevant training partition.

## Common mistakes

- Fitting an imputer, scaler, encoder, or feature selector on all data before cross-validation.
- Splitting train and test after preprocessing.
- Using a different preprocessing path during production inference than during training.
- Forgetting `handle_unknown="ignore"` when appropriate for one-hot encoded production categories.
- Treating encoded categories as ordinal when they are nominal.
- Using `fit_transform` on validation/test data instead of calling `transform` through the already fitted pipeline.
- Tuning on the final test set.
- Saving a model but not the preprocessing steps needed to interpret raw inputs.
- Loading pickle/joblib artifacts from untrusted sources; deserialization can execute code.

## Exercises

Try [`practice.md`](practice.md) and then review [`solution.md`](solution.md). The runnable notebook [`examples.ipynb`](examples.ipynb) demonstrates the main patterns.

## References

- [scikit-learn: Pipelines and composite estimators](https://scikit-learn.org/stable/modules/compose.html)
- [scikit-learn: Pipeline API](https://scikit-learn.org/stable/modules/generated/sklearn.pipeline.Pipeline.html)
- [scikit-learn: ColumnTransformer API](https://scikit-learn.org/stable/modules/generated/sklearn.compose.ColumnTransformer.html)
- [scikit-learn: Common pitfalls and data leakage](https://scikit-learn.org/stable/common_pitfalls.html)
- [scikit-learn: Preprocessing data](https://scikit-learn.org/stable/modules/preprocessing.html)
