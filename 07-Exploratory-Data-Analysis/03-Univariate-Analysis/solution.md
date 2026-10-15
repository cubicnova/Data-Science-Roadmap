# Solutions — Data Understanding

These are reference answers, not the only acceptable answers. Adapt thresholds and rules to the domain and document assumptions.

## Section A — Foundations

### 1. What is data understanding?
It is the systematic investigation of a dataset's meaning, structure, context, quality, and limitations. It helps ensure that later analysis and models address the intended question using suitable evidence.

### 2. Why can order-item rows differ from orders?
One order can contain several items and therefore several rows. Count unique order IDs to estimate orders; count rows to estimate order-item records.

### 3. Observation, feature, target, identifier
- Observation: one represented unit, such as one order.
- Feature: a measured attribute, such as order value.
- Target: an outcome to explain or predict, such as churn.
- Identifier: a key such as `customer_id`; it distinguishes entities but is not necessarily a useful predictive signal.

### 4. Churn questions
Business question: How can the company reduce customer churn over the next quarter?
Data questions: How is churn defined? What period is covered? Is churn recorded for every customer? Are tenure, plan, usage, and support interactions available?

### 5. Useful metadata
Examples: source system, extraction date, date coverage, row grain, column definitions, units/currency, collection method, inclusion/exclusion rules, refresh frequency, and known limitations.

### 6. Correlation and causation
An association can arise from confounding, selection effects, reverse causality, or coincidence. Establishing causality generally requires a defensible design and assumptions, not just a chart or correlation coefficient.

## Section B — Schema and Data Types

### 7. Semantic type examples
- `age`: discrete numerical in many datasets.
- `customer_id`: identifier.
- `membership_tier`: ordinal categorical if tiers have a defined order.
- `signup_date`: date/time.
- `is_active`: Boolean.
- `monthly_spend`: continuous numerical.

### 8. Postal code stored as integer
Arithmetic on postal codes is meaningless; leading zeros may be lost and numerical summaries do not describe a meaningful quantity. Store it as a string/category.

### 9. Inspect structure
```python
print(df.shape)
display(df.head())
print(df.dtypes)
```

### 10. `df.info()`
It reports row count, column names, non-null counts, data types, and memory use. A lower non-null count than total rows suggests missing values, but placeholders such as `"Unknown"` may not be counted as missing.

### 11. Distinct values per column
```python
df.nunique(dropna=False).sort_values()
```
Using `dropna=False` counts missing values as a distinct state.

### 12. Inspect categorical values
```python
for col in df.select_dtypes(include=["object", "category", "bool"]).columns:
    print(f"\n{col}:")
    print(df[col].value_counts(dropna=False).head(30))
```
For large/high-cardinality columns, inspect top frequencies and a bounded sample instead of printing every value.

## Section C — Descriptive Summaries

### 13. Numeric statistics
```python
df.describe()
```

### 14. All-column summaries
```python
df.describe(include="all")
```
The output differs by data type; for categories, `top` and `freq` describe the most common value and its count.

### 15. Rating statistics
`count` is the number of non-missing ratings; `mean` is the average; standard deviation measures spread around the mean; min/max are endpoints; `50%` is the median. Check the valid rating scale before interpreting them.

## Section D — Data Quality Assessment

### 16. Missing count and percentage
```python
missing = pd.DataFrame({
    "missing_count": df.isna().sum(),
    "missing_percent": df.isna().mean().mul(100).round(2),
})
missing.sort_values("missing_percent", ascending=False)
```

### 17. Placeholder missing values
First inspect frequency and meaning with the data owner. If they consistently mean missing, normalize them explicitly:
```python
placeholders = ["", "N/A", "Unknown"]
df = df.replace(placeholders, pd.NA)
```
Do not replace `"Unknown"` if it is a meaningful, intentionally recorded category. Strip whitespace first when needed.

### 18. Fully duplicated rows
```python
duplicate_count = int(df.duplicated().sum())
print(duplicate_count)
```
Inspect duplicates before removing them; repeated rows may be legitimate depending on the data grain.

### 19. Unique customer IDs
```python
non_null_ids = df["customer_id"].dropna()
print("Null IDs:", int(df["customer_id"].isna().sum()))
print("Duplicated non-null ID rows:", int(non_null_ids.duplicated().sum()))
print("Unique non-null IDs:", int(non_null_ids.nunique()))
```
Repeated customer IDs are expected in transaction-level data, so first confirm the row grain.

### 20. Negative transaction amounts
Possible explanations include refunds, chargebacks, reversals, or data-entry errors. Check transaction type, accounting conventions, and source-system rules before labeling them invalid.

