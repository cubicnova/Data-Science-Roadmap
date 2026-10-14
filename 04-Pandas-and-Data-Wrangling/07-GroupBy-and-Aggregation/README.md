# 07 — GroupBy and Aggregation in Pandas

Grouping lets you summarize rows by one or more categories. This module covers the split-apply-combine pattern, built-in aggregations, multiple aggregations, named output columns, grouped transformations, filtering groups, and common reporting patterns.

## Learning objectives
- Explain the split-apply-combine workflow.
- Group by one or multiple columns using `groupby()`.
- Apply aggregations such as `sum()`, `mean()`, `count()`, `size()`, `min()`, and `max()`.
- Use `agg()` for multiple calculations and readable output names.
- Distinguish aggregation from transformation and filtering.
- Handle missing group keys and choose appropriate aggregation methods.

## 1. Sample data

```python
import pandas as pd

sales = pd.DataFrame({
    "region": ["East", "West", "East", "North", "West", "East", "North"],
    "product": ["A", "A", "B", "B", "C", "A", "C"],
    "salesperson": ["Mira", "Dev", "Mira", "Arun", "Dev", "Zoya", "Arun"],
    "units": [10, 7, 5, 9, 4, 8, 6],
    "revenue": [1000, 700, 750, 1350, 600, 800, 900]
})
```

## 2. The split-apply-combine pattern

`groupby()` splits rows into groups, applies a calculation to each group, and combines the results.

```python
sales.groupby("region")["revenue"].sum()
sales.groupby("product")["units"].mean()
sales.groupby(["region", "product"])["revenue"].sum()
```

A groupby result often has the grouping keys as its index. Use `as_index=False` or `.reset_index()` when you prefer ordinary columns.

```python
sales.groupby("region", as_index=False)["revenue"].sum()
```

## 3. Common aggregation functions

- `sum()`: total.
- `mean()`: arithmetic average.
- `median()`: middle value.
- `min()` / `max()`: smallest / largest.
- `count()`: non-missing values in each column.
- `size()`: number of rows in each group, including rows where selected values are missing.
- `nunique()`: number of distinct non-missing values.
- `std()` / `var()`: standard deviation / variance.

```python
sales.groupby("region")["revenue"].agg(["sum", "mean", "min", "max", "count"])
sales.groupby("region").size()
```

`count()` and `size()` are not interchangeable: `count()` ignores missing values in the selected Series, while `size()` counts rows.

## 4. Aggregate multiple columns and functions

```python
sales.groupby("region").agg({
    "revenue": ["sum", "mean"],
    "units": ["sum", "max"],
    "product": "nunique"
})
```

This creates hierarchical column labels. For reports, named aggregation can produce cleaner column names.

```python
sales.groupby("region").agg(
    total_revenue=("revenue", "sum"),
    average_revenue=("revenue", "mean"),
    total_units=("units", "sum"),
    products_sold=("product", "nunique")
)
```

## 5. Group by multiple keys

```python
sales.groupby(["region", "product"], as_index=False).agg(
    total_revenue=("revenue", "sum"),
    transactions=("revenue", "size")
)
```

Use multiple keys when the metric should be summarized at a more detailed level, such as region and product or department and job level.

## 6. Transform: return one result per original row

`agg()` reduces each group to summary rows. `transform()` returns a value aligned to each original row, which is useful for adding group statistics back to the data.

```python
sales["region_total"] = sales.groupby("region")["revenue"].transform("sum")
sales["share_of_region"] = sales["revenue"] / sales["region_total"]
```

Other examples include group means, z-scores, and filling missing values with group-level statistics.

## 7. Filter groups

`filter()` keeps or removes whole groups based on a condition.

```python
sales.groupby("region").filter(lambda group: group["revenue"].sum() >= 1500)
```

This returns original rows from groups whose total revenue meets the condition. Use a Boolean mask based on a transform if you need a different row-level selection.

## 8. Missing group keys and ordering

By default, `groupby()` excludes rows whose grouping key is missing. Set `dropna=False` to include missing keys as a group.

```python
data.groupby("region", dropna=False)["revenue"].sum()
```

`sort=False` can avoid sorting group keys and preserve the order in which groups first appear. Choose options explicitly when output ordering matters.

## 9. Custom aggregation functions

Built-in functions are usually faster and clearer. For specialized summaries, pass a function to `agg()`.

```python
def revenue_range(series):
    return series.max() - series.min()

sales.groupby("region")["revenue"].agg(revenue_range)
```

For multiple outputs, prefer named aggregations or separate aggregation calls that remain easy to understand.

## 10. Best practices
1. Define the unit of analysis: row, transaction, customer, region, etc.
2. Choose grouping keys that match the business question.
3. Distinguish row count (`size`) from non-null count (`count`).
4. Prefer named aggregation for readable output columns.
5. Use `transform()` when output must align to the original rows.
6. Be intentional about missing group keys with `dropna`.
7. Verify totals by comparing group totals to the overall total where appropriate.
8. Avoid unnecessarily slow custom Python functions when built-in aggregations work.

## Quick reference

| Task | Expression |
|---|---|
| Total by region | `df.groupby("region")["sales"].sum()` |
| Count rows per group | `df.groupby("region").size()` |
| Non-null count | `df.groupby("region")["sales"].count()` |
| Multiple metrics | `df.groupby("region").agg(["sum", "mean"])` |
| Named output columns | `df.groupby("region").agg(total=("sales", "sum"))` |
| Multiple keys | `df.groupby(["region", "product"]).sum(numeric_only=True)` |
| Add group total to rows | `df.groupby("region")["sales"].transform("sum")` |
| Keep groups meeting a condition | `df.groupby("region").filter(...)` |

## Practice
Complete [`practice.md`](practice.md), run [`examples.ipynb`](examples.ipynb), and check answers in [`solutions.md`](solutions.md).
