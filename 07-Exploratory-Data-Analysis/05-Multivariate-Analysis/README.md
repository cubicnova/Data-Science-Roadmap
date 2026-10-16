# 05 — Multivariate Analysis

> **Exploratory Data Analysis (EDA) · Data Science Roadmap**

Multivariate analysis explores how three or more variables behave together. It helps reveal patterns, interactions, clusters, confounding relationships, and unusual observations that may be hidden in one-variable or pairwise analysis. This module focuses on exploratory analysis with Python, pandas, NumPy, Matplotlib, and Seaborn—not on proving causation.

## Learning objectives

- Distinguish univariate, bivariate, and multivariate analysis.
- Inspect numerical and categorical variables together.
- Calculate and interpret Pearson and Spearman correlations.
- Visualize relationships with heatmaps, pair plots, scatter plots, and facets.
- Compare multiple groups using groupby summaries, pivot tables, and cross-tabulations.
- Explore interactions, confounding, outliers, missingness, and multicollinearity.
- Communicate findings with limitations and useful next steps.

## Where this module fits

1. Define the business question and understand the dataset.
2. Explore individual variables (univariate analysis).
3. Explore pairs of variables (bivariate analysis).
4. **Explore several variables together (this module).**
5. Validate patterns, communicate findings, and decide whether further analysis or modeling is needed.

## Core concepts

### Levels of analysis

- **Univariate:** one variable, such as the distribution of exam scores.
- **Bivariate:** two variables, such as study hours versus exam score.
- **Multivariate:** three or more variables, such as exam score with study hours, attendance, prior score, and program.

### Correlation

Pearson correlation measures linear association; Spearman correlation measures rank-based monotonic association.

```python
numeric = df.select_dtypes(include="number")
pearson_corr = numeric.corr(method="pearson")
spearman_corr = numeric.corr(method="spearman")
```

Remember: correlation is not causation; Pearson can miss nonlinear patterns; outliers can strongly affect correlations; subgroup patterns can differ from pooled patterns; and correlation among predictors may complicate model interpretation. Do not correlate arbitrary integer encodings of nominal categories.

### Correlation heatmaps

```python
import seaborn as sns
import matplotlib.pyplot as plt

corr = df.select_dtypes(include="number").corr()
sns.heatmap(corr, annot=True, cmap="coolwarm", vmin=-1, vmax=1, center=0)
plt.title("Pearson correlation matrix")
plt.tight_layout()
plt.show()
```

For many columns, hide annotations, show one triangle, or select variables relevant to the question.

### Pair plots and scatter plots

```python
sns.pairplot(
    df[["study_hours", "attendance_pct", "prior_score", "exam_score", "program"]],
    hue="program", corner=True
)
```

Pair plots can become slow or crowded. Choose a useful subset and sample reproducibly when needed. Use color, size, or facets to add a third variable only when the plot remains readable.

### Grouped summaries and pivot tables

```python
summary = df.groupby(["program", "study_group"])["exam_score"].agg(
    ["count", "mean", "median", "std"]
)
pivot = df.pivot_table(
    values="exam_score", index="program", columns="study_group", aggfunc="mean"
)
```

Check group sizes: a mean based on a handful of observations is less stable than one based on a large sample.

### Categorical relationships

```python
counts = pd.crosstab(df["program"], df["result_band"])
row_percent = pd.crosstab(
    df["program"], df["result_band"], normalize="index"
).mul(100).round(1)
```

Row percentages answer “within each program, what proportion belongs to each result band?” State the denominator in your report.

### Interactions and confounding

An **interaction** occurs when the relationship between one variable and an outcome differs by another variable. A **confounder** is related to both an exposure and an outcome and can distort the observed association. Compare subgroup plots and summaries, inspect sample sizes, and note unavailable variables. Stratifying or adjusting for a variable does not by itself prove causation.

### Multicollinearity

Multicollinearity occurs when predictors contain overlapping information. It can make regression coefficients unstable or difficult to interpret. A correlation matrix is a screening tool, not a complete diagnosis; a variance inflation factor (VIF) can help in suitable regression settings.

## Practical workflow

1. State the question and define the outcome(s).
2. Confirm row grain, meanings, units, and valid ranges.
3. Select relevant variables; inspect missingness, outliers, and group sizes.
4. Combine numerical summaries with complementary plots.
5. Compare patterns across meaningful subgroups.
6. Consider nonlinearity, confounding, selection effects, and multiple comparisons.
7. Separate observations from hypotheses; document limitations and next steps.

| Question | Useful technique | Main caution |
|---|---|---|
| Which numeric variables move together? | Correlation matrix / heatmap | Pearson misses some nonlinear patterns |
| How do several numeric variables relate? | Pair plot | Can be cluttered |
| Does an outcome vary across two categories? | Grouped summary / pivot | Check sample sizes |
| How do category combinations appear? | Cross-tabulation | State denominator |
| Does a pattern differ across groups? | Hue, facets, stratified summaries | Confounding may explain differences |
| Are predictors redundant? | Correlation review, VIF | Correlation alone is not a full diagnosis |

## Setup and files

Install dependencies:

```bash
python -m pip install pandas numpy matplotlib seaborn jupyter
```

Open `examples.ipynb` in Jupyter Notebook, JupyterLab, or VS Code. The notebook generates synthetic data locally; no download is required.

- `README.md` — concepts and workflow
- `examples.ipynb` — runnable guided examples
- `practice.md` — progressive exercises and mini-project
- `solution.md` — reference answers and interpretation guidance

**Important:** multivariate analysis is exploratory. Patterns can suggest explanations but do not establish cause and effect. Report sample sizes, definitions, uncertainty, and limitations.
