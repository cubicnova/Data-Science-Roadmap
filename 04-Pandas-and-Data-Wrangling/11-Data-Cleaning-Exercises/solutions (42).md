# Solutions — Data Cleaning with Pandas

These are reference approaches. Adjust the rules to your data dictionary and business context.

## 1. Exercise 1

```python
df = pd.read_csv("data.csv")
print(df.head(10))
print(df.shape)
print(df.dtypes)
```

## 2. Exercise 2

```python
audit = pd.DataFrame({
    "dtype": df.dtypes.astype(str),
    "missing_count": df.isna().sum(),
    "missing_percent": df.isna().mean().mul(100).round(2)
})
audit
```

## 3. Exercise 3

```python
duplicate_count = int(df.duplicated().sum())
```

## 4. Exercise 4

```python
df.columns = (df.columns.astype("string").str.strip().str.lower()
              .str.replace(r"\s+", "_", regex=True))
```

## 5. Exercise 5

```python
normalized = pd.Index(df.columns.astype("string").str.strip().str.lower().str.replace(r"\s+", "_", regex=True))
duplicate_names = normalized[normalized.duplicated()].tolist()
```

## 6. Exercise 6

```python
text_cols = df.select_dtypes(include=["object", "string"]).columns
for col in text_cols:
    df[col] = df[col].astype("string").str.strip()
```

## 7. Exercise 7

```python
df["email"] = df["email"].astype("string").str.strip().str.lower()
```

## 8. Exercise 8

```python
status_map = {"complete": "completed", "done": "completed", "completed": "completed"}
df["status"] = df["status"].astype("string").str.strip().str.lower().replace(status_map)
```

## 9. Exercise 9

```python
parsed = pd.to_numeric(df["value"], errors="coerce")
failed_count = int((df["value"].notna() & parsed.isna()).sum())
df["value"] = parsed
```

## 10. Exercise 10

```python
parsed = pd.to_datetime(df["date"], errors="coerce")
invalid_count = int((df["date"].notna() & parsed.isna()).sum())
df["date"] = parsed
```

## 11. Exercise 11

```python
amount_text = (df["amount"].astype("string").str.replace(",", "", regex=False)
               .str.replace("₹", "", regex=False).str.strip())
df["amount"] = pd.to_numeric(amount_text, errors="coerce")
```

## 12. Exercise 12

```python
report = pd.DataFrame({"missing_count": df.isna().sum(),
                       "missing_percent": (df.isna().mean() * 100).round(2)})
```

## 13. Exercise 13

```python
df["city"] = df["city"].astype("string").fillna("Unknown")
```

## 14. Exercise 14

```python
was_missing = df["age"].isna()
df["age"] = df["age"].fillna(df["age"].median())
df["age_was_missing"] = was_missing
```

## 15. Exercise 15

```python
before = len(df)
df = df.dropna(subset=["customer_id"])
removed = before - len(df)
```

## 16. Exercise 16

```python
before = len(df)
df = df.drop_duplicates()
removed = before - len(df)
```

## 17. Exercise 17

```python
df["updated_at"] = pd.to_datetime(df["updated_at"], errors="coerce")
df = df.sort_values("updated_at").drop_duplicates(subset=["email"], keep="last")
```

## 18. Exercise 18

```python
age_num = pd.to_numeric(df["age"], errors="coerce")
df["invalid_age"] = ~age_num.between(0, 120) & age_num.notna()
```

## 19. Exercise 19

```python
df["invalid_email"] = ~df["email"].astype("string").str.fullmatch(
    r"[^@\s]+@[^@\s]+\.[^@\s]+", na=False
)
```

## 20. Exercise 20

```python
negative_mask = pd.to_numeric(df["amount"], errors="coerce") < 0
negative_count = int(negative_mask.sum())
```

## 21. Exercise 21

```python
q1, q3 = df["amount"].quantile([0.25, 0.75])
iqr = q3 - q1
df["amount_outlier"] = (df["amount"] < q1 - 1.5 * iqr) | (df["amount"] > q3 + 1.5 * iqr)
```

## 22. Exercise 22

