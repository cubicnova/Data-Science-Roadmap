# 07 — Feature Relationships

> Exploratory Data Analysis (EDA) · Data Science Roadmap

Feature relationship analysis studies how columns relate to one another and, when applicable, to a target variable. It helps identify useful signals, redundant features, nonlinear patterns, subgroup differences, and possible data-quality issues before feature engineering or modeling.

## Learning objectives

By completing this module, you will be able to:
- Explain positive, negative, weak, and nonlinear relationships.
- Use Pearson and Spearman correlation appropriately.
- Visualize relationships with scatter plots, pair plots, heatmaps, and grouped charts.
- Explore numeric–categorical and categorical–categorical relationships.
- Identify potentially redundant features and discuss multicollinearity.
- Compare feature-to-feature relationships with feature-to-target relationships.
- Avoid treating correlation as proof of causation or automatic evidence of feature importance.
- Document findings and propose follow-up analyses.

## Key concepts

### Relationship types
- **Positive association:** larger values of one variable tend to occur with larger values of another.
- **Negative association:** larger values of one variable tend to occur with smaller values of another.
- **Weak/no linear association:** little linear trend; a nonlinear pattern may still exist.
- **Nonlinear association:** variables relate in a curved or otherwise non-straight pattern.
- **Redundancy:** two features encode overlapping information.
- **Interaction:** a feature's relationship with an outcome changes across levels of another feature.

### Correlation methods
- **Pearson:** measures linear association between numeric variables; sensitive to outliers.
- **Spearman:** measures monotonic association using ranks; useful for ranked data or monotonic trends that are not linear.
- **Kendall:** rank-based association, often useful for ordinal data or smaller samples.

Correlation coefficients range from -1 to +1. The sign indicates direction; absolute magnitude indicates strength for the selected measure. No threshold universally defines a “strong” relationship.

```python
numeric = df.select_dtypes(include="number")
pearson = numeric.corr(method="pearson")
spearman = numeric.corr(method="spearman")
```

Always select numeric columns deliberately. A nominal category encoded as 1, 2, 3 does not automatically have meaningful numeric distances.

### Visual analysis
```python
import seaborn as sns
import matplotlib.pyplot as plt

sns.scatterplot(data=df, x="study_hours", y="exam_score", hue="program")
plt.tight_layout()
plt.show()

corr = df.select_dtypes(include="number").corr()
sns.heatmap(corr, annot=True, vmin=-1, vmax=1, center=0, cmap="coolwarm")
plt.title("Pearson correlation matrix")
plt.tight_layout()
plt.show()
```

- Scatter plots show direction, form, clusters, and outliers for two numeric variables.
- Pair plots compare multiple numeric pairs; use a subset to avoid clutter.
- Heatmaps summarize numeric pairwise correlations but can hide nonlinear relationships.
- Box/violin plots compare a numeric feature across categories.
- Cross-tabs summarize combinations of categorical variables.

### Feature-to-target vs feature-to-feature
These answer different questions:
- **Feature-to-target:** is a feature associated with the outcome of interest?
- **Feature-to-feature:** do two input features overlap or carry redundant information?

A feature can be correlated with the target and also highly correlated with another feature. Do not automatically drop a feature because of one correlation threshold. Consider domain meaning, missingness, data leakage, model type, validation performance, and interpretability.

### Redundancy and multicollinearity
Strongly correlated predictors can make linear-model coefficients unstable or difficult to interpret. Correlation is a screening tool, not a complete multicollinearity diagnosis. Variance inflation factor (VIF), domain reasoning, and model validation may help. Tree-based models respond differently from linear models.

### Categorical and mixed-type relationships
- Numeric vs category: grouped summaries, box plots, violin plots, or point plots.
- Category vs category: `pd.crosstab`, row percentages, stacked bars.
- Numeric vs numeric: scatter plot, Pearson/Spearman correlation, or trend lines.

State the denominator when reporting percentages. Check sample sizes and missingness by subgroup.

### Common pitfalls
- Correlation does not establish causation.
- Pearson correlation can miss U-shaped or other nonlinear patterns.
- Outliers can strongly influence correlation.
- A pooled relationship may differ from subgroup relationships.
- High correlation with a target can be misleading if a feature leaks information unavailable at prediction time.
- Repeated testing across many pairs can produce chance findings.
- Missing data can mean each pairwise correlation uses a different subset of records.

## Practical workflow
1. Define the question and target, if any.
2. Confirm row grain, feature definitions, units, and collection period.
3. Check missing values, types, outliers, and group sizes.
4. Inspect relevant feature-to-target relationships.
5. Inspect feature-to-feature relationships for overlap.
6. Use visualizations alongside numerical summaries.
7. Compare meaningful subgroups and look for interactions.
8. Record hypotheses, limitations, and next steps.
9. Validate any feature-selection decision during model training; fit preprocessing only on training data.

## Example questions
- Which features are most strongly associated with exam score?
- Which features are highly correlated with one another?
- Does the study-hours relationship look different by program?
- Do category distributions differ across groups?
- Are there suspiciously predictive columns that may leak the target?

## Setup
Python 3.10+ recommended.

```bash
python -m pip install pandas numpy matplotlib seaborn jupyter
```

Open `examples.ipynb` in Jupyter Notebook, JupyterLab, or VS Code. It generates synthetic data locally and requires no external download.

## Files
| File | Purpose |
|---|---|
| `README.md` | Concepts, workflow, cautions, and quick reference |
| `examples.ipynb` | Guided, executable notebook |
| `practice.md` | Progressive exercises and mini-project |
| `solution.md` | Reference code and interpretation guidance |

## Important reminder
Feature relationships support exploration and hypothesis generation. They do not prove causation or guarantee predictive usefulness. Make feature-selection decisions using domain context and appropriate validation.
