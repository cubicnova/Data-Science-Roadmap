# 05 — Missing Values in Pandas

Missing data is common in real-world datasets. This module covers how to detect, summarize, remove, fill, and reason about missing values without accidentally distorting your analysis.

## Learning objectives
- Identify missing values with `isna()` and `notna()`.
- Summarize missingness by column and row.
- Remove missing values with `dropna()`.
- Impute missing values with `fillna()`, statistics, and group-specific values.
- Forward-fill and backward-fill ordered data.
- Understand why missingness occurs and avoid misleading imputations.

## 1. Create a sample DataFrame

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "name": ["Asha", "Ravi", "Meera", "Kabir", None],
    "age": [21, np.nan, 23, 22, 20],
    "city": ["Indore", "Pune", None, "Delhi", "Pune"],
    "score": [88, 74, np.nan, 91, 85]
})
```

Pandas recognizes values such as `None`, `np.nan`, and `pd.NA` as missing in many common operations. An empty string (`""`) or a text value such as `"N/A"` is not automatically missing unless you convert it.

## 2. Detect missing values

```python
df.isna()            # True wherever a value is missing
df.notna()           # True wherever a value is present
df.isna().sum()      # missing count per column
df.isna().sum().sum()  # total missing cells
df[df["age"].isna()] # rows with missing age
```

For a missingness percentage by column:

```python
missing_pct = df.isna().mean().mul(100).round(1)
```

## 3. Remove missing data

`dropna()` returns a new DataFrame by default.

```python
df.dropna()                         # drop rows with any missing value
df.dropna(subset=["age"])           # require age to be present
df.dropna(subset=["age", "score"])  # require both columns
df.dropna(axis=1, how="all")        # drop columns that are entirely missing
df.dropna(thresh=3)                 # keep rows with at least 3 non-missing values
```

Use deletion only when it is justified. Removing rows can bias a dataset if missingness is systematic or if many rows are lost.

## 4. Fill missing values

```python
df["age"].fillna(df["age"].median())
df["city"].fillna("Unknown")
df.fillna({"age": df["age"].median(), "city": "Unknown"})
```

Common strategies:
- **Mean:** useful for some roughly symmetric numeric distributions; sensitive to outliers.
- **Median:** robust to outliers; often useful for skewed numeric data.
- **Mode:** common for categorical values, but may overrepresent the most common category.
- **Constant / “Unknown”:** appropriate when it has a meaningful interpretation.
- **Model-based imputation:** can be useful but adds complexity and assumptions.

Fit imputation values using training data only when building machine-learning models; then apply those same learned values to validation/test data to prevent leakage.

## 5. Forward fill and backward fill

These methods are useful for ordered observations, especially time series.

```python
df["reading"].ffill()  # use previous valid observation
df["reading"].bfill()  # use next valid observation
```

Sort by time first. Do not fill across unrelated groups or long gaps without considering the domain.

## 6. Group-specific imputation

A global median can hide differences between groups. A group-specific median can be more appropriate when the groups are meaningful and sufficiently populated.

```python
df["age_filled"] = df["age"].fillna(
    df.groupby("city")["age"].transform("median")
)
```

If a group has no valid values, its group median is also missing. Plan a fallback strategy, such as a global training-set median.

## 7. Convert placeholders to missing values

```python
df = df.replace(["N/A", "NA", "-", ""], pd.NA)
```

Only replace placeholders that are actually used in your source data. For CSV imports, `read_csv()` also supports `na_values=`.

```python
data = pd.read_csv("data.csv", na_values=["N/A", "-", "unknown"])
```

## 8. Missingness patterns and checks

```python
df.isna().sum().sort_values(ascending=False)
df.loc[df.isna().any(axis=1)]  # any row with at least one missing cell
```

Check missingness before and after cleaning, preserve the raw data, and document decisions. Missingness can be:
- **MCAR:** missing completely at random.
- **MAR:** missingness depends on observed information.
- **MNAR:** missingness depends on the missing value itself or unobserved factors.

These categories are conceptual assumptions, not labels that can be reliably assigned from a single missing-value count.

## 9. Best practices
1. Profile missingness before choosing a strategy.
2. Preserve a copy of raw data and compare row counts before and after cleaning.
3. Do not assume missing means zero.
4. Choose a strategy based on column meaning, distribution, and missingness mechanism.
5. Add an indicator column when the fact that a value was missing may itself be informative.
6. Avoid target leakage: never calculate imputation statistics from the test set.
7. Verify dtypes after filling, especially with nullable integer and categorical columns.
8. Validate that required fields are present before downstream processing.

## Quick reference

| Task | Expression |
|---|---|
| Detect missing values | `df.isna()` |
| Count per column | `df.isna().sum()` |
| Missing percentage | `df.isna().mean() * 100` |
| Drop incomplete rows | `df.dropna()` |
| Drop rows missing selected fields | `df.dropna(subset=["age"])` |
| Fill a numeric column | `df["age"].fillna(df["age"].median())` |
| Fill multiple columns | `df.fillna({"age": 0, "city": "Unknown"})` |
| Forward fill | `s.ffill()` |
| Backward fill | `s.bfill()` |
| Replace placeholders | `df.replace("N/A", pd.NA)` |

## Practice and examples
Complete [`practice.md`](practice.md), run [`examples.ipynb`](examples.ipynb), and use [`solutions.md`](solutions.md) to check your work.
