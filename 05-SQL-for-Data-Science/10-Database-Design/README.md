# SQL Database Design

Database design organizes information into tables and relationships to improve consistency, integrity, and queryability.

## Learning objectives
- Identify entities, attributes, keys, and relationships.
- Model 1:1, 1:N, and M:N relationships.
- Apply normalization (1NF, 2NF, 3NF).
- Enforce rules with constraints and choose useful indexes.
- Understand denormalization trade-offs.

## Core concepts
- **Entity:** a thing being tracked, such as Customer or Order.
- **Attribute:** a property, such as email or order_date.
- **Primary key (PK):** uniquely identifies each row.
- **Foreign key (FK):** references a key in another table.
- **Candidate key:** a minimal set of columns that uniquely identifies a row.
- **Composite key:** a key made from multiple columns.
- **Natural key:** a meaningful business value; **surrogate key:** a generated identifier.
- **Functional dependency** `X → Y`: X determines exactly one Y value.

## Relationships
- **1:1:** one user has at most one profile. Put a FK in `profiles` and add `UNIQUE(user_id)`.
- **1:N:** one customer has many orders. Put `customer_id` in `orders`.
- **M:N:** orders contain products and products appear in orders. Use a junction table `order_items`.

```sql
CREATE TABLE customers (
  customer_id INTEGER PRIMARY KEY,
  full_name TEXT NOT NULL,
  email TEXT NOT NULL UNIQUE
);

CREATE TABLE orders (
  order_id INTEGER PRIMARY KEY,
  customer_id INTEGER NOT NULL,
  order_date DATE NOT NULL,
  FOREIGN KEY (customer_id) REFERENCES customers(customer_id)
);

CREATE TABLE products (
  product_id INTEGER PRIMARY KEY,
  product_name TEXT NOT NULL,
  unit_price DECIMAL(10,2) NOT NULL CHECK (unit_price >= 0)
);

CREATE TABLE order_items (
  order_id INTEGER NOT NULL REFERENCES orders(order_id),
  product_id INTEGER NOT NULL REFERENCES products(product_id),
  quantity INTEGER NOT NULL CHECK (quantity > 0),
  unit_price DECIMAL(10,2) NOT NULL CHECK (unit_price >= 0),
  PRIMARY KEY (order_id, product_id)
);
```

The composite key prevents the same product from appearing twice per order in this model. If duplicate lines are allowed, use a separate `order_item_id` and define the business rule.

## ER modeling workflow
1. Gather requirements and representative queries/reports.
2. Identify entities and attributes.
3. Choose candidate and primary keys.
4. Define relationships, cardinality, and optionality.
5. Resolve M:N relationships with junction tables.
6. Convert the model to relational tables.
7. Add constraints, test sample data, and review query plans.

Text model:
```text
CUSTOMER 1 ---- N ORDER
ORDER    1 ---- N ORDER_ITEM
PRODUCT  1 ---- N ORDER_ITEM
```

## Normalization
Normalization reduces redundancy and insert, update, and delete anomalies.

### First Normal Form (1NF)
Each cell contains one value for its domain; avoid repeating columns or comma-separated lists. Store one order-item per row rather than `product_ids = '12,15,21'`.

### Second Normal Form (2NF)
The relation is in 1NF and every non-key attribute depends on the whole candidate key. With composite key `(order_id, product_id)`, `product_name` depends only on `product_id`; store it in `products`.

### Third Normal Form (3NF)
The relation is in 2NF and non-key attributes do not depend transitively on a key. If `department_id → department_name`, store department details in a department table instead of repeating them for every employee. Normalize according to actual functional dependencies, not column names alone.

## Constraints and types
- `PRIMARY KEY`, `FOREIGN KEY`, `NOT NULL`, `UNIQUE`, `CHECK`, `DEFAULT`.
- Combine `CHECK` with `NOT NULL` if NULL is not allowed; a CHECK may accept an unknown/NULL result.
- Use decimal/numeric types for exact monetary values rather than floating point.
- Use native date/time types, explicit time-zone policy, and appropriate NULLability.
- Avoid storing multiple unrelated facts in one column.

```sql
CREATE TABLE employees (
  employee_id INTEGER PRIMARY KEY,
  email TEXT NOT NULL UNIQUE,
  salary DECIMAL(10,2) NOT NULL CHECK (salary >= 0),
  status TEXT NOT NULL DEFAULT 'active'
);
```

## Referential actions
`ON DELETE CASCADE` deletes dependent child rows; `SET NULL` clears an optional FK; `RESTRICT`/`NO ACTION` prevents a delete that would leave invalid references. Exact support and semantics vary by engine. Preserve historical/financial records when required.

## Indexing and denormalization
Indexes can speed up filters, joins, and sorting but consume storage and add write overhead. Index based on real query patterns, consider composite-index column order, and validate using query plans. Denormalization (summary tables or cached values) can help measured read bottlenecks but requires a reliable consistency strategy.

## Design checklist
- Each table represents a clear entity or relationship.
- Keys, cardinality, and optionality are documented.
- M:N relationships use junction tables.
- Constraints enforce critical business rules.
- Types and NULLability fit the domain.
- Normalization avoids unintended anomalies.
- Indexes support actual workloads.
- Sensitive data is minimized and protected.
- Migration, backup, and recovery are considered.

See [`practice.md`](practice.md), [`solutions.md`](solutions.md), and [`examples.ipynb`](examples.ipynb).
