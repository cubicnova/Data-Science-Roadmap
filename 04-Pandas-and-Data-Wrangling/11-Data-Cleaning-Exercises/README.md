# 11 — Data Cleaning Exercises with Pandas

This module brings together core Pandas cleaning techniques in realistic, hands-on scenarios. The focus is on producing reliable, consistent datasets and documenting decisions made during cleaning.

## Learning objectives

- Inspect dataset structure, data types, missingness, and duplicate records.
- Standardize column names, text values, and categorical labels.
- Parse dates and numbers safely.
- Handle missing values and duplicates with deliberate rules.
- Detect invalid values, outliers, and inconsistent records.
- Validate cleaning results and preserve raw data for traceability.

## 1. Start with an audit

```python
import pandas as pd

df = pd.read_csv("data.csv")
df.head()
df.shape
df.info()
df.describe(include="all")
df.isna().sum()
df.duplicated().sum()
```

Inspect the data before changing it. Record row and column counts, data types, missing values, duplicate counts, and any business rules.

## 2. Standardize column names

```python
df.columns = (
    df.columns.astype("string")
      .str.strip()
      .str.lower()
      .str.replace(r"\s+", "_", regex=True)
      .str.replace(r"[^a-z0-9_]", "", regex=True)
)
```

Check for duplicate column names after normalization, because different original names may collapse to the same cleaned name.

## 3. Normalize text and categories

```python
df["name"] = df["name"].astype("string").str.strip().str.title()
df["email"] = df["email"].astype("string").str.strip().str.lower()
df["status"] = df["status"].astype("string").str.strip().str.lower()
df["status"] = df["status"].replace({
    "complete": "completed",
    "done": "completed",
    "in progress": "in_progress"
})
```

Normalize labels only when the equivalence is supported by the data or a business rule. Avoid automatically changing names or free-text fields in ways that might destroy meaningful capitalization.

## 4. Parse types safely

```python
df["amount"] = pd.to_numeric(df["amount"], errors="coerce")
df["created_at"] = pd.to_datetime(df["created_at"], errors="coerce")
```

`errors="coerce"` turns unparseable values into missing values. Audit these newly missing values so invalid input is not silently lost.

For currency strings, remove known symbols and separators first:
```python
clean_amount = (
    df["amount"].astype("string")
      .str.replace(",", "", regex=False)
      .str.replace("₹", "", regex=False)
      .str.strip()
)
df["amount"] = pd.to_numeric(clean_amount, errors="coerce")
```

## 5. Missing values

```python
df.isna().sum()
df["age"] = df["age"].fillna(df["age"].median())
df["city"] = df["city"].fillna("Unknown")
df = df.dropna(subset=["customer_id"])
```

Choose a strategy based on the field's meaning:
- Drop rows only when the missing field is essential and dropping is justified.
- Use median for skewed numeric data when appropriate; use mean only when it makes sense.
- Use a transparent category such as `"Unknown"` for missing categorical data.
- Consider retaining a missingness indicator.
- Do not impute labels or targets casually in machine-learning datasets.

## 6. Duplicates

```python
df.duplicated().sum()
df = df.drop_duplicates()
df = df.drop_duplicates(subset=["email"], keep="last")
```

Before removing duplicates, determine whether rows are true duplicates or valid repeated events. If keeping the latest record, sort by a trustworthy timestamp first.

## 7. Validate ranges and rules

```python
invalid_age = ~df["age"].between(0, 120)
invalid_email = ~df["email"].str.fullmatch(r"[^@\s]+@[^@\s]+\.[^@\s]+", na=False)
negative_amount = df["amount"] < 0
```

Validation rules depend on the domain. A negative amount may be invalid for a sale but valid for a refund or accounting adjustment.

## 8. Outlier review

A simple IQR review:
```python
q1 = df["amount"].quantile(0.25)
q3 = df["amount"].quantile(0.75)
iqr = q3 - q1
outlier_mask = (df["amount"] < q1 - 1.5 * iqr) | (df["amount"] > q3 + 1.5 * iqr)
```

Outliers are not automatically errors. Flag and investigate them before deleting or capping values.

## 9. A repeatable cleaning workflow

1. Load raw data and preserve an untouched copy.
2. Profile shape, types, missingness, duplicates, and value distributions.
3. Define cleaning rules before applying them.
4. Standardize column names and data types.
5. Clean text and categorical values.
6. Handle missing values and duplicates.
7. Validate ranges, formats, and cross-column logic.
8. Compare before/after counts and inspect sample rows.
9. Export cleaned data and a short quality report.

```python
raw = pd.read_csv("input.csv")
df = raw.copy()
# Apply explicit cleaning rules...
df.to_csv("cleaned.csv", index=False)
```

## 10. Best practices

- Keep raw data unchanged.
- Make transformations repeatable and idempotent where possible.
- Prefer vectorized Pandas operations.
- Track how many values were changed, coerced, dropped, or flagged.
- Avoid assumptions: document every imputation and deduplication rule.
- Validate results with assertions and before/after comparisons.
- Separate data-quality flags from destructive cleanup.

Complete `practice.md`, use `examples.ipynb` to explore the workflow, and compare your work with `solutions.md`.
