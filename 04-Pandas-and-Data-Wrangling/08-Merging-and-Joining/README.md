# 08 — Merging and Joining in Pandas

Real-world analysis often requires combining related datasets: customers with orders, products with inventory, or records from different sources. Pandas provides `merge()`, `join()`, and `concat()` for different combination patterns.

## Learning objectives
- Understand relational join types: inner, left, right, and outer.
- Merge on one or more key columns.
- Join using indexes.
- Combine datasets vertically or horizontally with `concat()`.
- Detect unmatched keys and validate join cardinality.
- Avoid duplicate-column confusion and accidental row multiplication.

## 1. Sample data

```python
import pandas as pd

customers = pd.DataFrame({
    "customer_id": [1, 2, 3],
    "name": ["Asha", "Ravi", "Meera"],
    "city": ["Indore", "Pune", "Delhi"]
})
orders = pd.DataFrame({
    "order_id": [101, 102, 103, 104],
    "customer_id": [1, 2, 2, 4],
    "amount": [500, 750, 250, 900]
})
```

`customers` and `orders` share a key, `customer_id`. The orders table contains customer 4, who is not in the customers table.

## 2. `merge()` join types

```python
pd.merge(orders, customers, on="customer_id", how="inner")
pd.merge(orders, customers, on="customer_id", how="left")
pd.merge(orders, customers, on="customer_id", how="right")
pd.merge(orders, customers, on="customer_id", how="outer")
```

- **Inner:** only matching keys from both sides.
- **Left:** every row from the left table plus matching right-side values.
- **Right:** every row from the right table plus matching left-side values.
- **Outer:** all keys from both tables; unmatched fields become missing.

The left and right sides are determined by argument order. Choose it based on which records must be preserved.

## 3. Merge using different key names

Use `left_on` and `right_on` when the join columns have different names.

```python
left = pd.DataFrame({"client_id": [1, 2], "name": ["Asha", "Ravi"]})
right = pd.DataFrame({"customer_id": [1, 2], "amount": [100, 200]})
pd.merge(left, right, left_on="client_id", right_on="customer_id")
```

For composite keys, pass lists to `on`, `left_on`, or `right_on`.

```python
pd.merge(left, right, on=["customer_id", "month"], how="left")
```

Both key columns must exist, and key types should be compatible.

## 4. Inspect unmatched records with `indicator=True`

```python
result = pd.merge(orders, customers, on="customer_id", how="outer", indicator=True)
result["_merge"].value_counts()
```

The indicator column labels each row as `left_only`, `right_only`, or `both`. It is useful for reconciliation and data-quality checks.

## 5. Validate join cardinality

Unexpected duplicate keys can multiply rows. Use `validate=` to assert the expected relationship.

```python
pd.merge(orders, customers, on="customer_id", validate="many_to_one")
```

Common values:
- `"one_to_one"`: keys unique in both inputs.
- `"one_to_many"`: left keys unique; right keys may repeat.
- `"many_to_one"`: right keys unique; left keys may repeat.
- `"many_to_many"`: duplicates allowed on both sides; no uniqueness check is enforced.

For customer-to-orders, `many_to_one` is often appropriate because each order has one customer while a customer can have multiple orders.

## 6. Overlapping column names

When non-key columns share names, Pandas adds suffixes by default.

```python
pd.merge(left_df, right_df, on="id", suffixes=("_source1", "_source2"))
```

Rename columns before merging if that makes the meaning clearer.

## 7. `join()` for index-based combinations

`DataFrame.join()` is convenient when the right-side key is its index.

```python
customers_by_id = customers.set_index("customer_id")
orders.join(customers_by_id, on="customer_id", how="left")
```

You can also join two DataFrames directly on their indexes.

## 8. `concat()` to stack or align data

`concat()` combines objects along an axis; it does not perform a relational key match in the same way as `merge()`.

```python
pd.concat([jan_sales, feb_sales], ignore_index=True)  # stack rows
pd.concat([left, right], axis=1)                      # align columns by index
```

- `axis=0` (default): stack rows.
- `axis=1`: align by index and place columns side by side.
- `ignore_index=True`: create a new sequential row index when stacking.

## 9. Common pitfalls and best practices
1. Inspect key uniqueness before merging.
2. Choose `how=` based on which records must be retained.
3. Use `validate=` to catch unexpected relationships.
4. Check row counts before and after merging.
5. Use `indicator=True` to find unmatched keys.
6. Ensure key columns have compatible data types and consistent formatting.
7. Beware of null keys and duplicate keys; they can create surprising matches or row multiplication.
8. Distinguish relational merging (`merge`) from stacking (`concat`) and index-based joining (`join`).
9. Select only needed columns before a merge to reduce ambiguity.
10. Confirm the output has the expected number of rows and columns.

## Quick reference

| Task | Expression |
|---|---|
| Inner merge | `left.merge(right, on="id", how="inner")` |
| Left merge | `left.merge(right, on="id", how="left")` |
| Different key names | `left.merge(right, left_on="x", right_on="y")` |
| Composite key | `left.merge(right, on=["id", "month"])` |
| Find unmatched keys | `merge(..., indicator=True)` |
| Validate relationship | `merge(..., validate="many_to_one")` |
| Index join | `left.join(right, on="id")` |
| Stack rows | `pd.concat([a, b], ignore_index=True)` |

## Practice
Complete [`practice.md`](practice.md), run [`examples.ipynb`](examples.ipynb), and use [`solutions.md`](solutions.md) to check your work.
