# Hypothesis Testing

Hypothesis testing is a structured way to evaluate how compatible observed data are with a null model. It is useful for comparing groups, evaluating changes, and testing scientific or business claims.

## Learning objectives
- Formulate null and alternative hypotheses.
- Choose one-sided or two-sided alternatives appropriately.
- Understand significance level, test statistics, p-values, and critical regions.
- Explain Type I and Type II errors, power, and effect size.
- Select common tests based on the study design and data type.
- Run and interpret introductory tests in Python using SciPy.

## 1. Hypotheses
- **Null hypothesis (\(H_0\))**: a reference claim, often no difference or no association.
- **Alternative hypothesis (\(H_1\) or \(H_a\))**: the competing claim.
- **Two-sided test**: asks whether a parameter differs in either direction, e.g. \(H_1:\mu\ne\mu_0\).
- **Right-tailed test**: asks whether it is greater, e.g. \(H_1:\mu>\mu_0\).
- **Left-tailed test**: asks whether it is lower, e.g. \(H_1:\mu<\mu_0\).

Choose the direction based on the research question before looking at the results. Do not switch to a one-sided test after seeing the observed direction.

## 2. Significance level and p-value
The **significance level** \(\alpha\) is the preselected long-run Type I error rate of the test procedure under its assumptions. A common choice is 0.05, but the threshold should reflect the context.

The **p-value** is the probability, assuming the null model and test assumptions, of observing a test statistic at least as extreme as the observed one in the direction(s) defined by the test.

A p-value:
- is not the probability that \(H_0\) is true;
- is not the probability that results are “due to chance”;
- does not measure effect size or practical importance;
- is meaningful only relative to the test, model, and assumptions.

A conventional decision rule is:
- if \(p\leq\alpha\), reject \(H_0\);
- if \(p>\alpha\), fail to reject \(H_0\).

“Fail to reject” is not the same as “accept” or prove the null hypothesis.

## 3. Test statistic and critical region
A test statistic summarizes how far the observed data are from what the null model predicts, scaled by estimated uncertainty. The exact statistic depends on the test.

For a one-sample t-test,

\[
t=\frac{\bar{x}-\mu_0}{s/\sqrt{n}}
\]

where \(\bar{x}\) is the sample mean, \(\mu_0\) is the null mean, \(s\) is the sample standard deviation, and \(n\) is the sample size. Under the usual assumptions, the statistic is compared with a t distribution with \(n-1\) degrees of freedom.

The **critical region** contains test-statistic values that lead to rejection at the chosen \(\alpha\). The p-value and critical-region approaches should give the same decision for the same test and assumptions.

## 4. Errors and power
- **Type I error:** reject a true \(H_0\). Its long-run probability is controlled by \(\alpha\) under the test assumptions.
- **Type II error:** fail to reject a false \(H_0\). Its probability is often denoted \(\beta\).
- **Power:** \(1-\beta\), the probability of rejecting \(H_0\) for a specified alternative.

Power generally increases with larger samples, larger true effects, lower measurement noise, and sometimes a higher \(\alpha\). Raising \(\alpha\) also raises the risk of Type I error.

## 5. Common tests

| Research question / data | Typical test | Notes |
|---|---|---|
| One numeric sample vs. a reference mean | One-sample t-test | Independent observations; assumptions should be checked |
| Numeric outcome in two independent groups | Welch's t-test | Does not require equal variances |
| Paired numeric measurements | Paired t-test | Tests within-pair differences |
| Association between categorical variables | Chi-square test of independence | Check expected counts and independence |
| Linear association between two numeric variables | Pearson correlation test | Correlation does not imply causation |
| Compare one proportion with a reference | One-proportion test | Check design and approximation assumptions |

The design matters: repeated measures, clustering, non-random selection, and multiple tests can change how uncertainty should be estimated.

## 6. Statistical vs. practical significance
A small p-value is evidence against the specified null model, not proof that an effect is large or useful. Report effect estimates and confidence intervals. For example, a tiny mean difference may be statistically significant in a huge sample but practically irrelevant.

A non-significant result does not prove there is no effect. The study may have low power or a wide confidence interval.

## 7. Assumptions and responsible workflow
1. Define the question and target population.
2. State \(H_0\), \(H_1\), and the test direction before examining the result.
3. Identify the sampling design and experimental unit.
4. Check independence, measurement quality, outliers, distributional conditions, and test-specific assumptions.
5. Select the test and \(\alpha\) in advance.
6. Report sample size, effect estimate, confidence interval, test statistic, degrees of freedom where applicable, and p-value.
7. Discuss practical importance and limitations.
8. Account for multiple testing when many hypotheses are explored.

## 8. Python examples

```python
import numpy as np
from scipy import stats

sample = np.array([51, 49, 52, 48, 50, 53, 47, 51])
result = stats.ttest_1samp(sample, popmean=50)
print("t statistic:", result.statistic)
print("p-value:", result.pvalue)
```

For an independent two-group comparison, Welch's test is often a sensible default when equal variances are not justified:

```python
stats.ttest_ind(group_a, group_b, equal_var=False)
```

## Best practices
- Do not select the test or tail only after inspecting the p-value.
- Do not interpret p-values as probabilities that hypotheses are true.
- Report effect size and uncertainty, not just a threshold decision.
- Avoid causal claims unless the design supports them.
- Be transparent about exclusions, transformations, and tests performed.

Use `practice.md` for exercises, `examples.ipynb` for runnable demonstrations, and `solutions.md` for reference answers.
