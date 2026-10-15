# Solutions: SQL CTEs

Use the sample tables created in `examples.ipynb`. A few questions introduce assumed `sales` or `customers` tables; the notebook focuses on the core CTE patterns.

## 1. High earners

```sql
WITH high_earners AS (
  SELECT employee_name, department_id, salary FROM employees WHERE salary >= 60000
)
SELECT * FROM high_earners ORDER BY salary DESC;
```

## 2. Average salary by department

```sql
WITH department_avg AS (
  SELECT department_id, AVG(salary) AS average_salary
  FROM employees GROUP BY department_id
)
SELECT * FROM department_avg;
```

## 3. Departments with average salary above 60000

```sql
WITH department_avg AS (
  SELECT department_id, AVG(salary) AS average_salary
  FROM employees GROUP BY department_id
)
SELECT * FROM department_avg WHERE average_salary > 60000;
```

## 4. Department employee counts with names

```sql
WITH employee_counts AS (
  SELECT department_id, COUNT(*) AS employee_count
  FROM employees GROUP BY department_id
)
SELECT d.department_name, COALESCE(ec.employee_count, 0) AS employee_count
FROM departments d LEFT JOIN employee_counts ec ON ec.department_id = d.department_id;
```

## 5. Payroll by department

```sql
WITH payroll AS (
  SELECT department_id, SUM(salary) AS total_payroll
  FROM employees GROUP BY department_id
)
SELECT * FROM payroll;
```

## 6. Two CTEs for high-average departments

```sql
WITH department_avg AS (
  SELECT department_id, AVG(salary) AS average_salary FROM employees GROUP BY department_id
),
high_departments AS (
  SELECT * FROM department_avg WHERE average_salary > 60000
)
SELECT d.department_name, h.average_salary
FROM high_departments h JOIN departments d ON d.department_id = h.department_id;
```

## 7. Employees above overall average

```sql
WITH overall_avg AS (
  SELECT AVG(salary) AS avg_salary FROM employees
)
SELECT employee_name, salary FROM employees
WHERE salary > (SELECT avg_salary FROM overall_avg);
```

## 8. Employees above department average

```sql
WITH department_avg AS (
  SELECT department_id, AVG(salary) AS avg_salary FROM employees GROUP BY department_id
)
SELECT e.employee_name, e.salary, e.department_id
FROM employees e JOIN department_avg d ON d.department_id = e.department_id
WHERE e.salary > d.avg_salary;
```

## 9. Order counts per customer including zero

```sql
WITH order_counts AS (
  SELECT customer_id, COUNT(*) AS order_count FROM orders GROUP BY customer_id
)
SELECT c.customer_id, c.customer_name, COALESCE(o.order_count, 0) AS order_count
FROM customers c LEFT JOIN order_counts o ON o.customer_id = c.customer_id;
```

## 10. Total order amount per customer

```sql
WITH customer_totals AS (
  SELECT customer_id, SUM(amount) AS total_amount FROM orders GROUP BY customer_id
)
SELECT * FROM customer_totals;
```

## 11. Customers spending above 500

```sql
WITH customer_totals AS (
  SELECT customer_id, SUM(amount) AS total_amount FROM orders GROUP BY customer_id
)
SELECT c.customer_name, t.total_amount
FROM customer_totals t JOIN customers c ON c.customer_id = t.customer_id
WHERE t.total_amount > 500;
```

## 12. Revenue by product

```sql
WITH product_revenue AS (
  SELECT product, SUM(quantity * unit_price) AS revenue FROM sales GROUP BY product
)
SELECT * FROM product_revenue ORDER BY revenue DESC;
```

## 13. Department names and employee counts

```sql
WITH employee_counts AS (
  SELECT department_id, COUNT(*) AS employee_count FROM employees GROUP BY department_id
)
SELECT d.department_name, ec.employee_count
FROM departments d JOIN employee_counts ec ON ec.department_id = d.department_id;
```

## 14. Departments with more than two employees

```sql
WITH employee_counts AS (
  SELECT department_id, COUNT(*) AS employee_count FROM employees GROUP BY department_id
),
larger_departments AS (
  SELECT department_id FROM employee_counts WHERE employee_count > 2
)
SELECT d.department_name FROM departments d JOIN larger_departments x ON x.department_id = d.department_id;
```

## 15. Filtered subset CTE

```sql
WITH indore_staff AS (
  SELECT employee_name, department_id, salary FROM employees WHERE location = 'Indore'
)
SELECT * FROM indore_staff;
```

## 16. Highest salary per department

```sql
WITH salary_max AS (
  SELECT department_id, MAX(salary) AS highest_salary FROM employees GROUP BY department_id
)
SELECT * FROM salary_max;
```

## 17. Employees at department maximum

```sql
WITH salary_max AS (
  SELECT department_id, MAX(salary) AS highest_salary FROM employees GROUP BY department_id
)
SELECT e.employee_name, e.salary, e.department_id
FROM employees e JOIN salary_max m ON m.department_id = e.department_id AND m.highest_salary = e.salary;
```

