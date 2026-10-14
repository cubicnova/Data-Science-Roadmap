# Solutions — GroupBy and Aggregation in Pandas

These are sample answers. Equivalent approaches are acceptable when they produce the requested grouping and metrics.

```python
import pandas as pd

sales = pd.DataFrame({
    "region": ["East", "West", "East", "North", "West", "East", "North"],
    "product": ["A", "A", "B", "B", "C", "A", "C"],
    "salesperson": ["Mira", "Dev", "Mira", "Arun", "Dev", "Zoya", "Arun"],
    "units": [10, 7, 5, 9, 4, 8, 6],
    "revenue": [1000, 700, 750, 1350, 600, 800, 900]
})
```

## 1. Create a sales DataFrame with region, product, units, and revenue columns.

```python
sales = pd.DataFrame({"region":["East","West","East"], "product":["A","A","B"], "units":[10,7,5], "revenue":[1000,700,750]})
```

## 2. Calculate total revenue for each region.

```python
sales.groupby("region")["revenue"].sum()
```

## 3. Calculate average units sold for each product.

```python
sales.groupby("product")["units"].mean()
```

## 4. Find the minimum and maximum revenue for each region.

```python
sales.groupby("region")["revenue"].agg(["min","max"])
```

## 5. Count the number of rows in each region using size().

```python
sales.groupby("region").size()
```

## 6. Count non-missing revenue values per region using count().

```python
sales.groupby("region")["revenue"].count()
```

## 7. Explain the difference between size() and count().

```python
# size() counts rows; count() counts non-missing values in the selected column.
```

## 8. Group by both region and product and sum revenue.

```python
sales.groupby(["region","product"])["revenue"].sum()
```

## 9. Return group keys as regular columns using as_index=False.

```python
sales.groupby("region", as_index=False)["revenue"].sum()
```

## 10. Calculate sum, mean, minimum, and maximum revenue in one aggregation.

```python
sales.groupby("region")["revenue"].agg(["sum","mean","min","max"])
```

## 11. Use named aggregation to produce total_revenue and average_revenue columns.

```python
sales.groupby("region").agg(total_revenue=("revenue","sum"), average_revenue=("revenue","mean"))
```

## 12. Calculate total units and distinct product count for each region.

```python
sales.groupby("region").agg(total_units=("units","sum"), distinct_products=("product","nunique"))
```

## 13. Count unique salespeople in each region.

```python
sales.groupby("region")["salesperson"].nunique()
```

## 14. Calculate median revenue by product.

```python
sales.groupby("product")["revenue"].median()
```

## 15. Calculate standard deviation of revenue by region.

```python
sales.groupby("region")["revenue"].std()
```

## 16. Sort a groupby summary by total revenue descending.

```python
sales.groupby("region", as_index=False)["revenue"].sum().sort_values("revenue", ascending=False)
```

## 17. Reset the index of a groupby result.

```python
sales.groupby("region")["revenue"].sum().reset_index()
```

## 18. Use transform() to add each region's total revenue to every row.

```python
sales["region_total"] = sales.groupby("region")["revenue"].transform("sum")
```

## 19. Calculate each transaction's share of its region's revenue.

```python
sales["region_total"] = sales.groupby("region")["revenue"].transform("sum"); sales["share"] = sales["revenue"] / sales["region_total"]
```

## 20. Add a column containing the average revenue for that row's region.

```python
sales["region_average"] = sales.groupby("region")["revenue"].transform("mean")
```

## 21. Filter to keep only regions whose total revenue is at least 1500.

```python
sales.groupby("region").filter(lambda g: g["revenue"].sum() >= 1500)
```

## 22. Group by department and role to calculate average salary.

```python
employees.groupby(["department","role"], as_index=False).agg(avg_salary=("salary","mean"))
```

## 23. Group by a date column's month and sum daily sales after parsing dates.

```python
df["date"] = pd.to_datetime(df["date"]); df.groupby(df["date"].dt.to_period("M"))["sales"].sum()
```

## 24. Include missing region values as a group using dropna=False.

```python
df.groupby("region", dropna=False)["revenue"].sum()
```

## 25. Demonstrate how sort=False affects the order of group keys.

```python
df.groupby("region", sort=False)["revenue"].sum()
```

## 26. Create a custom aggregation that returns the range (max minus min).

```python
sales.groupby("region")["revenue"].agg(lambda s: s.max() - s.min())
```

## 27. Compare a groupby aggregation with a transform result in terms of row count.

```python
# agg() typically returns one row per group; transform() returns one value per original row.
```

## 28. Calculate each product's percentage contribution to total revenue.

```python
product_total = sales.groupby("product")["revenue"].sum(); (product_total / product_total.sum() * 100).rename("percent_of_total")
```

## 29. Find the top revenue-generating product within each region.

```python
by_region_product = sales.groupby(["region","product"], as_index=False)["revenue"].sum(); by_region_product.sort_values(["region","revenue"], ascending=[True,False]).groupby("region").head(1)
```

## 30. Create a report with transaction count, total revenue, and average revenue per region.

```python
sales.groupby("region").agg(transaction_count=("revenue","size"), total_revenue=("revenue","sum"), average_revenue=("revenue","mean"))
```

## 31. Use agg() to apply different functions to different columns.

```python
sales.groupby("region").agg({"revenue":["sum","mean"], "units":["sum","max"]})
```

## 32. Verify that the sum of regional revenue equals overall revenue.

```python
regional = sales.groupby("region")["revenue"].sum(); assert regional.sum() == sales["revenue"].sum()
```

## 33. Group by customer and calculate total spend and number of transactions.

```python
orders.groupby("customer")["amount"].agg(total_spend="sum", transactions="size")
```

## 34. Create a groupby summary, rename output columns, and sort it for a report.

```python
report = sales.groupby("region", as_index=False).agg(total_revenue=("revenue","sum"), total_units=("units","sum")); report.sort_values("total_revenue", ascending=False)
```

## 35. Build an end-to-end analysis of revenue and units by region and product with readable column names.

```python
summary = sales.groupby(["region","product"], as_index=False).agg(total_revenue=("revenue","sum"), average_revenue=("revenue","mean"), total_units=("units","sum"), transactions=("revenue","size")); summary.sort_values(["region","total_revenue"], ascending=[True,False])
```

## Final reminders

- Grouping keys determine the level of detail in each result row.
- Use `size()` for row counts and `count()` for non-missing values.
- Use `agg()` for summaries and `transform()` for row-aligned group metrics.
