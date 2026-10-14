# Solutions — Sorting and Ranking in Pandas

The solutions below are illustrative. Variable names and sample values can differ, as long as the operation is correct.

```python
import pandas as pd

students = pd.DataFrame({
    "name": ["Asha", "Ravi", "Meera", "Kabir", "Zoya"],
    "department": ["IT", "Sales", "IT", "Sales", "HR"],
    "score": [88, 74, 95, 74, 91],
    "experience": [3, 5, 2, 7, 4],
    "salary": [60000, 52000, 72000, 58000, 65000]
})
```

## 1. Create a DataFrame with names and scores for at least five students.

```python
students = pd.DataFrame({"name":["Asha","Ravi","Meera","Kabir","Zoya"], "score":[88,74,95,74,91]})
```

## 2. Sort the student DataFrame by score in ascending order.

```python
students.sort_values("score")
```

## 3. Sort the student DataFrame by score in descending order.

```python
students.sort_values("score", ascending=False)
```

## 4. Sort employees by department alphabetically.

```python
students.sort_values("department")
```

## 5. Sort by department ascending and salary descending.

```python
students.sort_values(["department", "salary"], ascending=[True, False])
```

## 6. Sort a DataFrame by two numeric columns, with the first descending and the second ascending.

```python
students.sort_values(["score", "experience"], ascending=[False, True])
```

## 7. Sort names without case affecting alphabetical order using the `key` parameter.

```python
students.sort_values("name", key=lambda s: s.str.lower())
```

## 8. Create a DataFrame containing missing scores and sort with missing scores first.

```python
df = pd.DataFrame({"name":["A","B","C"], "score":[80, None, 95]}); df.sort_values("score", na_position="first")
```

## 9. Sort the same DataFrame with missing scores last.

```python
df.sort_values("score", na_position="last")
```

## 10. Sort a DataFrame by its index labels.

```python
students.set_index("name").sort_index()
```

## 11. Sort DataFrame columns alphabetically using `sort_index(axis=1)`.

```python
students.sort_index(axis=1)
```

## 12. Sort values descending and reset the index without keeping the old index.

```python
students.sort_values("score", ascending=False, ignore_index=True)
```

## 13. Use `rank()` to rank a Series from smallest to largest.

```python
pd.Series([30, 10, 20]).rank()
```

## 14. Rank a Series so the largest value receives rank 1.

```python
pd.Series([30, 10, 20]).rank(ascending=False)
```

## 15. Show the default average ranking for tied values.

```python
pd.Series([80, 90, 90, 70]).rank()  # ties at 90 receive average rank 3.5
```

## 16. Use `method='min'` to rank tied values.

```python
pd.Series([80, 90, 90, 70]).rank(method="min")
```

## 17. Use `method='max'` to rank tied values.

```python
pd.Series([80, 90, 90, 70]).rank(method="max")
```

## 18. Use `method='first'` and explain why row order matters.

```python
pd.Series([80, 90, 90, 70]).rank(method="first")  # tied values ranked in current order
```

## 19. Use `method='dense'` and compare it with `method='min'`.

```python
s = pd.Series([90, 90, 80, 70]); s.rank(ascending=False, method="dense")
```

## 20. Rank scores within each department, highest score first.

```python
students["department_rank"] = students.groupby("department")["score"].rank(ascending=False, method="dense")
```

## 21. Return the three rows with the largest score using `nlargest()`.

```python
students.nlargest(3, "score")
```

## 22. Return the two rows with the smallest salary using `nsmallest()`.

```python
students.nsmallest(2, "salary")
```

## 23. Find the top three rows by score, breaking ties with experience ascending.

```python
students.sort_values(["score", "experience"], ascending=[False, True]).head(3)
```

## 24. Sort a product table by category and then price ascending.

```python
products = pd.DataFrame({"category":["B","A","B","A"], "price":[12,8,5,10]}); products.sort_values(["category","price"])
```

## 25. Create an ordered categorical with Low, Medium, High and sort it.

```python
order = pd.CategoricalDtype(["Low","Medium","High"], ordered=True); p = pd.DataFrame({"priority":pd.Series(["High","Low","Medium"], dtype=order)}); p.sort_values("priority")
```

## 26. Sort dates chronologically after parsing a date column as datetime.

```python
dates = pd.DataFrame({"date":["2026-03-10","2026-01-02","2026-02-15"]}); dates["date"] = pd.to_datetime(dates["date"]); dates.sort_values("date")
```

## 27. Sort transactions by date ascending and amount descending.

```python
transactions = pd.DataFrame({"date":["2026-01-01","2026-01-01","2026-01-02"], "amount":[100,250,150]}); transactions.sort_values(["date","amount"], ascending=[True,False])
```

## 28. Rank sales within each region and show the best-ranked seller per region.

```python
sales = pd.DataFrame({"region":["East","East","West","West"], "seller":["A","B","C","D"], "sales":[100,140,90,120]}); sales["rank"] = sales.groupby("region")["sales"].rank(ascending=False, method="dense"); sales[sales["rank"] == 1]
```

## 29. Find the highest score in each department after sorting or grouping.

```python
students.groupby("department")["score"].max()
```

## 30. Sort by a column while keeping the original DataFrame unchanged.

```python
result = students.sort_values("score");  # students remains unchanged
```

## 31. Use `inplace=True` to sort a DataFrame and inspect the changed order.

```python
students.sort_values("score", inplace=True); students
```

## 32. Compare `sort_values()` and `sort_index()` with a small example.

```python
df = pd.DataFrame({"x":[20,10]}, index=["b","a"]); print(df.sort_values("x")); print(df.sort_index())
```

## 33. Demonstrate how `head(3)` depends on the current order.

```python
students.head(3)  # first three in current row order; sort first for a meaningful top 3
```

## 34. Sort a DataFrame with two keys and use `ignore_index=True`.

```python
students.sort_values(["department","score"], ascending=[True,False], ignore_index=True)
```

## 35. Build a final report sorted by department, then descending score, with a dense rank column.

```python
students["dense_rank"] = students.groupby("department")["score"].rank(ascending=False, method="dense"); students.sort_values(["department","score"], ascending=[True,False])
```

## Notes

- For ranking, decide whether highest or lowest should receive rank 1.
- Ties are not errors; choose `average`, `min`, `max`, `first`, or `dense` according to the required reporting rule.
- `head()` does not sort by itself. Always sort explicitly before selecting top rows.
