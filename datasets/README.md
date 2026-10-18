# Sample Datasets

Welcome to the **Sample Datasets** collection of the [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap).

This directory is intended for small, easy-to-understand datasets that help learners practice data loading, data cleaning, exploratory data analysis, statistics, visualization, and introductory machine learning.

These datasets serve as practical examples for following the roadmap from basic Python programming to data analysis and model development.

## Table of Contents

- [Purpose](#purpose)
- [Who Should Use These Datasets?](#who-should-use-these-datasets)
- [Suggested Directory Structure](#suggested-directory-structure)
- [Recommended Sample Datasets](#recommended-sample-datasets)
- [Dataset Format Guide](#dataset-format-guide)
- [Getting Started](#getting-started)
- [Loading Sample Data](#loading-sample-data)
- [Practice Exercises](#practice-exercises)
- [Suggested Learning Path](#suggested-learning-path)
- [Dataset Quality Guidelines](#dataset-quality-guidelines)
- [Using Datasets Responsibly](#using-datasets-responsibly)
- [Additional Dataset Resources](#additional-dataset-resources)
- [Contribution Guidelines](#contribution-guidelines)

---

## Purpose

The `sample/` directory should provide small datasets that learners can use without needing complex infrastructure or extensive preprocessing.

Sample datasets are useful for:

- Learning how to load CSV, JSON, and other data formats.
- Understanding rows, columns, data types, and missing values.
- Practicing filtering, sorting, grouping, and aggregation with Pandas.
- Performing exploratory data analysis.
- Creating charts using Matplotlib and Seaborn.
- Applying descriptive statistics and probability.
- Practicing feature engineering.
- Building introductory machine learning models.
- Learning how to identify data quality issues.
- Testing data-processing scripts and notebooks.

The goal is to make each dataset understandable, reusable, and suitable for hands-on learning.

## Who Should Use These Datasets?

| Learner level | Recommended activities |
|---|---|
| Beginner | Load data, inspect columns, filter rows, calculate summary statistics |
| Intermediate | Clean missing values, group data, visualize distributions, engineer features |
| Advanced beginner in ML | Train baseline regression and classification models |
| Project learner | Combine multiple operations into a small end-to-end analysis |

## Suggested Directory Structure

A possible structure for this directory is:

```text
datasets/
└── sample/
    ├── README.md
    ├── students.csv
    ├── sales.csv
    ├── employees.csv
    ├── house_prices.csv
    ├── iris.csv
    ├── customer_churn.csv
    ├── weather.csv
    ├── products.json
    └── data_dictionary.md
```

This is a **suggested structure**. Add only the dataset files that you actually create or maintain in the repository.

For larger datasets, prefer providing download instructions and source links rather than committing large files to Git.

## Recommended Sample Datasets

The following datasets are suitable candidates for this directory. They are recommendations, not a list of files confirmed to exist in the repository.

| Dataset | Format | Main concepts | Suggested difficulty |
|---|---|---|---|
| Students | CSV | Filtering, averages, grouping, missing values | Beginner |
| Sales | CSV | Revenue, aggregation, time-based analysis | Beginner |
| Employees | CSV | Grouping, categorical data, summary statistics | Beginner |
| Products | JSON | Nested data, JSON parsing, data transformation | Beginner |
| House Prices | CSV | Correlation, regression, feature engineering | Intermediate |
| Iris | CSV or built-in dataset | Classification, visualization, model evaluation | Beginner–Intermediate |
| Customer Churn | CSV | Classification, class imbalance, evaluation metrics | Intermediate |
| Weather | CSV | Dates, time series, trends, missing values | Beginner–Intermediate |

### Recommended File Documentation

For each dataset, document:

- File name and format.
- Purpose and learning objectives.
- Column names and data types.
- Units for numeric fields.
- Meaning of categorical values.
- Missing-value conventions.
- Source and license, if applicable.
- Known limitations and recommended exercises.

For example, a `students.csv` dataset might contain `student_id`, `age`, `attendance_percentage`, `study_hours`, and `exam_score`. Clearly label synthetic datasets so learners do not mistake generated examples for real observations.

## Dataset Format Guide

| Format | Common extension | Best use |
|---|---|---|
| CSV | `.csv` | Tabular data and introductory analysis |
| JSON | `.json` | Structured records and nested objects |
| Excel | `.xlsx` | Worksheets and business-style tabular data |
| Parquet | `.parquet` | Column-oriented analytics and larger tables |
| TXT | `.txt` | Plain text and simple unstructured data |
| SQLite | `.db` | Relational tables and SQL practice |

For beginner exercises, CSV is usually a practical default because it is readable in a text editor and supported by most data analysis tools.

## Getting Started

Clone the repository if you have not already done so:

```bash
git clone https://github.com/cubicnova/Data-Science-Roadmap.git
cd Data-Science-Roadmap
```

Create and activate a virtual environment.

**Windows PowerShell:**

```powershell
python -m venv .venv
.\.venv\Scripts\Activate.ps1
```

**Linux or macOS:**

```bash
python3 -m venv .venv
source .venv/bin/activate
```

Install the libraries needed for the exercises:

```bash
python -m pip install pandas numpy matplotlib seaborn scikit-learn jupyter
```

Start Jupyter Notebook:

```bash
jupyter notebook
```

Open a notebook in the repository and load the dataset using a path relative to the repository root.

## Loading Sample Data

### 1. Load a CSV file

```python
import pandas as pd

df = pd.read_csv("datasets/sample/students.csv")

print(df.head())
print(df.shape)
print(df.info())
print(df.describe())
```

### 2. Inspect missing values

```python
missing_values = df.isnull().sum()

print(missing_values)
```

### 3. Select columns

```python
# Replace these column names with columns
# that actually exist in your dataset.
selected_columns = df[["age", "exam_score"]]

print(selected_columns.head())
```

### 4. Filter rows

```python
# Example: select students with an exam score above 80.
high_scores = df[df["exam_score"] > 80]

print(high_scores)
```

### 5. Group and aggregate

```python
# Example: calculate the average exam score by class.
average_scores = (
    df.groupby("class")["exam_score"]
      .mean()
      .reset_index()
)

print(average_scores)
```

The examples above assume that the corresponding files and columns exist. Adapt the paths and column names to match the dataset you use.

### 6. Create a visualization

```python
import matplotlib.pyplot as plt

df["exam_score"].plot(
    kind="hist",
    bins=10,
    title="Distribution of Exam Scores"
)

plt.xlabel("Exam Score")
plt.ylabel("Number of Students")
plt.tight_layout()
plt.show()
```

### 7. Load a JSON file

```python
import pandas as pd

products = pd.read_json("datasets/sample/products.json")

print(products.head())
print(products.info())
```

For deeply nested JSON, inspect the original structure and consider `pandas.json_normalize()`.

## Practice Exercises

Use the sample datasets to complete the following tasks.

### Exercise 1: Data Inspection

- [ ] Load a dataset using Pandas.
- [ ] Display the first and last five rows.
- [ ] Identify the number of rows and columns.
- [ ] List all column names.
- [ ] Inspect the data types.
- [ ] Identify missing values and duplicate rows.

### Exercise 2: Data Cleaning

- [ ] Remove duplicate records where appropriate.
- [ ] Handle missing values using a justified strategy.
- [ ] Convert date columns into datetime format.
- [ ] Correct inconsistent category labels.
- [ ] Validate numeric ranges.
- [ ] Document each cleaning decision.

### Exercise 3: Exploratory Data Analysis

- [ ] Calculate mean, median, and standard deviation.
- [ ] Identify the most frequent category.
- [ ] Group observations by a relevant category.
- [ ] Calculate totals and averages.
- [ ] Identify unusual values and potential outliers.
- [ ] Write three observations supported by the data.

### Exercise 4: Data Visualization

- [ ] Create a histogram.
- [ ] Create a bar chart.
- [ ] Create a scatter plot for two numeric variables.
- [ ] Create a box plot.
- [ ] Add meaningful titles and axis labels.
- [ ] Explain what each chart reveals and what it cannot establish.

### Exercise 5: Introductory Machine Learning

Using a suitable labeled dataset:

- [ ] Define the prediction target.
- [ ] Separate input features from the target.
- [ ] Split the data into training and testing sets.
- [ ] Train a simple baseline model.
- [ ] Evaluate it with appropriate metrics.
- [ ] Compare results against a meaningful baseline.
- [ ] Document limitations and possible improvements.

Avoid using a dataset for supervised learning unless its target and intended prediction task are clearly defined.

## Suggested Learning Path

Follow this sequence to connect the datasets with the broader roadmap.

| Stage | Task | Skills developed |
|---|---|---|
| 1 | Load `students.csv` | Python, Pandas, data types |
| 2 | Analyze `sales.csv` | Aggregation, statistics, visualization |
| 3 | Clean `employees.csv` | Missing values, duplicates, categorical data |
| 4 | Parse `products.json` | JSON handling and data transformation |
| 5 | Explore `house_prices.csv` | Correlation, regression, feature engineering |
| 6 | Classify the Iris dataset | Classification and model evaluation |
| 7 | Analyze `customer_churn.csv` | Classification, imbalance, metrics |
| 8 | Analyze `weather.csv` | Datetime operations and time-series patterns |

Dataset names in this table are examples for planning. Use the actual available files in the repository or obtain the recommended datasets from their original sources.

## Dataset Quality Guidelines

A useful educational dataset should have:

1. **Clear structure:** Understandable column names and consistent types.
2. **Documented meaning:** Each feature and unit is explained.
3. **A learning objective:** Exercises explain what learners should discover.
4. **Appropriate size:** Files are small enough for a beginner's laptop.
5. **Reproducibility:** Data and preparation steps are documented.
6. **Realistic challenges:** Include data quality issues only when they serve a learning purpose.
7. **Transparent provenance:** Record where data came from and how it was created.
8. **No accidental leakage:** Do not include information that improperly reveals the intended prediction target.

For synthetic datasets, document the generation process and any assumptions used to produce the records.

## Using Datasets Responsibly

Before adding or using a dataset:

- Verify its license and permitted uses.
- Attribute the original source when required.
- Avoid committing confidential, sensitive, or personally identifiable information.
- Use synthetic data for exercises when real personal data is unnecessary.
- Document sampling and preprocessing decisions.
- Do not claim that synthetic or illustrative data represents real-world populations.
- Follow repository licensing and third-party dataset terms.

## Additional Dataset Resources

Use these resources to find suitable datasets when a sample file is not available locally.

| Resource | Description | Link |
|---|---|---|
| Kaggle Datasets | Community datasets and competitions | [Explore datasets](https://www.kaggle.com/datasets) |
| UCI Machine Learning Repository | Research and educational datasets | [Browse datasets](https://archive.ics.uci.edu/) |
| Google Dataset Search | Search for datasets published across the web | [Search datasets](https://datasetsearch.research.google.com/) |
| scikit-learn datasets | Built-in toy datasets and dataset-loading utilities | [Documentation](https://scikit-learn.org/stable/datasets.html) |
| Hugging Face Datasets | Datasets for NLP, computer vision, audio, and more | [Explore datasets](https://huggingface.co/datasets) |
| Open Government Data Platform India | Public datasets from Indian government sources | [Explore data](https://www.data.gov.in/) |

Always check the dataset's source, terms, and documentation before downloading or redistributing it.

## Contribution Guidelines

Contributions that improve the educational value of this directory are welcome.

When adding a dataset:

1. Use a descriptive, lowercase file name where practical.
2. Prefer common formats such as CSV or JSON for introductory exercises.
3. Add a short description and a data dictionary.
4. Document the source, license, and any transformations.
5. Include a small example or notebook when useful.
6. Check for invalid records, accidental sensitive information, and unnecessary file size.
7. Update this README to reflect the files actually added.

### Suggested Pull Request Checklist

- [ ] Dataset purpose and learning objectives are documented.
- [ ] Column definitions and units are available.
- [ ] Source and license have been checked.
- [ ] No confidential or unnecessary personal data is included.
- [ ] Dataset can be loaded with the documented instructions.
- [ ] README links and file paths have been verified.
- [ ] Exercises match the actual dataset schema.

---

**Next step:** Add the dataset files you want to maintain in this folder, then update the directory listing and loading examples to match those exact files. This will keep the sample dataset collection consistent with the CubicNova Data Science Roadmap.

