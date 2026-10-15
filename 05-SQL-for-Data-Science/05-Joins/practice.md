# Practice: SQL Joins

Use the sample `customers`, `orders`, and `products` tables created in `examples.ipynb`.

1. Display each order with its order ID, customer ID, and customer name using an INNER JOIN.
2. List all customers and their orders, including customers who have no orders.
3. Find customers who have never placed an order.
4. List orders that have a matching customer record.
5. Display customer name, city, order ID, and order amount for every matching order.
6. Show all customers from Indore and their orders, retaining customers without orders.
7. Show completed orders with customer names.
8. Use a LEFT JOIN to list all customers and only their completed orders, preserving customers with no completed orders.
9. Explain the difference between an INNER JOIN and a LEFT JOIN.
10. Explain what a CROSS JOIN returns.
11. Create every possible customer-product combination using CROSS JOIN.
12. Count orders per customer, including customers with zero orders.
13. Calculate total order amount per customer, including customers with zero orders.
14. Find customers whose total order amount exceeds 500.
15. Find orders with an amount greater than 200 and display the customer name.
16. List customers who have at least one cancelled order.
17. List customers who have no cancelled orders.
18. Join customers to orders and sort by order amount descending.
19. Show the three largest orders with customer names.
20. Find the average order amount for each customer.
21. Count orders by status.
22. Calculate total order amount by status.
23. Show customers with more than one order.
24. Display orders with customer names and customer cities.
25. Explain why joining two tables on a non-unique field can create duplicate-looking rows.
26. Explain why NULL = NULL does not match rows in a standard equality join.
27. Find customers whose city is the same as the city of at least one other customer, using a self join.
28. Using a self join on customers, list pairs of different customers from the same city without listing each pair twice.
29. Join orders to customers and return only orders whose customer city is Pune.
30. Return every customer and a flag indicating whether they have placed any order.
31. Find the number of unique customers who placed orders.
32. Show each customer and the number of distinct order statuses they have used.
33. Describe a reliable method for checking whether a join unexpectedly multiplies rows.
34. Explain the difference between placing a filter in the ON clause and placing it in WHERE for a LEFT JOIN.
35. Write a query that joins customers and orders, groups by city, and calculates total order amount per city.
