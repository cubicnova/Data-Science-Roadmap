# Pandas and NumPy Interview Preparation

A hands-on interview-preparation module for data science, analytics, and machine-learning roles. It covers NumPy arrays and vectorization alongside Pandas data selection, cleaning, aggregation, joins, reshaping, time series, and performance basics.

## Learning objectives

By completing this module, you should be able to:
- Create and manipulate NumPy arrays, inspect shapes and dtypes, and use broadcasting.
- Use vectorized operations, Boolean masks, aggregation, and basic linear algebra.
- Select, filter, sort, and transform Pandas DataFrames safely.
- Handle missing values, duplicates, types, and inconsistent categories.
- Group and aggregate data, merge DataFrames, and reshape data with pivot operations.
- Work with dates, rolling calculations, and basic time-series operations.
- Explain common performance and correctness pitfalls in interviews.

## Setup

Requires Python 3 and the following packages:

```bash
python -m pip install numpy pandas jupyter
jupyter notebook examples.ipynb
```

The notebook uses small synthetic datasets and does not require external downloads.

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | Learning objectives, roadmap, and interview checklist |
| `examples.ipynb` | Runnable NumPy and Pandas examples |
| `practice.md` | Interview questions and coding exercises |
| `solution.md` | Suggested answers with explanations |

## Suggested learning path

1. **NumPy foundations:** arrays, shape, dtype, indexing, slicing, masks.
2. **Vectorization and broadcasting:** avoid unnecessary Python loops.
3. **Numerical operations:** aggregation, axis semantics, sorting, unique values.
4. **Pandas fundamentals:** Series, DataFrames, selection with `loc` and `iloc`.
5. **Data cleaning:** missing values, duplicates, types, string normalization.
6. **Group operations:** `groupby`, `agg`, `transform`, and `value_counts`.
7. **Combining data:** `merge`, `concat`, join validation, and duplicate-key risks.
8. **Reshaping and dates:** `pivot_table`, `melt`, datetime accessors, rolling windows.
9. **Performance and reliability:** vectorization, memory, chained assignment, and tests.

## High-value interview reminders

- A NumPy array is typically homogeneous in dtype; a Pandas DataFrame can have different dtypes across columns.
- In NumPy, `axis=0` reduces down rows (one result per column), while `axis=1` reduces across columns (one result per row) for a 2-D array.
- Broadcasting aligns dimensions from the right. Dimensions are compatible when they are equal or one of them is `1`.
- Prefer `df.loc[rows, columns]` for label-based selection and `df.iloc[rows, columns]` for integer-position selection.
- Avoid chained assignment such as `df[df["score"] > 0]["flag"] = True`; use `.loc` instead.
- `groupby().agg()` reduces groups to summary rows; `groupby().transform()` returns values aligned to the original rows.
- A many-to-many merge can multiply rows. Check key uniqueness and use `validate=` when the expected relationship is known.
- `concat` stacks objects; `merge` matches rows using keys.
- Pandas missing-value checks should generally use `isna()` / `notna()`, not equality comparisons.
- Vectorization often improves speed and clarity, but profile real workloads before optimizing.

## Interview checklist

- [ ] Explain `shape`, `ndim`, `dtype`, `axis`, and broadcasting.
- [ ] Filter a DataFrame using multiple Boolean conditions and parentheses.
- [ ] Choose between `loc`, `iloc`, `at`, and `iat`.
- [ ] Fill or remove missing values without silently changing the wrong rows.
- [ ] Aggregate by group and add a group statistic back to each original row.
- [ ] Merge datasets and validate expected join cardinality.
- [ ] Reshape data between long and wide formats.
- [ ] Create datetime features and rolling summaries.
- [ ] Explain views versus copies at a practical level and avoid chained assignment.
- [ ] Compare a vectorized solution with a loop-based solution.

## Recommended practice routine

Run `examples.ipynb` from top to bottom, then attempt `practice.md` without opening `solution.md`. For each answer, explain the expected shape, index, dtype, missing-value behavior, and any assumptions about duplicate keys.
