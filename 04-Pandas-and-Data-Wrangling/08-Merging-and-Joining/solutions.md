# Solutions — Merging and Joining in Pandas

These are example solutions; alternative code is acceptable when it satisfies the same join requirements.

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

## 1. Create customers and orders DataFrames with a shared customer_id key.

```python
customers = pd.DataFrame({"customer_id":[1,2,3], "name":["Asha","Ravi","Meera"]}); orders = pd.DataFrame({"order_id":[101,102], "customer_id":[1,2], "amount":[500,750]})
```

## 2. Perform an inner merge between orders and customers.

```python
orders.merge(customers, on="customer_id", how="inner")
```

## 3. Perform a left merge that preserves every order.

```python
orders.merge(customers, on="customer_id", how="left")
```

## 4. Perform a right merge that preserves every customer.

```python
orders.merge(customers, on="customer_id", how="right")
```

## 5. Perform an outer merge and explain why missing values appear.

```python
orders.merge(customers, on="customer_id", how="outer")
```

## 6. Merge two DataFrames when the key columns have different names.

```python
left.merge(right, left_on="client_id", right_on="customer_id", how="inner")
```

## 7. Merge two tables using a composite key of customer_id and month.

```python
left.merge(right, on=["customer_id","month"], how="left")
```

## 8. Use indicator=True to identify rows that matched and did not match.

```python
orders.merge(customers, on="customer_id", how="outer", indicator=True)
```

## 9. Count the left_only, right_only, and both merge indicator values.

```python
result = orders.merge(customers, on="customer_id", how="outer", indicator=True); result["_merge"].value_counts()
```

## 10. Use validate='many_to_one' for an orders-to-customers merge.

```python
orders.merge(customers, on="customer_id", validate="many_to_one")
```

## 11. Explain how duplicate keys on both sides can multiply output rows.

```python
# If a key occurs m times on the left and n times on the right, that key can produce m*n rows.
```

## 12. Check whether customer_id is unique in a customer table.

```python
customers["customer_id"].is_unique
```

## 13. Find order rows whose customer_id does not exist in customers.

```python
orders.loc[~orders["customer_id"].isin(customers["customer_id"])]
```

## 14. Find customers who have no matching orders.

```python
customers.loc[~customers["customer_id"].isin(orders["customer_id"])]
```

## 15. Merge tables that contain overlapping non-key column names and set suffixes.

```python
left.merge(right, on="id", suffixes=("_old", "_new"))
```

## 16. Join orders to customers after setting customer_id as the customer index.

```python
orders.join(customers.set_index("customer_id"), on="customer_id", how="left")
```

## 17. Use DataFrame.join() to combine two DataFrames by index.

```python
left.join(right, how="left")
```

## 18. Use concat() to stack January and February transaction rows.

```python
pd.concat([jan_sales, feb_sales], ignore_index=True)
```

## 19. Use concat(axis=1) to place two index-aligned DataFrames side by side.

```python
pd.concat([left, right], axis=1)
```

## 20. Explain the difference between merge(), join(), and concat().

```python
# merge: relational key matching; join: often convenient for index-based joins; concat: stack or align objects along an axis.
```

## 21. Merge only selected columns from a larger customer table.

```python
orders.merge(customers[["customer_id","name"]], on="customer_id", how="left")
```

## 22. Convert merge keys to compatible data types before merging.

```python
left["id"] = left["id"].astype("string"); right["id"] = right["id"].astype("string"); left.merge(right, on="id")
```

## 23. Normalize whitespace in string keys before merging.

```python
left["key"] = left["key"].astype("string").str.strip(); right["key"] = right["key"].astype("string").str.strip(); left.merge(right, on="key")
```

## 24. Compare row counts before and after a merge.

```python
before_left, before_right = len(left), len(right); result = left.merge(right, on="id", how="left"); print(before_left, before_right, len(result))
```

## 25. Use an outer merge with an indicator to reconcile two ID lists.

```python
left_ids.merge(right_ids, on="id", how="outer", indicator=True)
```

## 26. Perform a one-to-one merge and validate the relationship.

```python
left.merge(right, on="id", validate="one_to_one")
```

## 27. Demonstrate a many-to-many merge and count the resulting rows.

```python
left.merge(right, on="key", how="inner")  # repeated keys on both sides create all matching combinations
```

## 28. Use suffixes to distinguish old and new versions of a column.

```python
old_new = old.merge(new, on="id", suffixes=("_old","_new"))
```

## 29. Merge product data with inventory data on product_id.

```python
products.merge(inventory, on="product_id", how="left")
```

## 30. Combine two monthly datasets and reset the index.

```python
pd.concat([jan, feb], ignore_index=True)
```

## 31. Merge employee and department tables on department_id.

```python
employees.merge(departments, on="department_id", how="left", validate="many_to_one")
```

## 32. Use merge indicator values to create a report of unmatched records.

```python
result = left_ids.merge(right_ids, on="id", how="outer", indicator=True); result[result["_merge"] != "both"]
```

## 33. Handle unmatched values after a left merge with fillna().

```python
result = orders.merge(customers, on="customer_id", how="left"); result["name"] = result["name"].fillna("Unknown")
```

## 34. Create a merge pipeline that validates key uniqueness and row counts.

```python
assert customers["customer_id"].is_unique; before = len(orders); result = orders.merge(customers, on="customer_id", how="left", validate="many_to_one"); assert len(result) == before
```

## 35. Build an end-to-end report combining customers and orders with total spend per customer.

```python
merged = orders.merge(customers, on="customer_id", how="left"); merged.groupby(["customer_id","name"], dropna=False, as_index=False).agg(total_spend=("amount","sum"), order_count=("order_id","nunique"))
```

## Final reminders

- Choose join type based on which side must be preserved.
- Check whether keys are unique and use `validate=` when possible.
- Investigate unmatched keys with `indicator=True` and verify row counts after merging.
