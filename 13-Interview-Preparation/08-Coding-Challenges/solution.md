# Data Science Coding Challenges — Reference Solutions

These implementations prioritize clarity and edge-case behavior. Alternative solutions are valid if they meet the stated requirements. Complexity notes assume average-case dictionary/set operations are O(1).

## Level 1 — Python fundamentals

### 1. Frequency counts

```python
from collections import Counter

def frequencies(items):
    return dict(Counter(items))
```

Time: O(n); space: O(u), where `u` is the number of unique items.

### 2. First non-repeating character

```python
from collections import Counter

def first_unique_char(text):
    counts = Counter(text)
    for char in text:
        if counts[char] == 1:
            return char
    return None
```

Time: O(n); space: O(u).

### 3. Remove duplicates preserving order

```python
def unique_in_order(items):
    seen = set()
    result = []
    for item in items:
        if item not in seen:
            seen.add(item)
            result.append(item)
    return result
```

Assumes hashable items. Time: O(n) average; space: O(u).

### 4. Anagram check

```python
from collections import Counter

def are_anagrams(a, b):
    normalize = lambda s: "".join(s.lower().split())
    return Counter(normalize(a)) == Counter(normalize(b))
```

This ignores case and whitespace but not punctuation.

### 5. Top k frequent values

```python
from collections import Counter

def top_k_frequent(items, k):
    counts = Counter(items)
    # Deterministic tie-break: first appearance in the input.
    first_seen = {}
    for i, value in enumerate(items):
        first_seen.setdefault(value, i)
    return sorted(counts, key=lambda x: (-counts[x], first_seen[x]))[:max(k, 0)]
```

Sorting costs O(u log u); space O(u).

### 6. Flatten one level

```python
def flatten_one_level(nested):
    return [item for sublist in nested for item in sublist]
```

Time and output space are O(n), where n is the number of flattened elements.

### 7. Missing integer in 0..n

```python
def missing_number(values):
    n = len(values)
    return n * (n + 1) // 2 - sum(values)
```

Assumes distinct values from `0..n` with exactly one missing. Time O(n), extra space O(1).

### 8. Merge sorted lists

```python
def merge_sorted(a, b):
    i = j = 0
    out = []
    while i < len(a) and j < len(b):
        if a[i] <= b[j]:
            out.append(a[i]); i += 1
        else:
            out.append(b[j]); j += 1
    out.extend(a[i:])
    out.extend(b[j:])
    return out
```

Time O(n + m); output space O(n + m).

## Level 2 — Arrays and numerical operations

### 9. Mean and population variance

```python
def mean_and_population_variance(values):
    if not values:
        return None, None
    mean = sum(values) / len(values)
    variance = sum((x - mean) ** 2 for x in values) / len(values)
    return mean, variance
```

Population variance divides by `n`; sample variance usually divides by `n - 1`.

### 10. Min-max scaling

```python
import numpy as np

def minmax_scale(values):
    arr = np.asarray(values, dtype=float)
    if arr.size == 0:
        return arr.copy()
    span = arr.max() - arr.min()
    if span == 0:
        return np.zeros_like(arr)
    return (arr - arr.min()) / span
```

The constant-array convention is to return zeros; document another choice if required.

### 11. Two-sum indices

```python
def two_sum(values, target):
    seen = {}
    for i, value in enumerate(values):
        complement = target - value
        if complement in seen:
            return seen[complement], i
        seen[value] = i
    return None
```

Time O(n) average; space O(n).

### 12. Clip negatives with NumPy

```python
import numpy as np

def clip_negatives(values):
    return np.maximum(np.asarray(values), 0)
```

This returns a new array-like result.

### 13. Row and column means

```python
arr = np.asarray(matrix, dtype=float)
row_means = arr.mean(axis=1)
column_means = arr.mean(axis=0)
```

For arrays with missing values represented by NaN, consider `np.nanmean`, while remembering that all-NaN slices still need handling.

### 14. Broadcasting

```python
a = np.ones((3, 1))
b = np.arange(4).reshape(1, 4)
result = a + b
assert result.shape == (3, 4)
```

Dimensions are aligned from the right; equal sizes or a size of one are compatible.

### 15. Cosine similarity

```python
import numpy as np

def cosine_similarity(a, b):
    a = np.asarray(a, dtype=float)
    b = np.asarray(b, dtype=float)
    denom = np.linalg.norm(a) * np.linalg.norm(b)
    if denom == 0:
        return None
    return float(np.dot(a, b) / denom)
```

Time O(d) for d-dimensional vectors; space depends on array conversion.

### 16. IQR outlier mask

```python
import numpy as np

def iqr_outlier_mask(values):
    arr = np.asarray(values, dtype=float)
    q1, q3 = np.nanpercentile(arr, [25, 75])
    iqr = q3 - q1
    return (arr < q1 - 1.5 * iqr) | (arr > q3 + 1.5 * iqr)
```

