# Solutions — Duplicates and Data Types in Pandas

The following are sample solutions. Use equivalent code if it meets the same requirement.

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "customer_id": ["C01", "C02", "C02", "C03"],
    "name": ["Asha", "Ravi", "Ravi", "Meera"],
    "age": ["21", "34", "34", "29"],
    "joined": ["2025-01-10", "2025-02-12", "2025-02-12", "2025-03-01"],
    "active": ["True", "False", "False", "True"]
})
```

## 1. Create a DataFrame containing at least two exact duplicate rows.

```python
df = pd.DataFrame({"id":[1,2,2,3], "name":["A","B","B","C"]})
```

## 2. Use duplicated() to mark duplicate rows.

```python
df.duplicated()
```

## 3. Count the number of duplicate rows in a DataFrame.

```python
int(df.duplicated().sum())
```

## 4. Display every row that belongs to a duplicate group.

```python
df[df.duplicated(keep=False)]
```

## 5. Detect duplicates based only on a customer_id column.

```python
df[df.duplicated(subset=["customer_id"], keep=False)]
```

## 6. Drop exact duplicate rows while keeping the first occurrence.

```python
df.drop_duplicates(keep="first")
```

## 7. Drop duplicate customer IDs while keeping the last occurrence.

```python
df.drop_duplicates(subset=["customer_id"], keep="last")
```

## 8. Explain the difference between keep='first', keep='last', and keep=False.

```python
# first keeps the first unmarked; last keeps the last unmarked; False marks every row in a duplicate group.
```

## 9. Sort by an updated_at column and keep the latest record for each ID.

```python
df.sort_values("updated_at").drop_duplicates(subset=["id"], keep="last")
```

## 10. Find duplicated email addresses while ignoring other columns.

```python
df[df.duplicated(subset=["email"], keep=False)]
```

## 11. Count the frequency of each customer ID using value_counts().

```python
df["customer_id"].value_counts(dropna=False)
```

## 12. Inspect a DataFrame's dtypes and info() output.

```python
print(df.dtypes); df.info()
```

## 13. Convert a column of numeric strings to integers.

```python
pd.to_numeric(pd.Series(["21","34","29"]), errors="raise").astype("int64")
```

## 14. Convert a numeric string column containing an invalid word with errors='coerce'.

```python
pd.to_numeric(pd.Series(["21","unknown","29"]), errors="coerce")
```

## 15. Identify which original values failed numeric conversion.

```python
s = pd.Series(["21","unknown","29"]); n = pd.to_numeric(s, errors="coerce"); s[n.isna() & s.notna()]
```

## 16. Convert a date string column into datetime values.

```python
pd.to_datetime(pd.Series(["2025-01-10","2025-02-12"]), errors="raise")
```

## 17. Convert a string column to Pandas' nullable string dtype.

```python
df["name"] = df["name"].astype("string")
```

## 18. Convert a numeric column to nullable Int64 while preserving missing values.

```python
pd.Series([21, None, 29]).astype("Int64")
```

## 19. Map 'true' and 'false' strings to actual Boolean values.

```python
pd.Series(["true","false","true"]).str.lower().map({"true":True,"false":False}).astype("boolean")
```

## 20. Explain why astype(bool) can be unsafe for strings like 'False'.

```python
# Every non-empty string, including "False", is truthy in Python; map known labels explicitly.
```

## 21. Clean comma-separated thousands values such as '1,250' before numeric conversion.

```python
s = pd.Series(["1,250","900","not known"]); clean = s.str.replace(",", "", regex=False).str.strip(); pd.to_numeric(clean, errors="coerce")
```

## 22. Convert invalid date strings to NaT and count the invalid dates.

```python
dates = pd.Series(["2025-01-10","not a date"]); parsed = pd.to_datetime(dates, errors="coerce"); int(parsed.isna().sum())
```

## 23. Convert a low-cardinality status column to category.

```python
df["status"] = df["status"].astype("category")
```

## 24. Check whether a customer_id column is unique after deduplication.

```python
df["customer_id"].is_unique
```

## 25. Create a mask that marks every duplicate record by a composite key of name and date.

```python
df[df.duplicated(subset=["name","date"], keep=False)]
```

## 26. Remove duplicates based on name and date while keeping the first record.

```python
df.drop_duplicates(subset=["name","date"], keep="first")
```

## 27. Show how duplicate rows can be legitimate repeated events rather than data errors.

```python
# Repeated purchases, visits, logins, or sensor readings can be distinct events; define a business key first.
```

## 28. Create a cleaning workflow that preserves a raw DataFrame copy.

```python
raw = df.copy(); cleaned = raw.drop_duplicates().copy()
```

## 29. Compare row count before and after dropping duplicates.

```python
before = len(df); cleaned = df.drop_duplicates(); after = len(cleaned); print({"before":before,"after":after})
```

## 30. Use duplicated(keep=False) and sort the resulting rows by the duplicate key.

```python
df[df.duplicated(subset=["customer_id"], keep=False)].sort_values("customer_id")
```

## 31. Convert a mixed string/number column to numeric and report missing values after conversion.

```python
s = pd.Series(["10","bad","20"]); numeric = pd.to_numeric(s, errors="coerce"); print(numeric); print("Missing after conversion:", int(numeric.isna().sum()))
```

## 32. Convert a timestamp column to datetime and extract its year.

```python
df["timestamp"] = pd.to_datetime(df["timestamp"], errors="coerce"); df["year"] = df["timestamp"].dt.year
```

## 33. Validate a cleaned dataset by checking dtypes, null counts, and duplicate keys.

```python
print(cleaned.dtypes); print(cleaned.isna().sum()); print("IDs unique:", cleaned["customer_id"].is_unique)
```

## 34. Write a reusable function that removes duplicates using a supplied subset and keep policy.

```python
def deduplicate(frame, subset=None, keep="first"):
    if keep not in ("first", "last", False):
        raise ValueError("keep must be first, last, or False")
    return frame.drop_duplicates(subset=subset, keep=keep)
```

## 35. Build an end-to-end example that deduplicates customer records and fixes ID, age, date, and active dtypes.

```python
raw = df.copy(); cleaned = raw.sort_values("joined").drop_duplicates(subset=["customer_id"], keep="last").copy(); cleaned["customer_id"] = cleaned["customer_id"].astype("string"); cleaned["age"] = pd.to_numeric(cleaned["age"], errors="coerce").astype("Int64"); cleaned["joined"] = pd.to_datetime(cleaned["joined"], errors="coerce"); cleaned["active"] = cleaned["active"].str.lower().map({"true":True,"false":False}).astype("boolean"); cleaned
```

## Final checklist

- Define the business key before removing duplicates.
- Decide which record should survive based on reliable fields such as update time or completeness.
- Review invalid values created by coercion and verify all important dtypes after conversion.
