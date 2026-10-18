# Pandas and NumPy Interview Practice — Suggested Solutions

The following are reference answers. Alternative implementations may also be correct. The examples favor explicit, readable operations and point out assumptions where data quality matters.

## Section A — NumPy

### 1. Create and reshape an array

```python
import numpy as np

arr = np.arange(12).reshape(3, 4)
print(arr)
```

Output shape is `(3, 4)`.

### 2. Select the second row and last two columns

```python
arr[1, -2:]
```

NumPy uses zero-based indexing, so row index `1` is the second row.

### 3. Mean per column and sum per row

```python
column_means = arr.mean(axis=0)
row_sums = arr.sum(axis=1)
```

For a 2-D array, `axis=0` reduces rows and leaves one value per column; `axis=1` reduces columns and leaves one value per row.

### 4. Boolean mask

```python
arr[arr > 10]
```

The result is a 1-D array of matching elements.

### 5. Broadcasting

```python
a = np.ones((3, 1))
b = np.ones((1, 4))
(a + b).shape  # (3, 4)
```

Dimensions are compared from the right. They are compatible if equal or one dimension is `1`.

### 6. Min-max scaling

```python
x = np.array([10.0, 20.0, 30.0])
denominator = x.max() - x.min()
scaled = np.zeros_like(x) if denominator == 0 else (x - x.min()) / denominator
```

When all values are identical, the denominator is zero. Decide on a documented convention, such as returning zeros, rather than dividing by zero.

### 7. `arr.sort()` versus `np.sort(arr)`

`arr.sort()` sorts the array in place and returns `None`. `np.sort(arr)` returns a sorted copy by default and leaves the original unchanged.

### 8. Vectorization

Vectorized operations delegate loops to optimized array code, reduce Python-level overhead, and often improve readability. Performance depends on the operation and data size, so benchmark realistic workloads.

## Section B — Pandas selection and cleaning

Assume a DataFrame named `df`.

### 9. Select columns

```python
df[["name", "score"]]
```

The double brackets select a DataFrame containing two columns.

### 10. Multiple conditions

```python
df.loc[
    (df["score"] >= 80) & (df["department"].isin(["Data", "AI"])),
    ["name", "department", "score"]
]
```

Each comparison is parenthesized because `&` combines Boolean Series element by element.

### 11. First three rows by position

```python
df.iloc[:3]
```

`iloc` uses integer positions rather than index labels.

### 12. Inspect and fill missing scores

```python
missing_scores = df.loc[df["score"].isna()]
df["score"] = df["score"].fillna(df["score"].median())
```

If the median is also missing because all values are missing, choose an explicit fallback or leave values missing.

### 13. Remove duplicate rows

```python
df = df.drop_duplicates()
```

For duplicate business keys, define which record should survive, for example the latest timestamp, then sort and use `drop_duplicates(subset=["customer_id"], keep="last")`.

### 14. Parse dates safely

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
invalid_dates = df.loc[df["date"].isna()]
```

`errors="coerce"` turns unparseable values into `NaT`, which can then be reviewed.

### 15. Standardize category text

```python
df["category"] = df["category"].astype("string").str.strip().str.lower()
```

The string dtype can preserve missing values as `pd.NA`; check downstream code for dtype assumptions.

### 16. Avoid chained assignment

Unsafe:

```python
df[df["score"] > 0]["flag"] = True
```

Safer:

```python
df.loc[df["score"] > 0, "flag"] = True
```

The `.loc` form assigns directly to the selected rows of the original DataFrame.

## Section C — Grouping and transformation

### 17. Mean score and count by department

```python
df.groupby("department", dropna=False).agg(
    mean_score=("score", "mean"),
    row_count=("score", "size")
).reset_index()
```

`size` counts rows in each group; `count` on the score column would exclude missing scores. `dropna=False` keeps a group for missing department values in supported Pandas versions.

### 18. Top-scoring row per department

```python
top_rows = (
    df.sort_values(["score", "employee_id"], ascending=[False, True])
      .drop_duplicates("department", keep="first")
)
```

This assumes `employee_id` exists and is a deterministic tie-breaker. If all tied top rows are needed, use `groupby("department")["score"].transform("max")` and filter rows equal to that maximum.

### 19. Add group mean without reducing rows

```python
df["department_mean"] = df.groupby("department")["score"].transform("mean")
```

`transform` returns values aligned to the original index.

### 20. Score as a share of department total

```python
totals = df.groupby("department")["score"].transform("sum")
df["score_share"] = df["score"].div(totals.where(totals.ne(0)))
```

Where a group total is zero, the share becomes missing instead of dividing by zero. Define a different business rule if needed.

### 21. `agg`, `transform`, and `apply`

- `agg`: summarizes each group, often reducing it to one row per group.
- `transform`: returns a result aligned to the original rows, such as a group mean per row.
- `apply`: flexible but more general; it can return varied shapes and may be slower or harder to reason about. Prefer specialized methods when possible.

## Section D — Merge, concatenate, and reshape

### 22. Merge with department metadata

```python
result = employees.merge(
    departments,
    on="department_id",
    how="left",
    validate="many_to_one"
)
```

`many_to_one` asserts that the department key is unique in the right-hand table.

### 23. Rows without a matching department

```python
unmatched = employees.merge(
    departments[["department_id"]],
    on="department_id",
    how="left",
    indicator=True,
    validate="many_to_one"
).query("_merge == 'left_only'")
```

### 24. Many-to-many row multiplication

If a key appears `m` times on the left and `n` times on the right, that key can produce `m * n` matching rows. Check key uniqueness and validate the expected relationship before trusting totals.

### 25. Stack DataFrames

```python
combined = pd.concat([df_january, df_february], axis=0, ignore_index=True)
```

`ignore_index=True` creates a fresh sequential index rather than retaining the original labels.

### 26. Long to wide with `pivot_table`

```python
wide = sales.pivot_table(
    index="date",
    columns="product",
    values="sales",
    aggfunc="sum",
    fill_value=0
)
```

`pivot_table` aggregates duplicate index/column combinations; `pivot` instead requires unique combinations.

### 27. Wide to long with `melt`

```python
long = wide.reset_index().melt(
    id_vars="date",
    var_name="product",
    value_name="sales"
)
```

This converts product columns into rows.

## Section E — Dates and performance

### 28. Extract year and month

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df["year"] = df["date"].dt.year
df["month"] = df["date"].dt.month
```

