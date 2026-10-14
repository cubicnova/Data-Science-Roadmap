# SQL Aggregations and `GROUP BY`

Learn to summarize rows with aggregate functions, group records into categories, and filter groups using `HAVING`.

## Learning objectives
- Use `COUNT`, `SUM`, `AVG`, `MIN`, and `MAX`.
- Understand how aggregate functions treat `NULL`.
- Group by one or multiple columns.
- Filter rows with `WHERE` and groups with `HAVING`.
- Sort aggregate results and use aliases.
- Use conditional aggregation for reports and KPIs.

## 1. Aggregate functions

Aggregate functions calculate a single value from multiple rows.

```sql
SELECT COUNT(*) AS employee_count,
       SUM(salary) AS total_salary,
       AVG(salary) AS average_salary,
       MIN(salary) AS lowest_salary,
       MAX(salary) AS highest_salary
FROM employees;
```

| Function | Purpose |
|---|---|
| `COUNT(*)` | Counts every row |
| `COUNT(column)` | Counts rows where the column is not `NULL` |
| `SUM(column)` | Adds non-`NULL` values |
| `AVG(column)` | Calculates the average of non-`NULL` values |
| `MIN(column)` | Finds the smallest value |
| `MAX(column)` | Finds the largest value |

`COUNT(*)` counts rows even when a column is `NULL`. Other aggregates generally ignore `NULL`. For no non-`NULL` inputs, `SUM`, `AVG`, `MIN`, and `MAX` generally return `NULL`; `COUNT` returns zero.

## 2. Grouping rows

`GROUP BY` puts rows with the same value or combination of values into groups. Aggregate functions calculate a result for each group.

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department;
```

Group by multiple columns for a more detailed breakdown:

```sql
SELECT department, role, AVG(salary) AS average_salary
FROM employees
GROUP BY department, role;
```

For portable SQL, every selected expression that is not aggregated should generally appear in `GROUP BY`.

## 3. `WHERE` versus `HAVING`

- `WHERE` filters individual rows before grouping.
- `HAVING` filters groups after aggregation.

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
WHERE salary >= 30000
GROUP BY department
HAVING AVG(salary) > 50000
ORDER BY average_salary DESC;
```

Employees earning less than 30,000 are removed before grouping. Then only departments whose remaining average exceeds 50,000 are returned.

## 4. Sorting aggregate results

```sql
SELECT department, SUM(salary) AS payroll
FROM employees
GROUP BY department
ORDER BY payroll DESC;
```

To return only the first few groups, use `LIMIT`:

```sql
SELECT department, COUNT(*) AS employee_count
FROM employees
GROUP BY department
ORDER BY employee_count DESC
LIMIT 3;
```

## 5. Conditional aggregation

Use `CASE` inside an aggregate to calculate multiple metrics in one query.

```sql
SELECT department,
       COUNT(*) AS total_employees,
       SUM(CASE WHEN salary >= 60000 THEN 1 ELSE 0 END) AS high_earners
FROM employees
GROUP BY department;
```

## 6. Aggregating sales

```sql
SELECT product, SUM(quantity * unit_price) AS revenue
FROM sales
GROUP BY product
ORDER BY revenue DESC;
```

Revenue, units sold, and number of sales rows are different metrics. Make sure your query matches the question being asked.

## 7. Conceptual query processing order

1. `FROM` — choose source tables.
2. `WHERE` — filter source rows.
3. `GROUP BY` — form groups.
4. Aggregate functions — calculate group metrics.
5. `HAVING` — filter groups.
6. `SELECT` — produce output expressions.
7. `ORDER BY` — sort output.
8. `LIMIT` — restrict returned rows.

This is a conceptual model, not a promise about the engine's physical execution plan.

## Common mistakes
- Writing `WHERE COUNT(*) > 2`; use `HAVING COUNT(*) > 2`.
- Selecting a regular column that is neither grouped nor aggregated.
- Using `COUNT(column)` when you intended to count every row.
- Assuming `AVG` treats `NULL` as zero; it ignores `NULL`.
- Grouping only by department when the question requires department and role.
- Confusing row count with distinct customer count.

## Best practices
- Use clear aliases such as `employee_count`, `average_salary`, and `total_revenue`.
- Make the grouping grain explicit: one row per department, product, month, or department-role combination.
- Handle `NULL` deliberately; replace it with zero only when that matches the business meaning.
- Add tie-breakers to `ORDER BY` when stable ordering matters.
- Validate important totals against the source data.
- Test queries on small, hand-checkable datasets before applying them to larger tables.

## Practice workflow
1. Attempt the questions in `practice.md`.
2. Run `examples.ipynb` from top to bottom.
3. Check your answers with `solutions.md`.
4. Modify the sample data and observe the results.

Examples use SQLite-compatible SQL and can be adapted to PostgreSQL and MySQL with minor syntax differences.
