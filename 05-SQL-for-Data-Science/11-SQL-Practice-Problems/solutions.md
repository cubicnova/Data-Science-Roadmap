# SQL Practice Problems — Solutions

These are reference solutions for the 40 exercises in `practice.md`. They assume the table and column names defined in that file. If you use a different database engine, adjust date functions where noted.

## Section A — SELECT, Filtering, Sorting, and Limits

### 1. Display every column for all customers

```sql
SELECT *
FROM customers;
```

`*` returns all columns. In production queries, selecting only the required columns is usually clearer and more efficient.

### 2. Display selected customer columns

```sql
SELECT customer_name, email, city
FROM customers;
```

### 3. Find customers in Indore

```sql
SELECT customer_id, customer_name, email, city
FROM customers
WHERE city = 'Indore';
```

Text comparisons may be case-sensitive depending on the database and collation.

### 4. Products priced above 1,000

```sql
SELECT product_id, product_name, list_price
FROM products
WHERE list_price > 1000;
```

### 5. Pending or cancelled orders

```sql
SELECT order_id, customer_id, order_date, status, total_amount
FROM orders
WHERE status IN ('Pending', 'Cancelled');
```

`IN` is concise for checking a value against a list.

### 6. Electronics with stock below 20

```sql
SELECT product_id, product_name, category, stock_quantity
FROM products
WHERE category = 'Electronics'
  AND stock_quantity < 20;
```

Both conditions must be true because they are joined with `AND`.

### 7. Products from highest to lowest price

```sql
SELECT product_id, product_name, list_price
FROM products
ORDER BY list_price DESC;
```

### 8. Five most expensive products

```sql
SELECT product_id, product_name, list_price
FROM products
ORDER BY list_price DESC
LIMIT 5;
```

`LIMIT` works in PostgreSQL, MySQL, and SQLite. SQL Server commonly uses `TOP (5)` or `OFFSET ... FETCH`.

### 9. Customers with missing email addresses

```sql
SELECT customer_id, customer_name, email
FROM customers
WHERE email IS NULL;
```

Do not use `email = NULL`; SQL nulls require `IS NULL`.

### 10. Customer names beginning with A

```sql
SELECT customer_id, customer_name
FROM customers
WHERE customer_name LIKE 'A%';
```

`%` matches zero or more characters. Case sensitivity depends on the database.

## Section B — Aggregation and Grouping

### 11. Count customers

```sql
SELECT COUNT(*) AS customer_count
FROM customers;
```

### 12. Average product list price

```sql
SELECT AVG(list_price) AS average_list_price
FROM products;
```

`AVG` ignores null values.

### 13. Total order value

```sql
SELECT SUM(total_amount) AS total_order_value
FROM orders;
```

This sums all order statuses. If the business definition of sales excludes pending or cancelled orders, add an appropriate status filter.

### 14. Number of orders per status

```sql
SELECT status, COUNT(*) AS order_count
FROM orders
GROUP BY status
ORDER BY order_count DESC;
```

### 15. Total order value per status

```sql
SELECT status, SUM(total_amount) AS total_amount
FROM orders
GROUP BY status
ORDER BY total_amount DESC;
```

This reports the total recorded order amount for each status, not necessarily recognised revenue.

### 16. Categories containing more than five products

```sql
SELECT category, COUNT(*) AS product_count
FROM products
GROUP BY category
HAVING COUNT(*) > 5;
```

`WHERE` filters rows before grouping; `HAVING` filters grouped results.

### 17. Average salary per department

```sql
SELECT department, AVG(salary) AS average_salary
FROM employees
GROUP BY department
ORDER BY department;
```

### 18. Highest and lowest salary

```sql
SELECT
    MAX(salary) AS highest_salary,
    MIN(salary) AS lowest_salary
FROM employees;
```

## Section C — Joins

### 19. Orders with customer names

```sql
SELECT
    o.order_id,
    o.order_date,
    o.status,
    o.total_amount,
    c.customer_name
FROM orders AS o
JOIN customers AS c
  ON c.customer_id = o.customer_id
ORDER BY o.order_date;
```

An inner join returns orders with a matching customer record.

### 20. Order items with product names

```sql
SELECT
    oi.order_id,
    p.product_name,
    oi.quantity,
    oi.unit_price
FROM order_items AS oi
JOIN products AS p
  ON p.product_id = oi.product_id
ORDER BY oi.order_id, p.product_name;
```

### 21. Every customer and any orders, including customers with no orders

