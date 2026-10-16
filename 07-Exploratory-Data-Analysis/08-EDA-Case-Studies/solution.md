# Solutions — EDA Case Studies

These are example approaches and reporting templates. Run `examples.ipynb` to get exact results from the synthetic datasets. The data are generated for learning and must not be presented as real business or student outcomes.

## Reusable first-pass profile

```python
def eda_profile(data):
    return {
        "shape": data.shape,
        "dtypes": data.dtypes.astype(str),
        "missing": pd.DataFrame({
            "count": data.isna().sum(),
            "percent": data.isna().mean().mul(100).round(2)
        }),
        "duplicate_rows": int(data.duplicated().sum()),
        "numeric_summary": data.describe(include="number").T,
        "categorical_columns": data.select_dtypes(
            include=["object", "category"]
        ).columns.tolist()
    }

profile = eda_profile(retail_df)
profile["shape"], profile["missing"], profile["numeric_summary"]
```

## Case Study 1 — Retail Sales Performance

### Inspect quality

```python
retail_df.head()
retail_df.shape
retail_df.dtypes
retail_df.isna().sum()
retail_df.duplicated().sum()
retail_df.describe(include="all").T
```

If `sales` is calculated from quantity, unit price, and discount, verify that calculation before interpreting sales. Keep the original dataset unchanged when cleaning.

### Summarize sales by group

```python
region_summary = (
    retail_df.groupby("region")["sales"]
    .agg(order_count="count", total_sales="sum", average_order_value="mean", median_order_value="median")
    .sort_values("total_sales", ascending=False)
)
display(region_summary.round(2))

category_summary = (
    retail_df.groupby("category")["sales"]
    .agg(order_count="count", total_sales="sum", average_order_value="mean", median_order_value="median")
    .sort_values("total_sales", ascending=False)
)
display(category_summary.round(2))
```

Visualize total sales:

```python
region_summary["total_sales"].plot(kind="bar", title="Total Sales by Region",
                                   xlabel="Region", ylabel="Sales")
plt.tight_layout()
plt.show()

category_summary["total_sales"].plot(kind="bar", title="Total Sales by Category",
                                     xlabel="Category", ylabel="Sales", rot=25)
plt.tight_layout()
plt.show()
```

### Inspect transaction amounts

```python
sns.histplot(data=retail_df, x="sales", bins=15, kde=True)
plt.title("Distribution of Retail Sales")
plt.xlabel("Sales")
plt.tight_layout()
plt.show()

sns.boxplot(data=retail_df, x="sales")
plt.title("Potential Outliers in Retail Sales")
plt.tight_layout()
plt.show()
```

### Reporting template

- “The region with the largest total sales in this sample is **[region]**, at **[value]** across **[count]** orders.”
- “The category with the largest total sales is **[category]**. Its average order value is **[value]**, which differs from its total-sales ranking because total sales also depend on order count.”
- “The dataset contains **[count]** missing values and **[count]** exact duplicate rows; these were reviewed before interpreting the results.”

Recommendations could include validating unusual transactions, checking inventory for high-performing categories, and comparing sales across a longer time period. Do not assume that the synthetic patterns represent actual demand.

## Case Study 2 — Student Performance

### Summaries and distributions

```python
student_df[["study_hours", "attendance_pct", "exam_score"]].describe().T

sns.histplot(data=student_df, x="exam_score", bins=15, kde=True)
plt.title("Exam Score Distribution")
plt.xlabel("Exam score")
plt.tight_layout()
plt.show()

sns.boxplot(data=student_df, x="department", y="exam_score")
plt.title("Exam Score by Department")
plt.xticks(rotation=25)
plt.tight_layout()
plt.show()
```

Group summaries:

```python
student_df.groupby("department")["exam_score"].agg(
    count="count", mean="mean", median="median", std="std"
).sort_values("median", ascending=False)
```

Correlations use complete pairs:

```python
pair = student_df[["study_hours", "exam_score"]].dropna()
print("Pairwise sample size:", len(pair))
print("Pearson:", pair["study_hours"].corr(pair["exam_score"], method="pearson"))
print("Spearman:", pair["study_hours"].corr(pair["exam_score"], method="spearman"))
```

### Interpretation
Pearson correlation measures linear association; Spearman measures monotonic rank association. Neither proves that study hours cause higher scores. Potential alternative explanations include prior knowledge, course difficulty, attendance, access to resources, and motivation. These would need to be measured and analyzed before stronger claims could be considered.

Follow-up work could validate the pattern on real, appropriately collected data and examine whether the relationship differs across courses or prior achievement groups.

## Case Study 3 — Customer Churn Exploration

### Churn count and percentage

```python
churn_counts = churn_df["churn"].value_counts(dropna=False)
churn_percent = churn_df["churn"].value_counts(normalize=True, dropna=False).mul(100).round(2)
display(pd.DataFrame({"count": churn_counts, "percent": churn_percent}))
```

### Compare numeric variables by churn status

```python
churn_summary = churn_df.groupby("churn")[["tenure_months", "monthly_charges"]].agg(
    ["count", "mean", "median", "std"]
)
display(churn_summary.round(2))

fig, axes = plt.subplots(1, 2, figsize=(12, 4))
sns.boxplot(data=churn_df, x="churn", y="tenure_months", ax=axes[0])
axes[0].set_title("Tenure by Churn Status")
sns.boxplot(data=churn_df, x="churn", y="monthly_charges", ax=axes[1])
axes[1].set_title("Monthly Charges by Churn Status")
plt.tight_layout()
plt.show()
```

### Compare churn by contract type

```python
contract_counts = pd.crosstab(churn_df["contract_type"], churn_df["churn"])
contract_churn_pct = pd.crosstab(
    churn_df["contract_type"], churn_df["churn"], normalize="index"
).mul(100).round(2)

display(contract_counts)
display(contract_churn_pct)
```

Report both counts and percentages. A group with a small denominator can show a volatile churn percentage. Churn differences may reflect customer tenure, price, service quality, customer support experiences, or contract conditions; descriptive EDA cannot establish causation.

## Cross-case-study report structure

For each case, include:
1. **Question:** what decision or uncertainty motivated the analysis?
2. **Data:** how many records and which variables were used?
3. **Quality:** missingness, duplicates, invalid values, and unusual observations.
4. **Methods:** descriptive statistics and charts selected for the question.
5. **Findings:** three claims supported by numerical outputs or visuals.
6. **Recommendations:** concrete follow-up actions.
7. **Limitations:** synthetic data, sampling, missing fields, and causal limits.

## Optional challenge: export charts

```python
from pathlib import Path
Path("figures").mkdir(exist_ok=True)

plt.figure(figsize=(7, 4))
sns.histplot(data=retail_df, x="sales", bins=15)
plt.title("Retail Sales Distribution")
plt.tight_layout()
plt.savefig("figures/retail_sales_distribution.png", dpi=150)
plt.close()
```

Use clear filenames and save figures only after verifying that the chart accurately represents the intended analysis.
