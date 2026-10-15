# SQL Database Design — Solutions and Explanations

These are reference answers, not the only valid designs. A database schema should reflect actual business rules. Adjust data types and identity/auto-increment syntax for your SQL engine.

## 1. Entities and attributes

- `Student`: `student_id`, `full_name`
- `Course`: `course_id`, `course_title`
- `Enrollment`: `student_id`, `course_id`, `enrollment_date`

Students and courses have a many-to-many relationship, resolved by `Enrollment`. A composite key `(student_id, course_id)` works if a student can enroll in a course only once.

## 2. Entity vs. attribute

An entity is something the system tracks; an attribute is a property of it. In a library, `Book` is an entity and `title` is an attribute. `Member` is another entity, related to books through loans.

## 3. Choosing a primary key

`student_id` is normally preferable because it is a stable identifier intended to distinguish students. Email can change or be missing; it can be protected with a `UNIQUE` constraint if the business requires uniqueness.

## 4. Candidate keys

A candidate key is a minimal column set that uniquely identifies each row. If both values are unique and non-null by business rule, `employee_id` and `work_email` are candidate keys. Select one as the primary key and enforce the other with `UNIQUE`.

## 5. Natural vs. surrogate keys

A natural key has business meaning, such as a country code. A surrogate key is generated for identification, such as an integer ID or UUID. Natural values can change and may expose sensitive information; surrogate keys are often convenient for stable relationships.

## 6. One-to-many relationship

Store `customer_id` as a foreign key in `orders`, the many-side table. Many orders can reference one customer, while each order has one customer.

## 7. One-to-one relationship

```sql
CREATE TABLE profiles (
    profile_id INTEGER PRIMARY KEY,
    user_id INTEGER NOT NULL UNIQUE,
    display_name TEXT,
    FOREIGN KEY (user_id) REFERENCES users(user_id)
);
```

`UNIQUE(user_id)` allows at most one profile per user. `NOT NULL` means every profile must reference a user.

## 8. Many-to-many relationship

Use a junction table such as `order_items`, with one row per product on an order. It can also store relationship attributes such as quantity and the price charged.

## 9. Departments table

```sql
CREATE TABLE departments (
    department_id INTEGER PRIMARY KEY,
    department_name TEXT NOT NULL UNIQUE
);
```

Use `UNIQUE` if department names must be unique under the business rules.

## 10. Employees table

```sql
CREATE TABLE employees (
    employee_id INTEGER PRIMARY KEY,
    full_name TEXT NOT NULL,
    department_id INTEGER,
    FOREIGN KEY (department_id) REFERENCES departments(department_id)
);
```

Make `department_id` `NOT NULL` if every employee must belong to a department. The referenced table must exist first.

## 11. Order items table

```sql
CREATE TABLE order_items (
    order_id INTEGER NOT NULL,
    product_id INTEGER NOT NULL,
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    unit_price DECIMAL(10,2) NOT NULL CHECK (unit_price >= 0),
    PRIMARY KEY (order_id, product_id),
    FOREIGN KEY (order_id) REFERENCES orders(order_id),
    FOREIGN KEY (product_id) REFERENCES products(product_id)
);
```

This design allows one line per product per order. If duplicate lines are allowed, add an `order_item_id` key and define the rule separately.

## 12. Composite primary key

A composite key contains multiple columns. `(order_id, product_id)` identifies a product line within an order: neither column alone is sufficient because each order can contain many products and each product can appear in many orders.

## 13. ER modeling workflow

Gather requirements; identify entities and attributes; find candidate keys; select primary keys; define relationships, cardinality, and optionality; resolve many-to-many relationships with junction tables; add constraints; and validate against sample scenarios and common queries.

## 14. Functional dependency

`customer_id → email` means a given customer ID determines one email value. This assumes IDs uniquely identify customers and email is modeled as one attribute per customer. Use `NOT NULL` if required and `UNIQUE` if emails must be unique.

## 15. Repeated customer details

Repeating customer details on every order creates redundancy and can cause inconsistent updates. Store customer details once in `customers`; put `customer_id` in `orders` as a foreign key.

## 16. Comma-separated IDs

A list such as `12,15,21` is difficult to validate, join, search, index, and constrain. Store one row per order-product association in `order_items`, with columns such as `order_id`, `product_id`, and `quantity`.

## 17. Product name in order_items

`product_name` depends on `product_id`, which is only part of the composite key. This is a partial dependency. Store the current product name in `products`; keep order-specific facts in `order_items`.

## 18. Transitive dependency and 3NF

If `department_id` determines `department_name`, then department name depends on employee ID through department ID. Store department details in `departments` and reference it from `employees` to avoid repeated department names.

## 19. Anomalies

- **Insert anomaly:** a fact cannot be added without unrelated data.
- **Update anomaly:** the same fact must be changed in several rows, risking inconsistency.
- **Delete anomaly:** deleting a row unintentionally removes the only stored copy of another fact.

