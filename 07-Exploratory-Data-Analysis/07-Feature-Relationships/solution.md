# Solutions — Feature Relationships

These are reference solutions. Alternative implementations are acceptable if they are correct and well explained. Correlation is an exploratory measure, not proof of causation or a guarantee of predictive value.

## Foundations

### 1. Feature-to-target vs feature-to-feature
Feature-to-target asks whether an input feature is associated with the outcome. Feature-to-feature asks whether two input features overlap or relate to each other, which can reveal redundancy or multicollinearity.

### 2. Relationship types
Positive: both variables tend to increase together. Negative: one tends to decrease as the other increases. Weak: little association under the selected measure. Nonlinear: a curved pattern such as a U-shape may exist despite low Pearson correlation.

### 3. Pearson r = -0.82
It indicates a strong negative linear association in the analyzed sample. It does not prove causation, rule out confounding, or imply that the relationship will generalize to every population.

### 4. Pearson, Spearman, Kendall
Pearson measures linear association; Spearman measures monotonic association using ranks; Kendall measures rank concordance and can be useful for ordinal data. Select based on variable meaning and pattern, not solely on a normality rule.

### 5. Nominal categories
Numeric labels such as city A=1, city B=2, city C=3 imply arbitrary order and distances. Use categorical summaries, suitable encodings, or methods designed for categorical data instead.

## Data inspection and correlation

### 6. Inspect the data
```python
print(df.shape)
print(df.dtypes)
display(df.head())
display(df.isna().sum().sort_values(ascending=False))
```

### 7. Pearson correlation
```python
numeric = df.select_dtypes(include="number").drop(columns=["student_id"], errors="ignore")
pearson_corr = numeric.corr(method="pearson")
display(pearson_corr)
```
Identifiers should not be treated as meaningful numeric quantities even if stored numerically.

### 8. Spearman correlation
```python
spearman_corr = numeric.corr(method="spearman")
display(spearman_corr)
```
Differences can occur when relationships are monotonic but not linear or when ranks reduce sensitivity to extreme numeric spacing. Interpret both alongside plots.

### 9. Data dictionary
Example: `student_id` = unique identifier; `program` = nominal category; `study_group` = ordinal-like category if its order is defined; `study_hours` = hours/week; `attendance_pct` = percentage; `prior_score` and `exam_score` = scores; `result_band` = ordered outcome category. `exam_score` is the illustrative target.

### 10. Unique pairs sorted by absolute correlation
```python
corr = numeric.corr()
upper = corr.where(np.triu(np.ones(corr.shape), k=1).astype(bool))
pairs = (
    upper.stack()
    .rename("correlation")
    .reset_index()
    .rename(columns={"level_0": "feature_a", "level_1": "feature_b"})
)
pairs["abs_correlation"] = pairs["correlation"].abs()
pairs.sort_values("abs_correlation", ascending=False)
```

### 11. Threshold above 0.75
```python
pairs[pairs["abs_correlation"] > 0.75]
```
This is a screening rule only. The appropriate threshold depends on the domain, model, sample size, and purpose.

### 12. Features associated with target
```python
target_corr = numeric.corr(method="spearman")["exam_score"].drop("exam_score")
target_corr.reindex(target_corr.abs().sort_values(ascending=False).index)
```
Report sign, magnitude, method, sample size, and caveats. Correlation alone does not establish feature importance.

### 13. Why correlation is not enough
It can miss nonlinear patterns, reflect confounding, be distorted by outliers, or be driven by leakage. Predictive usefulness should be assessed with a validation design appropriate to the task.

## Visualization

### 14. Scatter plot
```python
sns.scatterplot(data=df, x="study_hours", y="exam_score", hue="program", alpha=0.75)
plt.title("Study hours and exam score by program")
plt.tight_layout()
plt.show()
```

### 15. Heatmap
```python
corr = numeric.corr()
sns.heatmap(corr, annot=True, fmt=".2f", vmin=-1, vmax=1, center=0, cmap="coolwarm")
plt.title("Pearson correlation matrix")
plt.tight_layout()
plt.show()
```

### 16. Pair plot
```python
sns.pairplot(
    df[["study_hours", "attendance_pct", "prior_score", "exam_score", "program"]],
    hue="program", corner=True
)
plt.show()
```
Choose a subset of variables to keep the plot readable.

### 17. Box plot
```python
sns.boxplot(data=df, x="program", y="exam_score")
plt.title("Exam-score distribution by program")
plt.tight_layout()
plt.show()
```

### 18. Compare program and study group
```python
sns.pointplot(data=df, x="program", y="exam_score", hue="study_group", errorbar=None)
plt.title("Mean exam score by program and study group")
plt.xticks(rotation=20)
plt.tight_layout()
plt.show()
```
For inferential claims, add appropriate uncertainty estimates and account for the study design.

