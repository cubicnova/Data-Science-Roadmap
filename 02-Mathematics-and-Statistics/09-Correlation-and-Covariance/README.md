# Correlation and Covariance

Correlation and covariance describe how two variables vary together. They are widely used in exploratory data analysis, feature analysis, finance, and statistical modeling.

## Learning objectives
- Explain covariance and correlation.
- Calculate and interpret Pearson correlation and sample covariance.
- Distinguish Pearson, Spearman, and Kendall association measures.
- Understand the range, units, and limitations of correlation coefficients.
- Identify outliers, nonlinear patterns, and confounding as interpretation risks.
- Analyze variable relationships using NumPy, pandas, and Matplotlib.

## 1. Covariance
Covariance indicates whether two variables tend to vary in the same or opposite directions.

For paired observations \(x_i,y_i\), sample covariance is

\[
s_{xy}=\frac{\sum_{i=1}^{n}(x_i-\bar{x})(y_i-\bar{y})}{n-1}
\]

- Positive covariance: values above their means tend to occur together.
- Negative covariance: one variable tends to be above its mean when the other is below its mean.
- Covariance near zero: little linear co-variation, but a nonlinear relationship may still exist.

Covariance depends on the units and scale of the variables. For example, converting meters to centimeters changes the numerical covariance.

## 2. Pearson correlation
Pearson's correlation coefficient standardizes covariance:

\[
r=\frac{s_{xy}}{s_xs_y}
\]

where \(s_x\) and \(s_y\) are sample standard deviations.

The coefficient ranges from \(-1\) to \(+1\):
- \(r=+1\): perfect positive linear relationship.
- \(r=-1\): perfect negative linear relationship.
- \(r\approx0\): little linear association; not necessarily no relationship.

Correlation is unitless. Its sign gives the direction of a linear association and its magnitude describes the strength of that linear pattern. There is no universal threshold that makes a correlation “strong”; context matters.

## 3. Correlation is not causation
A correlation does not establish that one variable causes another. Possible explanations include:
- direct causal influence;
- reverse causation;
- a third variable affecting both;
- selection effects or coincidence.

Always consider the data collection process and plausible confounders before making causal claims.

## 4. Pearson, Spearman, and Kendall

| Measure | What it captures | Useful when | Cautions |
|---|---|---|---|
| Pearson \(r\) | Linear association | Numeric variables with a roughly linear pattern | Sensitive to outliers; nonlinear patterns may be missed |
| Spearman \(\rho\) | Association between ranks; monotonic relationships | Ordinal data or monotonic patterns | Ties affect calculation; not a general detector of all dependence |
| Kendall \(\tau\) | Rank concordance | Ordinal data and rank association | Interpretation and magnitude differ from Pearson |

A scatter plot should usually come before selecting a coefficient. A nonlinear U-shaped pattern can have Pearson correlation near zero even when the variables are clearly related.

## 5. Covariance matrix and correlation matrix
For multiple variables, covariance and correlation matrices summarize pairwise relationships.

- Covariance matrix diagonal entries are variances.
- Correlation matrix diagonal entries are 1 for non-constant variables.
- Correlation values are standardized, making different feature pairs easier to compare.
- Constant variables have undefined correlation because their standard deviation is zero.

A high pairwise correlation between features may indicate multicollinearity in a model, but model impact depends on the complete design and analysis.

## 6. Outliers and data quality
Correlation can change substantially due to one or two influential observations. Before interpreting a coefficient:
1. Plot the paired observations.
2. Check missing values and data types.
3. Look for outliers and data-entry errors.
4. Consider whether the relationship is linear or monotonic.
5. Examine subgroup patterns and possible confounding.
6. Avoid removing outliers solely because they weaken a desired conclusion; use a documented, defensible rule.

## 7. Python examples

```python
import numpy as np
from scipy.stats import pearsonr, spearmanr, kendalltau

hours = np.array([1, 2, 3, 4, 5, 6])
scores = np.array([52, 55, 61, 64, 70, 74])

print("Sample covariance:", np.cov(hours, scores, ddof=1)[0, 1])
print("Pearson:", pearsonr(hours, scores))
print("Spearman:", spearmanr(hours, scores))
print("Kendall:", kendalltau(hours, scores))
```

Using pandas:

```python
import pandas as pd

df = pd.DataFrame({"hours": hours, "scores": scores})
print(df.cov(numeric_only=True))
print(df.corr(numeric_only=True, method="pearson"))
```

Plot the data:

```python
import matplotlib.pyplot as plt

plt.scatter(hours, scores)
plt.xlabel("Hours studied")
plt.ylabel("Score")
plt.title("Hours studied vs. score")
plt.show()
```

## 8. Best practices
- Visualize before interpreting a coefficient.
- State which correlation method you used.
- Report sample size and, when relevant, uncertainty or a test result.
- Do not compare covariance values across variables with different units as if they were standardized.
- Do not infer causation from correlation alone.
- Remember that near-zero Pearson correlation does not rule out nonlinear dependence.
- For many pairwise correlations, consider multiple-testing issues and avoid overinterpreting chance patterns.

Use `practice.md` for exercises, `examples.ipynb` for runnable demonstrations, and `solutions.md` for reference answers.
