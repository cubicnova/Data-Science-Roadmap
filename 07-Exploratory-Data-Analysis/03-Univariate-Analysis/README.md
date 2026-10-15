# 02 — Data Understanding

> **Exploratory Data Analysis (EDA) · Data Science Roadmap**

Data understanding is the process of learning what a dataset represents, how it was collected, what each row and column means, whether the data is trustworthy, and which questions it can reasonably answer. It comes before detailed cleaning, feature engineering, and modeling.

## Learning objectives

By completing this module, you will be able to:

- Explain the role of data understanding in a data science workflow.
- Identify the unit of observation, records, features, target variables, and data types.
- Inspect a dataset's dimensions, schema, sample records, and descriptive statistics with pandas.
- Distinguish numerical, categorical, ordinal, Boolean, date/time, and identifier fields.
- Assess data quality using completeness, uniqueness, validity, consistency, and plausibility.
- Detect missing values, duplicate records, suspicious values, and possible outliers.
- Form initial hypotheses and translate a business question into useful data questions.
- Document dataset limitations and decide whether data is fit for purpose.

## Where this module fits

1. **EDA Workflow** — organize the investigation.
2. **Data Understanding** — learn the dataset and assess its suitability. *(This module.)*
3. Data quality assessment and cleaning — resolve confirmed problems.
4. Univariate, bivariate, and multivariate analysis — investigate distributions and relationships.
5. Communicate findings and decide what to investigate next.

## Core concepts

### 1. Start with the question

Before exploring columns, clarify the decision or problem the analysis should support. For example:

- Business question: *How can an online store reduce customer churn?*
- Data questions: Which customers left? What time period is covered? Is churn explicitly recorded? Are customer activity and support interactions available?
- Analysis questions: Does churn vary by tenure, plan, or support contacts? These are hypotheses to investigate, not conclusions to assume.

### 2. Understand rows and columns

- **Observation / record:** one unit represented by a row, such as one customer, transaction, or daily measurement.
- **Feature / variable:** a property represented by a column.
- **Target / outcome:** the value to explain or predict, when a target is defined.
- **Identifier:** a value used to distinguish records, such as `customer_id`. An identifier is not automatically a meaningful predictive feature.
- **Grain:** what one row represents. Confirm whether rows represent customers, orders, order items, or something else before calculating totals.

### 3. Recognize data types

| Type | Example | Typical questions |
|---|---|---|
| Numerical, continuous | temperature, revenue | Range? Skew? Outliers? |
| Numerical, discrete | number of visits | Can it be negative? Are fractional values valid? |
| Nominal categorical | city, product category | Which categories occur? Are labels consistent? |
| Ordinal categorical | low / medium / high | Is there a meaningful order? |
| Boolean | subscribed, is_active | Are both states represented? |
| Date/time | signup date, event time | What period and timezone? Gaps? |
| Identifier | customer ID, order ID | Unique? Missing? Reused? |

A column's storage type does not always describe its meaning. A ZIP code stored as an integer is usually a category or identifier, not a quantity.

### 4. A practical first inspection

```python
import pandas as pd

df = pd.read_csv("your_data.csv")

print(df.shape)            # rows, columns
display(df.head())         # sample records
df.info()                  # types and non-null counts
display(df.describe())     # numeric summary
display(df.describe(include="all"))  # summary across types
display(df.isna().sum())   # missing values per column
display(df.nunique())     # distinct values per column
```

Use `display()` in Jupyter. For a plain Python script, use `print()`.

### 5. Data-quality dimensions

- **Completeness:** are required values present?
- **Uniqueness:** are records or identifiers duplicated where they should not be?
- **Validity:** do values follow allowed formats, ranges, or categories?
- **Consistency:** do related fields agree, and are labels represented consistently?
- **Accuracy / plausibility:** do values make sense in context? Plausibility checks can flag issues, but they do not prove a value is wrong.
- **Timeliness:** does the dataset cover the period needed for the decision?

Do not automatically delete every duplicate or outlier. First determine whether it is an error, a valid repeated event, or a legitimate rare case.

### 6. Missingness and suspicious values

A missing value can mean unknown, not collected, not applicable, or withheld. These meanings are different. Record both the count and percentage of missing values:

```python
missing = pd.DataFrame({
    "missing_count": df.isna().sum(),
    "missing_percent": df.isna().mean().mul(100).round(2),
})
```

Also inspect placeholder strings such as `"Unknown"`, `"N/A"`, `"?"`, and blank text. Pandas does not always interpret these as missing unless configured or normalized.

### 7. Initial profiling checklist

- [ ] Business question and intended decision are clear.
- [ ] Source, collection method, and time period are documented.
- [ ] Unit of observation / row grain is confirmed.
- [ ] Column meanings and units are understood.
- [ ] Data types are appropriate for the meaning of each field.
- [ ] Missingness and placeholder values are quantified.
- [ ] Duplicate records and identifier uniqueness are checked.
- [ ] Ranges, categories, and cross-field rules are reviewed.
- [ ] Potential outliers are flagged for investigation, not automatically removed.
- [ ] Limitations and follow-up questions are written down.

## Run the examples notebook

### Requirements

- Python 3.10+
- pandas
- numpy
- matplotlib (used for an optional plot)

Install dependencies:

```bash
python -m pip install pandas numpy matplotlib jupyter
```

Open `examples.ipynb` in Jupyter Notebook, JupyterLab, or VS Code. The notebook creates a small example dataset in memory, so no external data download is required.

## Files in this module

| File | Purpose |
|---|---|
| `README.md` | Concepts, workflow, and reference checklist |
| `examples.ipynb` | Guided, runnable pandas examples |
| `practice.md` | Progressive exercises and a mini-project |
| `solution.md` | Reference answers with code and explanations |

## Recommended workflow

1. Read the concepts in this README.
2. Run `examples.ipynb` from top to bottom.
3. Attempt `practice.md` without opening the solutions.
4. Compare your work with `solution.md`; alternative valid approaches are welcome.
5. Summarize the dataset's strengths, risks, and unanswered questions in plain language.

## Important reminder

Data understanding is not the same as proving data is correct or establishing causation. It is an evidence-based first assessment. Record assumptions, verify them with data owners where possible, and keep conclusions proportional to the available data.
