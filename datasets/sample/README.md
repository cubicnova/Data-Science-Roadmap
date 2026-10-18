# Sample Datasets

This folder contains small datasets for practicing Python, NumPy, Pandas, data visualization, statistics, and introductory machine learning.

## Files

| File | Purpose | Type |
|---|---|---|
| `students.csv` | Student attendance, study time, and exam scores | Synthetic tabular data |
| `sales.csv` | Transactions, quantities, discounts, and revenue | Synthetic tabular data |
| `employees.csv` | Department, experience, salary, and location analysis | Synthetic tabular data |
| `house_prices.csv` | Property features and price prediction | Synthetic regression data |
| `iris.csv` | Flower measurements and species classification | scikit-learn's built-in copy of the classic Iris dataset |
| `customer_churn.csv` | Customer features and churn classification | Synthetic tabular data |
| `weather.csv` | Daily weather measurements for time-series practice | Synthetic time-series data |
| `products.json` | Product catalog with nested tags | Synthetic JSON records |

## Load the data

Run Python from the repository root:

```python
import pandas as pd

students = pd.read_csv("datasets/sample/students.csv")
sales = pd.read_csv("datasets/sample/sales.csv")
employees = pd.read_csv("datasets/sample/employees.csv")
houses = pd.read_csv("datasets/sample/house_prices.csv")
iris = pd.read_csv("datasets/sample/iris.csv")
churn = pd.read_csv("datasets/sample/customer_churn.csv")
weather = pd.read_csv("datasets/sample/weather.csv")
products = pd.read_json("datasets/sample/products.json")

print(students.head())
```

Install the main libraries if needed:

```bash
python -m pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

## Suggested exercises

1. Inspect shapes, column names, data types, and missing values.
2. Group sales by product and calculate total revenue.
3. Compare average exam scores by class.
4. Explore salary distributions by department.
5. Build a regression baseline for house prices.
6. Train a classification model on the Iris dataset.
7. Explore customer churn rates by contract type.
8. Plot weather trends over time.
9. Parse and filter products from the JSON catalog.

## Important notes

- All datasets except the classic Iris dataset are synthetic and created for educational use. They do not represent real students, employees, customers, transactions, property listings, or weather observations.
- Synthetic relationships are illustrative, not validated real-world patterns. Do not use these datasets to make real business, employment, housing, or other consequential decisions.
- `iris.csv` was generated from scikit-learn's built-in copy of the classic Iris dataset. If it is the classic Iris dataset, attribute it to R. A. Fisher (1936), via the [UCI Machine Learning Repository](https://archive.ics.uci.edu/dataset/53/iris), CC BY 4.0. If the fallback sample was used, replace it with the full canonical dataset before describing it as the full Iris dataset.
- Check source and licensing requirements before redistributing third-party datasets.
