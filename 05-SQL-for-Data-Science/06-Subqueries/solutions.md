# Solutions: SQL Subqueries

Use the fictional sample tables in `examples.ipynb`. Question 22 assumes `order_items(product_id)` exists.

## 1. Above overall average

```sql
SELECT employee_name, salary FROM employees WHERE salary > (SELECT AVG(salary) FROM employees);
```

## 2. Maximum salary

```sql
SELECT employee_name, salary FROM employees WHERE salary = (SELECT MAX(salary) FROM employees);
```

## 3. Employees in departments located in Indore

```sql
SELECT employee_name FROM employees WHERE department_id IN (SELECT department_id FROM departments WHERE location = 'Indore');
```

## 4. Departments with employees

```sql
SELECT d.department_name FROM departments d WHERE EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.department_id);
```

## 5. Departments without employees

```sql
SELECT d.department_name FROM departments d WHERE NOT EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.department_id);
```

## 6. Above own department average

```sql
SELECT e.employee_name, e.salary FROM employees e WHERE e.salary > (SELECT AVG(e2.salary) FROM employees e2 WHERE e2.department_id = e.department_id);
```

## 7. Employee count in SELECT

```sql
SELECT d.department_name, (SELECT COUNT(*) FROM employees e WHERE e.department_id = d.department_id) AS employee_count FROM departments d;
```

## 8. Derived table average above 50000

```sql
SELECT department_id, average_salary FROM (SELECT department_id, AVG(salary) AS average_salary FROM employees GROUP BY department_id) AS department_stats WHERE average_salary > 50000;
```

## 9. More than every salary in department 2

```sql
SELECT employee_name, salary FROM employees WHERE salary > (SELECT MAX(salary) FROM employees WHERE department_id = 2);
```

## 10. More than at least one salary in department 2

```sql
SELECT employee_name, salary FROM employees WHERE salary > (SELECT MIN(salary) FROM employees WHERE department_id = 2);
```

## 11. Orders above average

```sql
SELECT order_id, amount FROM orders WHERE amount > (SELECT AVG(amount) FROM orders);
```

## 12. Customers with orders

```sql
SELECT c.customer_id, c.customer_name FROM customers c WHERE EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

## 13. Customers without orders

```sql
SELECT c.customer_id, c.customer_name FROM customers c WHERE NOT EXISTS (SELECT 1 FROM orders o WHERE o.customer_id = c.customer_id);
```

## 14. Departments with high earner

```sql
SELECT d.department_name FROM departments d WHERE EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.department_id AND e.salary >= 70000);
```

## 15. Above Indore average

```sql
SELECT employee_name, salary FROM employees WHERE salary > (SELECT AVG(salary) FROM employees WHERE location = 'Indore');
```

## 16. Second-highest distinct salary

```sql
SELECT employee_name, salary FROM employees WHERE salary = (SELECT MAX(salary) FROM employees WHERE salary < (SELECT MAX(salary) FROM employees));
```

## 17. Employees sharing salary

```sql
SELECT e.employee_name, e.salary FROM employees e WHERE EXISTS (SELECT 1 FROM employees e2 WHERE e2.salary = e.salary AND e2.employee_id <> e.employee_id);
```

## 18. Above Asha's salary

```sql
SELECT employee_name, salary FROM employees WHERE salary > (SELECT salary FROM employees WHERE employee_name = 'Asha');
```

## 19. Maximum-amount orders

```sql
SELECT order_id, amount FROM orders WHERE amount = (SELECT MAX(amount) FROM orders);
```

## 20. Payroll above average department payroll

```sql
SELECT department_id, total_payroll FROM (SELECT department_id, SUM(salary) AS total_payroll FROM employees GROUP BY department_id) p WHERE total_payroll > (SELECT AVG(total_payroll) FROM (SELECT SUM(salary) AS total_payroll FROM employees GROUP BY department_id) x);
```

## 21. Below department maximum

```sql
SELECT e.employee_name, e.salary FROM employees e WHERE e.salary < (SELECT MAX(e2.salary) FROM employees e2 WHERE e2.department_id = e.department_id);
```

## 22. Products never ordered

```sql
SELECT p.product_id, p.product_name FROM products p WHERE NOT EXISTS (SELECT 1 FROM order_items oi WHERE oi.product_id = p.product_id);
```

## 23. Scalar vs multi-row

A scalar subquery returns one value. A multi-row subquery can return several rows and is commonly used with IN or EXISTS.

## 24. Correlated vs non-correlated

A correlated subquery references a column from the outer query and depends on the current row; a non-correlated subquery can run independently.

## 25. IN rewritten as EXISTS

```sql
SELECT d.department_name FROM departments d WHERE EXISTS (SELECT 1 FROM employees e WHERE e.department_id = d.department_id);
```

## 26. NOT EXISTS vs NOT IN

A NULL returned by a NOT IN subquery can make the predicate unknown. NOT EXISTS directly checks for a matching row and avoids that NULL trap.

## 27. Department average in SELECT

```sql
SELECT e.employee_name, e.salary, (SELECT AVG(e2.salary) FROM employees e2 WHERE e2.department_id = e.department_id) AS department_average FROM employees e;
```

## 28. Departments with more than two employees

```sql
SELECT department_id FROM employees GROUP BY department_id HAVING COUNT(*) > 2;
```

## 29. Same department as Asha

```sql
SELECT employee_name FROM employees WHERE department_id = (SELECT department_id FROM employees WHERE employee_name = 'Asha');
```

## 30. Customers with total orders above 500

```sql
SELECT c.customer_id, c.customer_name FROM customers c WHERE (SELECT COALESCE(SUM(o.amount), 0) FROM orders o WHERE o.customer_id = c.customer_id) > 500;
```

## 31. Above median salary

```sql
WITH ranked AS (SELECT salary, ROW_NUMBER() OVER (ORDER BY salary) rn, COUNT(*) OVER () n FROM employees WHERE salary IS NOT NULL) SELECT employee_name, salary FROM employees WHERE salary > (SELECT AVG(salary) FROM ranked WHERE rn IN ((n + 1) / 2, (n + 2) / 2));
```

## 32. Departments with no related orders

```sql
SELECT d.department_id, d.department_name FROM departments d WHERE NOT EXISTS (SELECT 1 FROM employees e JOIN orders o ON o.employee_id = e.employee_id WHERE e.department_id = d.department_id);
```

## 33. Join vs subquery

Both are valid. Use EXISTS to test for related rows, joins when you need columns from both tables, and inspect the query plan when performance matters.

## 34. Investigate slow correlated query

Inspect the query plan, check indexes on correlation/filter columns, measure with representative data, and compare an equivalent join or grouped query.

## 35. Above department average, sorted

```sql
SELECT e.employee_name, e.salary, e.department_id FROM employees e WHERE e.salary > (SELECT AVG(e2.salary) FROM employees e2 WHERE e2.department_id = e.department_id) ORDER BY e.salary DESC;
```

