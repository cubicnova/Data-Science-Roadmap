# Pandas Indexing and Filtering

Indexing and filtering let you select the rows and columns needed for analysis. Pandas supports label-based selection, position-based selection, Boolean masks, query expressions, and index management.

## Learning objectives
- Select rows and columns with `.loc` and `.iloc`.
- Understand label slices versus positional slices.
- Filter rows using Boolean conditions and combined masks.
- Use `isin`, `between`, `isna`, `notna`, `query`, and `where`.
- Select rows by index labels and manage indexes.
- Avoid common chained-assignment and operator-precedence mistakes.

## 1. Sample data

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "name": ["Asha", "Dev", "Mira", "Ravi"],
    "age": [21, 24, 20, 27],
    "score": [78, 91, 85, 62],
    "city": ["Indore", "Pune", "Indore", "Bhopal"],
})
```

## 2. Select columns

```python
df["score"]              # Series
df[["name", "score"]]    # DataFrame
```

A single column selection returns a Series; a list of column names returns a DataFrame.

## 3. Label-based selection with `.loc`

`.loc` selects by index label and column label.

```python
df.loc[0, "name"]
df.loc[1:3, ["name", "score"]]
df.loc[df["score"] >= 80, ["name", "score"]]
```

Label-based slices include the stop label when it exists. If the index labels are strings or dates, use those labels rather than assuming row positions.

## 4. Position-based selection with `.iloc`

`.iloc` selects by zero-based integer position.

```python
df.iloc[0, 0]      # first row, first column
df.iloc[:2, :3]    # first two rows, first three columns
df.iloc[-1]        # final row
```

Positional slices exclude the stop position.

## 5. Boolean filtering

A comparison such as `df["score"] >= 80` creates a Boolean Series. Use it to select matching rows.

```python
df[df["score"] >= 80]
df[(df["age"] >= 21) & (df["city"] == "Indore")]
```

Use `&` for AND, `|` for OR, and `~` for NOT. Put each comparison in parentheses; do not use Python's `and` / `or` with Series.

## 6. Common filter helpers

```python
df[df["city"].isin(["Indore", "Pune"])]
df[df["age"].between(20, 25)]
df[df["score"].notna()]
df[df["score"].isna()]
```

`isin` tests membership, `between` tests an inclusive range by default, and `isna` / `notna` identify missing values.

## 7. `query` and `where`

```python
df.query("age >= 21 and score >= 80")
df["score"].where(df["score"] >= 80)
```

`query` provides a readable expression syntax for many filters. `where` keeps values where a condition is true and replaces other values with missing values by default.

## 8. Index management

```python
by_name = df.set_index("name")
by_name.loc["Asha"]
df.reset_index()
```

Index labels can be duplicated. Use `index.is_unique` to check uniqueness. Use `.loc` for labels even when labels look numeric; `.iloc` is always positional.

## 9. Safely updating filtered data

Avoid chained assignment such as `df[df["score"] < 70]["score"] = 0`. It can update a temporary result rather than the original DataFrame.

Use `.loc`:

```python
df.loc[df["score"] < 70, "score"] = 0
```

If you need an independent filtered result, use `.copy()`:

```python
low_scores = df.loc[df["score"] < 70].copy()
```

## 10. Best practices
1. Choose `.loc` for labels and `.iloc` for positions.
2. Parenthesize each comparison in compound Boolean masks.
3. Check the index before assuming it is a sequential integer range.
4. Use `.copy()` when you intend to modify a filtered subset independently.
5. Use `.isna()` / `.notna()` rather than comparing to `None` or `np.nan`.
6. Check result shape and row count after filtering.
7. Keep filters readable by naming intermediate masks for complex conditions.

## Quick reference

| Task | Example |
|---|---|
| Select columns | `df[["name", "score"]]` |
| Label-based selection | `df.loc[rows, columns]` |
| Position-based selection | `df.iloc[row_positions, col_positions]` |
| AND / OR / NOT | `&`, `|`, `~` |
| Membership | `df["city"].isin([...])` |
| Inclusive range | `df["age"].between(20, 25)` |
| Missing values | `df["score"].isna()` |
| Expression filter | `df.query("score >= 80")` |
| Safe update | `df.loc[mask, "column"] = value` |

## Further reading
- [Indexing and selecting data](https://pandas.pydata.org/docs/user_guide/indexing.html)
- [DataFrame.loc](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.loc.html)
- [DataFrame.iloc](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.iloc.html)
