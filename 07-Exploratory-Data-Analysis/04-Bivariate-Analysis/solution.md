# Solutions — Bivariate Analysis

These are example approaches, not the only acceptable answers. Run the notebook first and use its output for numeric findings. Because the dataset is synthetic, it is suitable for learning code and interpretation—not for making claims about real students.

## 1–4. Select methods and inspect data

- Numerical + numerical: scatter plot and correlation.
- Numerical + categorical: grouped summary and box plot.
- Categorical + categorical: cross-tabulation and stacked bar chart.

Check missingness for the selected columns:

```python
cols = ["study_hours", "exam_score", "attendance_pct"]
df[cols].isna().sum()
```

Use a pairwise complete sample when calculating an association between two columns:

```python
pair = df[["study_hours", "exam_score"]].dropna()
print("Pairwise sample size:", len(pair))
```

## 5–12. Numerical vs numerical

Scatter plot:

```python
import matplotlib.pyplot as plt
import seaborn as sns

sns.scatterplot(data=df, x="study_hours", y="exam_score")
plt.title("Study Hours vs Exam Score")
plt.xlabel("Study hours per week")
plt.ylabel("Exam score (out of 100)")
plt.tight_layout()
plt.show()
```

Attendance and exam score with a trend line:

```python
pair_attendance = df[["attendance_pct", "exam_score"]].dropna()
sns.regplot(data=pair_attendance, x="attendance_pct", y="exam_score")
plt.title("Attendance vs Exam Score")
plt.xlabel("Attendance (%)")
plt.ylabel("Exam score (out of 100)")
plt.tight_layout()
plt.show()
```

Correlation:

```python
pair = df[["study_hours", "exam_score"]].dropna()
pearson_r = pair["study_hours"].corr(pair["exam_score"], method="pearson")
spearman_r = pair["study_hours"].corr(pair["exam_score"], method="spearman")
print("Pearson:", pearson_r)
print("Spearman:", spearman_r)
```

Pearson captures linear association. Spearman uses ranks and captures monotonic association. Their values may differ if the pattern is nonlinear, contains outliers, or has a few influential observations. Neither coefficient establishes causation.

Covariance:

```python
pair = df[["attendance_pct", "exam_score"]].dropna()
pair.cov()
```

Covariance's magnitude depends on the units of both variables, so correlation is usually easier for comparing association strength.

Potentially influential observations can be examined with a scatter plot and source checks:

```python
display(df[["student_id", "study_hours", "attendance_pct", "exam_score"]]
        .sort_values("study_hours", ascending=False).head(10))
```

Do not remove a point just because it changes the coefficient. Verify whether it is erroneous and document any exclusion.

## 13–18. Numerical vs categorical

Grouped summaries:

```python
department_scores = (
    df.groupby("department", observed=True)["exam_score"]
      .agg(count="count", mean="mean", median="median", std="std")
      .sort_values("median", ascending=False)
)
department_scores.round(2)
```

Box plot:

```python
sns.boxplot(data=df, x="department", y="exam_score")
plt.title("Exam Score Distribution by Department")
plt.xlabel("Department")
plt.ylabel("Exam score")
plt.xticks(rotation=25)
plt.tight_layout()
plt.show()
```

Study mode:

```python
study_mode_scores = (
    df.groupby("study_mode", observed=True)["exam_score"]
      .agg(count="count", mean="mean", median="median", std="std")
      .sort_values("median", ascending=False)
)
display(study_mode_scores.round(2))

sns.boxplot(data=df, x="study_mode", y="exam_score")
plt.title("Exam Scores by Study Mode")
plt.xlabel("Study mode")
plt.ylabel("Exam score")
plt.xticks(rotation=20)
plt.tight_layout()
plt.show()
```

Mean and median provide complementary information. The mean can be influenced by extreme scores, while the median is more robust. Group sizes and spread help determine whether apparent differences are stable or driven by a few observations.

These are descriptive comparisons. Students may select study modes or departments for reasons related to prior preparation, resources, motivation, or course difficulty. The plot alone cannot identify a causal effect.

## 19–23. Categorical vs categorical

Cross-tabulation:

```python
counts = pd.crosstab(df["department"], df["scholarship"], margins=True)
counts
```

Row percentages:

```python
row_percent = pd.crosstab(
    df["department"], df["scholarship"], normalize="index"
).mul(100).round(2)
row_percent
```

