# Solutions: Advanced Feature Engineering

These are reference implementations. Feature engineering is problem-dependent; other valid approaches may work as well or better.

## Exercise 1 — Feature Design

| Scenario | Candidate features | Leakage risk |
|---|---|---|
| E-commerce | Basket value, purchase recency, items per order | Return/refund recorded after prediction time |
| Churn | Days since last login, recent support tickets | Cancellation reason recorded after churn |
| House prices | Area per room, property age at sale | Valuation or sale price recorded after target sale |
| Traffic forecasting | Hour, weekday, lagged traffic | Traffic observed after forecast timestamp |

Every feature should have a clear definition and availability timestamp.

## Exercise 2 — Missing Values and Encoding

```python
import pandas as pd
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler

df = pd.DataFrame({
    "age": [22, 35, None, 48, 29, None],
    "income": [32000, 52000, 41000, None, 39000, 61000],
    "city": ["Indore", "Pune", "Indore", None, "Bhopal", "Pune"],
})
numeric = ["age", "income"]
categorical = ["city"]

preprocessor = ColumnTransformer([
    ("num", Pipeline([
        ("imputer", SimpleImputer(strategy="median")),
        ("scaler", StandardScaler()),
    ]), numeric),
    ("cat", Pipeline([
        ("imputer", SimpleImputer(strategy="most_frequent")),
        ("onehot", OneHotEncoder(handle_unknown="ignore")),
    ]), categorical),
])
```

For a supervised task, define the target independently. Fit the preprocessor on `X_train` only so learned medians, scales, and category vocabularies do not use held-out data.

## Exercise 3 — Nonlinear and Interaction Features

```python
import pandas as pd
from sklearn.datasets import make_classification
from sklearn.model_selection import train_test_split
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.linear_model import LogisticRegression
from sklearn.metrics import roc_auc_score

X, y = make_classification(
    n_samples=1200, n_features=6, n_informative=4,
    n_redundant=0, random_state=42
)
X = pd.DataFrame(X, columns=[f"x{i}" for i in range(X.shape[1])])
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)

baseline = make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000))
baseline.fit(X_train, y_train)
print("Baseline ROC-AUC:",
      roc_auc_score(y_test, baseline.predict_proba(X_test)[:, 1]))

def add_features(frame):
    out = frame.copy()
    out["x0_squared"] = out["x0"] ** 2
    out["x0_x_x1"] = out["x0"] * out["x1"]
    return out

engineered = make_pipeline(StandardScaler(), LogisticRegression(max_iter=2000))
engineered.fit(add_features(X_train), y_train)
print("Engineered ROC-AUC:", roc_auc_score(
    y_test, engineered.predict_proba(add_features(X_test))[:, 1]
))
```

These particular features are illustrative and are not guaranteed to improve performance. Use cross-validation for selection and reserve the test set for final evaluation.

## Exercise 4 — Date and Time Features

```python
import pandas as pd

df = pd.DataFrame({
    "timestamp": pd.to_datetime([
        "2026-01-05 09:15:00Z",
        "2026-01-10 18:30:00Z",
        "2026-02-12 13:00:00Z",
    ], utc=True)
})
df["month"] = df["timestamp"].dt.month
df["day_of_week"] = df["timestamp"].dt.dayofweek
df["hour"] = df["timestamp"].dt.hour
df["is_weekend"] = (df["day_of_week"] >= 5).astype(int)
print(df)
```

Convert timestamps to the relevant business time zone before extracting local calendar features. For forecasting, compute lag and rolling features in chronological order and shift values so future observations cannot enter features.

## Exercise 5 — Binning and Log Transformation

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt

rng = np.random.default_rng(42)
amount = rng.lognormal(mean=3.0, sigma=1.0, size=1000)
log_amount = np.log1p(amount)
bins = pd.cut(amount, bins=5, labels=False)

plt.hist(amount, bins=30)
plt.title("Original right-skewed data")
plt.show()

plt.hist(log_amount, bins=30)
plt.title("After log1p")
plt.show()

print(pd.Series(bins).value_counts().sort_index())
```

`log1p(x)` requires `x > -1`. A log transform can reduce right skew but changes scale and interpretation. Binning captures coarse thresholds but loses within-bin detail and may be sensitive to cut points.

## Exercise 6 — Leakage-Safe Pipeline

```python
from sklearn.datasets import load_breast_cancer
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import train_test_split, cross_val_score, StratifiedKFold
from sklearn.pipeline import make_pipeline
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import roc_auc_score

data = load_breast_cancer()
X_train, X_test, y_train, y_test = train_test_split(
    data.data, data.target, test_size=0.2,
    stratify=data.target, random_state=42
)
pipe = make_pipeline(
    StandardScaler(),
    LogisticRegression(max_iter=3000, random_state=42)
)
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
scores = cross_val_score(pipe, X_train, y_train, cv=cv, scoring="roc_auc")
print("CV ROC-AUC:", scores.mean(), "+/-", scores.std())
pipe.fit(X_train, y_train)
print("Test ROC-AUC:", roc_auc_score(
    y_test, pipe.predict_proba(X_test)[:, 1]
))
```

The scaler is refitted inside each fold using only that fold's training portion.

## Exercise 7 — Feature Selection

```python
from functools import partial
from sklearn.datasets import load_breast_cancer
from sklearn.feature_selection import SelectKBest, mutual_info_classif
from sklearn.linear_model import LogisticRegression
from sklearn.model_selection import StratifiedKFold, cross_val_score
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import StandardScaler

data = load_breast_cancer()
X, y = data.data, data.target
cv = StratifiedKFold(n_splits=5, shuffle=True, random_state=42)
mi = partial(mutual_info_classif, random_state=42)

selected = Pipeline([
    ("scale", StandardScaler()),
    ("select", SelectKBest(score_func=mi, k=10)),
    ("model", LogisticRegression(max_iter=3000, random_state=42)),
])
all_features = Pipeline([
    ("scale", StandardScaler()),
    ("model", LogisticRegression(max_iter=3000, random_state=42)),
])

for name, estimator in [("All features", all_features), ("Select 10", selected)]:
    scores = cross_val_score(estimator, X, y, cv=cv, scoring="roc_auc")
    print(name, scores.mean(), scores.std())

selected.fit(X, y)
mask = selected.named_steps["select"].get_support()
print("Selected:", list(data.feature_names[mask]))
```

Feature selection inside the pipeline is repeated independently in each fold. Selecting features once on all data before cross-validation leaks validation-fold information.

## Exercise 8 — Mini Project

A strong submission includes a defined prediction time, a simple baseline, documented feature hypotheses, a pipeline, cross-validation mean and variability, one final test evaluation, and a discussion of leakage, fairness, missingness, drift, and limitations. More features do not automatically make a better model.
