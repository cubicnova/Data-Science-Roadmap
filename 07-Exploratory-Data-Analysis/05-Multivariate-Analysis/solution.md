# Solutions — Multivariate Analysis

These are reference solutions; other approaches can be valid. Focus on sound interpretation, not only producing output.

## Foundations

### 1. Levels of analysis
- Univariate: distribution of exam scores.
- Bivariate: study hours versus exam score.
- Multivariate: exam score alongside study hours, attendance, prior score, and program.

### 2. Example questions
- How are study hours, attendance, and prior score associated with exam score, and does the pattern differ by program?
- Do result bands vary by program and study group?

### 3. More variables are not always better
Extra encodings can create clutter and overlapping points. Include variables only when they answer the question and can be interpreted accurately.

### 4. Association, interaction, confounding
Association means variables vary together. An interaction means the association changes across levels of another variable. Confounding occurs when a third variable related to both exposure and outcome distorts the observed association.

### 5. Unit of observation
If rows represent students, each row is a student. If rows represent student-course attempts, a student may appear many times and observations may not be independent.

## Preparation and summaries

### 6. Inspect structure
```python
print(df.shape)
print(df.dtypes)
display(df.head())
```

### 7–8. Pearson and Spearman
```python
numeric = df.select_dtypes(include="number").drop(columns=["student_id"], errors="ignore")
pearson_corr = numeric.corr(method="pearson")
spearman_corr = numeric.corr(method="spearman")
display(pearson_corr)
display(spearman_corr)
```
Spearman is useful for rank-based monotonic relationships that may not be linear. Neither method proves causation.

### 9. Group summary
```python
df.groupby("program")["exam_score"].agg(
    count="count", mean="mean", median="median", std="std"
).sort_values("mean", ascending=False)
```

### 10. Pivot table
```python
df.pivot_table(values="exam_score", index="program", columns="study_group", aggfunc="mean")
```
Review group counts too; empty or small cells make means unstable.

### 11–12. Cross-tabulation and row percentages
```python
counts = pd.crosstab(df["program"], df["result_band"], dropna=False)
row_pct = pd.crosstab(df["program"], df["result_band"], normalize="index", dropna=False).mul(100).round(1)
display(counts)
display(row_pct)
```
Each row percentage is relative to the records in that program represented in the table.

### 13. Missingness by program
```python
df.groupby("program").apply(lambda g: g.isna().mean().mul(100), include_groups=False)
```
If your pandas version does not support `include_groups`, select columns before grouping or calculate the rates in a loop. Missingness rates do not explain why values are missing.

## Visualization

### 14. Heatmap
```python
import seaborn as sns
import matplotlib.pyplot as plt
corr = df.select_dtypes(include="number").drop(columns=["student_id"], errors="ignore").corr()
sns.heatmap(corr, annot=True, fmt=".2f", cmap="coolwarm", vmin=-1, vmax=1, center=0)
plt.title("Pearson correlation matrix")
plt.tight_layout()
plt.show()
```

### 15. Pair plot
```python
sns.pairplot(df[["study_hours", "attendance_pct", "prior_score", "exam_score", "program"]], hue="program", corner=True)
plt.show()
```

### 16. Scatter plot with a third variable
```python
sns.scatterplot(data=df, x="study_hours", y="exam_score", hue="program", alpha=0.8)
plt.tight_layout()
plt.show()
```
Use facets or point size only if the plot remains legible.

### 18. Box plot
```python
sns.boxplot(data=df, x="program", y="exam_score", hue="study_group")
plt.tight_layout()
plt.show()
```
A box plot summarizes distributions but can hide individual points and small group sizes.

### 19. Faceted scatter plot
```python
g = sns.relplot(data=df, x="study_hours", y="exam_score", col="program", kind="scatter", height=3.5)
g.set_axis_labels("Study hours per week", "Exam score")
plt.show()
```

### 20–21. Categorical visualization and cautious description
Use a heatmap of a pivot table or small-multiple bar charts for three categorical variables. Example description: “In this sample, higher reported study hours tend to co-occur with higher exam scores; this association does not establish causation.”

## Interpretation and diagnostics

