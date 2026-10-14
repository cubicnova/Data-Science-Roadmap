# Solutions — Missing Values in Pandas

These are example solutions. Different values and variable names are fine if the operations meet each question's requirements.

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "name": ["Asha", "Ravi", "Meera", "Kabir", None],
    "age": [21, np.nan, 23, 22, 20],
    "city": ["Indore", "Pune", None, "Delhi", "Pune"],
    "score": [88, 74, np.nan, 91, 85]
})
```

## 1. Create a DataFrame with at least three columns and some missing values.

```python
df = pd.DataFrame({"name":["Asha","Ravi",None], "age":[21,np.nan,20], "score":[88,74,np.nan]})
```

## 2. Display a Boolean DataFrame showing where values are missing.

```python
df.isna()
```

## 3. Count missing values in each column.

```python
df.isna().sum()
```

## 4. Calculate the total number of missing cells in a DataFrame.

```python
int(df.isna().sum().sum())
```

## 5. Calculate the percentage of missing values in each column.

```python
(df.isna().mean() * 100).round(1)
```

## 6. Select only rows where the age column is missing.

```python
df[df["age"].isna()]
```

## 7. Select rows where any column contains a missing value.

```python
df[df.isna().any(axis=1)]
```

## 8. Drop rows containing at least one missing value.

```python
df.dropna()
```

## 9. Drop rows only when the score column is missing.

```python
df.dropna(subset=["score"])
```

## 10. Drop rows when either age or score is missing.

```python
df.dropna(subset=["age", "score"])
```

## 11. Drop columns that contain only missing values.

```python
df.dropna(axis=1, how="all")
```

## 12. Keep only rows that have at least three non-missing values.

```python
df.dropna(thresh=3)
```

## 13. Fill missing city values with the string 'Unknown'.

```python
df["city"] = df["city"].fillna("Unknown")
```

## 14. Fill missing numeric values with the column mean.

```python
df["score"].fillna(df["score"].mean())
```

## 15. Fill missing numeric values with the column median.

```python
df["age"].fillna(df["age"].median())
```

## 16. Fill missing categorical values with the mode.

```python
df["city"].fillna(df["city"].mode().iloc[0])
```

## 17. Fill two different columns with different replacement values using a dictionary.

```python
df.fillna({"age": df["age"].median(), "city": "Unknown", "score": 0})
```

## 18. Use forward fill on an ordered Series with missing values.

```python
pd.Series([10, np.nan, np.nan, 15]).ffill()
```

## 19. Use backward fill on an ordered Series with missing values.

```python
pd.Series([10, np.nan, np.nan, 15]).bfill()
```

## 20. Explain why time-series data should be sorted before forward filling.

```python
# Sort by timestamp first, otherwise a previous row may not be the previous point in time.
```

## 21. Replace the placeholders 'N/A' and '-' with actual missing values.

```python
df.replace(["N/A", "-"], pd.NA)
```

## 22. Read a CSV while treating 'N/A' and '-' as missing values (write the code).

```python
pd.read_csv("data.csv", na_values=["N/A", "-"])
```

## 23. Create a Boolean missingness indicator column for a score field.

```python
df["score_was_missing"] = df["score"].isna()
```

## 24. Compare row count before and after calling dropna().

```python
before = len(df); cleaned = df.dropna(); after = len(cleaned); print(before, after)
```

## 25. Find the column with the largest number of missing values.

```python
df.isna().sum().idxmax()
```

## 26. Fill age using the median age within each city.

```python
df["age_filled"] = df["age"].fillna(df.groupby("city")["age"].transform("median"))
```

## 27. Add a global-median fallback for groups with no available median.

```python
group_median = df.groupby("city")["age"].transform("median"); df["age_filled"] = df["age"].fillna(group_median).fillna(df["age"].median())
```

## 28. Demonstrate how filling a missing numeric value with zero can change the mean.

```python
s = pd.Series([10, 20, np.nan, 40]); print("Original mean:", s.mean()); print("Zero-filled mean:", s.fillna(0).mean())
```

## 29. Explain one risk of dropping every row with any missing value.

```python
# Dropping rows can reduce sample size and bias results if missingness is systematic.
```

## 30. Explain the difference between MCAR, MAR, and MNAR.

```python
# MCAR: unrelated to observed/unobserved values; MAR: related to observed variables; MNAR: related to the missing value or unobserved factors.
```

## 31. Show how to use notna() to select rows with a known email address.

```python
contacts = pd.DataFrame({"email":["a@example.com", None, "b@example.com"]}); contacts[contacts["email"].notna()]
```

## 32. Fill missing values in a dictionary-style call to DataFrame.fillna().

```python
df.fillna({"age": 0, "city": "Unknown", "score": df["score"].median()})
```

## 33. Build a cleaned DataFrame while leaving the original DataFrame unchanged.

```python
raw = df.copy(); cleaned = raw.fillna({"city": "Unknown", "age": raw["age"].median()})
```

## 34. After filling missing values, check whether any missing values remain.

```python
cleaned.isna().sum()
```

## 35. Create a short missing-data cleaning pipeline and report missing counts before and after.

```python
raw = df.copy(); before = raw.isna().sum(); cleaned = raw.copy(); cleaned["age"] = cleaned["age"].fillna(cleaned["age"].median()); cleaned["city"] = cleaned["city"].fillna("Unknown"); after = cleaned.isna().sum(); print("Before:\n", before, "\nAfter:\n", after)
```

## Important reminder

Imputation choices depend on the data and task. For machine learning, fit imputation rules on the training split only and apply the same rules to validation and test data.
