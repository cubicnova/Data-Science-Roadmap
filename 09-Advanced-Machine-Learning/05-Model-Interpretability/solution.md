# Solutions: Model Interpretability

These are reference approaches. Exact rankings and values can vary with scikit-learn versions, split choices, and random seeds.

## Exercise 1 — Core Concepts

1. Intrinsic interpretability comes from the model structure itself (for example, a small decision tree). Post-hoc methods explain a model after fitting it.
2. A global explanation describes overall model behavior; a local explanation focuses on one prediction.
3. Importance describes how a model uses available signals. A feature can be predictive because it is correlated with a cause, a proxy, or an artifact; this does not establish a causal effect.
4. Correlated features can substitute for one another, and importance can vary by metric, sample, and subgroup.

## Exercise 2 — Permutation Importance

```python
import pandas as pd
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.inspection import permutation_importance
from sklearn.model_selection import train_test_split
from sklearn.metrics import roc_auc_score

data = load_breast_cancer(as_frame=True)
X, y = data.data, data.target
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)

model = RandomForestClassifier(
    n_estimators=300, random_state=42
)
model.fit(X_train, y_train)
prob = model.predict_proba(X_test)[:, 1]
print("Test ROC-AUC:", roc_auc_score(y_test, prob))

result = permutation_importance(
    model, X_test, y_test,
    scoring="roc_auc", n_repeats=20, random_state=42, n_jobs=-1
)
importance = pd.DataFrame({
    "feature": X.columns,
    "mean_importance": result.importances_mean,
    "std_importance": result.importances_std,
}).sort_values("mean_importance", ascending=False)

print(importance.head(10))
```

The mean estimates the metric decrease after shuffling a feature; the standard deviation reflects variation across permutations. Negative values can occur due to sampling noise or because shuffling happened to help. Repeat with another score, such as `accuracy`, to see that importance is metric-dependent.

## Exercise 3 — Correlated Features

```python
import numpy as np
from sklearn.ensemble import RandomForestRegressor
from sklearn.inspection import permutation_importance
from sklearn.model_selection import train_test_split
from sklearn.metrics import r2_score

rng = np.random.default_rng(42)
signal = rng.normal(size=1500)
X = np.column_stack([
    signal + rng.normal(scale=0.08, size=1500),
    signal + rng.normal(scale=0.08, size=1500),
    rng.normal(size=1500),
])
y = 3 * signal + rng.normal(scale=0.5, size=1500)

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)
model = RandomForestRegressor(n_estimators=250, random_state=42)
model.fit(X_train, y_train)
print("Test R²:", r2_score(y_test, model.predict(X_test)))
print(permutation_importance(
    model, X_test, y_test, scoring="r2", n_repeats=20, random_state=42
).importances_mean)
```

Each correlated feature can retain much of the signal when the other is shuffled, so individually shuffling one may have only a modest impact. Examine correlation, permute correlated features as a group, or compare models trained with and without the group. Grouped approaches answer a different question than individual permutation importance.

## Exercise 4 — Partial Dependence

```python
import matplotlib.pyplot as plt
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.inspection import PartialDependenceDisplay
from sklearn.model_selection import train_test_split

data = load_breast_cancer(as_frame=True)
X, y = data.data, data.target
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)
model = RandomForestClassifier(n_estimators=250, random_state=42)
model.fit(X_train, y_train)

features = ["mean radius", "mean texture"]
PartialDependenceDisplay.from_estimator(
    model, X_train, features, kind="average"
)
plt.tight_layout()
plt.show()
```

Describe what the model's average predicted response does over the plotted feature range. Do not say changing a feature will cause the outcome to change. Partial dependence can average over unrealistic combinations when features are correlated; ICE curves or conditional approaches may provide additional context.

## Exercise 5 — Local Explanation

A simple model-agnostic local check can start with one prediction and compare it with similar rows. For SHAP, first install the optional package:

```bash
python -m pip install shap
```

Then try:

```python
import shap

# For a tree-based model:
explainer = shap.TreeExplainer(model)
shap_values = explainer.shap_values(X_test.iloc[[0]])
print("Predicted probability:", model.predict_proba(X_test.iloc[[0]])[0])
```

SHAP APIs and returned shapes differ across model types and package versions. For binary classifiers, explicitly check which class/output the explanation refers to before interpreting signs. The baseline is the explainer's expected output under its background-data assumptions. Contributions explain the model's output relative to that baseline, not a causal effect.

## Exercise 6 — Explanation Stability

```python
import pandas as pd
from sklearn.datasets import load_breast_cancer
from sklearn.ensemble import RandomForestClassifier
from sklearn.inspection import permutation_importance
from sklearn.model_selection import train_test_split

data = load_breast_cancer(as_frame=True)
X, y = data.data, data.target
X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, stratify=y, random_state=42
)

rankings = {}
for seed in [7, 21, 42]:
    model = RandomForestClassifier(n_estimators=250, random_state=seed)
    model.fit(X_train, y_train)
    result = permutation_importance(
        model, X_test, y_test, scoring="roc_auc",
        n_repeats=10, random_state=seed, n_jobs=-1
    )
    rankings[seed] = pd.Series(result.importances_mean, index=X.columns).sort_values(ascending=False)
    print(f"Seed {seed} top 5:", list(rankings[seed].head(5).index))

```

Compare the overlap of top-ranked features and the range of importance values. If rankings change substantially, report the instability rather than presenting a single ranking as definitive. Also consider whether model performance varies across seeds.

## Exercise 7 — Communicating Results

Example structure:

> The model achieved [metric] on a held-out sample. Permutation analysis suggests that [features] are most useful for the selected metric on this dataset. For one example, the model predicted [output], with the local explanation identifying [contributions] relative to its baseline. These results describe model behavior, not causality. Correlated features and sample variability may affect the rankings. Before deployment, we should test explanation stability and performance across relevant subgroups.

Replace bracketed text with actual measured results. Do not invent scores or causal conclusions.

## Exercise 8 — Mini Project

A strong report includes the target, split strategy, metric, held-out performance, permutation importance with variability, one partial-dependence or local explanation, checks for correlation and stability, and limitations. State the exact model output being explained (for example, probability of class 1) and the population represented by the evaluation sample.
