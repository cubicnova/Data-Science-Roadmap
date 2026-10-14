# 06 — Duplicates and Data Types in Pandas

Reliable data analysis depends on identifying repeated records and ensuring that columns use appropriate data types. This module explains how to inspect duplicates, keep or remove repeated rows, convert data types safely, parse dates, work with nullable types, and validate cleaned data.

## Learning objectives
- Detect duplicated rows and duplicated business keys.
- Remove duplicates with explicit keep rules.
- Distinguish exact duplicates from records that merely share a field.
- Inspect and convert Pandas data types.
- Clean numeric strings and parse date/time columns.
- Use nullable integer, Boolean, and string dtypes.
- Validate conversions and handle errors without silently losing information.

## 1. Sample data and dtype inspection

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "customer_id": ["C01", "C02", "C02", "C03"],
    "name": ["Asha", "Ravi", "Ravi", "Meera"],
    "age": ["21", "34", "34", "29"],
    "joined": ["2025-01-10", "2025-02-12", "2025-02-12", "2025-03-01"],
    "active": ["True", "False", "False", "True"]
})
df.dtypes
```

Use `df.info()` for a concise summary of columns, non-null counts, and dtypes. `df.dtypes` returns the dtype of each column.

## 2. Detect duplicates

```python
df.duplicated()                    # duplicate rows, considering all columns
df.duplicated().sum()              # count duplicate rows
df.duplicated(subset=["customer_id"])  # duplicate business keys
df[df.duplicated(keep=False)]      # show every member of duplicate groups
```

The `keep` argument controls which matching row is marked as duplicate:
- `keep="first"` (default): keep the first occurrence unmarked.
- `keep="last"`: keep the last occurrence unmarked.
- `keep=False`: mark every occurrence in each duplicate group as duplicate.

## 3. Remove duplicates deliberately

```python
df.drop_duplicates()
df.drop_duplicates(subset=["customer_id"], keep="first")
df.drop_duplicates(subset=["customer_id"], keep="last")
```

Use `subset` to define the columns that identify a duplicate record. Do not remove rows based on a single shared field unless that field is intended to be unique. Before dropping records, decide which record should survive (for example, latest update timestamp or most complete row).

## 4. Find duplicate groups for investigation

```python
dupe_mask = df.duplicated(subset=["customer_id"], keep=False)
df.loc[dupe_mask].sort_values("customer_id")
```

For a count by key:

```python
df["customer_id"].value_counts()
```

Duplicates may represent repeated ingestion, legitimate repeated events, or different people sharing an attribute. Investigate before deleting.

## 5. Inspect and convert data types

Common Pandas dtypes include:
- `int64`, `float64`: numeric values.
- `object`: often strings or mixed Python objects.
- `string`: Pandas string dtype.
- `bool` and nullable `boolean`.
- `datetime64[ns]`: date/time values.
- `category`: repeated labels with a finite set of values.

```python
df["age"] = pd.to_numeric(df["age"], errors="raise")
df["joined"] = pd.to_datetime(df["joined"], errors="raise")
df["name"] = df["name"].astype("string")
```

Use `errors="raise"` while developing to catch bad values. `errors="coerce"` converts invalid numeric/date values to missing (`NaN`/`NaT`) and should be followed by an explicit quality check.

## 6. Convert numeric strings safely

Real files may contain currency symbols, thousands separators, or whitespace.

```python
amount = pd.Series(["1,250", " 900 ", "not known"])
clean = amount.astype("string").str.replace(",", "", regex=False).str.strip()
numeric = pd.to_numeric(clean, errors="coerce")
```

Review values that became missing during coercion rather than ignoring them:

```python
invalid = clean[numeric.isna() & clean.notna()]
```

Do not remove punctuation blindly if it has meaning in the source format (for example, decimal separators).

## 7. Nullable dtypes

Traditional integer columns may become floating-point when missing values are introduced. Pandas nullable types can preserve integer or Boolean semantics.

```python
df["age"] = df["age"].astype("Int64")  # capital I
flags = pd.Series([True, False, pd.NA], dtype="boolean")
```

The Pandas `"string"` dtype supports missing values and string methods more predictably than a mixed `object` column.

## 8. Boolean conversion

Text such as `"False"` is still a non-empty string, so `astype(bool)` can produce surprising results. Map known values explicitly:

```python
mapping = {"true": True, "false": False, "yes": True, "no": False}
converted = df["active"].str.lower().map(mapping).astype("boolean")
```

Inspect unexpected labels before conversion.

## 9. Dates, categories, and memory

```python
df["joined"] = pd.to_datetime(df["joined"], errors="coerce")
df["status"] = df["status"].astype("category")
```

Use categories when values repeat among a small set of labels and category ordering is appropriate. For dates with a known format, pass `format=` to make parsing clearer and more reliable.

## 10. Best practices
1. Preserve a raw copy before cleaning.
2. Define duplicates using business meaning, not convenience.
3. Inspect all members of duplicate groups before removal.
4. Use explicit conversion targets and check results.
5. Track values invalidated by `errors="coerce"`.
6. Avoid `astype(bool)` on arbitrary strings.
7. Parse dates to datetime before sorting or date arithmetic.
8. Validate row counts, missing counts, unique keys, and dtypes after cleaning.
9. Document which record survives when duplicate rows conflict.

## Quick reference

| Task | Expression |
|---|---|
| Dtype of each column | `df.dtypes` |
| DataFrame summary | `df.info()` |
| Mark duplicate rows | `df.duplicated()` |
| Mark duplicates by key | `df.duplicated(subset=["id"])` |
| Show all duplicate members | `df[df.duplicated(subset=["id"], keep=False)]` |
| Drop exact duplicate rows | `df.drop_duplicates()` |
| Convert numeric | `pd.to_numeric(s, errors="coerce")` |
| Convert datetime | `pd.to_datetime(s, errors="coerce")` |
| Convert string dtype | `s.astype("string")` |
| Nullable integer | `s.astype("Int64")` |

## Practice
Complete [`practice.md`](practice.md), execute [`examples.ipynb`](examples.ipynb), and check your work with [`solutions.md`](solutions.md).
