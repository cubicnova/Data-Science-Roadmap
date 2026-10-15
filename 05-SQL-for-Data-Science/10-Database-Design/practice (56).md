# SQL Database Design — Practice Exercises

**Level:** Beginner to intermediate  
**Instructions:** Attempt the questions before reading `solutions.md`. For SQL tasks, use standard relational SQL and adapt identity/auto-increment syntax to your database engine.

## Exercises

### Concepts and entities
1. A school stores students, courses, and course enrollments. Identify three entities and give two attributes for each.
2. Explain the difference between an entity and an attribute using a library system.
3. A `students` table contains `student_id`, `name`, and `email`. Which column would you normally choose as the primary key, and why?
4. A table has both `employee_id` and `work_email`, and both are unique. Explain candidate keys and identify the candidate keys.
5. Compare a natural key and a surrogate key. Give one example of each.

### Relationships and keys
6. One customer can place many orders, but each order belongs to one customer. Identify the relationship and the table that should contain the foreign key.
7. A user can have at most one profile. Show how a one-to-one relationship could be enforced with a foreign key and a `UNIQUE` constraint.
8. An order can contain many products, and a product can appear on many orders. Explain why this is many-to-many and name the table that resolves it.
9. Write SQL to create a `departments` table with a primary key and a required department name.
10. Write SQL to create an `employees` table with an employee ID, required name, and a department foreign key.
11. Design an `order_items` table linking orders and products. Include quantity, unit price, and suitable constraints.
12. What is a composite primary key? Explain how `(order_id, product_id)` could work as a key in `order_items`.

### ER modeling and normalization
13. List the main steps for turning business requirements into an entity-relationship model.
14. What does the functional dependency `customer_id → email` mean? What assumptions must hold?
15. A table repeats `customer_name` and `customer_phone` on every order row. What redundancy can result, and how could the design improve?
16. A row stores `product_ids` as the text `12,15,21`. Explain why this is usually poor relational design and suggest an alternative.
17. An `order_items` table uses `(order_id, product_id)` as its key and also stores `product_name`. Explain the normalization issue and fix.
18. An employee table stores `employee_id`, `department_id`, and `department_name`. If each department ID determines one department name, explain the possible 3NF issue.
19. Explain insert, update, and delete anomalies.

### Constraints and referential integrity
20. Write SQL to create `customers` with an integer primary key, required full name, and required unique email.
21. Write a product table definition that prevents negative prices and requires a product name.
22. Why should `quantity` normally have both `NOT NULL` and `CHECK (quantity > 0)`?
23. Explain `DEFAULT` and give an example for an employee status column.
24. What happens when an order references a customer ID that does not exist, assuming the foreign key is enforced?
25. Compare `ON DELETE CASCADE`, `SET NULL`, and `RESTRICT`. When might cascading order deletion be inappropriate?

### Data types, indexes, and denormalization
26. Why is `DECIMAL` or `NUMERIC` generally preferable to floating point for monetary values?
27. A page finds orders by `customer_id` and sorts by `order_date`. Suggest an index and explain how it may help.
28. Name two costs or drawbacks of adding too many indexes.
29. A dashboard repeatedly calculates daily revenue by region from millions of rows. Describe a possible denormalization strategy and its trade-off.

### Design review and practical modeling
30. A table has `customer_id`, `customer_name`, `phone1`, `phone2`, and `phone3`. What design concern does this raise, and what alternative may be better?
31. A developer proposes storing a customer's age rather than date of birth. What data-quality issue can arise?
32. Why should a designer understand common application queries and reports before finalizing a schema?
33. List at least five checks for reviewing a proposed database schema.
34. Outline tables and relationships for an online store with customers, products, orders, and multiple products per order.
35. The catalog price can change, but the business must retain the price charged at purchase time. Where should that price be stored, and why?

## Suggested workflow

1. Identify entities and relationships before writing table definitions.
2. Choose keys and cardinality before adding constraints.
3. Test SQL with representative sample data.
4. Compare your answers with [`solutions.md`](solutions.md).