### 21. Implausible ages
```python
invalid_age = df["age"].notna() & ~df["age"].between(0, 120)
df.loc[invalid_age, ["age"]]
```
The bounds are illustrative. Use subject-matter rules and investigate flagged records rather than silently deleting them.

### 22. Inconsistent category labels
```python
df["city"].astype("string").str.strip().str.casefold().value_counts(dropna=False)
```
Use normalized values to identify possible variants, then map variants only after confirming they refer to the same entity. Preserve original values or keep a mapping log for auditability.

### 23. Duplicate row vs repeated customer ID
A duplicate row repeats all selected fields. A repeated customer ID can be perfectly valid when each row represents a different purchase or event. Uniqueness expectations depend on grain.

### 24. Mixed date formats
```python
parsed = pd.to_datetime(df["timestamp"], errors="coerce", format="mixed")
invalid = df["timestamp"].notna() & parsed.isna()
print(df.loc[invalid, "timestamp"])
```
`format="mixed"` is supported by recent pandas versions. For older versions, inspect formats and parse with an explicit format or a controlled fallback. Keep invalid original strings for review.

### 25. Outliers
Outliers can be valid rare events, measurement errors, or signs of a different process. Check units, source records, domain limits, and their effect on summaries. Do not remove them solely because they are extreme.

## Section E — Sampling, Bias, and Context

### 26. Why inspect records?
Summary statistics can hide malformed records, mixed units, impossible combinations, and data-entry patterns. A sample helps understand context and detect issues that aggregates conceal.

### 27. Sources of bias
Examples: convenience sampling, nonresponse, survivorship bias, measurement errors, outdated coverage, selection based on an outcome, and missing entire customer groups.

### 28. Email-open survey
Customers who do not open email, have limited email access, or are less engaged may be underrepresented. The sample may not represent all customers.

### 29. January–March coverage
It cannot reliably characterize the whole year's seasonal pattern, holidays, weather-related changes, or annual growth. State the period explicitly and obtain longer history if annual seasonality matters.

### 30. Questions before comparing stores
Examples: Are dates aligned? Are store sizes and operating hours comparable? Are returns/refunds included? Is currency consistent? Are sales net or gross? Are product mixes and promotions different? Are missing records distributed unevenly?

## Section F — Business Interpretation

### 31–38. Practical pandas tasks
Answers depend on the learner's chosen dataset. A strong submission should:
- Use meaningful columns and a clearly stated row grain.
- Document units, expected ranges, categories, and definitions.
- Report missing count and percentage, distinct count, types, and duplicates.
- Treat IDs as identifiers unless there is a justified alternative.
- Express validation rules as explicit Boolean conditions and count failures.
- Compare mean and median to understand possible skew, not to prove it by themselves.
- Report category counts and percentages with missing values considered.
- Name unsupported questions and explain which fields or time coverage are absent.

Example quality-report pattern:
```python
quality = pd.DataFrame({
    "dtype": df.dtypes.astype(str),
    "missing_count": df.isna().sum(),
    "missing_percent": (df.isna().mean() * 100).round(2),
    "distinct_count": df.nunique(dropna=False),
})
print("shape:", df.shape)
print("duplicate rows:", int(df.duplicated().sum()))
display(quality)
```

Category counts and percentages:
```python
counts = df["category"].value_counts(dropna=False)
summary = pd.DataFrame({
    "count": counts,
    "percent": (counts / len(df) * 100).round(2),
})
display(summary)
```

## Section G — Practical pandas Tasks

### 39. Mini-project report structure
Use the synthetic retail dataset in `examples.ipynb`. Include:
1. **Context:** what business decision could the dataset support?
2. **Grain:** what does one row represent?
3. **Schema:** column definitions, semantic types, and units.
4. **Coverage:** number of records and date range.
5. **Quality:** missing values, duplicate records/IDs, range issues, and category inconsistencies.
6. **Initial observations:** descriptive patterns supported by outputs.
7. **Limitations:** possible bias, short coverage, absent variables, and uncertain definitions.
8. **Next steps:** clarifications, targeted cleaning, and follow-up analyses.

### 40. Readiness recommendation
A defensible recommendation could be **ready for deeper EDA after targeted validation** if the schema and grain are understood and the remaining issues are documented. Recommend **cleaning first** when known quality problems materially distort the intended analysis. Recommend **clarification from the data owner** when key definitions, units, population, or row grain are ambiguous. State the evidence and the next action rather than relying on a vague overall quality score.

## Mini-project rubric (10 points)

| Criterion | Points |
|---|---:|
| Business context and row grain | 2 |
| Schema and data dictionary | 2 |
| Quality checks with evidence | 2 |
| Limitations and bias considerations | 2 |
| Clear readiness decision and next steps | 2 |
