# Pandas Cheat Sheet

A practical, beginner-friendly reference for data manipulation and analysis in Python. This cheat sheet covers Pandas fundamentals, DataFrame operations, data cleaning, filtering, aggregation, merging, reshaping, time-series analysis, and data preparation for machine learning.

**Repository:** [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap)

---

## Table of Contents

1. [Introduction to Pandas](#1-introduction-to-pandas)
2. [Installation and Imports](#2-installation-and-imports)
3. [Series and DataFrame](#3-series-and-dataframe)
4. [Loading and Saving Data](#4-loading-and-saving-data)
5. [Inspecting a Dataset](#5-inspecting-a-dataset)
6. [Selecting Rows and Columns](#6-selecting-rows-and-columns)
7. [Filtering and Sorting](#7-filtering-and-sorting)
8. [Adding, Updating, and Removing Columns](#8-adding-updating-and-removing-columns)
9. [Handling Missing Data](#9-handling-missing-data)
10. [Data Cleaning and Type Conversion](#10-data-cleaning-and-type-conversion)
11. [String Operations](#11-string-operations)
12. [Grouping and Aggregation](#12-grouping-and-aggregation)
13. [Merging, Joining, and Concatenation](#13-merging-joining-and-concatenation)
14. [Reshaping Data](#14-reshaping-data)
15. [Date and Time Operations](#15-date-and-time-operations)
16. [Statistical Operations](#16-statistical-operations)
17. [Apply, Map, and Vectorization](#17-apply-map-and-vectorization)
18. [Duplicates and Index Management](#18-duplicates-and-index-management)
19. [Data Visualization](#19-data-visualization)
20. [Data Preparation for Machine Learning](#20-data-preparation-for-machine-learning)
21. [Useful Functions Reference](#21-useful-functions-reference)
22. [Best Practices](#22-best-practices)
23. [Common Errors and Solutions](#23-common-errors-and-solutions)
24. [Practice Exercises](#24-practice-exercises)
25. [Quick Revision](#25-quick-revision)
26. [Official Resources](#26-official-resources)

---

## 1. Introduction to Pandas

Pandas is an open-source Python library for working with structured and labeled data.

### Common use cases

- Reading CSV, Excel, JSON, and database data.
- Cleaning incomplete or inconsistent datasets.
- Filtering and transforming records.
- Performing statistical analysis.
- Grouping and summarizing business data.
- Preparing datasets for machine learning.
- Analyzing dates, times, and time-series data.

### Core data structures

| Structure | Description | Example |
|---|---|---|
| `Series` | One-dimensional labeled data | A column of prices |
| `DataFrame` | Two-dimensional labeled table | A customer dataset |
| `Index` | Labels identifying rows or columns | Customer IDs |

### Why use Pandas?

- Convenient tabular data operations.
- Label-based and position-based selection.
- Built-in missing-data handling.
- Integration with NumPy, Matplotlib, and scikit-learn.
- Support for data import, export, grouping, and reshaping.

---

## 2. Installation and Imports

### Install Pandas

```bash
python -m pip install pandas
```

Install additional libraries for data analysis:

```bash
python -m pip install numpy matplotlib openpyxl
```

### Import libraries

```python
import pandas as pd
import numpy as np
```

Optional visualization import:

```python
import matplotlib.pyplot as plt
```

### Check the installed version

```python
print(pd.__version__)
```

### Create a sample dataset

The following dataset will be reused throughout this cheat sheet.

```python
import pandas as pd
import numpy as np

df = pd.DataFrame({
    "Employee": ["Amit", "Priya", "Rahul", "Neha", "Amit"],
    "Department": ["IT", "HR", "IT", "Finance", "IT"],
    "Age": [25, 30, 28, 35, 25],
    "Salary": [50000, 60000, 55000, 70000, 50000],
    "City": ["Indore", "Pune", "Indore", "Mumbai", "Indore"]
})

print(df)
```

---

## 3. Series and DataFrame

### 3.1 Create a Series

```python
scores = pd.Series([85, 90, 78, 92])

print(scores)
```

Create a Series with custom index labels:

```python
scores = pd.Series(
    [85, 90, 78],
    index=["Amit", "Priya", "Rahul"]
)

print(scores)
```

Access values:

```python
print(scores["Amit"])
print(scores.iloc[0])
```

### 3.2 Create a DataFrame

```python
data = {
    "Name": ["Amit", "Priya", "Rahul"],
    "Age": [25, 30, 28],
    "Score": [85, 90, 78]
}

df = pd.DataFrame(data)

print(df)
```

### 3.3 Create a DataFrame from a list

```python
data = [
    ["Amit", 25, 85],
    ["Priya", 30, 90],
    ["Rahul", 28, 78]
]

df = pd.DataFrame(
    data,
    columns=["Name", "Age", "Score"]
)
```

### 3.4 Create a DataFrame with an index

```python
df = pd.DataFrame(
    {
        "Age": [25, 30, 28],
        "Score": [85, 90, 78]
    },
    index=["Amit", "Priya", "Rahul"]
)
```

### 3.5 Important attributes

```python
df.shape
df.columns
df.index
df.dtypes
df.size
df.ndim
```

| Attribute | Meaning |
|---|---|
| `df.shape` | Number of rows and columns |
| `df.columns` | Column labels |
| `df.index` | Row labels |
| `df.dtypes` | Data type of each column |
| `df.size` | Total number of elements |
| `df.ndim` | Number of dimensions |

---

## 4. Loading and Saving Data

Pandas provides functions for reading data from different file formats and methods for exporting results.

### 4.1 Read a CSV file

```python
df = pd.read_csv("data.csv")
```

Read selected columns:

```python
df = pd.read_csv(
    "data.csv",
    usecols=["Name", "Age", "Salary"]
)
```

Read a limited number of rows:

```python
df = pd.read_csv("data.csv", nrows=100)
```

Specify a delimiter:

```python
df = pd.read_csv("data.txt", sep="\t")
```

### 4.2 Save to CSV

```python
df.to_csv("output.csv", index=False)
```

Use `index=False` when the DataFrame index should not be written as an extra column.

### 4.3 Read an Excel file

```python
df = pd.read_excel("data.xlsx")
```

Read a specific worksheet:

```python
df = pd.read_excel(
    "data.xlsx",
    sheet_name="Employees"
)
```

### 4.4 Save to Excel

```python
df.to_excel("output.xlsx", index=False)
```

For `.xlsx` files, an Excel engine such as `openpyxl` may be required.

### 4.5 Read and write JSON

```python
df = pd.read_json("data.json")
df.to_json("output.json", orient="records", indent=2)
```

### 4.6 Read SQL query results

```python
from sqlalchemy import create_engine

engine = create_engine("sqlite:///company.db")

df = pd.read_sql_query(
    "SELECT * FROM employees",
    engine
)
```

For databases containing sensitive or untrusted data, use parameterized queries rather than building SQL statements from user input.

### 4.7 Read and write Parquet

```python
df = pd.read_parquet("data.parquet")
df.to_parquet("output.parquet", index=False)
```

A compatible Parquet engine, such as PyArrow, may need to be installed.

### File I/O quick reference

| Function | Purpose |
|---|---|
| `pd.read_csv()` | Read CSV |
| `pd.read_excel()` | Read Excel |
| `pd.read_json()` | Read JSON |
| `pd.read_parquet()` | Read Parquet |
| `pd.read_sql_query()` | Read SQL query results |
| `df.to_csv()` | Export CSV |
| `df.to_excel()` | Export Excel |
| `df.to_json()` | Export JSON |
| `df.to_parquet()` | Export Parquet |

---

## 5. Inspecting a Dataset

Inspect a dataset before modifying it.

### View the first rows

```python
df.head()
df.head(10)
```

### View the last rows

```python
df.tail()
df.tail(3)
```

### Inspect dataset dimensions

```python
print(df.shape)
```

### View column names

```python
print(df.columns.tolist())
```

### Inspect data types and non-null counts

```python
df.info()
```

### Generate descriptive statistics

```python
df.describe()
```

Include non-numeric columns:

```python
df.describe(include="all")
```

### Count unique values

```python
df["Department"].value_counts()
```

Include missing values in the count:

```python
df["Department"].value_counts(dropna=False)
```

### Count unique values in a column

```python
df["Department"].nunique()
```

### Get unique values

```python
df["Department"].unique()
```

### Count missing values

```python
df.isna().sum()
```

### Quick inspection checklist

```python
print("Shape:", df.shape)
print("Columns:", df.columns.tolist())

df.info()
print(df.describe(include="all"))
print("Missing values:\n", df.isna().sum())
print("Duplicate rows:", df.duplicated().sum())
```

---

## 6. Selecting Rows and Columns

### 6.1 Select a single column

```python
df["Employee"]
```

Returns a Series.

### 6.2 Select multiple columns

```python
df[["Employee", "Salary"]]
```

Returns a DataFrame.

### 6.3 Select rows using `loc`

`loc` selects rows and columns by labels.

```python
df.loc[0]
df.loc[0:2]
df.loc[0, "Employee"]
```

Important: label-based slices with `loc` include the ending label when it exists.

Select specific rows and columns:

```python
df.loc[0:2, ["Employee", "Salary"]]
```

### 6.4 Select rows using `iloc`

`iloc` selects by integer position.

```python
df.iloc[0]
df.iloc[0:3]
df.iloc[0, 1]
```

Select specific positions:

```python
df.iloc[0:3, 0:2]
```

Unlike `loc`, the ending position in an `iloc` slice is excluded.

### 6.5 Select a scalar value

```python
df.at[0, "Salary"]
df.iat[0, 3]
```

- `at`: access one value using labels.
- `iat`: access one value using integer positions.

### 6.6 Select columns by data type

```python
numeric_df = df.select_dtypes(include="number")
```

Select text columns:

```python
text_df = df.select_dtypes(include="object")
```

For modern string and categorical dtypes, use the appropriate dtype selectors for the actual data.

### Selection summary

| Method | Selection type |
|---|---|
| `df["Column"]` | Single column |
| `df[["A", "B"]]` | Multiple columns |
| `df.loc[]` | Label-based selection |
| `df.iloc[]` | Position-based selection |
| `df.at[]` | Single labeled value |
| `df.iat[]` | Single positional value |
| `df.filter()` | Select rows or columns by labels |

---

## 7. Filtering and Sorting

### 7.1 Filter rows using a condition

```python
high_salary = df[df["Salary"] > 55000]
```

### 7.2 Filter using multiple conditions

Use `&` for AND and `|` for OR. Put each condition inside parentheses.

```python
result = df[
    (df["Age"] > 25) &
    (df["Salary"] >= 55000)
]
```

OR condition:

```python
result = df[
    (df["Department"] == "IT") |
    (df["Department"] == "HR")
]
```

### 7.3 Filter using `isin()`

```python
result = df[
    df["Department"].isin(["IT", "HR"])
]
```

### 7.4 Exclude values

```python
result = df[
    ~df["Department"].isin(["HR"])
]
```

### 7.5 Filter text

```python
result = df[
    df["Employee"].str.contains(
        "am",
        case=False,
        na=False
    )
]
```

### 7.6 Filter using `query()`

```python
result = df.query("Age > 25 and Salary >= 55000")
```

For variable values, use the `@` prefix:

```python
minimum_salary = 55000

result = df.query("Salary >= @minimum_salary")
```

### 7.7 Sort by one column

```python
df.sort_values("Salary")
```

Descending order:

```python
df.sort_values("Salary", ascending=False)
```

### 7.8 Sort by multiple columns

```python
df.sort_values(
    by=["Department", "Salary"],
    ascending=[True, False]
)
```

### 7.9 Sort by index

```python
df.sort_index()
```

### 7.10 Select top or bottom values

```python
df.nlargest(3, "Salary")
df.nsmallest(3, "Age")
```

These methods are useful for quickly identifying high- and low-value records.

---

## 8. Adding, Updating, and Removing Columns

### 8.1 Add a new column

```python
df["Bonus"] = df["Salary"] * 0.10
```

### 8.2 Create a column using conditions

```python
df["Salary_Level"] = np.where(
    df["Salary"] >= 60000,
    "High",
    "Standard"
)
```

### 8.3 Use multiple conditions

```python
df["Category"] = np.select(
    [
        df["Salary"] >= 65000,
        df["Salary"] >= 55000
    ],
    [
        "High",
        "Medium"
    ],
    default="Low"
)
```

Conditions are evaluated in order, so place more specific or higher-threshold conditions first when appropriate.

### 8.4 Rename columns

```python
df = df.rename(
    columns={
        "Employee": "Name",
        "Salary": "Monthly_Salary"
    }
)
```

### 8.5 Rename all columns

```python
df.columns = [
    "Name",
    "Department",
    "Age",
    "Monthly_Salary",
    "City"
]
```

The replacement list must have the same length as the number of columns.

### 8.6 Update values

```python
df.loc[df["Department"] == "IT", "Salary"] = 60000
```

This changes salaries for all matching IT records.

### 8.7 Remove a column

```python
df = df.drop(columns=["Bonus"])
```

### 8.8 Remove multiple columns

```python
df = df.drop(columns=["Bonus", "Category"])
```

### 8.9 Remove rows

```python
df = df.drop(index=[0, 1])
```

### 8.10 Insert a column at a position

```python
df.insert(
    1,
    "Country",
    ["India"] * len(df)
)
```

---

## 9. Handling Missing Data

Missing values may be represented by `NaN`, `None`, `pd.NA`, or `NaT`, depending on the column and data type.

### 9.1 Detect missing values

```python
df.isna()
df.isna().sum()
df.isna().sum().sum()
```

### 9.2 Detect non-missing values

```python
df.notna()
```

### 9.3 Remove rows with missing values

```python
df.dropna()
```

Remove rows missing values in specific columns:

```python
df.dropna(subset=["Age", "Salary"])
```

### 9.4 Remove columns containing missing values

```python
df.dropna(axis=1)
```

Use this carefully because it removes every column containing at least one missing value.

### 9.5 Fill missing values with a constant

```python
df["City"] = df["City"].fillna("Unknown")
```

### 9.6 Fill numerical missing values with the mean

```python
df["Age"] = df["Age"].fillna(
    df["Age"].mean()
)
```

### 9.7 Fill with the median

```python
df["Salary"] = df["Salary"].fillna(
    df["Salary"].median()
)
```

The median is often more robust to extreme values than the mean.

### 9.8 Fill with the mode

```python
mode_values = df["Department"].mode()

if not mode_values.empty:
    df["Department"] = df["Department"].fillna(
        mode_values.iloc[0]
    )
```

### 9.9 Forward fill and backward fill

```python
df["Price"] = df["Price"].ffill()
df["Price"] = df["Price"].bfill()
```

- `ffill()`: propagate the previous valid value.
- `bfill()`: propagate the next valid value.

These methods are useful for suitable ordered data, but should not be applied indiscriminately.

### 9.10 Interpolate numerical values

```python
df["Temperature"] = df["Temperature"].interpolate()
```

Interpolation estimates missing values from surrounding observations.

### Important machine learning note

When building predictive models, calculate imputation values using the training data only. Fit the imputer on training data and apply the same fitted transformation to validation and test data to avoid data leakage.

---

## 10. Data Cleaning and Type Conversion

### 10.1 Convert a column to integer

```python
df["Age"] = df["Age"].astype("int64")
```

This fails if the column contains missing values or invalid strings that cannot be converted. Use a nullable integer dtype when missing values must be retained:

```python
df["Age"] = df["Age"].astype("Int64")
```

### 10.2 Convert to string

```python
df["Employee"] = df["Employee"].astype("string")
```

### 10.3 Convert invalid numerical values to missing

```python
df["Salary"] = pd.to_numeric(
    df["Salary"],
    errors="coerce"
)
```

`errors="coerce"` replaces values that cannot be parsed with missing values.

### 10.4 Convert dates

```python
df["Date"] = pd.to_datetime(
    df["Date"],
    errors="coerce"
)
```

### 10.5 Clean column names

```python
df.columns = (
    df.columns
    .str.strip()
    .str.lower()
    .str.replace(" ", "_")
)
```

### 10.6 Remove extra spaces from text

```python
df["Employee"] = df["Employee"].str.strip()
```

### 10.7 Replace inconsistent values

```python
df["Department"] = df["Department"].replace({
    "Information Technology": "IT",
    "Human Resources": "HR"
})
```

### 10.8 Detect duplicates

```python
df.duplicated()
df.duplicated().sum()
```

### 10.9 Remove duplicate rows

```python
df = df.drop_duplicates()
```

Remove duplicates based on selected columns:

```python
df = df.drop_duplicates(
    subset=["Employee", "Department"]
)
```

### 10.10 Replace values

```python
df["Department"] = df["Department"].replace(
    "Information Technology",
    "IT"
)
```

**Important:** Before removing duplicates or replacing values, confirm what counts as a duplicate or equivalent value for your business problem.

---

## 11. String Operations

Pandas provides the `.str` accessor for vectorized text operations.

### 11.1 Convert text to lowercase

```python
df["Employee"] = df["Employee"].str.lower()
```

### 11.2 Convert text to uppercase

```python
df["Employee"] = df["Employee"].str.upper()
```

### 11.3 Remove whitespace

```python
df["Employee"] = df["Employee"].str.strip()
```

### 11.4 Replace text

```python
df["City"] = df["City"].str.replace(
    "Indore City",
    "Indore",
    regex=False
)
```

### 11.5 Check whether text contains a pattern

```python
mask = df["Employee"].str.contains(
    "ri",
    case=False,
    na=False
)
```

### 11.6 Check string prefixes and suffixes

```python
df["Email"].str.startswith("admin", na=False)
df["Email"].str.endswith(".com", na=False)
```

### 11.7 Extract a pattern using regular expressions

```python
df["Domain"] = df["Email"].str.extract(
    r"@([^@]+)$"
)
```

### 11.8 Split text into columns

```python
df[["First_Name", "Last_Name"]] = (
    df["Full_Name"]
    .str.split(" ", n=1, expand
