# Inferential Statistics

Inferential statistics uses sample data to estimate population characteristics, quantify uncertainty, and evaluate claims.

## Learning objectives
- Distinguish descriptive and inferential statistics.
- Understand estimators, standard errors, confidence intervals, and hypothesis tests.
- Formulate null and alternative hypotheses.
- Interpret p-values, significance levels, Type I and Type II errors, and statistical power.
- Choose a basic test based on the question and data.
- Run introductory inferential analyses in Python with SciPy.

## 1. Descriptive vs. inferential statistics
- **Descriptive statistics** summarize the observations you have, such as mean, median, and standard deviation.
- **Inferential statistics** use sample data and assumptions to draw conclusions about a wider population.

Inference does not remove uncertainty. The sample design, measurement quality, assumptions, and analysis plan all affect the conclusions.

## 2. Population, sample, parameter, and estimator
A population parameter is a fixed but often unknown quantity, such as population mean \(\mu\). A sample statistic, such as sample mean \(\bar{x}\), is computed from observed data and can estimate the parameter.

An **estimator** is a rule for using sample data to estimate a parameter. Its realized value is an **estimate**. An estimator can be evaluated by bias, variance, consistency, and mean squared error.

## 3. Standard error and sampling distributions
The sampling distribution is the distribution of a statistic over repeated samples under a sampling procedure. The standard error measures the variability of that statistic.

For an independent sample of size \(n\), the estimated standard error of the sample mean is

\[
SE(\bar{x})=\frac{s}{\sqrt{n}}
\]

The Central Limit Theorem says that, under suitable conditions, the sampling distribution of a sample mean becomes approximately normal as sample size grows. It does not say the original data must be normally distributed, and the required sample size depends on the data and dependence structure.

## 4. Confidence intervals
A confidence interval is produced by a procedure designed to cover a population parameter at a stated long-run rate under its assumptions.

For a large independent sample, an approximate mean interval is

\[
\bar{x}\pm z^*\frac{s}{\sqrt{n}}
\]

For a mean with unknown population standard deviation, especially with small samples from a normal population, use a t critical value:

\[
\bar{x}\pm t^*_{n-1}\frac{s}{\sqrt{n}}
\]

A 95% confidence procedure covers the fixed parameter in about 95% of repeated samples under its assumptions. It does not mean that the fixed parameter has a 95% probability of lying in the particular interval already calculated.

## 5. Hypothesis testing
Hypothesis testing evaluates how compatible data are with a specified null model.

1. State the **null hypothesis** \(H_0\) and **alternative hypothesis** \(H_1\).
2. Choose a test and significance level \(\alpha\) before inspecting results when possible.
3. Check assumptions and compute a test statistic.
4. Obtain a p-value under the null model.
5. Compare the p-value with \(\alpha\), and report the effect estimate and uncertainty.

- **Null hypothesis:** a reference claim, such as no mean difference.
- **Alternative hypothesis:** the competing claim, such as a non-zero mean difference.
- **Significance level \(\alpha\):** the tolerated Type I error rate for the procedure under its assumptions.
- **p-value:** assuming the null model and test assumptions, the probability of a test statistic at least as extreme as the observed statistic, in the specified direction(s).

A p-value is not the probability that the null hypothesis is true, nor is it the probability the result happened “by chance.” Statistical significance does not necessarily mean practical importance.

## 6. Errors and power
- **Type I error:** reject a true null hypothesis; its long-run probability is controlled by \(\alpha\) under the model.
- **Type II error:** fail to reject a false null hypothesis.
- **Power:** probability of rejecting the null for a specified alternative; power is \(1-\beta\), where \(\beta\) is the Type II error probability.

Power tends to increase with larger sample size, larger true effects, lower measurement noise, and higher \(\alpha\), although increasing \(\alpha\) also raises Type I error risk.

## 7. Common introductory tests

| Test | Typical use | Key caution |
|---|---|---|
| One-sample t-test | Compare a sample mean with a reference value | Independent observations; distribution of mean/differences should be suitable |
| Independent two-sample t-test | Compare means in two independent groups | Prefer Welch's test when equal variances are not justified |
| Paired t-test | Compare paired measurements | Analyze within-pair differences |
| Chi-square test of independence | Association between categorical variables | Expected cell counts and independence matter |
| Pearson correlation | Linear association between numeric variables | Correlation is not causation; outliers can matter |
| Proportion test | Compare one or more proportions | Check sample design and approximation conditions |

Test choice should follow the research question, data type, design, and assumptions—not whichever test returns a small p-value.

## 8. Effect size and practical importance
Report effect estimates, not only p-values. Examples include mean differences, risk differences, odds ratios, and correlation coefficients. Confidence intervals help communicate precision. A tiny effect can be statistically significant in a very large sample; a meaningful effect can be uncertain in a small sample.

## 9. Multiple testing and reproducibility
Testing many hypotheses increases the chance of at least one false positive. Plan primary analyses in advance and consider corrections such as Bonferroni or false discovery rate procedures when appropriate. Record data exclusions, test assumptions, software versions, and all analyses conducted.

## 10. Python example

```python
import numpy as np
from scipy import stats

sample = np.array([51, 49, 52, 48, 50, 53, 47, 51])
result = stats.ttest_1samp(sample, popmean=50)
print("t statistic:", result.statistic)
print("p-value:", result.pvalue)
```

A test result should be interpreted alongside the estimate, confidence interval, design, assumptions, and practical context.

## Best practices
- Define the target population and question before choosing a test.
- Check sampling and measurement methods, independence, outliers, and test assumptions.
- Report sample size, effect estimate, confidence interval, test statistic, degrees of freedom where relevant, and p-value.
- Do not equate correlation with causation or statistical significance with importance.
- Do not interpret a non-significant result as proof of no effect.
- Avoid selective reporting and unplanned repeated testing.

Use `practice.md` for exercises, `examples.ipynb` for runnable analyses, and `solutions.md` for reference answers.
