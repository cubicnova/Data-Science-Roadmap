# Solutions: SQL Aggregations and `GROUP BY`

The queries use the sample `employees` and `sales` tables created in `examples.ipynb`.

## 1. Count all employees
```sql
SELECT COUNT(*) AS employee_count FROM employees;
```

## 2. Count non-NULL salaries
```sql
SELECT COUNT(salary) AS employees_with_salary FROM employees;
```

## 3. Total salaries
```sql
SELECT SUM(salary) AS total_salary FROM employees;
```

## 4. Average salary
```sql
SELECT AVG(salary) AS average_salary FROM employees;
```

## 5. Minimum and maximum salary
```sql
SELECT MIN(salary) AS minimum_salary, MAX(salary) AS maximum_salary FROM employees;
```

## 6. Employees per department
```sql
SELECT department, COUNT(*) AS employee_count FROM employees GROUP BY department;
```

## 7. Average salary per department
```sql
SELECT department, AVG(salary) AS average_salary FROM employees GROUP BY department;
```

## 8. Payroll per department
```sql
SELECT department, SUM(salary) AS total_payroll FROM employees GROUP BY department;
```

## 9. Highest salary per department
```sql
SELECT department, MAX(salary) AS highest_salary FROM employees GROUP BY department;
```

## 10. Count by department and role
```sql
SELECT department, role, COUNT(*) AS employee_count FROM employees GROUP BY department, role;
```

## 11. Average salary by department and role
```sql
SELECT department, role, AVG(salary) AS average_salary FROM employees GROUP BY department, role;
```

## 12. Departments with more than three employees
```sql
SELECT department, COUNT(*) AS employee_count
FROM employees GROUP BY department HAVING COUNT(*) > 3;
```

## 13. Departments with average salary above 50,000
```sql
SELECT department, AVG(salary) AS average_salary
FROM employees GROUP BY department HAVING AVG(salary) > 50000;
```

## 14. Departments with payroll above 150,000
```sql
SELECT department, SUM(salary) AS total_payroll
FROM employees GROUP BY department HAVING SUM(salary) > 150000;
```

## 15. Employees by location
```sql
SELECT location, COUNT(*) AS employee_count FROM employees GROUP BY location;
```

## 16. Departments ordered by employee count
```sql
SELECT department, COUNT(*) AS employee_count
FROM employees GROUP BY department ORDER BY employee_count DESC, department ASC;
```

## 17. Three largest department payrolls
```sql
SELECT department, SUM(salary) AS total_payroll
FROM employees GROUP BY department ORDER BY total_payroll DESC, department ASC LIMIT 3;
```

## 18. Count distinct roles overall
```sql
SELECT COUNT(DISTINCT role) AS distinct_roles FROM employees;
```

## 19. Distinct roles per department
```sql
SELECT department, COUNT(DISTINCT role) AS distinct_roles
FROM employees GROUP BY department;
```

## 20. Average salary among employees earning at least 30,000
```sql
SELECT AVG(salary) AS average_salary FROM employees WHERE salary >= 30000;
```

## 21. Department averages after excluding salaries below 30,000
```sql
SELECT department, AVG(salary) AS average_salary
FROM employees WHERE salary >= 30000 GROUP BY department;
```

## 22. Count employees earning at least 60,000 in each department
```sql
SELECT department, COUNT(*) AS high_salary_count
FROM employees WHERE salary >= 60000 GROUP BY department;
```

## 23. Conditional count of high earners by department
```sql
SELECT department, COUNT(*) AS total_employees,
       SUM(CASE WHEN salary >= 60000 THEN 1 ELSE 0 END) AS high_earners
FROM employees GROUP BY department;
```

## 24. Employee count and average salary, sorted by average
```sql
SELECT department, COUNT(*) AS employee_count, AVG(salary) AS average_salary
FROM employees GROUP BY department ORDER BY average_salary DESC, department ASC;
```

## 25. Lowest and highest salary per department
```sql
SELECT department, MIN(salary) AS lowest_salary, MAX(salary) AS highest_salary
FROM employees GROUP BY department;
```

## 26. Number of sales rows per product
```sql
SELECT product, COUNT(*) AS sales_rows FROM sales GROUP BY product;
```

## 27. Quantity sold per product
```sql
SELECT product, SUM(quantity) AS units_sold FROM sales GROUP BY product;
```

## 28. Revenue per product
```sql
SELECT product, SUM(quantity * unit_price) AS revenue FROM sales GROUP BY product;
```

## 29. Revenue by region
```sql
SELECT region, SUM(quantity * unit_price) AS revenue FROM sales GROUP BY region;
```

## 30. Regions with revenue above 1,000
```sql
SELECT region, SUM(quantity * unit_price) AS revenue
FROM sales GROUP BY region HAVING SUM(quantity * unit_price) > 1000;
```

## 31. Unique customers in sales
```sql
SELECT COUNT(DISTINCT customer_id) AS unique_customers FROM sales;
```
`COUNT(DISTINCT ...)` excludes `NULL` customer IDs.

## 32. Average unit price by product
```sql
SELECT product, AVG(unit_price) AS average_unit_price FROM sales GROUP BY product;
```

## 33. Revenue by product with deterministic tie-breaking
```sql
SELECT product, SUM(quantity * unit_price) AS revenue
FROM sales GROUP BY product ORDER BY revenue DESC, product ASC;
```

## 34. `WHERE` versus `HAVING`

`WHERE` filters individual source rows before groups are created. `HAVING` filters groups after aggregation.

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
WHERE salary >= 30000
GROUP BY department
HAVING AVG(salary) > 50000;
```

The `WHERE` clause excludes salaries below 30,000 before calculating averages. `HAVING` keeps only departments whose resulting average exceeds 50,000.

## 35. `COUNT(*)` versus `COUNT(column)`

- `COUNT(*)` counts every row, including rows where a particular field is `NULL`.
- `COUNT(column)` counts only rows where that column is not `NULL`.

```sql
SELECT COUNT(*) AS all_rows, COUNT(salary) AS rows_with_salary
FROM employees;
```