Stacked bar chart:

```python
ct = pd.crosstab(df["department"], df["scholarship"])
ct.plot(kind="bar", stacked=True)
plt.title("Scholarship Status by Department")
plt.xlabel("Department")
plt.ylabel("Number of students")
plt.xticks(rotation=25)
plt.tight_layout()
plt.show()
```

Calculate the scholarship share and show the denominator:

```python
scholarship_share = pd.crosstab(
    df["department"], df["scholarship"], normalize="index"
).mul(100)

scholarship_counts = pd.crosstab(df["department"], df["scholarship"])
result = pd.DataFrame({
    "scholarship_yes_percent": scholarship_share.get("Yes"),
    "students_in_department": scholarship_counts.sum(axis=1),
    "scholarship_yes_count": scholarship_counts.get("Yes")
}).sort_values("scholarship_yes_percent", ascending=False)

result.round(2)
```

A percentage based on a small group can fluctuate substantially with only one or two records. Always display the count or denominator alongside the percentage.

## 24. Correlation matrix

```python
numeric_pair = df[["study_hours", "attendance_pct", "exam_score"]]
corr = numeric_pair.corr(method="pearson")
display(corr.round(2))

sns.heatmap(corr, annot=True, fmt=".2f", center=0, cmap="coolwarm")
plt.title("Correlation Matrix")
plt.tight_layout()
plt.show()
```

The matrix summarizes pairwise linear associations among the selected numerical variables. Inspect scatter plots as well: a coefficient can hide clusters, outliers, or nonlinear patterns.

## 25–28. Reporting findings

Use the output from your notebook to fill in values. A responsible report might state:

- “The observed Pearson correlation between **[variable A]** and **[variable B]** is **[r]** for **[n]** complete pairs, indicating **[direction and approximate strength]** of linear association in this sample.”
- “The median exam score for **[group]** is **[value]**, compared with **[value]** for **[other group]**. Group sizes are **[counts]**.”
- “The share of scholarship recipients is **[percentage]%** in **[department]**, based on **[count]** recipients among **[denominator]** students.”

Possible confounders for study-hours and exam-score patterns include prior knowledge, course difficulty, attendance, access to learning resources, health, and motivation. These are possibilities to investigate, not variables whose effects can be assumed from this dataset.

A good summary reports the question, methods, sample size, results, missingness, and limitations. It should explicitly say that the data is synthetic and that observed association is not causal proof.

## 29. Optional method-selection helper

```python
def bivariate_quick_check(data, x, y):
    pair = data[[x, y]].dropna()
    x_numeric = pd.api.types.is_numeric_dtype(pair[x])
    y_numeric = pd.api.types.is_numeric_dtype(pair[y])

    result = {"columns": (x, y), "pairwise_n": len(pair)}
    if x_numeric and y_numeric:
        result["type"] = "numerical-numerical"
        result["pearson"] = pair[x].corr(pair[y], method="pearson")
        result["spearman"] = pair[x].corr(pair[y], method="spearman")
    elif x_numeric != y_numeric:
        result["type"] = "numerical-categorical"
        category_col = y if x_numeric else x
        numeric_col = x if x_numeric else y
        result["group_summary"] = pair.groupby(category_col, observed=True)[numeric_col].agg(
            ["count", "mean", "median", "std"]
        )
    else:
        result["type"] = "categorical-categorical"
        result["cross_tab"] = pd.crosstab(pair[x], pair[y])
        result["row_percent"] = pd.crosstab(pair[x], pair[y], normalize="index")
    return result

bivariate_quick_check(df, "study_hours", "exam_score")
```

This helper is intentionally basic. For production use, handle dates, identifiers, ordinal variables, low sample sizes, missing categories, and plotting separately.

## 30–32. Additional investigation

Compare Pearson and Spearman on the same complete pairs. If excluding an observation, define a defensible rule and report results with and without it rather than hiding the difference.

For subgroup exploration:

```python
g = sns.lmplot(
    data=df, x="study_hours", y="exam_score",
    col="department", col_wrap=2, height=3.5,
    scatter_kws={"alpha": 0.7}, ci=None
)
g.set_axis_labels("Study hours per week", "Exam score")
g.fig.suptitle("Study Hours vs Exam Score by Department", y=1.02)
plt.show()
```

Different subgroup patterns may reflect sample size or other differences. Treat this as exploratory and avoid causal conclusions.
