# Solutions — Outlier Detection

These are example solutions. Run `examples.ipynb` to inspect exact results from the reproducible synthetic dataset. A statistical flag indicates an unusual value under a rule; it does not prove the observation is incorrect.

## 1–6. Initial inspection

```python
df.head()
df.shape
df.dtypes
df.isna().sum()
df.describe(include="all").T
```

Visualize transaction amounts:

```python
import matplotlib.pyplot as plt
import seaborn as sns

fig, axes = plt.subplots(1, 2, figsize=(12, 4))
sns.histplot(data=df, x="transaction_amount", bins=20, kde=True, ax=axes[0])
axes[0].set_title("Transaction Amount Distribution")
sns.boxplot(data=df, x="transaction_amount", ax=axes[1])
axes[1].set_title("Transaction Amount Box Plot")
plt.tight_layout()
plt.show()
```

An outlier is unusual relative to a statistical pattern. An invalid value violates a known rule. Some outliers are valid; some invalid values are not extreme enough to be statistical outliers.

## 7–12. IQR method

```python
series = df["transaction_amount"].dropna()
q1 = series.quantile(0.25)
q3 = series.quantile(0.75)
iqr = q3 - q1
lower_fence = q1 - 1.5 * iqr
upper_fence = q3 + 1.5 * iqr

print("Q1:", q1, "Q3:", q3, "IQR:", iqr)
print("Lower fence:", lower_fence, "Upper fence:", upper_fence)

iqr_mask = df["transaction_amount"].notna() & (
    (df["transaction_amount"] < lower_fence) |
    (df["transaction_amount"] > upper_fence)
)
iqr_outliers = df.loc[iqr_mask]
display(iqr_outliers)
```

Flag count and percentage:

```python
valid_n = df["transaction_amount"].notna().sum()
flagged_n = int(iqr_mask.sum())
flagged_pct = flagged_n / valid_n * 100 if valid_n else 0
print(f"Flagged: {flagged_n} of {valid_n} ({flagged_pct:.2f}%)")
```

Repeat for `items_count`. In skewed data, valid observations in a long tail may be flagged. Interpret flags in context.

## 13–17. Z-score method

```python
s = df["transaction_amount"]
mean = s.mean()
std = s.std(ddof=1)

if pd.notna(std) and std > 0:
    z_scores = (s - mean) / std
    z_mask = s.notna() & (z_scores.abs() > 3)
else:
    z_scores = pd.Series(np.nan, index=s.index)
    z_mask = pd.Series(False, index=s.index)

z_outliers = df.loc[z_mask]
display(z_outliers)
```

Compare methods:

```python
comparison = pd.DataFrame({"iqr_flag": iqr_mask, "zscore_flag": z_mask})
display(comparison.value_counts().rename("row_count").to_frame())
display(df.loc[iqr_mask & z_mask])
```

Z-scores use the mean and standard deviation, both of which can be pulled toward extremes. The IQR rule uses quartiles and is often less sensitive to extreme magnitudes, but it can still flag valid tail values.

Compare summaries on a temporary subset:

```python
filtered_for_comparison = df.loc[~iqr_mask].copy()
comparison_stats = pd.DataFrame({
    "original": df["transaction_amount"].describe(),
    "without_iqr_flags": filtered_for_comparison["transaction_amount"].describe()
})
comparison_stats
```

This illustrates the impact of filtering; it is not a recommendation to remove all flagged observations.

## 18–19. Investigate flagged records

```python
display(df.loc[iqr_mask, [
    "transaction_id", "transaction_amount", "items_count",
    "customer_age", "region", "payment_method"
]])

sns.scatterplot(data=df, x="items_count", y="transaction_amount", hue="region")
plt.title("Transaction Amount vs Item Count")
plt.xlabel("Number of items")
plt.ylabel("Transaction amount")
plt.tight_layout()
plt.show()
```

Check whether the amount matches the item count, region, payment type, or known business rules. Validate unusual entries against source records when possible.

## 20. Capping / winsorization