Normalization helps reduce these problems.

## 20. Customers table

```sql
CREATE TABLE customers (
    customer_id INTEGER PRIMARY KEY,
    full_name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE
);
```

Adapt generated-key syntax if your database requires it.

## 21. Product price constraint

```sql
CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    product_name TEXT NOT NULL,
    price DECIMAL(10,2) NOT NULL CHECK (price >= 0)
);
```

`NOT NULL` requires a value; `CHECK` disallows negative prices.

## 22. NOT NULL and CHECK

A `CHECK` expression may evaluate to unknown for NULL and still pass in many engines. `NOT NULL` requires a value, while `CHECK (quantity > 0)` requires a positive value. Together they enforce both rules.

## 23. DEFAULT constraint

A default supplies a value when an insert omits a column or uses `DEFAULT`. Example: `status TEXT NOT NULL DEFAULT 'active'`. It generally does not replace an explicitly supplied NULL.

## 24. Foreign-key violation

The database rejects the insert because the referenced customer row does not exist, assuming the foreign key is enabled and enforced. This prevents an orphaned order.

## 25. Referential actions

- `CASCADE`: delete dependent child rows.
- `SET NULL`: clear a nullable foreign key.
- `RESTRICT` / `NO ACTION`: prevent a delete that would break the relationship, with exact behavior varying by engine.

Cascading deletion may be inappropriate for financial or audit records that must be retained. A cancellation status or soft-delete policy may be safer than deleting historical transactions.

## 26. Monetary data types

Floating-point values approximate many decimal fractions and can introduce rounding artifacts. `DECIMAL`/`NUMERIC` is designed for exact decimal arithmetic and is usually more suitable for prices and financial totals.

## 27. Composite index

```sql
CREATE INDEX idx_orders_customer_date
ON orders (customer_id, order_date);
```

This may help queries filtering by customer and sorting or filtering by date. Validate with the query plan and representative data; usefulness depends on the workload.

## 28. Index trade-offs

Indexes consume storage and make inserts, updates, and deletes more expensive because index entries must be maintained. Too many indexes also increase maintenance effort and can complicate query-plan choices.

## 29. Denormalization for a dashboard

Create a summary table keyed by date and region with precomputed revenue. Dashboard queries can read fewer rows rather than aggregating millions of details each time. The trade-off is extra storage and the need to refresh or update summaries reliably when source orders change.

## 30. Multiple phone columns

`phone1`, `phone2`, and `phone3` impose an arbitrary limit and make validation, searching, and per-number metadata awkward. A separate `customer_phones` table with `phone_id`, `customer_id`, and `phone_number` is more flexible.

## 31. Storing age

Age changes over time, so a stored age becomes stale unless updated regularly. If genuinely needed and appropriate, store date of birth and calculate age when required. Date of birth is personal data, so collect and protect it only when there is a valid need.

## 32. Understand query patterns

Common queries show how tables are joined, filtered, sorted, and aggregated. This informs table boundaries, keys, constraints, and indexes, and helps ensure the schema supports real workflows without unnecessary complexity.

## 33. Schema review checklist

Check that each table has a clear purpose; keys are appropriate; foreign keys, cardinality, and optionality are correct; many-to-many relationships use junction tables; types and NULL rules fit the domain; constraints enforce business rules; normalization avoids unintended anomalies; indexes support measured workloads; sensitive data is minimized; and migration, backup, and recovery plans exist.

## 34. Online store model

A reasonable starting point:

```sql
CREATE TABLE customers (
    customer_id INTEGER PRIMARY KEY,
    full_name TEXT NOT NULL,
    email TEXT NOT NULL UNIQUE
);

CREATE TABLE products (
    product_id INTEGER PRIMARY KEY,
    product_name TEXT NOT NULL,
    current_price DECIMAL(10,2) NOT NULL CHECK (current_price >= 0)
);

CREATE TABLE orders (
    order_id INTEGER PRIMARY KEY,
    customer_id INTEGER NOT NULL REFERENCES customers(customer_id),
    order_date DATE NOT NULL,
    status TEXT NOT NULL DEFAULT 'pending'
);

CREATE TABLE order_items (
    order_id INTEGER NOT NULL REFERENCES orders(order_id),
    product_id INTEGER NOT NULL REFERENCES products(product_id),
    quantity INTEGER NOT NULL CHECK (quantity > 0),
    unit_price_at_purchase DECIMAL(10,2) NOT NULL CHECK (unit_price_at_purchase >= 0),
    PRIMARY KEY (order_id, product_id)
);
```

Relationships: one customer has many orders; one order has many order items; one product can appear in many order items. The junction table resolves the many-to-many relationship between orders and products.

## 35. Preserve the purchase price

Store the price in `order_items`, for example as `unit_price_at_purchase`. The catalog price may change, but the order line must retain the price agreed at the time of purchase for historical accuracy. This is an intentional snapshot of a transaction fact.
