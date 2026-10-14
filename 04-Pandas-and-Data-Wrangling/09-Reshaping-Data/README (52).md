# 09 — Reshaping Data in Pandas

Reshaping changes the layout of a dataset without necessarily changing the information it represents. This module covers wide and long formats, `melt()`, `pivot()`, `pivot_table()`, `stack()`, `unstack()`, and `crosstab()`.

## Learning objectives
- Recognize wide and long data layouts.
- Convert wide data to long form using `melt()`.
- Convert long data to wide form using `pivot()`.
- Summarize repeated observations with `pivot_table()`.
- Reshape hierarchical indexes with `stack()` and `unstack()`.
- Create frequency tables with `crosstab()`.
- Diagnose duplicate keys and missing combinations during reshaping.

## 1. Wide versus long data

Wide data often stores repeated measurements in separate columns:

| student | math | science |
|---|---:|---:|
| Asha | 90 | 85 |
| Ravi | 78 | 88 |

Long data stores the measurement name and value in separate columns:

| student | subject | score |
|---|---|---:|
| Asha | math | 90 |
| Asha | science | 85 |
| Ravi | math | 78 |
| Ravi | science | 88 |

Neither layout is universally better. Long format is often convenient for grouping and visualization; wide format can be useful for reports and some modeling inputs.

## 2. `melt()` — wide to long

```python
wide = pd.DataFrame({
    "student": ["Asha", "Ravi"],
    "math": [90, 78],
    "science": [85, 88]
})
long = wide.melt(
    id_vars="student",
    var_name="subject",
    value_name="score"
)
```

- `id_vars`: columns to keep as identifiers.
- `value_vars`: columns to unpivot (defaults to all columns not in `id_vars`).
- `var_name`: name of the new variable-label column.
- `value_name`: name of the new measurement column.

Use `ignore_index=False` if retaining original row labels is important.

## 3. `pivot()` — long to wide

```python
long.pivot(index="student", columns="subject", values="score")
```

`pivot()` requires each `index` and `columns` combination to identify at most one value. If duplicates exist, it raises an error rather than deciding how to combine them. Inspect duplicates or use `pivot_table()` if aggregation is intended.

Use `.reset_index()` if you want the index returned as a regular column.

## 4. `pivot_table()` — aggregate and reshape

`pivot_table()` handles repeated combinations by aggregating them.

```python
sales = pd.DataFrame({
    "region": ["East", "East", "West", "West"],
    "product": ["A", "A", "A", "B"],
    "revenue": [100, 150, 200, 120]
})
sales.pivot_table(
    index="region",
    columns="product",
    values="revenue",
    aggfunc="sum",
    fill_value=0
)
```

Common options:
- `aggfunc="mean"`, `"sum"`, `"count"`, or a custom function.
- `fill_value=` fills missing cells in the resulting table.
- `margins=True` adds totals.
- `dropna=` controls handling of all-missing columns/rows in relevant cases.

Be clear about whether repeated records should be summed, averaged, counted, or otherwise aggregated.

## 5. `stack()` and `unstack()`

These operations move labels between the columns and index.

```python
wide.stack()      # move a column level into the row index
series.unstack()  # move an index level into columns
```

They are useful for MultiIndex data. Depending on Pandas version and parameters, missing combinations may be dropped or retained; inspect the output and version-specific options when that distinction matters.

## 6. `crosstab()` — frequency and summary tables

```python
pd.crosstab(df["region"], df["product"])
pd.crosstab(df["region"], df["product"], normalize="index")
```

Normalization options include `"all"`, `"index"`, and `"columns"`. You can also pass `values=` with `aggfunc=` to summarize a numeric field rather than count frequencies.

## 7. Handling duplicates before pivoting

```python
long.duplicated(subset=["student", "subject"]).sum()
```

If the intended data has one score per student and subject, duplicate pairs should be investigated. Do not use `pivot_table()` to silently hide data-quality problems unless aggregation is the correct business rule.

## 8. Best practices
1. Identify the intended row unit before reshaping.
2. Keep identifier columns intact with `id_vars`.
3. Choose `pivot()` when combinations are unique and `pivot_table()` when aggregation is required.
4. Check duplicate key combinations before pivoting.
5. Give `var_name` and `value_name` meaningful labels in `melt()`.
6. Use `fill_value` only when filling missing combinations has a valid interpretation.
7. Use `reset_index()` when downstream code expects ordinary columns.
8. Validate row counts and values before and after reshaping.

## Quick reference

| Task | Expression |
|---|---|
| Wide to long | `df.melt(id_vars=["id"], var_name="metric", value_name="value")` |
| Long to wide | `df.pivot(index="id", columns="metric", values="value")` |
| Aggregate while pivoting | `df.pivot_table(index="id", columns="metric", values="value", aggfunc="mean")` |
| Move columns into index | `df.stack()` |
| Move index level to columns | `df.unstack()` |
| Count category combinations | `pd.crosstab(df["a"], df["b"])` |

## Practice
Complete [`practice.md`](practice.md), run [`examples.ipynb`](examples.ipynb), and check your work with [`solutions.md`](solutions.md).