### 29. Three-observation rolling mean

```python
df = df.sort_values("date")
df["rolling_mean_3"] = df["value"].rolling(window=3, min_periods=1).mean()
```

This uses the current observation and up to two previous observations.

### 30. Row-based versus time-based rolling

`rolling(window=3)` uses three observations. A time-based window such as `rolling("7D")` uses observations in a seven-day interval and requires a datetime-like index or an appropriate datetime column configuration. Missing dates and irregular sampling can make the results differ.

### 31. Reduce memory use

- Use appropriate numeric dtypes and downcast only after checking value ranges.
- Convert repeated low-cardinality strings to `category` where suitable.
- Load only needed columns and use chunking for large files.
- Avoid unnecessary copies and intermediate DataFrames.
- Inspect memory with `df.memory_usage(deep=True)`.

### 32. Merge doubles row count

Check duplicated join keys on both sides, confirm expected cardinality, inspect unmatched rows, and use `validate=`. A many-to-many relationship may be correct, but totals must be calculated at the intended grain.

### 33. Slow `apply(axis=1)`

First profile and confirm the bottleneck. Replace row-wise Python functions with vectorized arithmetic, Boolean masks, `.str`/`.dt` accessors, `np.select`, or group operations when possible. For genuinely complex logic, consider whether a loop, compiled function, or database-side operation is more appropriate.

## Section F — Concepts and edge cases

### 34. NumPy array versus Pandas DataFrame

A NumPy array is an N-dimensional container typically using one dtype per array. A DataFrame is a labeled 2-D structure with row and column labels and potentially different dtypes by column. Pandas builds on NumPy and other array implementations.

### 35. `loc` / `iloc` and `at` / `iat`

- `loc`: label-based selection.
- `iloc`: integer-position selection.
- `at`: fast scalar access by row and column labels.
- `iat`: fast scalar access by integer positions.

### 36. Axis semantics

For a 2-D array with shape `(rows, columns)`, `axis=0` reduces across rows and returns one value per column; `axis=1` reduces across columns and returns one value per row.

### 37. Missing values

`None`, floating-point `NaN`, `NaT`, and `pd.NA` are missing-value representations used in different contexts and dtypes. Equality checks are not reliable for detecting all missing values; use `isna()` and `notna()`.

### 38. View versus copy

A view shares underlying data with another object, so changes may be reflected in both; a copy owns separate data. Exact behavior depends on the operation and dtype. Prefer explicit `.copy()` when an independent object is required and use `.loc` for assignments.

### 39. `validate="one_to_one"`

It checks that the merge keys are unique on both sides. Other useful options include `"many_to_one"` and `"one_to_many"`.

### 40. Category dtype

`category` can reduce memory when many rows repeat a small set of labels. It may not help for high-cardinality columns where nearly every value is unique, and some operations may require conversion or careful category handling.

## Bonus — Sales quality report

```python
import pandas as pd

orders["order_date"] = pd.to_datetime(orders["order_date"], errors="coerce")
orders["revenue"] = orders["quantity"] * orders["unit_price"]

revenue_by_region = orders.groupby("region", dropna=False).agg(
    revenue=("revenue", "sum"),
    order_rows=("order_id", "size"),
    unique_orders=("order_id", "nunique")
)

duplicate_order_ids = orders.loc[
    orders["order_id"].duplicated(keep=False)
].sort_values("order_id")

missing_by_column = orders.isna().sum()

orders["month"] = orders["order_date"].dt.to_period("M").astype("string")
product_month = orders.pivot_table(
    index="product",
    columns="month",
    values="revenue",
    aggfunc="sum",
    fill_value=0
)
```

Before reporting, determine whether `order_id` is unique per row or whether the table contains order line items. If there are multiple lines per order, `order_rows` is not the same as distinct order count. Validate totals against source data and aggregate at the intended business grain.
