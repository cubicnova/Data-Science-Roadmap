# Solutions: Beginner Real-World Projects

These are reference examples, not the only valid answers. Use the notebook's synthetic retail-style dataset to run the code.

## Exercise 1 — Example problem framing

**Question:** How do monthly sales and product categories vary in a small retail dataset?

**Audience:** A small business owner deciding which categories to investigate further.

**Outcome:** Describe sales patterns, not prove why those patterns occur.

**Limitations:** The data is synthetic and small; observed patterns may not generalize to real customers or future periods.

## Exercise 2 — Inspect and clean data

```python
import pandas as pd

print(df.shape)
print(df.dtypes)
print(df.isna().sum().sort_values(ascending=False))
print("Duplicate rows:", df.duplicated().sum())

# Example: remove exact duplicate records only after verifying they are true duplicates.
df = df.drop_duplicates().copy()

# Example: parse a date column and inspect values that fail parsing.
df["date"] = pd.to_datetime(df["date"], errors="coerce")
print("Unparseable dates:", int(df["date"].isna().sum()))
```

Do not automatically replace all missing values with zero. A missing value may mean unknown, not an actual zero.

## Exercise 3 — Example EDA

```python
import matplotlib.pyplot as plt

# Sales distribution
df["sales"].plot(kind="hist", bins=20, title="Distribution of Sales")
plt.xlabel("Sales")
plt.ylabel("Number of records")
plt.tight_layout()
plt.show()

# Sales by category
category_sales = df.groupby("category")["sales"].sum().sort_values()
category_sales.plot(kind="barh", title="Total Sales by Category")
plt.xlabel("Total sales")
plt.ylabel("Category")
plt.tight_layout()
plt.show()

# Monthly trend
monthly_sales = df.groupby(df["date"].dt.to_period("M"))["sales"].sum()
monthly_sales.plot(kind="line", marker="o", title="Monthly Sales")
plt.xlabel("Month")
plt.ylabel("Total sales")
plt.tight_layout()
plt.show()
```

Example observations should refer to actual output. For instance, identify the category with the largest observed total and whether the time series appears stable or variable. Do not invent findings before running the analysis.

## Exercise 4 — Baseline model

For a regression task, compare against a simple mean predictor:

```python
from sklearn.dummy import DummyRegressor
from sklearn.metrics import mean_absolute_error
from sklearn.model_selection import train_test_split

X = df[["feature_1", "feature_2"]]
y = df["target"]

X_train, X_test, y_train, y_test = train_test_split(
    X, y, test_size=0.2, random_state=42
)

baseline = DummyRegressor(strategy="mean")
baseline.fit(X_train, y_train)
predictions = baseline.predict(X_test)
print("Baseline MAE:", mean_absolute_error(y_test, predictions))
```

For a classification task, use `DummyClassifier` and compare accuracy, precision, recall, and F1 as appropriate. Use a stratified split when class proportions matter. Fit all learned preprocessing on training data only.

## Exercise 5 — Example executive summary outline

- **Problem:** State the business question and why it matters.
- **Data:** Name the source, time period, row count, and known limitations.
- **Findings:** Give two findings backed by tables or charts.
- **Caveats:** Explain missingness, sample bias, synthetic data, or inability to infer causation.
- **Next step:** Suggest a follow-up analysis or a small, measurable experiment.

Write the final summary using the results from your own notebook rather than copying hypothetical numbers.

## Exercise 6 — Reproducibility

Example `.gitignore`:

```text
.venv/
__pycache__/
.ipynb_checkpoints/
.env
data/private/
*.pyc
```

Example setup commands:

```bash
python -m pip install -r requirements.txt
jupyter notebook
```

Keep only data that is licensed and appropriate to share. For large datasets, document how to obtain them rather than committing them.

## Exercise 7 — Peer reproduction

A successful handoff should include the environment setup, data source or generation steps, notebook execution order, expected output files, and any known caveats. A fresh user should not need your local absolute file paths or credentials.

## Reflection questions — sample answers

1. Descriptive analysis summarizes what is observed; prediction estimates an unknown outcome for new cases.
2. Documentation makes the analysis auditable and helps reveal how assumptions affect results.
3. Correlation may arise from confounding, selection effects, reverse direction, or coincidence.
4. A baseline provides a simple reference point for judging whether a more complex model adds value.
5. Repeated tuning on the test set leaks information into model selection and makes final performance estimates optimistic.
