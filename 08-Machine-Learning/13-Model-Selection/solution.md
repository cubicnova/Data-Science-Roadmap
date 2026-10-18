# Solutions: Pipelines

These solutions are reference implementations. Exact metrics may vary by scikit-learn version and split. Focus on the workflow and leakage prevention.

## 1. Explain the value of a pipeline

A pipeline:
1. Keeps preprocessing and prediction in one reusable estimator.
2. Prevents many forms of leakage by fitting transformations separately inside each training fold.
3. Makes hyperparameter tuning and production inference more consistent.
4. Reduces the chance that training and deployment apply different transformations.

## 2. Basic Iris pipeline

```python
from sklearn.datasets import load_iris
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import accuracy_score

X, y = load_iris(return_X_y=True)
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
model = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000, random_state=42)
)
model.fit(X_train, y_train)
predictions = model.predict(X_test)
print("Accuracy:", accuracy_score(y_test, predictions))
```

## 3. Inspect steps and order

```python
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.pipeline import Pipeline

pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("model", LogisticRegression(max_iter=1000)),
])
print(pipe.named_steps)
```

Imputation comes first because most scalers and estimators do not accept missing numeric values. Scaling follows imputation, then the estimator is trained on transformed features.

## 4. Prevent leakage

The scaler learns means and standard deviations from all samples before the CV split, so validation-fold information influences training transformations. Instead:

```python
from sklearn.model_selection import cross_val_score
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression

pipe = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=1000)
)
scores = cross_val_score(pipe, X, y, cv=5, scoring="accuracy")
print(scores, scores.mean(), scores.std())
```

## 5. Mixed-type preprocessor

```python
import numpy as np
import pandas as pd
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.metrics import accuracy_score

rng = np.random.default_rng(42)
n = 200
df = pd.DataFrame({
    "age": rng.integers(18, 75, n).astype(float),
    "income": rng.normal(60000, 18000, n),
    "city": rng.choice(["Indore", "Pune", "Delhi"], n),
    "plan": rng.choice(["basic", "plus", "premium"], n),
})
df.loc[rng.choice(n, 15, replace=False), "age"] = np.nan
df.loc[rng.choice(n, 12, replace=False), "city"] = None
target = ((df["income"].fillna(60000) > 62000) &
          (df["plan"].eq("premium") | df["plan"].eq("plus"))).astype(int)

numeric_features = ["age", "income"]
categorical_features = ["city", "plan"]

numeric_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
])
categorical_pipe = Pipeline([
    ("imputer", SimpleImputer(strategy="most_frequent")),
    ("onehot", OneHotEncoder(handle_unknown="ignore")),
])
preprocessor = ColumnTransformer([
    ("numeric", numeric_pipe, numeric_features),
    ("categorical", categorical_pipe, categorical_features),
])
pipe = Pipeline([
    ("preprocessor", preprocessor),
    ("model", LogisticRegression(max_iter=1000)),
])

X_train, X_test, y_train, y_test = train_test_split(
    df, target, test_size=0.2, stratify=target, random_state=42
)
pipe.fit(X_train, y_train)
print("Test accuracy:", accuracy_score(y_test, pipe.predict(X_test)))
```

## 6. Unknown categories

```python
from sklearn.preprocessing import OneHotEncoder
encoder = OneHotEncoder(handle_unknown="ignore")
```

When a category not observed during fitting appears at transform time, its encoded values are all zeros for that feature's one-hot block instead of raising an unknown-category error. This protects the transformation from failing, but does not guarantee that the model has learned a meaningful response to that unseen category.

## 7. Cross-validation

Using the `pipe` from exercise 5:

```python
from sklearn.model_selection import StratifiedKFold, cross_val_score

cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(pipe, df, target, cv=cv, scoring="f1")
print("Fold F1:", scores.round(3))
print(f"Mean F1: {scores.mean():.3f} ± {scores.std():.3f}")
```

The entire preprocessing pipeline is refitted within each fold.

## 8. Tune model parameters

```python
from sklearn.model_selection import GridSearchCV

search = GridSearchCV(
    pipe,
    param_grid={"model__C": [0.01, 0.1, 1, 10]},
    scoring="f1",
    cv=cv,
    n_jobs=-1,
)
search.fit(df, target)
print(search.best_params_)
print(search.best_score_)
```

A step named `model` uses the parameter prefix `model__`. The `__` separates the step name from its parameter.

## 9. Tune preprocessing and model together

```python
from sklearn.preprocessing import MinMaxScaler

grid = {
    "preprocessor__numeric__scaler": [StandardScaler(), MinMaxScaler()],
    "model__C": [0.1, 1, 10],
}
search = GridSearchCV(pipe, grid, scoring="f1", cv=cv, n_jobs=-1)
search.fit(df, target)
print(search.best_params_)
print(search.best_score_)
```

For a larger search, consider randomized search or a more focused parameter space. Ensure each candidate uses the same evaluation design.

## 10. Feature names

```python
preprocessor.fit(df)
names = preprocessor.get_feature_names_out()
print(names)
```

Feature names make it easier to trace transformed columns, debug unexpected encodings, and connect model coefficients or importances back to source features. Exact names depend on the scikit-learn version and transformer settings.

## 11. Save and reload a pipeline

```python
import joblib
import numpy as np

pipe.fit(X_train, y_train)
before = pipe.predict(X_test)

joblib.dump(pipe, "customer_pipeline.joblib")
loaded = joblib.load("customer_pipeline.joblib")
after = loaded.predict(X_test)

assert np.array_equal(before, after)
print("Predictions match:", np.array_equal(before, after))
```

Only load joblib/pickle artifacts from trusted sources because deserialization can execute arbitrary code. Also keep track of package versions and the expected input schema.

## 12. Regression pipeline

```python
from sklearn.datasets import make_regression
from sklearn.impute import SimpleImputer
from sklearn.linear_model import Ridge
from sklearn.model_selection import KFold, cross_val_score
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
import numpy as np

X_reg, y_reg = make_regression(
    n_samples=300, n_features=8, noise=15, random_state=42
)
reg = make_pipeline(
    SimpleImputer(strategy="median"),
    StandardScaler(),
    Ridge(alpha=1.0)
)
cv_reg = KFold(n_splits=5, shuffle=True, random_state=42)
neg_mae = cross_val_score(
    reg, X_reg, y_reg, cv=cv_reg,
    scoring="neg_mean_absolute_error"
)
mae = -neg_mae
print("MAE per fold:", mae.round(2))
print(f"Mean MAE: {mae.mean():.2f} ± {mae.std():.2f}")
```

Scikit-learn uses negative loss scores so that a larger score always indicates better performance. Negate the scores to report positive MAE.

## 13. Challenge project

A good submission includes:
- A reproducible synthetic dataset and a fixed random seed.
- A train/test split before model fitting.
- A `ColumnTransformer` for numeric and categorical fields.
- Imputation, scaling, and encoding inside the pipeline.
- Stratified CV and a justified metric.
- `GridSearchCV` with parameter names such as `preprocessor__numeric__imputer__strategy` and `model__C`.
- A single final test evaluation after model selection.
- A note that production data validation, monitoring, and schema/version management are still required.