### 19. Group-specific relationship
```python
g = sns.relplot(
    data=df, x="study_hours", y="exam_score",
    col="program", kind="scatter", height=3.5, alpha=0.7
)
g.set_axis_labels("Study hours per week", "Exam score")
plt.show()
```

## Mixed-type relationships

### 20. Grouped statistics
```python
df.groupby("program")["exam_score"].agg(
    count="count", mean="mean", median="median", std="std"
)
```

### 21. Cross-tab counts and row percentages
```python
counts = pd.crosstab(df["program"], df["result_band"], dropna=False)
row_pct = pd.crosstab(
    df["program"], df["result_band"], normalize="index", dropna=False
).mul(100).round(1)
display(counts)
display(row_pct)
```
Row-normalized percentages describe the result-band distribution within each program. Column-normalized percentages describe program composition within each result band.

## Redundancy and modeling

### 22. Multicollinearity
Predictors contain overlapping information. In linear models this can make coefficients unstable and inflate standard errors, making individual coefficients harder to interpret. It does not necessarily ruin predictive performance.

### 23. Why not automatically drop a correlated feature?
It may add domain-relevant information, be more reliable or cheaper to collect, work better with a particular model, or have a different relationship in future data. Check leakage, validation performance, stability, and interpretability before deciding.

### 24. Leakage and validation
Feature selection and preprocessing must be learned using training data only. Using validation/test information to select features can make reported performance optimistically biased. Use a pipeline and cross-validation when appropriate.

### 25. Target leakage example
Predicting whether a loan defaults using a column populated only after the loan has defaulted, such as a collections status recorded afterward. The feature would not be available at prediction time.

## Interpretation

### 26. Non-causal description
Example: “In this synthetic sample, students reporting more study hours generally tend to have higher exam scores; this pattern alone does not show that study hours caused the score difference.”

### 27. Possible confounders
Prior preparation, motivation, course difficulty, access to resources, health, and attendance could be related to both study hours and outcomes.

### 28. Outliers and missing values
Outliers can pull Pearson correlation up or down. Missing values may cause each pairwise correlation to use a different subset, reducing comparability and possibly introducing selection bias.

### 29. Aggregation can hide relationships
Groups can have different baselines, compositions, or ranges. A pooled relationship may weaken or reverse relative to within-group patterns. Inspect subgroup summaries and plots.

## Practical coding

### 30. Validate columns
```python
def safe_corr(data, columns, method="pearson"):
    missing = [c for c in columns if c not in data.columns]
    if missing:
        raise KeyError(f"Missing columns: {missing}")
    non_numeric = [c for c in columns if not pd.api.types.is_numeric_dtype(data[c])]
    if non_numeric:
        raise TypeError(f"Non-numeric columns: {non_numeric}")
    return data[columns].corr(method=method)
```

### 31. Tidy pair table
Use the upper-triangle approach in answer 10; it removes self-correlations and mirrored pairs.

### 32. Group-specific correlations with sample sizes
```python
rows = []
for group_name, group in df.groupby("program", observed=False):
    valid = group[["study_hours", "exam_score"]].dropna()
    rows.append({
        "program": group_name,
        "n": len(valid),
        "pearson_r": valid["study_hours"].corr(valid["exam_score"])
    })
pd.DataFrame(rows)
```

### 33. Relationship report
A useful report includes: feature-target associations, feature-feature pairs above a documented review threshold, missingness and sample sizes, plots for selected pairs, and limitations. Avoid turning a fixed threshold into an automatic keep/drop decision.

### 34. Quality checks
Examples: validate row grain and IDs; inspect missingness and outliers; verify units and valid ranges; ensure the target is not leaking into features; check sample sizes across categories; review whether repeated observations violate independence assumptions.

## Mini-project

### 35–38. Suggested report structure
1. **Question:** Which features appear associated with exam score, and which inputs may be redundant?
2. **Methods:** Pearson/Spearman correlations, a heatmap, a scatter/pair plot, and grouped summaries.
3. **Findings:** Cite values and visible patterns from your own notebook output.
4. **Candidate features:** List features worth investigating further, with a rationale.
5. **Risks:** Confounding, small groups, missingness, synthetic-data limitations, and leakage.
6. **Next steps:** Validate relationships on relevant data, use train-only feature selection, compare model performance, and review domain meaning.

Do not invent numerical results; use the outputs from your notebook run.

## Mini-project rubric (10 points)

| Criterion | Points |
|---|---:|
| Question and variable definitions | 2 |
| Appropriate relationship methods | 2 |
| Clear plots and summary table | 2 |
| Interpretation and redundancy discussion | 2 |
| Limitations and follow-up steps | 2 |