```sql
SELECT
    c.customer_id,
    c.customer_name,
    o.order_id,
    o.order_date,
    o.total_amount
FROM customers AS c
LEFT JOIN orders AS o
  ON o.customer_id = c.customer_id
ORDER BY c.customer_id, o.order_date;
```

A left join preserves every customer. Columns from `orders` are `NULL` when no matching order exists.

### 22. Products never ordered

```sql
SELECT p.product_id, p.product_name
FROM products AS p
LEFT JOIN order_items AS oi
  ON oi.product_id = p.product_id
WHERE oi.product_id IS NULL;
```

An equivalent anti-join can be written with `NOT EXISTS`.

### 23. Employees and manager names

```sql
SELECT
    e.employee_id,
    e.employee_name,
    e.department,
    m.employee_name AS manager_name
FROM employees AS e
LEFT JOIN employees AS m
  ON m.employee_id = e.manager_id
ORDER BY e.employee_id;
```

This is a self-join. A left join keeps employees whose `manager_id` is `NULL`.

### 24. Total quantity sold per product, including zero-sales products

```sql
SELECT
    p.product_id,
    p.product_name,
    COALESCE(SUM(oi.quantity), 0) AS total_quantity_sold
FROM products AS p
LEFT JOIN order_items AS oi
  ON oi.product_id = p.product_id
GROUP BY p.product_id, p.product_name
ORDER BY total_quantity_sold DESC, p.product_name;
```

The left join preserves products with no order items. `COALESCE` converts the resulting null sum to zero.

## Section D — Subqueries and CTEs

### 25. Products priced above the average

```sql
SELECT product_id, product_name, list_price
FROM products
WHERE list_price > (
    SELECT AVG(list_price)
    FROM products
)
ORDER BY list_price DESC;
```

The scalar subquery calculates one average value for comparison.

### 26. Customers who have placed at least one order

```sql
SELECT c.customer_id, c.customer_name
FROM customers AS c
WHERE EXISTS (
    SELECT 1
    FROM orders AS o
    WHERE o.customer_id = c.customer_id
);
```

`EXISTS` tests whether a matching row is present without needing to return its columns.

### 27. Customers who have never placed an order

```sql
SELECT c.customer_id, c.customer_name
FROM customers AS c
WHERE NOT EXISTS (
    SELECT 1
    FROM orders AS o
    WHERE o.customer_id = c.customer_id
);
```

`NOT EXISTS` is generally safer than `NOT IN` when the subquery might return nulls.

### 28. CTE for customers with completed-order revenue above 10,000

```sql
WITH customer_revenue AS (
    SELECT
        customer_id,
        SUM(total_amount) AS completed_revenue
    FROM orders
    WHERE status = 'Completed'
    GROUP BY customer_id
)
SELECT
    c.customer_id,
    c.customer_name,
    cr.completed_revenue
FROM customer_revenue AS cr
JOIN customers AS c
  ON c.customer_id = cr.customer_id
WHERE cr.completed_revenue > 10000
ORDER BY cr.completed_revenue DESC;
```

The CTE aggregates completed orders first; the outer query filters the aggregated revenue and adds customer names.

### 29. CTE for the top five products by quantity sold

```sql
WITH product_sales AS (
    SELECT
        p.product_id,
        p.product_name,
        COALESCE(SUM(oi.quantity), 0) AS total_quantity_sold
    FROM products AS p
    LEFT JOIN order_items AS oi
      ON oi.product_id = p.product_id
    GROUP BY p.product_id, p.product_name
)
SELECT product_id, product_name, total_quantity_sold
FROM product_sales
ORDER BY total_quantity_sold DESC, product_name
LIMIT 5;
```

This includes products with zero sales. The secondary sort makes ties deterministic.

## Section E — Date and Text Functions

### 30. Orders placed during January 2025

```sql
SELECT order_id, customer_id, order_date, total_amount
FROM orders
WHERE order_date >= '2025-01-01'
  AND order_date <  '2025-02-01'
ORDER BY order_date;
```

The half-open interval includes every time on January 31 when `order_date` is a timestamp. It also avoids relying on a database-specific “end of day” value.

### 31. Uppercase customer names and lowercase cities

```sql
SELECT
    UPPER(customer_name) AS customer_name_upper,
    LOWER(city) AS city_lower
FROM customers;
```

### 32. Length of each customer's name

```sql
SELECT
    customer_name,
    CHAR_LENGTH(customer_name) AS name_length
FROM customers;
```

Some engines use `LENGTH()` for character length; in SQL Server, use `LEN()`. Byte length and character length can differ for multibyte text.

### 33. Email addresses ending with example.com

