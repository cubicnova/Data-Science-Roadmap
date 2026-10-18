# SQL Interview Preparation

A practical SQL interview-preparation module for data science, data analytics, and machine-learning roles. It progresses from query fundamentals to joins, aggregation, window functions, CTEs, and interview-style problem solving.

## Learning objectives

By the end of this module, you should be able to:
- Write readable `SELECT`, `WHERE`, `ORDER BY`, and `LIMIT` queries.
- Aggregate data using `GROUP BY` and filter aggregates with `HAVING`.
- Choose correctly among `INNER`, `LEFT`, `RIGHT`, and self joins.
- Work confidently with `NULL`, `CASE`, date functions, and string functions.
- Use subqueries, CTEs, and window functions such as `ROW_NUMBER`, `RANK`, and `LAG`.
- Explain keys, constraints, indexes, normalization, and query performance basics.
- Solve common interview prompts involving duplicates, top-N records, ranking, and retention-style summaries.

## Recommended setup

Use SQLite for the runnable examples in `examples.ipynb`; it requires no database server. SQL syntax varies by database engine, so check date functions, `LIMIT`/`TOP`, and some window-function details when interviewing for PostgreSQL, MySQL, SQL Server, or Oracle.

Requirements:
- Python 3
- `pandas`
- `ipython` / Jupyter Notebook

Install optional notebook dependencies:

```bash
python -m pip install pandas jupyter
jupyter notebook examples.ipynb
```

## Folder contents

| File | Purpose |
|---|---|
| `README.md` | Learning path, concepts, and interview checklist |
| `examples.ipynb` | Runnable SQLite examples with sample tables and queries |
| `practice.md` | Interview questions and hands-on SQL tasks |
| `solution.md` | Suggested answers with explanations |

## Learning path

1. **Query basics:** `SELECT`, aliases, `DISTINCT`, filtering, sorting, limits.
2. **Aggregation:** `COUNT`, `SUM`, `AVG`, `MIN`, `MAX`, `GROUP BY`, `HAVING`.
3. **Joins:** inner/outer joins, self joins, join keys, duplicate-row pitfalls.
4. **Conditional and missing data:** `CASE`, `COALESCE`, `NULL` semantics.
5. **Subqueries and CTEs:** breaking a query into understandable steps.
6. **Window functions:** ranking, running totals, previous-row comparisons.
7. **Data modeling:** primary/foreign keys, constraints, normalization, indexes.
8. **Performance and communication:** inspect filters and joins, avoid unnecessary columns, explain assumptions, and validate row counts.

## Core interview reminders

- `WHERE` filters rows before grouping; `HAVING` filters groups after aggregation.
- `COUNT(*)` counts rows; `COUNT(column)` ignores `NULL` in that column; `COUNT(DISTINCT column)` counts distinct non-NULL values.
- A `LEFT JOIN` preserves every left-side row. Conditions on the right table in `WHERE` can unintentionally remove unmatched rows; sometimes the condition belongs in `ON`.
- `NULL` is not equal to anything, including another `NULL`; use `IS NULL` / `IS NOT NULL`.
- `ROW_NUMBER()` assigns a unique sequence, while `RANK()` leaves gaps after ties and `DENSE_RANK()` does not.
- A window function preserves row-level output, whereas `GROUP BY` usually collapses rows.
- Indexes can speed up reads but require storage and can slow inserts/updates; verify performance with the database's query plan.

## Suggested study routine

1. Read a topic and type the query yourself.
2. Run the notebook and inspect both the SQL and returned rows.
3. Solve `practice.md` without looking at `solution.md`.
4. Explain the query out loud: expected grain, joins, filters, NULL behavior, and edge cases.
5. Re-run solutions with duplicate values and missing data to test assumptions.

## Interview checklist

- [ ] Can I explain the difference between `WHERE` and `HAVING`?
- [ ] Can I find duplicate values and keep one row per entity?
- [ ] Can I return the second-highest salary and handle ties?
- [ ] Can I find employees who have no matching department?
- [ ] Can I rank records within each group?
- [ ] Can I calculate a running total or compare with the previous row?
- [ ] Can I explain primary keys, foreign keys, indexes, and normalization?
- [ ] Can I identify why a join unexpectedly multiplies rows?

## Dialect note

The notebook uses SQLite-compatible SQL. In production interviews, confirm the SQL dialect and date/time requirements before writing a final answer.
