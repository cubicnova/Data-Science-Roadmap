# Statistics Interview Practice — Solutions

These solutions emphasize interpretation and assumptions. Statistical methods should be chosen based on the study design, not only on the shape of the data.

## Part A — Conceptual answer guide

1. Prefer the median for a skewed distribution or when extreme values distort the mean; report spread such as IQR as well.
2. Variance measures squared spread around the mean; standard deviation is its square root in the original units; standard error measures uncertainty of an estimator across repeated samples (for a sample mean, commonly \(s/\sqrt{n}\)).
3. A parameter describes a population (for example, population mean \(\mu\)); a statistic is computed from a sample (for example, sample mean \(\bar{x}\)).
4. Conditional probability is \(P(A\mid B)=P(A\cap B)/P(B)\) when \(P(B)>0\). Independence means learning one event does not change the probability of the other. Bayes' theorem updates \(P(A)\) using evidence \(B\).
5. Under suitable conditions, standardized sums/means approach a normal distribution as sample size grows. It does not require raw observations to be normally distributed, and conditions such as dependence and extreme tails matter.
6. A sampling distribution is the distribution of a statistic across repeated samples. Standard error estimates the spread of that statistic's sampling distribution.
7. A 95% confidence procedure captures the fixed parameter in 95% of repeated samples under its assumptions. It is not, in the usual frequentist interpretation, a 95% probability that this particular fixed parameter lies in the computed interval.
8. A p-value is the probability, assuming the null model and test assumptions, of observing a result at least as incompatible with the null as the observed result, according to the test statistic. It is not the probability the null is true or the probability results happened “by chance.”
9. Type I error: reject a true null. Type II error: fail to reject a false null. Power is \(1-\beta\), the probability of rejecting the null under a specified alternative.
10. For a fixed effect and variability, larger samples generally increase power. Larger true effects are easier to detect; noisier measurements reduce power.
11. Statistical significance concerns compatibility with a null under a specified procedure; practical significance concerns whether the effect matters operationally or economically.
12. A t-test is commonly used for means when population variance is unknown and estimated from the sample. A z-test is appropriate in settings where the relevant standard deviation is known or under a justified large-sample approximation. In practice, Welch's t-test is a robust default for two independent means.
13. Independent t-tests compare separate groups; paired t-tests analyze within-pair differences, such as before/after measurements on the same people.
14. Chi-square tests assess categorical count relationships when expected counts and assumptions are adequate. Fisher's exact test is useful for small 2×2 tables with sparse expected counts.
15. Pearson measures linear association in numeric variables; Spearman measures monotonic association using ranks and is less sensitive to the exact scale.
16. A third variable may influence both variables, or reverse causality may exist. Ice-cream sales and drowning can correlate because both rise in warm weather; that correlation does not show ice cream causes drowning.
17. Check linearity, residual behavior, independence, variance patterns, influential observations, and multicollinearity; inferential claims also rely on suitable error assumptions and study design.
18. Multicollinearity occurs when predictors are strongly related. It can make individual coefficient estimates unstable and inflate standard errors, even when overall prediction remains useful.
19. Each test has a false-positive risk under its null. Running many tests increases the chance of at least one false positive; use pre-specified hypotheses and appropriate procedures such as Holm or false-discovery-rate control.
20. Sample-ratio mismatch means observed allocation differs from the planned allocation more than expected. It may indicate logging, eligibility, assignment, or exposure bugs and can invalidate interpretation.
21. Simpson's paradox occurs when an association seen within groups reverses or changes after groups are combined, often due to different group sizes or a confounder.
22. Selection bias occurs when inclusion is related to variables relevant to the question; survivorship bias focuses only on entities that remain; non-response bias occurs when respondents differ systematically from non-respondents.
23. Bootstrap resamples observations with replacement to approximate an estimator's sampling distribution. It is useful when analytic standard errors are difficult, but it cannot fix biased or non-representative data and requires care for dependent data.
24. Explain the effect in real units, its uncertainty, the expected impact at product scale, costs/risks, and whether the result crosses a pre-agreed decision threshold.

## Part B — Calculation answers

1. Mean \(=40/8=5\). Median \(=(4+5)/2=4.5\). Range \(=9-2=7\). Mode \(=4\).
2. \(SE=20/\sqrt{100}=2\).
3. \(\binom{3}{2}(1/2)^3=3/8=0.375\).
4. By Bayes' theorem:
   - True positive contribution: \(0.01\times0.90=0.009\).
   - False positive contribution: \(0.99\times(1-0.95)=0.0495\).
   - \(P(\text{condition}\mid +)=0.009/(0.009+0.0495)\approx0.1538\), or about **15.4%**.
5. \(120\pm1.96(4)=120\pm7.84\), so approximately **[112.16, 127.84]**. This uses a normal approximation and assumes the standard error is appropriate.
6. Absolute change \(=5\%-4\%=1\) percentage point. Relative lift \(=(5\%-4\%)/4\%=25\%\).
7. Since \(0.03<0.05\), reject the null under the pre-specified test. This does not prove the alternative is true, quantify practical importance, or give the probability that the null is true.
8. Type II error.

## Part C — Applied scenarios

