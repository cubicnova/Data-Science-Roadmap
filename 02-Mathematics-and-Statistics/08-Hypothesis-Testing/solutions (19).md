# Reference Solutions: Hypothesis Testing

These are sample answers; code output may vary slightly across software versions.

1. A statistical hypothesis is a claim about a population or model. \(H_0\) is the reference claim; \(H_1\) is the competing claim.
2. \(H_0:\mu=100\); \(H_1:\mu\ne100\).
3. \(H_0:\mu=50\); \(H_1:\mu>50\).
4. \(H_0:\mu=5\); \(H_1:\mu<5\).
5. The preselected long-run Type I error rate for a procedure under its assumptions.
6. Assuming the null model and test assumptions, the probability of observing a test statistic at least as extreme as the observed one in the specified tail(s).
7. The p-value is calculated under the null model; it is not a posterior probability for the null hypothesis.
8. Since 0.02 < 0.05, reject \(H_0\) under the test assumptions.
9. Since 0.08 > 0.05, fail to reject \(H_0\). The data do not provide enough evidence at that threshold; this does not prove \(H_0\).
10. A test statistic summarizes the evidence relative to the null model; the critical region contains values that lead to rejection at the chosen alpha.
11. \(t=(\bar{x}-\mu_0)/(s/\sqrt{n})\), where \(\bar{x}\) is sample mean, \(\mu_0\) null mean, \(s\) sample standard deviation, and \(n\) sample size.
12. Example: a medical screening declares a condition present when the person truly does not have it.
13. Example: a test fails to detect a real improvement because the sample is small or measurements are noisy.
14. Power is \(1-\beta\), the probability of rejecting the null under a specified alternative. Larger samples and larger true effects often increase power.
15. Statistical significance is a decision relative to a null model and threshold; practical significance concerns whether the effect matters in context.
16. To compare one independent sample mean with a reference value when the assumptions are reasonable.
17. To compare means from two independent groups without assuming equal variances.
18. It analyzes within-pair differences; pairing accounts for the link between observations.
19. It tests whether two categorical variables are associated.
20. It evaluates evidence for a linear association; it does not prove causation.
21. They show magnitude and precision, which the p-value alone does not communicate.
22. It makes the test choice data-dependent and can inflate false-positive risk.
23. Each test creates another opportunity for a false positive; preplanning and suitable corrections can help.
24. Since 0.04 > 0.01, fail to reject \(H_0\) at alpha=0.01.
25. No. A p-value measures compatibility with a null model, not effect magnitude; inspect the effect estimate and interval.
26. ```python
    import numpy as np
    from scipy import stats
    x = np.array([12, 11, 13, 10, 14])
    print(stats.ttest_1samp(x, popmean=10))
    ```
27. ```python
    from scipy import stats
    group_a = [12, 14, 13, 15, 11]
    group_b = [10, 9, 12, 11, 8]
    print(stats.ttest_ind(group_a, group_b, equal_var=False))
    ```
28. ```python
    from scipy import stats
    before = [10, 12, 9, 14, 11]
    after = [11, 13, 10, 15, 12]
    print(stats.ttest_rel(after, before))
    ```
29. ```python
    import numpy as np
    from scipy.stats import chi2_contingency
    table = np.array([[30, 20], [15, 35]])
    print(chi2_contingency(table))
    ```
30. ```python
    from scipy.stats import pearsonr
    r, p = pearsonr([1, 2, 3, 4, 5], [2, 4, 5, 4, 6])
    print("r:", r, "p-value:", p)
    ```
31. \(t=(54-50)/(8/\sqrt{16})=4/2=2\).
32. A smaller alpha makes rejection harder and reduces the nominal Type I error rate, but can also reduce power if everything else stays the same.
33. Examples: independent observations, reasonable distribution of residuals/differences, no influential outliers, and appropriate measurement/sampling design.
34. Example: “The intervention group had a mean outcome 2.4 units higher than the comparison group (95% CI [0.5, 4.3], p=0.02). This difference is statistically significant under the specified model; the study design does not by itself establish causation.”
35. Answers vary. State the population question, \(H_0\), \(H_1\), test choice and assumptions, alpha, effect estimate, confidence interval, test statistic, degrees of freedom where applicable, p-value, and limitations.