```sql
SELECT customer_id, customer_name, email
FROM customers
WHERE email LIKE '%@example.com';
```

This matches addresses ending in the specified domain. Case sensitivity depends on the database collation.

### 34. Number of orders per month

PostgreSQL example:

```sql
SELECT
    DATE_TRUNC('month', order_date) AS order_month,
    COUNT(*) AS order_count
FROM orders
GROUP BY DATE_TRUNC('month', order_date)
ORDER BY order_month;
```

MySQL alternative:

```sql
SELECT
    DATE_FORMAT(order_date, '%Y-%m-01') AS order_month,
    COUNT(*) AS order_count
FROM orders
GROUP BY DATE_FORMAT(order_date, '%Y-%m-01')
ORDER BY order_month;
```

Use the expression supported by your database. Grouping by year and month together prevents January from different years from being merged.

## Section F — Window Functions

### 35. Rank products by price

```sql
SELECT
    product_id,
    product_name,
    list_price,
    RANK() OVER (ORDER BY list_price DESC) AS price_rank
FROM products
ORDER BY price_rank, product_name;
```

`RANK()` gives tied prices the same rank and leaves gaps after ties.

### 36. Number each customer's orders from earliest to latest

```sql
SELECT
    customer_id,
    order_id,
    order_date,
    ROW_NUMBER() OVER (
        PARTITION BY customer_id
        ORDER BY order_date, order_id
    ) AS customer_order_number
FROM orders
ORDER BY customer_id, customer_order_number;
```

The `order_id` tie-breaker makes ordering deterministic when dates match.

### 37. Each order amount and the overall average

```sql
SELECT
    order_id,
    customer_id,
    total_amount,
    AVG(total_amount) OVER () AS overall_average_order_amount
FROM orders
ORDER BY order_id;
```

Unlike a grouped aggregate, the window function preserves one result row per order.

### 38. Rank employees by salary within each department

```sql
SELECT
    employee_id,
    employee_name,
    department,
    salary,
    RANK() OVER (
        PARTITION BY department
        ORDER BY salary DESC
    ) AS department_salary_rank
FROM employees
ORDER BY department, department_salary_rank, employee_name;
```

`PARTITION BY department` restarts the ranking for each department.

## Section G — Data Quality and Design

### 39. Duplicate non-null customer email addresses

```sql
SELECT email, COUNT(*) AS occurrence_count
FROM customers
WHERE email IS NOT NULL
GROUP BY email
HAVING COUNT(*) > 1
ORDER BY occurrence_count DESC, email;
```

This identifies duplicate values without treating missing emails as duplicates. To compare addresses case-insensitively, group by `LOWER(email)`.

### 40. Primary keys and foreign keys for the retail tables

A suitable relational design is:

- `customers`: primary key `customer_id`.
- `orders`: primary key `order_id`; foreign key `customer_id` references `customers(customer_id)`.
- `products`: primary key `product_id`.
- `order_items`: primary key `order_item_id`; foreign key `order_id` references `orders(order_id)`; foreign key `product_id` references `products(product_id)`.

Example constraints:

```sql
CREATE TABLE customers (
    customer_id   INTEGER PRIMARY KEY,
    customer_name VARCHAR(150) NOT NULL,
    email         VARCHAR(255),
    city          VARCHAR(100),
    signup_date   DATE
);

CREATE TABLE products (
    product_id     INTEGER PRIMARY KEY,
    product_name   VARCHAR(150) NOT NULL,
    category       VARCHAR(100),
    list_price     DECIMAL(10, 2) NOT NULL CHECK (list_price >= 0),
    stock_quantity INTEGER NOT NULL DEFAULT 0 CHECK (stock_quantity >= 0)
);

CREATE TABLE orders (
    order_id     INTEGER PRIMARY KEY,
    customer_id  INTEGER NOT NULL,
    order_date   DATE NOT NULL,
    status       VARCHAR(30) NOT NULL,
    total_amount DECIMAL(12, 2) NOT NULL CHECK (total_amount >= 0),
    FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE order_items (
    order_item_id INTEGER PRIMARY KEY,
    order_id      INTEGER NOT NULL,
    product_id    INTEGER NOT NULL,
    quantity      INTEGER NOT NULL CHECK (quantity > 0),
    unit_price    DECIMAL(10, 2) NOT NULL CHECK (unit_price >= 0),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

Foreign keys prevent an order from referring to a customer that does not exist, and prevent an order item from referring to a nonexistent order or product. Primary keys uniquely identify rows. In a real implementation, choose integer identity/auto-increment syntax appropriate to your database and decide whether email should have a unique constraint based on business requirements.