## 18. Average order amount by status

```sql
WITH status_avg AS (
  SELECT status, AVG(amount) AS average_amount FROM orders GROUP BY status
)
SELECT * FROM status_avg;
```

## 19. Statuses with total amount above 500

```sql
WITH status_totals AS (
  SELECT status, SUM(amount) AS total_amount FROM orders GROUP BY status
)
SELECT * FROM status_totals WHERE total_amount > 500;
```

## 20. Integers from 1 to 10

```sql
WITH RECURSIVE numbers(n) AS (
  SELECT 1 UNION ALL SELECT n + 1 FROM numbers WHERE n < 10
)
SELECT n FROM numbers;
```

## 21. First 10 multiples of 3

```sql
WITH RECURSIVE multiples(n) AS (
  SELECT 3 UNION ALL SELECT n + 3 FROM multiples WHERE n < 30
)
SELECT n FROM multiples;
```

## 22. Anchor and recursive parts

The anchor query seeds the first row(s). The recursive term references the CTE to generate subsequent rows. A termination condition is essential to prevent unbounded recursion.

## 23. CTE versus derived table

Both are statement-scoped query expressions. A CTE is named in a WITH clause and can be easier to reuse within the statement; a derived table is nested directly in FROM.

## 24. CTE versus temporary table

A CTE exists for one statement. A temporary table is a database object that can generally be reused across multiple statements in its session, subject to the database.

## 25. Monthly revenue and running total

```sql
WITH monthly_revenue AS (
  SELECT substr(order_date, 1, 7) AS month, SUM(amount) AS revenue
  FROM orders GROUP BY substr(order_date, 1, 7)
)
SELECT month, revenue, SUM(revenue) OVER (ORDER BY month) AS running_revenue
FROM monthly_revenue ORDER BY month;
```

## 26. Top three employees

```sql
WITH ranked_employees AS (
  SELECT employee_name, salary, ROW_NUMBER() OVER (ORDER BY salary DESC) AS rn
  FROM employees WHERE salary IS NOT NULL
)
SELECT employee_name, salary FROM ranked_employees WHERE rn <= 3;
```

## 27. Rank employees within departments

```sql
WITH ranked AS (
  SELECT employee_name, department_id, salary,
         RANK() OVER (PARTITION BY department_id ORDER BY salary DESC) AS salary_rank
  FROM employees WHERE salary IS NOT NULL
)
SELECT * FROM ranked ORDER BY department_id, salary_rank;
```

## 28. Departments without employees

```sql
WITH employee_departments AS (
  SELECT DISTINCT department_id FROM employees
)
SELECT d.department_name FROM departments d
LEFT JOIN employee_departments e ON e.department_id = d.department_id
WHERE e.department_id IS NULL;
```

## 29. Customers with a completed order

```sql
WITH completed_customers AS (
  SELECT DISTINCT customer_id FROM orders WHERE status = 'completed'
)
SELECT c.customer_id, c.customer_name
FROM customers c JOIN completed_customers x ON x.customer_id = c.customer_id;
```

## 30. Customers with no orders

```sql
WITH ordering_customers AS (
  SELECT DISTINCT customer_id FROM orders
)
SELECT c.customer_id, c.customer_name
FROM customers c LEFT JOIN ordering_customers o ON o.customer_id = c.customer_id
WHERE o.customer_id IS NULL;
```

## 31. Payroll versus average department payroll

```sql
WITH payroll AS (
  SELECT department_id, SUM(salary) AS total_payroll FROM employees GROUP BY department_id
),
average_payroll AS (
  SELECT AVG(total_payroll) AS avg_payroll FROM payroll
)
SELECT p.department_id, p.total_payroll, a.avg_payroll
FROM payroll p CROSS JOIN average_payroll a
WHERE p.total_payroll > a.avg_payroll;
```

## 32. Deduplicate city names

```sql
WITH cities(city) AS (
  SELECT 'Indore' UNION ALL SELECT 'Pune' UNION ALL SELECT 'Indore' UNION ALL SELECT 'Mumbai'
)
SELECT DISTINCT city FROM cities ORDER BY city;
```

## 33. Two CTE syntax mistakes

Common errors include forgetting the comma between CTE definitions and omitting the final SELECT/INSERT/UPDATE/DELETE statement. Add the comma, then follow the WITH clause with the statement that consumes the CTE.

## 34. Why a CTE is not automatically faster

The optimizer may inline, transform, or materialize a CTE depending on the database and query. Performance depends on data, indexes, query plan, and engine; benchmark equivalent alternatives.

## 35. Two CTEs for employees above department average

```sql
WITH department_avg AS (
  SELECT department_id, AVG(salary) AS avg_salary FROM employees GROUP BY department_id
),
above_avg AS (
  SELECT e.employee_name, e.salary, e.department_id
  FROM employees e JOIN department_avg d ON d.department_id = e.department_id
  WHERE e.salary > d.avg_salary
)
SELECT * FROM above_avg ORDER BY salary DESC;
```