### 22–23. Unique correlation pairs and threshold screening
```python
corr = df.select_dtypes(include="number").drop(columns=["student_id"], errors="ignore").corr()
upper = corr.where(np.triu(np.ones(corr.shape), k=1).astype(bool))
pairs = upper.stack().rename("correlation").reset_index()
pairs.columns = ["variable_1", "variable_2", "correlation"]
pairs["absolute_correlation"] = pairs["correlation"].abs()
display(pairs.sort_values("absolute_correlation", ascending=False))
display(pairs[pairs["absolute_correlation"] > 0.75])
```
The 0.75 threshold is a review flag, not a universal cutoff.

### 24. Within-program relationship
```python
rows = []
for program_name, group in df.groupby("program"):
    valid = group[["study_hours", "exam_score"]].dropna()
    rows.append({"program": program_name, "n": len(valid), "pearson_r": valid["study_hours"].corr(valid["exam_score"])})
pd.DataFrame(rows)
```
Always report the sample size with each coefficient.

### 25. Aggregation and group structure
Groups can have different baselines, ranges, or compositions. Compare subgroup plots and summaries; do not assume a pooled pattern applies equally to every group.

### 26. Group-size balance
```python
pd.crosstab(df["program"], df["study_group"], margins=True)
```
Small or imbalanced groups produce noisier estimates and can cause pooled summaries to be dominated by large groups.

### 27. Potential confounders
Examples: prior knowledge, course difficulty, motivation, learning resources, attendance, health, or time available to study. Some may be unavailable or measured imperfectly.

### 28. Nonlinear relationships
Pearson measures linear association. A symmetric U-shaped relationship can have Pearson correlation near zero despite a strong nonlinear pattern.

### 29. Multicollinearity
Highly overlapping predictors can make regression coefficients unstable and standard errors larger. Correlation is a screening method; consider model purpose, domain knowledge, and VIF where appropriate.

### 30. Missing-data effects
Pairwise correlations may use different record subsets. Complete-case analysis can disproportionately exclude groups. Report sample sizes and inspect missingness patterns.

### 31. Many correlations
Limitations include false positives from multiple comparisons, unstable small-sample estimates, outlier sensitivity, nonlinearity, confounding, and selection bias. Use domain-informed hypotheses and validate findings on independent data when possible.

## Practical coding

### 32. Validate columns
```python
def correlation_matrix(data, columns, method="pearson"):
    missing = [c for c in columns if c not in data.columns]
    if missing:
        raise KeyError(f"Columns not found: {missing}")
    non_numeric = [c for c in columns if not pd.api.types.is_numeric_dtype(data[c])]
    if non_numeric:
        raise TypeError(f"Columns must be numeric: {non_numeric}")
    return data[columns].corr(method=method)

correlation_matrix(df, ["study_hours", "attendance_pct", "exam_score"])
```

### 33. Tidy correlation table
Use the `upper.stack()` approach shown in solutions 22–23 to remove diagonal and mirrored pairs.

### 34–36. Group summaries and category tables
Use `groupby(...).agg(...)`, `pd.crosstab(...)`, and `normalize="index"` as shown above. For visualizations, show individual observations when practical and use transparency or sampling for large datasets.

### 37. Missing vs zero
Missing means unknown or unavailable; zero is a recorded value. Zero study hours can be meaningful, whereas missing study hours are unknown.

### 38. Checks before trusting results
Confirm row grain; check duplicates, missingness, group sizes, units, ranges, outliers, nonlinear patterns, and category consistency. Report the method and sample size.

## Mini-project (questions 39–42)

Suggested report structure:
1. **Question:** Which variables are associated with exam score in this synthetic sample?
2. **Data:** Describe row grain, sample size, variables, and missingness.
3. **Methods:** Correlation matrix, pair/scatter plot, and group summary or cross-tabulation.
4. **Findings:** State at least three patterns backed by the notebook outputs.
5. **Interaction/confounding:** Describe a possible group-specific pattern and a plausible confounder.
6. **Limitations:** Synthetic data, observational relationships, measurement limits, and small subgroup sizes.
7. **Next steps:** Collect relevant confounders, inspect nonlinear patterns, quantify uncertainty, or validate on independent data.

Do not copy conclusions from a chart without checking the actual output.

## Mini-project rubric (10 points)

| Criterion | Points |
|---|---:|
| Question, data grain, and variable definitions | 2 |
| Appropriate multivariate methods | 2 |
| Two clear visualizations and a grouped summary | 2 |
| Interpretation and interaction/confounding discussion | 2 |
| Limitations and actionable next steps | 2 |
