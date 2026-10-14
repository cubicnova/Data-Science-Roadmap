# Solutions — Reshaping Data in Pandas

The following are illustrative solutions. Alternative code is acceptable when it produces the required layout and aggregation.

```python
import pandas as pd

wide = pd.DataFrame({
    "student": ["Asha", "Ravi"],
    "math": [90, 78],
    "science": [85, 88]
})
```

## 1. Create a wide DataFrame with student names and scores in two subjects.

```python
wide = pd.DataFrame({"student":["Asha","Ravi"], "math":[90,78], "science":[85,88]})
```

## 2. Use melt() to convert the wide student DataFrame to long format.

```python
wide.melt(id_vars="student", var_name="subject", value_name="score")
```

## 3. Set custom var_name and value_name labels in melt().

```python
wide.melt(id_vars="student", var_name="course", value_name="marks")
```

## 4. Use value_vars to melt only selected measurement columns.

```python
wide.melt(id_vars="student", value_vars=["math"], var_name="subject", value_name="score")
```

## 5. Keep an identifier column while melting three measurement columns.

```python
wide3 = pd.DataFrame({"student":["Asha","Ravi"], "math":[90,78], "science":[85,88], "history":[82,91]}); wide3.melt(id_vars="student", var_name="subject", value_name="score")
```

## 6. Explain one situation where long format is more useful than wide format.

```python
# Long format makes it easy to group, filter, or plot all measurements using one value column.
```

## 7. Use pivot() to convert a unique long-format dataset back to wide format.

```python
long.pivot(index="student", columns="subject", values="score")
```

## 8. Reset the index after pivoting so the identifier becomes a column.

```python
long.pivot(index="student", columns="subject", values="score").reset_index()
```

## 9. Demonstrate why pivot() fails when index-column combinations are duplicated.

```python
# Duplicate (index, columns) pairs are ambiguous; pivot() raises ValueError instead of choosing a value.
```

## 10. Use pivot_table() with aggfunc='mean' for duplicated combinations.

```python
long.pivot_table(index="student", columns="subject", values="score", aggfunc="mean")
```

## 11. Use pivot_table() with aggfunc='sum' to summarize sales by region and product.

```python
sales.pivot_table(index="region", columns="product", values="revenue", aggfunc="sum")
```

## 12. Fill missing pivot-table combinations with zero using fill_value.

```python
sales.pivot_table(index="region", columns="product", values="revenue", aggfunc="sum", fill_value=0)
```

## 13. Add row and column totals to a pivot table using margins=True.

```python
sales.pivot_table(index="region", columns="product", values="revenue", aggfunc="sum", margins=True)
```

## 14. Create a frequency table with crosstab() for region and product.

```python
pd.crosstab(df["region"], df["product"])
```

## 15. Normalize a crosstab by index to get row percentages.

```python
pd.crosstab(df["region"], df["product"], normalize="index")
```

## 16. Normalize a crosstab by columns to get column proportions.

```python
pd.crosstab(df["region"], df["product"], normalize="columns")
```

## 17. Use crosstab() with values and aggfunc to summarize a numeric measure.

```python
pd.crosstab(df["region"], df["product"], values=df["revenue"], aggfunc="sum")
```

## 18. Use stack() to move columns into a row index.

```python
wide.set_index("student").stack()
```

## 19. Use unstack() to move an index level into columns.

```python
series.unstack()
```

## 20. Create a MultiIndex DataFrame and demonstrate unstack().

```python
index = pd.MultiIndex.from_product([["East","West"], ["A","B"]], names=["region","product"]); s = pd.Series([10,20,30,40], index=index); s.unstack("product")
```

## 21. Check for duplicate key combinations before using pivot().

```python
long.duplicated(subset=["student","subject"]).sum()
```

## 22. Reshape monthly sales columns into month and sales columns using melt().

```python
monthly.melt(id_vars=["product"], var_name="month", value_name="sales")
```

## 23. Convert a long temperature dataset to wide format by city and day.

```python
temps.pivot(index="city", columns="day", values="temperature")
```

## 24. Create a pivot table of average scores by class and subject.

```python
scores.pivot_table(index="class", columns="subject", values="score", aggfunc="mean")
```

## 25. Compare pivot() and pivot_table() and state when each is appropriate.

```python
# pivot() requires unique index-column pairs; pivot_table() aggregates repeated pairs.
```

## 26. Use crosstab() to count outcomes by department.

```python
pd.crosstab(df["department"], df["outcome"])
```

## 27. Create a normalized crosstab showing the overall proportion of each combination.

```python
pd.crosstab(df["region"], df["product"], normalize="all")
```

## 28. Reshape a DataFrame with id, height, and weight into long format.

```python
measurements.melt(id_vars="id", value_vars=["height","weight"], var_name="measurement", value_name="value")
```

## 29. Use pivot_table() to calculate multiple aggregation functions.

```python
df.pivot_table(index="region", columns="product", values="revenue", aggfunc=["sum","mean"])
```

## 30. After reshaping, verify that the total of a numeric measure is preserved.

```python
assert abs(wide["Jan"].sum() + wide["Feb"].sum() - long["sales"].sum()) < 1e-9  # adapt column names to your example
```

## 31. Use melt() with ignore_index=False and inspect the resulting index.

```python
wide.melt(id_vars="student", ignore_index=False)
```

## 32. Handle missing combinations in a pivot table and explain the meaning of fill_value.

```python
# fill_value=0 means an absent combination is represented as zero; use it only if absence really means zero.
```

## 33. Create a report with regions as rows, products as columns, and total revenue as values.

```python
sales.pivot_table(index="region", columns="product", values="revenue", aggfunc="sum", fill_value=0)
```

## 34. Reshape a dataset and then use groupby() to calculate an aggregate.

```python
long.groupby("region", as_index=False)["sales"].sum()
```

## 35. Build an end-to-end example converting wide sales data to long, summarizing it, and pivoting it back to a report.

```python
long = wide.melt(id_vars=["region"], var_name="product", value_name="revenue"); summary = long.groupby(["region","product"], as_index=False)["revenue"].sum(); summary.pivot(index="region", columns="product", values="revenue")
```

## Final reminders

- `melt()` turns columns into rows; `pivot()` turns unique key-value combinations into columns.
- `pivot_table()` is for reshaping with aggregation.
- Use `crosstab()` for category counts or normalized proportions.
- Confirm the meaning of missing combinations before filling them with zero.