```python
df["text"] = df["text"].astype("string").str.strip().replace(
    {"": pd.NA, "N/A": pd.NA, "na": pd.NA, "n/a": pd.NA}
)
```

## 23. Exercise 23

```python
df["quantity"] = pd.to_numeric(df["quantity"], errors="coerce").astype("Int64")
```

## 24. Exercise 24

```python
df["phone"] = df["phone"].astype("string").str.replace(r"[\s()\-]", "", regex=True)
```

## 25. Exercise 25

```python
df["order_date"] = pd.to_datetime(df["order_date"], errors="coerce")
df["shipped_date"] = pd.to_datetime(df["shipped_date"], errors="coerce")
df["date_order_error"] = df["shipped_date"].notna() & df["order_date"].notna() & (df["shipped_date"] < df["order_date"])
```

## 26. Exercise 26

```python
mask = df["name"].isna() & df["email"].notna()
records = df.loc[mask]
```

## 27. Exercise 27

```python
df["has_missing_value"] = df.isna().any(axis=1)
```

## 28. Exercise 28

```python
def quality_report(frame):
    return {
        "rows": len(frame),
        "duplicate_rows": int(frame.duplicated().sum()),
        "missing_cells": int(frame.isna().sum().sum())
    }
before_report = quality_report(raw)
after_report = quality_report(df)
```

## 29. Exercise 29

```python
df.to_csv("cleaned.csv", index=False)
```

## 30. Exercise 30

```python
assert df["customer_id"].is_unique
# Or: assert not df["customer_id"].duplicated().any()
```

## 31. Exercise 31

```python
parts = df["full_name"].astype("string").str.strip().str.split(r"\s+", n=1, expand=True)
df["first_name"] = parts[0]
df["remaining_name"] = parts[1] if parts.shape[1] > 1 else pd.NA
```

## 32. Exercise 32

```python
country_map = {"usa": "United States", "u.s.a.": "United States", "uk": "United Kingdom"}
df["country"] = df["country"].astype("string").str.strip().str.lower().replace(country_map)
```

## 33. Exercise 33

```python
df.columns = (df.columns.astype("string").str.strip().str.lower()
              .str.replace(r"\s+", "_", regex=True))
for col in df.select_dtypes(include=["object", "string"]).columns:
    df[col] = df[col].astype("string").str.strip()
df["amount"] = pd.to_numeric(df["amount"], errors="coerce")
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df = df.drop_duplicates()
```

## 34. Exercise 34

```python
def clean_customer_data(frame):
    result = frame.copy()
    before_rows = len(result)
    before_missing = int(result.isna().sum().sum())
    result.columns = (result.columns.astype("string").str.strip().str.lower()
                      .str.replace(r"\s+", "_", regex=True))
    if "email" in result:
        result["email"] = result["email"].astype("string").str.strip().str.lower()
    result = result.drop_duplicates()
    metrics = {
        "rows_before": before_rows,
        "rows_after": len(result),
        "missing_cells_before": before_missing,
        "missing_cells_after": int(result.isna().sum().sum())
    }
    return result, metrics
```

## 35. Exercise 35

```python
raw = pd.DataFrame({
    " Customer Name ": [" ALICE ", "Bob", "Bob"],
    "Email ": ["ALICE@EXAMPLE.COM", "bob@example.com", "bob@example.com"],
    "Amount": ["₹1,200", "500", "500"],
    "Order Date": ["2026-01-02", "bad-date", "bad-date"]
})
df = raw.copy()
df.columns = df.columns.str.strip().str.lower().str.replace(r"\s+", "_", regex=True)
df["customer_name"] = df["customer_name"].astype("string").str.strip().str.title()
df["email"] = df["email"].astype("string").str.strip().str.lower()
df["amount"] = pd.to_numeric(df["amount"].astype("string").str.replace("₹", "", regex=False).str.replace(",", "", regex=False), errors="coerce")
df["order_date"] = pd.to_datetime(df["order_date"], errors="coerce")
df = df.drop_duplicates()
# Preserve invalid dates as NaT and report them rather than guessing a replacement.
invalid_dates = int(df["order_date"].isna().sum())
```
