# Reference Solutions: Inferential Statistics

These are sample answers. Numerical test outputs may vary slightly with software versions.

1. Inferential statistics uses sample data to estimate population quantities or evaluate claims; descriptive statistics summarizes observed data.
2. Parameter: population quantity. Statistic: sample quantity. Estimator: rule for estimating a parameter. Estimate: realized numerical result.
3. The distribution a statistic would have over repeated samples drawn under a specified process.
4. Standard error describes sampling variability of a statistic. For a mean, \(SE=s/\sqrt{n}\) under independent sampling.
5. Under suitable conditions, the sample mean's sampling distribution becomes approximately normal as sample size increases; raw observations themselves need not be normal.
6. It communicates an estimate and uncertainty under a procedure and its assumptions.
7. If the sampling procedure were repeated many times, about 95% of intervals produced by a 95% procedure would contain the fixed parameter, assuming the model/design is valid.
8. \(H_0\) is the reference claim; \(H_1\) is the competing claim.
9. The preselected long-run Type I error rate for the test procedure under its assumptions.
10. Assuming the null model and test assumptions, the probability of a test statistic at least as extreme as observed, in the specified tail(s).
11. The p-value is calculated assuming the null model; it is not a posterior probability for the hypothesis.
12. Type I: reject a true null. Type II: fail to reject a false null.
13. Power is \(1-\beta\), the probability of rejecting the null under a specified alternative. Larger samples and larger true effects often increase power.
14. Statistical significance concerns compatibility with a null model at a threshold; practical significance concerns whether the effect matters in context.
15. To compare the mean of one sample with a reference value when the design and assumptions are suitable.
16. To compare the means of two independent groups without assuming equal population variances.
17. It compares paired differences; pairing carries information and should not be discarded as if observations were independent.
18. It tests whether two categorical variables are associated in a contingency table.
19. Pearson correlation measures direction and strength of linear association; it does not establish causation.
20. They communicate magnitude and precision, which a p-value alone does not.
21. More tests create more opportunities for false positives; preplanning and suitable correction help.
22. \(SE=8/\sqrt{64}=1\).
23. \(100\pm1.96(3)=[94.12,105.88]\).
24. Reject \(H_0\) at the 0.05 level under the test assumptions. This does not prove a large or practically important effect; inspect the estimate, interval, and design.
25. No. It means the data did not provide enough evidence against \(H_0\) at the chosen threshold; uncertainty and power matter.
26. ```python
    import numpy as np
    from scipy import stats
    x = np.array([11, 12, 10, 13, 9])
    result = stats.ttest_1samp(x, popmean=10)
    print(result)
    ```
27. ```python
    from scipy import stats
    group_a = [12, 14, 13, 15, 11]
    group_b = [10, 9, 12, 11, 8]
    result = stats.ttest_ind(group_a, group_b, equal_var=False)
    print(result)
    ```
28. ```python
    from scipy import stats
    before = [10, 12, 9, 14, 11]
    after = [11, 13, 10, 15, 12]
    print(stats.ttest_rel(before, after))
    ```
29. ```python
    import numpy as np
    from scipy.stats import chi2_contingency
    table = np.array([[30, 20], [15, 35]])
    chi2, p, dof, expected = chi2_contingency(table)
    print(chi2, p, dof)
    print(expected)
    ```
30. ```python
    from scipy.stats import pearsonr
    x = [1, 2, 3, 4, 5]
    y = [2, 4, 5, 4, 6]
    r, p = pearsonr(x, y)
    print("r:", r, "p-value:", p)
    ```
    A positive r indicates a positive linear association in these data; it does not establish cause.
31. ```python
    import numpy as np
    import matplotlib.pyplot as plt
    rng = np.random.default_rng(42)
    population = rng.normal(50, 10, 10000)
    means = [rng.choice(population, 40, replace=False).mean() for _ in range(500)]
    plt.hist(means, bins=25, edgecolor="black")
    plt.xlabel("Sample mean")
    plt.ylabel("Frequency")
    plt.show()
    ```
32. ```python
    import numpy as np
    x = np.array([4, 6, 7, 8, 10], dtype=float)
    mean = x.mean()
    sd = x.std(ddof=1)
    se = sd / np.sqrt(len(x))
    print(mean, sd, se)
    ```
33. Example: “Group A's mean was 2.1 units higher than Group B's (95% CI [0.4, 3.8], p=0.02). This is an association/difference in the sampled groups and does not by itself establish causation.”
34. Examples: independence, distribution of differences, equal/unequal variance assumptions, outliers, sample selection, measurement quality, expected cell counts, and multiple testing.
35. Answers vary. Include the research question, data type/design, H0/H1, test choice, assumptions, effect estimate, confidence interval, p-value, and contextual limitation.
