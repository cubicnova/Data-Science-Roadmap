# Solutions — DateTime and Text in Pandas

Solutions are examples; variable names may differ. Each answer assumes the relevant columns exist.

## 1. Exercise 1

```python
df = pd.DataFrame({"date": ["2026-01-01", "2026-01-03", "2026-01-05", "2026-02-01", "2026-02-10"]})
df["date"] = pd.to_datetime(df["date"])
```

## 2. Exercise 2

```python
df["date"] = pd.to_datetime(df["date"], format="%d/%m/%Y")
```

## 3. Exercise 3

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
```

## 4. Exercise 4

```python
missing_count = pd.to_datetime(df["date"], errors="coerce").isna().sum()
```

## 5. Exercise 5

```python
dt = pd.to_datetime(df["date"], errors="coerce")
df["year"], df["month"], df["day"] = dt.dt.year, dt.dt.month, dt.dt.day
```

## 6. Exercise 6

```python
df["weekday"] = pd.to_datetime(df["date"]).dt.day_name()
```

## 7. Exercise 7

```python
df["is_month_end"] = pd.to_datetime(df["date"]).dt.is_month_end
```

## 8. Exercise 8

```python
df["date"] = pd.to_datetime(df["date"])
result = df.loc[df["date"] >= "2026-01-01"]
```

## 9. Exercise 9

```python
df["timestamp"] = pd.to_datetime(df["timestamp"])
result = df.loc[(df["timestamp"] >= "2026-01-01") & (df["timestamp"] < "2026-02-01")]
```

## 10. Exercise 10

```python
df["days_to_deliver"] = (pd.to_datetime(df["delivered"]) - pd.to_datetime(df["ordered"])).dt.days
```

## 11. Exercise 11

```python
dates = pd.date_range(start="2026-01-01", periods=10, freq="D")
```

## 12. Exercise 12

```python
times = pd.date_range("2026-01-01", periods=8, freq="6h")
```

## 13. Exercise 13

```python
df["hour"] = pd.to_datetime(df["timestamp"]).dt.hour
```

## 14. Exercise 14

```python
df["timestamp_utc"] = pd.to_datetime(df["timestamp"], utc=True)
df["timestamp_india"] = df["timestamp_utc"].dt.tz_convert("Asia/Kolkata")
```

## 15. Exercise 15

```python
df["quarter"] = pd.to_datetime(df["date"]).dt.quarter
```

## 16. Exercise 16

```python
df["name"] = df["name"].astype("string").str.strip()
```

## 17. Exercise 17

```python
df["name"] = df["name"].astype("string").str.strip().str.title()
```

## 18. Exercise 18

```python
df["text_lower"] = df["text"].astype("string").str.lower()
```

## 19. Exercise 19

```python
mask = df["product"].astype("string").str.contains("pro", case=False, na=False)
result = df.loc[mask]
```

## 20. Exercise 20

```python
result = df.loc[df["email"].astype("string").str.endswith("example.com", na=False)]
```

## 21. Exercise 21

```python
df["code"] = df["code"].astype("string").str.replace("-", " ", regex=False)
```

## 22. Exercise 22

```python
df["masked"] = df["value"].astype("string").str.replace(r"\d+", "#", regex=True)
```

## 23. Exercise 23

```python
df[["city", "state"]] = df["city_state"].astype("string").str.split(",", n=1, expand=True)
df["city"] = df["city"].str.strip()
df["state"] = df["state"].str.strip()
```

## 24. Exercise 24

```python
df[["username", "domain"]] = df["email"].astype("string").str.extract(r"(?P<username>[^@]+)@(?P<domain>[^@]+)")
```

## 25. Exercise 25

```python
df["description_length"] = df["description"].astype("string").str.len()
```

## 26. Exercise 26

```python
df["is_invoice"] = df["code"].astype("string").str.startswith("INV-", na=False)
```

## 27. Exercise 27

```python
df["valid"] = df["code"].astype("string").str.fullmatch(r"[A-Za-z]{3}\d{4}", na=False)
```

## 28. Exercise 28

```python
df["text"] = df["text"].astype("string").str.replace(r"\s+", " ", regex=True).str.strip()
```

## 29. Exercise 29

```python
df["value"] = df["value"].astype("string").str.replace(r"^ID:\s*", "", regex=True)
```

## 30. Exercise 30

```python
df["prefix"] = df["product_code"].astype("string").str.slice(0, 5)
```

## 31. Exercise 31

```python
mask = df["text"].astype("string").str.contains("urgent", case=False, na=False)
result = df.loc[mask]
```

## 32. Exercise 32

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df["customer"] = df["customer"].astype("string").str.strip().str.title()
```

## 33. Exercise 33

```python
df["month_start"] = pd.to_datetime(df["date"]).dt.to_period("M").dt.to_timestamp()
```

## 34. Exercise 34

```python
df["date"] = pd.to_datetime(df["date"], errors="coerce")
df = df.sort_values("date", na_position="last")
```

## 35. Exercise 35

```python
df["created"] = pd.to_datetime(df["created"], errors="coerce")
invalid_date_count = df["created"].isna().sum()
df["customer"] = df["customer"].astype("string").str.strip().str.title()
df["ticket_type"] = df["ticket"].astype("string").str.extract(r"^([A-Z]+)-")
```
