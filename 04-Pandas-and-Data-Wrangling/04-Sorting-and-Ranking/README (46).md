# 04 — Sorting and Ranking in Pandas

Learn to sort rows and columns, control sort order, rank values, handle ties, and create ordered views of data using Pandas.

## Learning objectives

By the end of this module, you will be able to:
- Sort a DataFrame by one or more columns with `sort_values()`.
- Sort rows by index and columns by labels with `sort_index()`.
- Control ascending/descending order and null placement.
- Use stable sorting and custom sort priorities.
- Rank values with `rank()` and choose tie-breaking methods.
- Combine sorting, filtering, grouping, and ranking for practical analysis.

## 1. Setup and sample data

```python
import pandas as pd

df = pd.DataFrame({
    "name": ["Asha", "Ravi", "Meera", "Kabir", "Zoya", "Ishan"],
    "department": ["IT", "Sales", "IT", "Sales", "HR", "IT"],
    "score": [88, 74, 95, 74, 91, 88],
    "experience": [3, 5, 2, 7, 4, 1],
    "city": ["Indore", "Pune", "Indore", "Delhi", "Pune", "Delhi"]
})
```

## 2. Sort by one column

Use `sort_values()` to reorder rows by a column.

```python
df.sort_values("score")                  # ascending (default)
df.sort_values("score", ascending=False) # descending
```

By default, Pandas returns a new DataFrame and does not modify `df`. Assign the result or use `inplace=True` when an in-place change is intentional.

## 3. Sort by multiple columns

Pass a list of column names. `ascending` can be a Boolean or a list of Booleans matching the sort keys.

```python
df.sort_values(["department", "score"])
df.sort_values(["department", "score"], ascending=[True, False])
```

The second example sorts departments A–Z and scores high-to-low within each department.

## 4. Missing values and sort options

- `na_position="last"` is the default; use `"first"` to put missing values first.
- `kind="stable"` requests a stable sorting algorithm where supported; it matters when equal keys should retain their relative order.
- `key=` applies a vectorized transformation to each sort key, such as case-folding text.

```python
df.sort_values("score", na_position="first")
df.sort_values("name", key=lambda s: s.str.lower())
```

For reproducibility, use explicit sort keys and document any custom transformations.

## 5. Sort by index

`sort_index()` sorts index labels rather than data values.

```python
df.sort_index()
df.sort_index(axis=1)  # sort column labels
df.sort_index(ascending=False)
```

Use `ignore_index=True` with `sort_values()` when you want a fresh integer index after sorting.

## 6. Ranking values

`rank()` assigns ranks to values. By default, rank 1 is assigned to the smallest value, and ties receive the average rank.

```python
df["score_rank"] = df["score"].rank()
df["best_score_rank"] = df["score"].rank(ascending=False)
```

Common tie methods:
- `average`: average rank for tied values (default).
- `min`: lowest rank in the tied group.
- `max`: highest rank in the tied group.
- `first`: ranks tied values in their existing row order.
- `dense`: like `min`, but the next distinct value follows without gaps.

```python
df["rank_dense"] = df["score"].rank(ascending=False, method="dense")
```

## 7. Ranking within groups

Rank separately within each department with `groupby().rank()`.

```python
df["department_rank"] = (
    df.groupby("department")["score"]
      .rank(ascending=False, method="dense")
)
```

This is useful for top performers within teams, best-selling products within categories, or highest-scoring students within classes.

## 8. Top and bottom records

- `nlargest(n, columns)` returns the top `n` rows by one or more columns.
- `nsmallest(n, columns)` returns the bottom `n` rows.
- `head(n)` returns the first `n` rows of the current order; sort first if “top” has a meaning.

```python
df.nlargest(3, "score")
df.nsmallest(2, "experience")
df.sort_values(["score", "experience"], ascending=[False, True]).head(3)
```

## 9. Custom category order

When categories have a meaningful order (for example, Low, Medium, High), use an ordered categorical rather than alphabetical order.

```python
priority = pd.CategoricalDtype(
    categories=["Low", "Medium", "High"],
    ordered=True
)
df["priority"] = pd.Series(["High", "Low", "Medium"], dtype=priority)
df.sort_values("priority")
```

## 10. Common pitfalls and best practices

1. `sort_values()` sorts data values; `sort_index()` sorts labels.
2. Remember that descending ranking usually needs `ascending=False`.
3. Ties can be handled in several valid ways—choose one that matches the business rule.
4. `first` depends on current row order; sort by a deterministic secondary key first if needed.
5. Missing values are placed last by default in `sort_values()`.
6. Avoid relying on a prior display order; explicitly sort immediately before selecting top rows.
7. Use `reset_index(drop=True)` or `ignore_index=True` when a clean sequential index is useful.
8. Check whether ranks should be global or within each group.

## Quick reference

| Task | Pandas expression |
|---|---|
| Sort one column | `df.sort_values("score")` |
| Descending sort | `df.sort_values("score", ascending=False)` |
| Multiple keys | `df.sort_values(["department", "score"])` |
| Sort column labels | `df.sort_index(axis=1)` |
| Put nulls first | `df.sort_values("score", na_position="first")` |
| Rank highest as 1 | `s.rank(ascending=False)` |
| Dense rank | `s.rank(ascending=False, method="dense")` |
| Rank within group | `df.groupby("department")["score"].rank(ascending=False)` |
| Top N | `df.nlargest(3, "score")` |

## Practice

Complete the 35 questions in [`practice.md`](practice.md), review the worked answers in [`solutions.md`](solutions.md), and run [`examples.ipynb`](examples.ipynb) in Jupyter.
