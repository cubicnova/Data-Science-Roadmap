# SQL Window Functions

Window functions calculate a value across a set of related rows while keeping each row in the result. Unlike `GROUP BY`, they do not collapse rows into one row per group.

## Learning objectives

By the end of this module, you should be able to:
- Explain the difference between aggregate and window functions.
- Use `OVER`, `PARTITION BY`, and `ORDER BY`.
- Rank rows with `ROW_NUMBER`, `RANK`, and `DENSE_RANK`.
- Calculate running totals, moving averages, and row-to-row comparisons.
- Use window frames and common analytical patterns.

## 1. Basic syntax

```sql
SELECT
    column_name,
    window_function() OVER (
        PARTITION BY group_column
        ORDER BY sort_column
        ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
    ) AS result
FROM table_name;
```

- `OVER (...)` turns a function into a window calculation.
- `PARTITION BY` divides rows into independent groups; omit it to use the full result set.
- `ORDER BY` defines the order used by ranking and ordered calculations.
- A frame clause specifies which rows around the current row are included. Exact defaults vary by database and function.

## 2. Sample data

Assume a table named `sales`:

| sale_id | salesperson | region | sale_date | amount |
|---:|---|---|---|---:|
| 1 | Asha | North | 2026-01-03 | 100 |
| 2 | Ravi | North | 2026-01-04 | 150 |
| 3 | Asha | North | 2026-01-05 | 200 |
| 4 | Neha | South | 2026-01-03 | 120 |
| 5 | Ravi | South | 2026-01-06 | 180 |

## 3. Ranking functions

```sql
SELECT salesperson, region, amount,
       ROW_NUMBER() OVER (PARTITION BY region ORDER BY amount DESC) AS row_num,
       RANK() OVER (PARTITION BY region ORDER BY amount DESC) AS sales_rank,
       DENSE_RANK() OVER (PARTITION BY region ORDER BY amount DESC) AS dense_sales_rank
FROM sales;
```

- `ROW_NUMBER()` gives each row a unique sequential number.
- `RANK()` assigns equal ranks to ties and leaves gaps after ties.
- `DENSE_RANK()` assigns equal ranks to ties without gaps.
- Include a unique tie-breaker in `ORDER BY` when deterministic row numbering matters.

## 4. Aggregate window functions

### Running total

```sql
SELECT sale_date, amount,
       SUM(amount) OVER (
           ORDER BY sale_date, sale_id
           ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW
       ) AS running_total
FROM sales;
```

### Average per group while retaining rows

```sql
SELECT salesperson, amount,
       AVG(amount) OVER (PARTITION BY salesperson) AS salesperson_avg
FROM sales;
```

### Moving average over the current and previous two rows

```sql
SELECT sale_date, amount,
       AVG(amount) OVER (
           ORDER BY sale_date, sale_id
           ROWS BETWEEN 2 PRECEDING AND CURRENT ROW
       ) AS moving_avg_3_rows
FROM sales;
```

`ROWS` counts physical rows in the ordered partition. `RANGE` groups rows based on ordering values and can treat ties differently. Check your database's support and defaults.

## 5. Offset functions: LAG and LEAD

```sql
SELECT sale_date, amount,
       LAG(amount, 1) OVER (ORDER BY sale_date, sale_id) AS previous_amount,
       LEAD(amount, 1) OVER (ORDER BY sale_date, sale_id) AS next_amount,
       amount - LAG(amount) OVER (ORDER BY sale_date, sale_id) AS change_from_previous
FROM sales;
```

`LAG` accesses an earlier row; `LEAD` accesses a later row. The first `LAG` result and last `LEAD` result are `NULL` unless a default is supplied (where supported).

## 6. First and last values

```sql
SELECT salesperson, sale_date, amount,
       FIRST_VALUE(amount) OVER (
           PARTITION BY salesperson ORDER BY sale_date, sale_id
       ) AS first_sale_amount
FROM sales;
```

`FIRST_VALUE`, `LAST_VALUE`, and `NTH_VALUE` depend on the window frame. To get the final value in a partition with `LAST_VALUE`, explicitly set a frame ending at `UNBOUNDED FOLLOWING` when supported:

```sql
LAST_VALUE(amount) OVER (
    PARTITION BY salesperson ORDER BY sale_date, sale_id
    ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
)
```

## 7. Percentages and distribution

```sql
SELECT salesperson, amount,
       100.0 * amount / SUM(amount) OVER () AS pct_of_all_sales,
       PERCENT_RANK() OVER (ORDER BY amount) AS percent_rank,
       CUME_DIST() OVER (ORDER BY amount) AS cumulative_distribution,
       NTILE(4) OVER (ORDER BY amount DESC) AS quartile_group
FROM sales;
```

`NTILE(n)` divides ordered rows into approximately equal-sized buckets. `PERCENT_RANK` and `CUME_DIST` describe relative position, with definitions that differ.

## 8. Filtering window results

Many databases do not allow a window function directly in `WHERE`, because window calculations are evaluated after filtering. Use a CTE or subquery:

```sql
WITH ranked AS (
    SELECT salesperson, region, amount,
           ROW_NUMBER() OVER (
               PARTITION BY region ORDER BY amount DESC, salesperson
           ) AS rn
    FROM sales
)
SELECT salesperson, region, amount
FROM ranked
WHERE rn <= 2;
```

Some database systems also support `QUALIFY`; syntax varies.

## 9. Common use cases

- Top N records per category.
- Running balances and cumulative totals.
- Month-over-month or row-over-row change.
- Moving averages and rolling metrics.
- Deduplicating records with `ROW_NUMBER`.
- Comparing each item with its group average.
- Percent-of-total and distribution analysis.

## 10. Best practices and pitfalls

1. Make `ORDER BY` deterministic when row order affects the result.
2. Use explicit `ROWS` frames for running totals and row-based moving windows.
3. Remember that `PARTITION BY` does not sort output; add a query-level `ORDER BY` for presentation.
4. Do not confuse `RANK` with `ROW_NUMBER`; ties behave differently.
5. Window functions preserve rows; ordinary grouped aggregates usually reduce rows.
6. Check how your SQL engine handles `NULL`, frames, `FILTER`, and advanced functions.
7. Avoid relying on implicit frame defaults for `LAST_VALUE` or cumulative calculations.
8. Use CTEs/subqueries to filter on calculated window values when `QUALIFY` is unavailable.

## Quick reference

| Function | Purpose |
|---|---|
| `ROW_NUMBER` | Unique sequential row number |
| `RANK` | Ranking with gaps after ties |
| `DENSE_RANK` | Ranking without gaps after ties |
| `SUM`, `AVG`, `COUNT`, `MIN`, `MAX` with `OVER` | Windowed aggregates |
| `LAG`, `LEAD` | Read previous/next row values |
| `FIRST_VALUE`, `LAST_VALUE`, `NTH_VALUE` | Select values within a window |
| `NTILE` | Divide ordered rows into buckets |
| `PERCENT_RANK`, `CUME_DIST` | Relative distribution measures |

## Practice

Complete the 35 exercises in [`practice.md`](practice.md), check your work against [`solutions.md`](solutions.md), and run the examples in [`examples.ipynb`](examples.ipynb).