1. Report median and IQR, possibly alongside mean and standard deviation for context. Inspect data quality and outliers rather than removing them automatically.
2. Example: primary metric is completed-checkout conversion per randomized eligible user; guardrails include payment failures, refunds, latency, and revenue per user. Randomize at user/account level to prevent cross-device/session contamination when possible. Predefine hypothesis, sample size/power, duration, exclusions, stopping rule, and analysis.
3. A paired t-test on within-user differences may fit if those differences are reasonably suitable for the test. Check dependence structure, outliers, and the distribution of differences; consider robust or nonparametric alternatives when appropriate.
4. Welch's two-sample t-test is a reasonable starting point because it does not assume equal population variances.
5. Evaluate confidence interval, absolute business impact, costs, guardrails, heterogeneity, data quality, and whether the decision threshold was specified in advance.
6. Check assignment logic, eligibility filters, bucketing, logging, exposure, missing events, and whether the mismatch exceeds what random variation could plausibly explain. Do not interpret the outcome until the discrepancy is understood.
7. Accuracy can be high by predicting the majority class. Inspect precision, recall, F1, PR-AUC, confusion matrix, calibration, and the costs of false positives/negatives.
8. Check leakage, temporal validity, confounding, data quality, out-of-sample performance, stability, nonlinearity, and whether the question is prediction or causation.

## Part D — Python coding solutions

```python
from collections import Counter
import random
import numpy as np
from scipy import stats


def mean_median(values):
    if not values:
        raise ValueError("values must not be empty")
    ordered = sorted(values)
    n = len(ordered)
    mean_value = sum(ordered) / n
    middle = n // 2
    median_value = ordered[middle] if n % 2 else (ordered[middle - 1] + ordered[middle]) / 2
    return mean_value, median_value


def sample_variance_std(values):
    if len(values) < 2:
        raise ValueError("at least two observations are required")
    mean_value = sum(values) / len(values)
    variance = sum((value - mean_value) ** 2 for value in values) / (len(values) - 1)
    return variance, variance ** 0.5


def simulate_two_heads(trials=10_000, seed=42):
    if trials <= 0:
        raise ValueError("trials must be positive")
    rng = random.Random(seed)
    successes = 0
    for _ in range(trials):
        tosses = [rng.choice((0, 1)) for _ in range(3)]
        successes += sum(tosses) == 2
    return successes / trials


def welch_t_test(group_a, group_b):
    # H0: population means are equal; H1: population means differ.
    return stats.ttest_ind(group_a, group_b, equal_var=False)


def correlations(x, y):
    if len(x) != len(y) or len(x) < 2:
        raise ValueError("x and y must have equal length of at least 2")
    return stats.pearsonr(x, y), stats.spearmanr(x, y)


def conversion_rate(outcomes):
    if not outcomes:
        raise ValueError("outcomes must not be empty")
    if any(value not in (0, 1) for value in outcomes):
        raise ValueError("outcomes must contain only 0 and 1")
    return sum(outcomes) / len(outcomes)


def bootstrap_mean_ci(values, repetitions=5000, confidence=0.95, seed=42):
    if not values:
        raise ValueError("values must not be empty")
    if repetitions <= 0 or not 0 < confidence < 1:
        raise ValueError("invalid repetitions or confidence")
    rng = random.Random(seed)
    n = len(values)
    estimates = [
        sum(rng.choices(values, k=n)) / n
        for _ in range(repetitions)
    ]
    alpha = 1 - confidence
    lower, upper = np.quantile(estimates, [alpha / 2, 1 - alpha / 2])
    return float(lower), float(upper)


def binary_precision_recall_f1(y_true, y_pred):
    if len(y_true) != len(y_pred) or not y_true:
        raise ValueError("labels must have equal non-zero length")
    if any(value not in (0, 1) for value in y_true + y_pred):
        raise ValueError("labels must contain only 0 and 1")
    tp = sum(t == 1 and p == 1 for t, p in zip(y_true, y_pred))
    fp = sum(t == 0 and p == 1 for t, p in zip(y_true, y_pred))
    fn = sum(t == 1 and p == 0 for t, p in zip(y_true, y_pred))
    precision = tp / (tp + fp) if tp + fp else 0.0
    recall = tp / (tp + fn) if tp + fn else 0.0
    f1 = 2 * precision * recall / (precision + recall) if precision + recall else 0.0
    return {"precision": precision, "recall": recall, "f1": f1}
```

Notes:
- The simulation is approximate; with 10,000 trials it will vary slightly by seed.
- Welch's test returns a test statistic and p-value; interpret the p-value only under the test's assumptions.
- Bootstrap percentile intervals can perform poorly with tiny samples, severe dependence, or highly biased samples. For time series, resample in a way that respects dependence.
- The metric implementation defines zero precision/recall/F1 as `0.0` when the corresponding denominator is zero; other libraries may expose configurable behavior.

## Part E — Mock interview scoring rubric

Score each category from 1 to 5:
- Conceptual accuracy
- Choice of method and assumptions
- Correctness and edge-case handling
- Complexity and efficiency
- Communication and interpretation

A strong answer is not just a correct number: it explains what the result means, what assumptions support it, and what could make the conclusion unreliable.
