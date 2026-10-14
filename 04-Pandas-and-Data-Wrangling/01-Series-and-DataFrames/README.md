# Pandas Series and DataFrames

Pandas is a Python library for working with labeled, tabular data. Its two core structures are `Series` (one-dimensional labeled data) and `DataFrame` (two-dimensional labeled data).

## Learning objectives
- Create Series and DataFrames from lists, dictionaries, and NumPy arrays.
- Understand indexes, columns, shapes, and data types.
- Select rows and columns with `[]`, `.loc`, and `.iloc`.
- Add, rename, and remove columns.
- Inspect data with `head`, `tail`, `info`, and `describe`.
- Handle missing values and choose suitable data types.

## 1. Import Pandas

```python
import pandas as pd
import numpy as np
```

## 2. Series

A `Series` contains values and an index. By default, Pandas assigns integer labels starting at zero.

```python
scores = pd.Series([78, 85, 92], name="score")
print(scores)
print(scores.index)
print(scores.values)
```

Create a Series with custom labels:

```python
scores = pd.Series([78, 85, 92], index=["Asha", "Dev", "Mira"])
print(scores["Dev"])
```

Use `.iloc` for position-based selection and `.loc` for label-based selection.

## 3. DataFrames

A DataFrame is a table with labeled rows and columns. Columns can have different data types.

```python
data = {
    "name": ["Asha", "Dev", "Mira"],
    "age": [21, 22, 20],
    "score": [78, 85, 92],
}
df = pd.DataFrame(data)
```

Create a DataFrame from a list of dictionaries:

```python
df = pd.DataFrame([
    {"name": "Asha", "score": 78},
    {"name": "Dev", "score": 85},
])
```

## 4. Inspecting data

- `df.head(n)` / `df.tail(n)`: preview rows.
- `df.shape`: row and column counts.
- `df.columns`: column labels.
- `df.index`: row labels.
- `df.dtypes`: each column's dtype.
- `df.info()`: compact structure and non-null counts.
- `df.describe()`: summary statistics for numeric columns by default.

## 5. Selecting columns and rows

```python
df["score"]               # Series
df[["name", "score"]]     # DataFrame
df.loc[0, "name"]         # label-based
df.iloc[0, 1]             # integer-position based
df.loc[0:2, ["name", "score"]]
df.iloc[:2, :2]
```

`.loc` uses index labels; `.iloc` uses integer positions. A label slice with `.loc` includes the end label when it exists, whereas a positional slice with `.iloc` excludes the stop position.

## 6. Adding, renaming, and removing columns

```python
df["passed"] = df["score"] >= 40
df = df.rename(columns={"score": "marks"})
df = df.drop(columns=["passed"])
```

`drop` returns a new DataFrame by default. Assign the result if you want to retain the change.

## 7. Indexes

The index labels rows and need not be a simple range. Use `set_index` to make a column the index, and `reset_index` to return to a default integer index.

```python
indexed = df.set_index("name")
restored = indexed.reset_index()
```

Avoid assuming the index is unique unless you have checked it.

## 8. Missing values and dtypes

Use `isna()` / `notna()` to locate missing values. Missing values can affect calculations and may influence inferred dtypes.

```python
df.isna()
df["score"].fillna(df["score"].mean())
df.dropna()
```

Choose a handling strategy based on the data meaning. Do not automatically fill every missing value with zero. Pandas may use nullable dtypes such as `Int64` when integers need to represent missing values.

## 9. Series alignment

Pandas aligns Series by index labels during many operations, not merely by position.

```python
a = pd.Series([10, 20], index=["x", "y"])
b = pd.Series([1, 2], index=["y", "x"])
print(a + b)  # aligned by labels
```

## 10. Best practices
1. Inspect `.shape`, `.columns`, and `.dtypes` before transformations.
2. Prefer `.loc` / `.iloc` when the intended selection should be explicit.
3. Use descriptive column names and consistent data types.
4. Treat missing values deliberately and document assumptions.
5. Be aware that Series operations align by index labels.
6. Use `df.copy()` when you need an independent copy before changing data.
7. Avoid relying on row positions when labels are meaningful.

## Quick reference

| Task | Example |
|---|---|
| Create Series | `pd.Series([1, 2, 3])` |
| Create DataFrame | `pd.DataFrame({"x": [1, 2]})` |
| Preview | `df.head()` |
| Shape / types | `df.shape`, `df.dtypes` |
| Select column(s) | `df["x"]`, `df[["x", "y"]]` |
| Select by label / position | `df.loc[...]`, `df.iloc[...]` |
| Missing-value check | `df.isna()` |
| Rename / drop | `df.rename(...)`, `df.drop(...)` |

## Further reading
- [Pandas getting started](https://pandas.pydata.org/docs/getting_started/intro_tutorials/)
- [Series documentation](https://pandas.pydata.org/docs/reference/api/pandas.Series.html)
- [DataFrame documentation](https://pandas.pydata.org/docs/reference/api/pandas.DataFrame.html)
