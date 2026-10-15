# SQL Practice Problems

This worksheet contains **40 scenario-based SQL exercises** for beginner-to-intermediate learners. The questions use a small retail database so you can practise filtering, aggregation, joins, subqueries, CTEs, window functions, data cleaning, and database design.

Try to solve each problem before checking `solutions.md`. SQL syntax can vary slightly between PostgreSQL, MySQL, SQL Server, and SQLite; use the equivalent function when your database differs.

## Practice Database

Assume the following tables are available.

### `customers`

| Column | Description |
|---|---|
| `customer_id` | Unique customer ID |
| `customer_name` | Customer's full name |
| `email` | Email address |
| `city` | Customer's city |
| `signup_date` | Date the customer registered |

### `orders`

| Column | Description |
|---|---|
| `order_id` | Unique order ID |
| `customer_id` | Customer who placed the order |
| `order_date` | Date the order was placed |
| `status` | Order status, such as `Completed`, `Pending`, or `Cancelled` |
| `total_amount` | Total order value |

### `order_items`

| Column | Description |
|---|---|
| `order_item_id` | Unique order-line ID |
| `order_id` | Related order |
| `product_id` | Product purchased |
| `quantity` | Units purchased |
| `unit_price` | Price per unit at the time of purchase |

### `products`

| Column | Description |
|---|---|
| `product_id` | Unique product ID |
| `product_name` | Product name |
| `category` | Product category |
| `list_price` | Current list price |
| `stock_quantity` | Units currently in stock |

### `employees`

| Column | Description |
|---|---|
| `employee_id` | Unique employee ID |
| `employee_name` | Employee's name |
| `department` | Department |
| `salary` | Annual salary |
| `manager_id` | Employee ID of the manager; may be `NULL` |

## Section A — SELECT, Filtering, Sorting, and Limits

1. Display every column for all customers.
2. Display only `customer_name`, `email`, and `city` for each customer.
3. Find customers who live in `Indore`.
4. Find products with a `list_price` greater than 1,000.
5. Find orders whose status is either `Pending` or `Cancelled`.
6. Find products whose category is `Electronics` and whose stock quantity is below 20.
7. Display all products ordered from the highest to the lowest list price.
8. Return the five most expensive products.
9. Find customers whose email address is missing (`NULL`).
10. Find customers whose names begin with the letter `A`.

## Section B — Aggregation and Grouping

11. Count the total number of customers.
12. Calculate the average list price of all products.
13. Calculate total order value across all orders.
14. Count how many orders exist for each status.
15. Calculate total sales amount for each order status.
16. Find categories with more than five products.
17. Calculate the average salary for each department.
18. Find the highest and lowest salary in the employees table.

## Section C — Joins

19. Display each order with the name of the customer who placed it.
20. Display each order item with its product name, quantity, and unit price.
21. List every customer and any orders they have placed, including customers who have never ordered.
22. Find products that have never appeared in `order_items`.
23. Show each employee alongside their manager's name. Employees without a manager should still appear.
24. Calculate total quantity sold for each product. Include products with zero sales.

## Section D — Subqueries and CTEs

25. Find products priced above the average product list price.
26. Find customers who have placed at least one order.
27. Find customers who have never placed an order using `NOT EXISTS`.
28. Use a CTE to calculate completed-order revenue by customer, then display customers whose completed-order revenue exceeds 10,000.
29. Use a CTE to calculate each product's total sales quantity and return the top five products by quantity sold.

## Section E — Date and Text Functions

30. Find orders placed during January 2025. Use a half-open date range so timestamps on January 31 are included.
31. Display each customer's name in uppercase and their city in lowercase.
32. Return each customer's name and the length of that name.
33. Find customers whose email ends with `example.com`.
34. Group orders by month and calculate the number of orders per month. State or use a database-appropriate date-truncation expression.

## Section F — Window Functions

35. Rank products from most expensive to least expensive using `RANK()`.
36. For each customer, number their orders from earliest to latest using `ROW_NUMBER()`.
37. Show each order's amount alongside the average order amount across the entire table.
38. For each department, rank employees by salary from highest to lowest.

## Section G — Data Quality and Design

39. Identify duplicate customer email addresses, ignoring `NULL` values. Return only email addresses used more than once.
40. Propose a primary key and foreign keys for the four related retail tables (`customers`, `orders`, `order_items`, and `products`). Explain how the relationships prevent orphan records.

## Submission Checklist

- [ ] Attempt all 40 questions.
- [ ] Use clear aliases for calculated columns.
- [ ] Use `IS NULL` / `IS NOT NULL` for null checks.
- [ ] Avoid accidental duplicate rows when joining tables.
- [ ] Test queries with empty results and boundary dates.
- [ ] Explain any database-specific syntax in your submission.
