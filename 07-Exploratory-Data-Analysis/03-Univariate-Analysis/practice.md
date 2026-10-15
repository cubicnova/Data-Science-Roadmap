# Practice — Data Understanding

**Instructions:** Work through the exercises in order. Use Python and pandas where requested. Do not jump straight to cleaning: first identify and document the issue, its likely impact, and what evidence would confirm it.

## Section A — Foundations

### 1. In your own words, define data understanding and explain why it should happen before modeling.

**Your answer / notes:**


### 2. A dataset contains one row per order item. Why might counting rows not equal counting orders?

**Your answer / notes:**


### 3. Explain the difference between an observation, a feature, a target, and an identifier.

**Your answer / notes:**


### 4. For a customer-churn problem, write one business question and three data questions.

**Your answer / notes:**


### 5. List four pieces of metadata you would request from the dataset owner.

**Your answer / notes:**


### 6. Explain why correlation observed during exploration does not establish causation.

**Your answer / notes:**


## Section B — Schema and Data Types

### 7. Classify `age`, `customer_id`, `membership_tier`, `signup_date`, `is_active`, and `monthly_spend` by semantic data type.

**Your answer / notes:**


### 8. A postal code is stored as an integer. Explain why treating it as a numeric measurement can be misleading.

**Your answer / notes:**


### 9. Write pandas code to print the shape, first five rows, and column data types of `df`.

**Your answer / notes:**


### 10. Use `df.info()` and explain what the non-null count can tell you.

**Your answer / notes:**


### 11. Write code to list each column's number of distinct values.

**Your answer / notes:**


### 12. How can you inspect all categorical columns' unique values? Include a safe approach for a large dataset.

**Your answer / notes:**


## Section C — Descriptive Summaries

### 13. Write code to generate numeric descriptive statistics.

**Your answer / notes:**


### 14. Write code to generate descriptive summaries for both numeric and non-numeric columns.

**Your answer / notes:**


### 15. For a `rating` column, what do count, mean, standard deviation, minimum, median (50%), and maximum describe?

**Your answer / notes:**


## Section D — Data Quality Assessment

### 16. Write code that returns the number and percentage of missing values per column, sorted by missing percentage.

**Your answer / notes:**


### 17. A column uses `''`, `'N/A'`, and `'Unknown'` for missing information. How would you investigate and normalize these values?

**Your answer / notes:**


### 18. Write code to count fully duplicated rows.

**Your answer / notes:**


### 19. Check whether `customer_id` is unique and report how many duplicated non-null IDs exist.

**Your answer / notes:**


### 20. A transaction amount includes negative values. Give three possible explanations before deciding whether they are invalid.

**Your answer / notes:**


### 21. Write checks to find ages below 0 or above 120. Explain why thresholds should be domain-informed.

**Your answer / notes:**


### 22. How would you identify inconsistent category labels such as `Indore`, `indore`, and `INDORE` without blindly changing legitimate labels?

**Your answer / notes:**


### 23. Explain the difference between a duplicate row and a repeated customer ID across multiple transaction rows.

**Your answer / notes:**


### 24. A timestamp column contains dates in two formats. Describe how you would inspect and parse it while preserving invalid values for review.

**Your answer / notes:**


### 25. Explain why an outlier should be investigated before it is removed.

**Your answer / notes:**


## Section E — Sampling, Bias, and Context

### 26. Why should you inspect a sample of records rather than relying only on summary statistics?

**Your answer / notes:**


### 27. Name three ways data collection can introduce bias or make a dataset unrepresentative.

**Your answer / notes:**


### 28. A survey has responses only from customers who opened an email. What population may be underrepresented?

**Your answer / notes:**


## Section F — Business Interpretation

### 29. A dataset covers only January through March. What limitations arise if you use it to describe annual seasonality?

**Your answer / notes:**


### 30. Write five questions you would ask before using a sales dataset to compare stores.

**Your answer / notes:**


## Section G — Practical pandas Tasks

### 31. Create a small DataFrame with at least six rows and five columns, including numeric, categorical, date, and identifier fields.

**Your answer / notes:**


### 32. For your DataFrame, create a compact data dictionary containing column name, meaning, semantic type, and expected values/units.

**Your answer / notes:**


### 33. Produce a quality report with shape, dtype, missing count/percentage, distinct count, and duplicate-row count.

**Your answer / notes:**


### 34. Identify columns that may be IDs or high-cardinality categories. Explain how you made the determination.

**Your answer / notes:**


### 35. Write at least three validation rules for your dataset and count records that fail each rule.

**Your answer / notes:**


### 36. Compare mean and median for a skewed numeric column. What might a large difference suggest?

**Your answer / notes:**


### 37. Create a concise summary of category frequencies, including percentages.

**Your answer / notes:**


### 38. Create a list of questions that the dataset cannot answer reliably, given its columns and collection period.

**Your answer / notes:**


## Section H — Mini-Project

### 39. Use the synthetic retail dataset from `examples.ipynb`. Write a 1-page data-understanding report: business context, grain, schema, quality risks, initial observations, limitations, and next steps.

**Your answer / notes:**


### 40. Recommend whether the dataset is ready for deeper EDA, needs cleaning first, or needs clarification from the data owner. Support the recommendation with evidence.

**Your answer / notes:**


## Submission checklist

- [ ] Code runs without errors.
- [ ] Claims are supported by outputs or domain knowledge.
- [ ] Data issues are flagged before being changed.
- [ ] The data dictionary includes units and semantic types.
- [ ] The report distinguishes observations, hypotheses, and limitations.
- [ ] The final recommendation includes clear next steps.