```python
capped_df = df.copy()
capped_df["transaction_amount"] = capped_df["transaction_amount"].clip(
    lower=lower_fence, upper=upper_fence
)
```

Capping preserves row count but replaces values beyond the fences. It can reduce the influence of extremes, but it changes valid values and may conceal important high-value transactions.

## 21. Filtering

```python
filtered_df = df.loc[~iqr_mask].copy()
print("Original rows:", len(df))
print("Rows after filtering:", len(filtered_df))
display(df["transaction_amount"].describe().to_frame("original").join(
    filtered_df["transaction_amount"].describe().to_frame("filtered")
))
```

Only do this when the records meet a documented exclusion rule. Retain an audit trail and consider whether the target population changes.

## 22–23. Treatment decisions
- **Keep:** a large transaction is valid and represents an important high-value customer or rare event.
- **Correct:** a verified source record shows an amount was entered with an extra zero.
- **Remove:** the row is a confirmed test transaction or outside the defined study population.
- **Cap:** the analysis requires limiting the influence of extreme magnitudes and the cap is defensible.
- **Transform:** the values are valid but heavily right-skewed and a log-scale analysis is appropriate.

Never decide based on the statistical flag alone.

## 24. Summary table for numeric columns

```python
def iqr_summary(series, factor=1.5):
    s = series.dropna()
    if s.empty:
        return {"count": 0, "q1": np.nan, "q3": np.nan, "iqr": np.nan,
                "lower_fence": np.nan, "upper_fence": np.nan,
                "outlier_count": 0, "outlier_percent": np.nan}
    q1, q3 = s.quantile(0.25), s.quantile(0.75)
    iqr = q3 - q1
    lower, upper = q1 - factor * iqr, q3 + factor * iqr
    mask = (s < lower) | (s > upper)
    return {"count": int(s.count()), "q1": q1, "q3": q3, "iqr": iqr,
            "lower_fence": lower, "upper_fence": upper,
            "outlier_count": int(mask.sum()), "outlier_percent": float(mask.mean() * 100)}

numeric_cols = ["transaction_amount", "items_count", "customer_age"]
summary_table = pd.DataFrame({col: iqr_summary(df[col]) for col in numeric_cols}).T
summary_table
```

## 25–28. Reporting

Use values from your notebook, not invented figures. A good report should state:
1. Which variables were checked and why.
2. Which method and thresholds were used.
3. How many observations were flagged and what percentage they represent.
4. What was learned from checking the flagged records.
5. Whether values were retained, corrected, capped, or excluded—and why.
6. The limitations of the method.

For machine learning, split data into training and validation/test sets first. Estimate thresholds on the training set, then apply those fixed thresholds to other sets. Do not use information from the test set to set preprocessing thresholds.

## 29. Reusable IQR mask

```python
def iqr_outlier_mask(series, factor=1.5):
    s = series
    valid = s.dropna()
    if valid.empty:
        return pd.Series(False, index=s.index)
    q1, q3 = valid.quantile(0.25), valid.quantile(0.75)
    iqr = q3 - q1
    lower, upper = q1 - factor * iqr, q3 + factor * iqr
    return s.notna() & ((s < lower) | (s > upper))

iqr_outlier_mask(df["transaction_amount"]).sum()
```

## 30. Reusable z-score mask

```python
def zscore_outlier_mask(series, threshold=3):
    s = series
    valid = s.dropna()
    if len(valid) < 2:
        return pd.Series(False, index=s.index)
    std = valid.std(ddof=1)
    if pd.isna(std) or std == 0:
        return pd.Series(False, index=s.index)
    z = (s - valid.mean()) / std
    return s.notna() & (z.abs() > threshold)

zscore_outlier_mask(df["transaction_amount"]).sum()
```

## 31–32. Sensitivity and contextual analysis

Increasing the IQR multiplier from 1.5 to 3.0 widens the fences and typically flags fewer observations. A stricter threshold may reduce false positives but miss unusual observations worth investigating.

The amount-per-item ratio can help identify transactions that are unusual relative to item count, but it may mislead when items have very different prices. Consider product-level information if available.