For an empty/all-missing array, add an explicit policy; outliers should be investigated rather than automatically deleted.

## Level 3 — Pandas and data wrangling

### 17. Fill numeric missing values by column median

```python
numeric = df.select_dtypes(include="number")
df[numeric.columns] = numeric.fillna(numeric.median())
```

All-missing columns have a missing median and remain missing unless a documented fallback is provided.

### 18. Top three rows per group

```python
top_three = (
    df.sort_values(["group", "score", "record_id"],
                   ascending=[True, False, True])
      .groupby("group", group_keys=False)
      .head(3)
)
```

This assumes `record_id` is a deterministic tie-breaker.

### 19. Group mean on each original row

```python
df["group_mean"] = df.groupby("group")["value"].transform("mean")
```

`transform` preserves row alignment.

### 20. Merge and detect unmatched keys

```python
result = left.merge(right, on="key", how="left", indicator=True)
unmatched = result.loc[result["_merge"] == "left_only"]
```

Validate expected cardinality with `validate="many_to_one"` or another appropriate option.

### 21. Wide to long

```python
long = wide.melt(
    id_vars=["customer_id"],
    var_name="month",
    value_name="revenue"
)
```

### 22. Parse date and rolling mean

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df = df.sort_values("date")
df["month"] = df["date"].dt.month
df["rolling_mean"] = df["value"].rolling(window=7, min_periods=1).mean()
```

This is a seven-observation rolling mean, not necessarily seven calendar days.

### 23. Keep latest customer record

```python
df["timestamp"] = pd.to_datetime(df["timestamp"], errors="coerce")
latest = (
    df.sort_values(["timestamp", "record_id"])
      .drop_duplicates("customer_id", keep="last")
)
```

Define a deterministic tie-breaker and decide how invalid timestamps should be treated.

### 24. Data-quality report

```python
def data_quality_report(df):
    return pd.DataFrame({
        "non_missing": df.notna().sum(),
        "missing": df.isna().sum(),
        "missing_pct": df.isna().mean().mul(100),
        "distinct_non_missing": df.nunique(dropna=True)
    })
```

An empty DataFrame returns an empty report with the corresponding columns.

## Level 4 — Statistics and metrics

### 25. Binary classification metrics

```python
def binary_metrics(y_true, y_pred):
    if len(y_true) != len(y_pred):
        raise ValueError("Inputs must have equal lengths")
    tp = sum(t == 1 and p == 1 for t, p in zip(y_true, y_pred))
    tn = sum(t == 0 and p == 0 for t, p in zip(y_true, y_pred))
    fp = sum(t == 0 and p == 1 for t, p in zip(y_true, y_pred))
    fn = sum(t == 1 and p == 0 for t, p in zip(y_true, y_pred))
    precision = tp / (tp + fp) if tp + fp else 0.0
    recall = tp / (tp + fn) if tp + fn else 0.0
    f1 = 2 * precision * recall / (precision + recall) if precision + recall else 0.0
    accuracy = (tp + tn) / len(y_true) if len(y_true) else 0.0
    return {"accuracy": accuracy, "precision": precision, "recall": recall, "f1": f1}
```

Time O(n), extra space O(1).

### 26. Confusion matrix

```python
def binary_confusion_counts(y_true, y_pred):
    counts = {"tn": 0, "fp": 0, "fn": 0, "tp": 0}
    for truth, pred in zip(y_true, y_pred):
        if truth == 0 and pred == 0: counts["tn"] += 1
        elif truth == 0 and pred == 1: counts["fp"] += 1
        elif truth == 1 and pred == 0: counts["fn"] += 1
        elif truth == 1 and pred == 1: counts["tp"] += 1
        else: raise ValueError("Labels must be binary 0/1")
    return counts
```

### 27. MAE and RMSE

```python
import math

def regression_metrics(y_true, y_pred):
    if len(y_true) != len(y_pred) or not y_true:
        raise ValueError("Inputs must have equal, non-zero lengths")
    errors = [float(a) - float(p) for a, p in zip(y_true, y_pred)]
    mae = sum(abs(e) for e in errors) / len(errors)
    rmse = math.sqrt(sum(e * e for e in errors) / len(errors))
    return {"mae": mae, "rmse": rmse}
```

RMSE penalizes large errors more strongly than MAE.

### 28. Train/test and preprocessing

Split the rows first. Fit imputers, scalers, feature selection, and other learned transformations on the training data only; apply the fitted transformations to validation/test data. A scikit-learn `Pipeline` helps enforce this. Random splits suit independent observations; grouped or time-dependent data often require specialized splitting.

### 29. Bootstrap confidence interval for a mean

```python
import numpy as np

