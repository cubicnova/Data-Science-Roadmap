# SQL Window Functions — Solutions

These are reference answers. Table and column names may be adapted to your database. Some questions have multiple valid solutions.

## 1. Solution

A window function computes a value across related rows while preserving each row. `GROUP BY` normally collapses rows into one row per group; a window aggregate returns a group-level value alongside each detail row.
## 2. Solution

`OVER()` defines the window of rows for a window function. With no partition or ordering, many window aggregates operate over the full result set.
## 3. Solution

`PARTITION BY` splits rows into independent groups; `ORDER BY` defines row order within a window for ranking and ordered calculations. Neither necessarily sorts the final output.
## 4. Solution

For amounts 80, 80, 60: `ROW_NUMBER` is 1, 2, 3 in tie-break order; `RANK` is 1, 1, 3; `DENSE_RANK` is 1, 1, 2.
## 5. Solution

```sql
SELECT sale_id, sale_date, amount,
       ROW_NUMBER() OVER (ORDER BY sale_date, sale_id) AS rn
FROM sales;
```
## 6. Solution

```sql
SELECT employee_id, department_id, salary,
       RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS salary_rank
FROM employees;
```
## 7. Solution

```sql
WITH ranked AS (
  SELECT employee_id, department_id, salary,
         DENSE_RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS dr
  FROM employees
)
SELECT * FROM ranked WHERE dr <= 3;
```
## 8. Solution

```sql
SELECT sale_id, region, amount,
       SUM(amount) OVER (PARTITION BY region) AS region_total
FROM sales;
```
## 9. Solution

```sql
SELECT sale_id, sale_date, amount,
       SUM(amount) OVER (ORDER BY sale_date, sale_id
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS running_total
FROM sales;
```
## 10. Solution

It makes the frame explicitly include all preceding rows and the current row, avoiding unintended peer/tie behavior or engine-specific defaults.
## 11. Solution

```sql
SELECT salesperson, amount,
       AVG(amount) OVER (PARTITION BY salesperson) AS salesperson_avg
FROM sales;
```
## 12. Solution

```sql
SELECT sale_id, sale_date, amount,
       AVG(amount) OVER (ORDER BY sale_date, sale_id
         ROWS BETWEEN 2 PRECEDING AND CURRENT ROW) AS moving_avg
FROM sales;
```
## 13. Solution

The three-row frame uses at most the current row plus the two immediately preceding ordered rows. A whole-partition average uses every row in the partition for every output row.
## 14. Solution

```sql
SELECT sale_id, amount,
       LAG(amount) OVER (ORDER BY sale_date, sale_id) AS previous_amount
FROM sales;
```
## 15. Solution

```sql
SELECT sale_id, amount,
       LEAD(amount) OVER (ORDER BY sale_date, sale_id) AS next_amount
FROM sales;
```
## 16. Solution

```sql
WITH x AS (
  SELECT sale_id, sale_date, amount,
         LAG(amount) OVER (ORDER BY sale_date, sale_id) AS prev_amount
  FROM sales
)
SELECT *, amount - prev_amount AS absolute_change,
       100.0 * (amount - prev_amount) / NULLIF(prev_amount, 0) AS pct_change
FROM x;
```
## 17. Solution

```sql
SELECT salesperson, sale_date, amount,
       FIRST_VALUE(amount) OVER (
         PARTITION BY salesperson ORDER BY sale_date, sale_id
       ) AS first_amount
FROM sales;
```
## 18. Solution

The default frame for an ordered window often ends at the current row (or current peer group). Thus `LAST_VALUE` may mean last value in that frame, not last value in the partition.
## 19. Solution

```sql
SELECT salesperson, sale_date, amount,
       LAST_VALUE(amount) OVER (
         PARTITION BY salesperson ORDER BY sale_date, sale_id
         ROWS BETWEEN UNBOUNDED PRECEDING AND UNBOUNDED FOLLOWING
       ) AS final_amount
FROM sales;
```
## 20. Solution

```sql
SELECT sale_id, amount,
       100.0 * amount / NULLIF(SUM(amount) OVER (), 0) AS pct_total
FROM sales;
```
## 21. Solution

```sql
SELECT product_id, price,
       NTILE(4) OVER (ORDER BY price) AS price_quartile
FROM products;
```
## 22. Solution

`RANK` assigns ordinal ranks and leaves gaps after ties; `PERCENT_RANK` scales rank position from 0 to 1; `CUME_DIST` gives the fraction of rows whose ordering value is less than or equal to the current row's value.
## 23. Solution

```sql
WITH ranked AS (
  SELECT *, ROW_NUMBER() OVER (
    PARTITION BY region ORDER BY amount DESC, sale_id
  ) AS rn
  FROM sales
)
SELECT * FROM ranked WHERE rn <= 2;
```
## 24. Solution

Window calculations are evaluated after `WHERE` filtering in the logical query processing order. Calculate the window value in a CTE/subquery, then filter the outer query; some engines support `QUALIFY`.
## 25. Solution

```sql
SELECT * FROM (
  SELECT p.*, ROW_NUMBER() OVER (
    PARTITION BY category_id ORDER BY price DESC, product_id
  ) AS rn
  FROM products p
) q
WHERE rn <= 3;
```
## 26. Solution

```sql
SELECT *, ROW_NUMBER() OVER (
  PARTITION BY email ORDER BY created_at, user_id
) AS email_copy_number
FROM users;
```
Rows with `email_copy_number > 1` are later duplicates; decide separately which row to retain.
## 27. Solution

```sql
SELECT sale_id, region, amount,
       amount - AVG(amount) OVER (PARTITION BY region) AS difference_from_region_avg
FROM sales;
```
## 28. Solution

```sql
SELECT sale_id, salesperson,
       COUNT(*) OVER (PARTITION BY salesperson) AS salesperson_sale_count
FROM sales;
```
## 29. Solution

```sql
SELECT sale_id, region, amount,
       MIN(amount) OVER (PARTITION BY region) AS region_min,
       MAX(amount) OVER (PARTITION BY region) AS region_max
FROM sales;
```
## 30. Solution

`ROW_NUMBER` always gives different sequential numbers; `RANK` gives ties equal ranks and skips subsequent rank numbers; `DENSE_RANK` gives ties equal ranks without gaps.
## 31. Solution

No. `PARTITION BY` defines calculation groups, not final presentation order. Add a query-level `ORDER BY region, ...` to sort the result.
## 32. Solution

```sql
SELECT sale_id, sale_date,
       COUNT(*) OVER (ORDER BY sale_date, sale_id
         ROWS BETWEEN UNBOUNDED PRECEDING AND CURRENT ROW) AS cumulative_count
FROM sales;
```
## 33. Solution

```sql
WITH x AS (
  SELECT e.*, DENSE_RANK() OVER (
    PARTITION BY department_id ORDER BY salary DESC
  ) AS salary_level
  FROM employees e
)
SELECT x.*, MAX(CASE WHEN salary_level = 2 THEN salary END)
         OVER (PARTITION BY department_id) AS second_highest_distinct_salary
FROM x;
```
This returns the second-highest distinct salary value for the department, or `NULL` if no second level exists.
## 34. Solution

Examples include top-N products per category, running revenue totals, moving averages, comparing current and previous month performance, deduplication, and percent-of-total analysis.
## 35. Solution

Use deterministic tie-breakers; specify frames explicitly; filter window results in a CTE/subquery or `QUALIFY`; understand tie behavior and NULL handling; add a final `ORDER BY` when output order matters.
