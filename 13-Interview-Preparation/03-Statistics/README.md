# Statistics Interview Preparation

A practical interview-preparation module covering descriptive statistics, probability, distributions, inference, hypothesis testing, confidence intervals, correlation, regression, and experimentation for data science and machine learning roles.

## Learning outcomes

By completing this module, you should be able to:

- Summarize data using mean, median, variance, standard deviation, quantiles, and robust statistics.
- Apply probability rules, conditional probability, Bayes' theorem, and independence.
- Explain common distributions and when they are useful.
- Interpret sampling distributions, standard errors, confidence intervals, p-values, and statistical power.
- Choose and explain suitable statistical tests while checking assumptions.
- Distinguish correlation from causation and interpret regression outputs carefully.
- Design and interpret A/B tests, including practical significance and common pitfalls.
- Implement small statistical calculations in Python and communicate results clearly.

## Files

| File | Purpose |
|---|---|
| `README.md` | Roadmap, key concepts, and study plan |
| `examples.ipynb` | Python examples using NumPy, pandas, SciPy, and Matplotlib where appropriate |
| `practice.md` | Theory, calculations, interpretation, and coding questions |
| `solution.md` | Worked solutions, formulas, reasoning, and code |

## Suggested 5-day plan

### Day 1 — Descriptive statistics
Mean, median, mode, percentiles, IQR, variance, standard deviation, skew, outliers, and sampling bias.

### Day 2 — Probability and distributions
Conditional probability, Bayes' theorem, independence, expected value, variance, Bernoulli/binomial, normal, Poisson, and the Central Limit Theorem.

### Day 3 — Inference and hypothesis testing
Population vs sample, standard error, confidence intervals, null/alternative hypotheses, p-values, Type I/II errors, power, and multiple testing.

### Day 4 — Tests, correlation, and regression
Choosing tests, assumptions, effect sizes, correlation, confounding, linear regression, residuals, and interpreting coefficients.

### Day 5 — Experimentation and mock interview
A/B testing, randomization, sample-ratio mismatch, practical significance, metric selection, and communicating conclusions.

## Formula sheet

| Concept | Formula / interpretation |
|---|---|
| Sample mean | \(\bar{x} = \frac{1}{n}\sum_{i=1}^{n}x_i\) |
| Sample variance | \(s^2 = \frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2\) |
| Standard error of mean | \(SE(\bar{x}) = s/\sqrt{n}\) |
| z-score | \(z = (x-\mu)/\sigma\) |
| Bayes' theorem | \(P(A\mid B)=P(B\mid A)P(A)/P(B)\), when \(P(B)>0\) |
| Confidence interval (large-sample mean) | Estimate \(\pm\) critical value \(\times\) standard error; use the appropriate distribution and assumptions |
| Relative risk | \(P(\text{outcome}\mid \text{treated}) / P(\text{outcome}\mid \text{control})\) |
| Correlation | Measures direction and strength of a linear relationship for Pearson's \(r\); it does not establish causation |

## Choosing a statistical test

- **Compare two independent means:** independent two-sample t-test; Welch's version avoids assuming equal variances.
- **Compare paired measurements:** paired t-test, when the paired differences are reasonably modeled by the test assumptions.
- **Compare three or more independent means:** ANOVA, followed by appropriate post-hoc comparisons if warranted.
- **Compare categorical counts/proportions:** chi-square test; Fisher's exact test can be useful for small expected counts in a 2×2 table.
- **Association between two numeric variables:** Pearson correlation for linear association; Spearman correlation for monotonic association based on ranks.
- **Predict a numeric outcome:** linear regression, with residual and assumption checks.
- **Predict a binary outcome:** logistic regression or another suitable classifier; assess calibration and discrimination separately.

Test choice depends on study design, sample size, independence, measurement scale, distribution, and the question being asked. No single assumption checklist replaces understanding the design.

## Interview answer framework

1. Clarify the population, sample, metric, and study design.
2. State the statistical question and assumptions.
3. Choose the method and explain why it fits.
4. Interpret uncertainty and effect size, not only a p-value.
5. Discuss limitations, possible bias, and what additional evidence would help.

## Common interview traps

- A p-value is not the probability that the null hypothesis is true.
- A 95% confidence procedure has 95% long-run coverage under its assumptions; a frequentist interval is not literally a 95% posterior probability statement about a fixed parameter.
- Statistical significance does not guarantee practical importance.
- Failure to reject the null does not prove that there is no effect.
- A large sample can make a tiny effect statistically significant.
- Correlation alone does not prove causality.
- Randomization helps balance confounders on average; it does not guarantee perfect balance in every realized experiment.
- Repeatedly checking results or testing many hypotheses without correction can inflate false-positive rates.
- The Central Limit Theorem concerns the distribution of suitable sums/means under conditions; it does not say the raw data must be normal.
- Outliers should be investigated, not automatically removed.

## Further reading

- [SciPy statistics reference](https://docs.scipy.org/doc/scipy/reference/stats.html)
- [NumPy random sampling](https://numpy.org/doc/stable/reference/random/index.html)
- [NIST/SEMATECH e-Handbook of Statistical Methods](https://www.itl.nist.gov/div898/handbook/)
- [OpenIntro Statistics](https://www.openintro.org/book/os/)