def bootstrap_mean_ci(values, n_boot=2000, confidence=0.95, seed=42):
    arr = np.asarray(values, dtype=float)
    arr = arr[~np.isnan(arr)]
    if arr.size == 0:
        raise ValueError("Need at least one non-missing value")
    rng = np.random.default_rng(seed)
    means = np.empty(n_boot)
    for i in range(n_boot):
        sample = rng.choice(arr, size=arr.size, replace=True)
        means[i] = sample.mean()
    alpha = 1 - confidence
    return tuple(np.quantile(means, [alpha / 2, 1 - alpha / 2]))
```

This percentile interval is an approximation; use a sampling scheme that matches the data's dependence structure.

### 30–32. Threshold, imbalance, leakage

- Lowering a binary threshold generally increases predicted positives, often raising recall and lowering precision; the exact behavior depends on the data.
- With 99% negatives, predicting every row negative yields 99% accuracy but zero recall for the positive class. Include PR-AUC, precision/recall, and cost-aware metrics.
- Leakage examples include a post-outcome status, a feature computed using future data, target encoding performed before splitting, and duplicates spanning train/test.

## Level 5 — ML workflow and debugging

### 33. Mixed-type preprocessing pipeline

```python
from sklearn.compose import ColumnTransformer
from sklearn.impute import SimpleImputer
from sklearn.pipeline import Pipeline
from sklearn.preprocessing import OneHotEncoder, StandardScaler
from sklearn.linear_model import LogisticRegression

numeric_features = ["age", "income"]
categorical_features = ["region", "plan"]

preprocess = ColumnTransformer([
    ("num", Pipeline([
        ("imputer", SimpleImputer(strategy="median")),
        ("scale", StandardScaler())
    ]), numeric_features),
    ("cat", Pipeline([
        ("imputer", SimpleImputer(strategy="most_frequent")),
        ("onehot", OneHotEncoder(handle_unknown="ignore"))
    ]), categorical_features)
])

model = Pipeline([
    ("preprocess", preprocess),
    ("classifier", LogisticRegression(max_iter=1000))
])
# model.fit(X_train, y_train)
```

### 34. Baseline versus logistic regression

Use the same train/validation folds and the same metric for both models. For imbalanced labels, consider average precision, PR curves, and threshold-level precision/recall rather than accuracy alone. Do not select the model based on final test performance repeatedly.

### 35. Choosing cross-validation

- Independent IID observations: shuffled K-fold or stratified K-fold as appropriate.
- Multiple rows per person/store/device: group-based splitting to keep entities separated.
- Time series: expanding-window or rolling-origin validation that trains on the past and validates on the future.

### 36. High train score, low test score

Investigate overfitting, leakage in the split, distribution shift, small sample size, train/test preprocessing differences, and hyperparameter tuning on the test set. Start with learning curves and a simpler baseline.

### 37. Unit tests for normalization

```python
import numpy as np

def test_minmax_scale():
    assert np.allclose(minmax_scale([2, 4, 6]), [0.0, 0.5, 1.0])
    assert np.allclose(minmax_scale([5, 5]), [0.0, 0.0])
    assert minmax_scale([]).size == 0
```

### 38. Merge doubles row count

Check duplicates in merge keys on both sides, inspect key frequencies, verify join cardinality, use `validate=`, and confirm that the intended output grain matches the joined tables.

### 39. Sudden metric change after a pipeline release

1. Check data freshness and job completion.
2. Compare input row counts, schemas, and null rates.
3. Verify metric definitions, time zones, filters, and denominators.
4. Check logging/instrumentation and duplicate handling.
5. Segment the metric by platform, region, source, and funnel step; compare with raw events.

### 40. Refactor a notebook cell

Separate the workflow into functions such as `load_data`, `validate_schema`, `clean_data`, `build_features`, `train_model`, and `evaluate_model`. Give each function explicit inputs/outputs, avoid hidden global state, and test each unit.

## Bonus — Numeric summary

```python
def summarize_numeric(df):
    numeric = df.select_dtypes(include="number")
    if numeric.shape[1] == 0:
        return pd.DataFrame(columns=[
            "column", "non_missing", "missing", "mean", "std",
            "min", "median", "max", "nunique"
        ])
    rows = []
    for col in numeric.columns:
        s = numeric[col]
        rows.append({
            "column": col,
            "non_missing": int(s.notna().sum()),
            "missing": int(s.isna().sum()),
            "mean": s.mean(),
            "std": s.std(),
            "min": s.min(),
            "median": s.median(),
            "max": s.max(),
            "nunique": int(s.nunique(dropna=True))
        })
    return pd.DataFrame(rows)
```

The default standard deviation is sample standard deviation (`ddof=1`); use `ddof=0` for population standard deviation.
