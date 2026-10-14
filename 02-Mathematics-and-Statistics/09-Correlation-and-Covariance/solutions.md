# Reference Solutions: Correlation and Covariance

Answers are examples; equivalent explanations and correct code are acceptable.

1. Covariance measures how two variables vary together. Positive means deviations from their means tend to have the same sign; negative means opposite signs.
2. \(s_{xy}=\sum (x_i-\bar{x})(y_i-\bar{y})/(n-1)\). The n−1 denominator is the usual sample-covariance correction under independent sampling.
3. Covariance changes when units change; scaling one variable by a constant scales the covariance by that constant.
4. \(r=s_{xy}/(s_xs_y)\), with values from -1 to +1.
5. A perfect positive linear relationship.
6. A perfect negative linear relationship.
7. It indicates little linear association. It does not rule out a nonlinear relationship.
8. Correlation divides covariance by both standard deviations, removing the measurement units.
9. Association alone does not establish direction or mechanism; confounding, reverse causality, selection, and coincidence are possible.
10. Ice-cream sales and drowning incidents may both rise in hot weather; temperature can affect both.
11. Pearson measures linear association on original values; Spearman measures rank-based monotonic association.
12. Kendall's tau is useful for ordinal/rank data and measuring concordance between pairs.
13. The plot reveals nonlinearity, outliers, clusters, and changing variance that a single coefficient hides.
14. An influential outlier can inflate, weaken, or reverse the coefficient.
15. A U-shaped relationship such as \(y=x^2\) for symmetric x values can have near-zero Pearson correlation despite a clear pattern.
16. A matrix whose entries are pairwise covariances; diagonal entries are variances.
17. A matrix of pairwise correlations; its diagonal entries are 1 when variables have nonzero variance.
18. Correlation is undefined because the constant variable's standard deviation is zero; software may return NaN.
19. Larger-than-average study hours tend to occur with larger-than-average scores, though causation is not established.
20. Higher temperatures tend to be associated with lower heating demand.
21. ```python
    import numpy as np
    print(np.corrcoef([1, 2, 3, 4], [2, 4, 6, 8])[0, 1])  # 1.0
    ```
22. ```python
    import numpy as np
    print(np.cov([1, 2, 3], [2, 4, 5], ddof=1)[0, 1])
    ```
23. ```python
    from scipy.stats import pearsonr, spearmanr, kendalltau
    x = [1, 2, 3, 4, 5]
    y = [2, 4, 5, 4, 6]
    print(pearsonr(x, y))
    print(spearmanr(x, y))
    print(kendalltau(x, y))
    ```
24. ```python
    import matplotlib.pyplot as plt
    x = [1, 2, 3, 4, 5]
    y = [2, 4, 5, 7, 8]
    plt.scatter(x, y)
    plt.xlabel("x")
    plt.ylabel("y")
    plt.title("Positive relationship")
    plt.show()
    ```
25. ```python
    import numpy as np
    x = np.arange(1, 6)
    y = -2*x + 10
    print(np.corrcoef(x, y)[0, 1])  # -1.0
    ```
26. ```python
    import numpy as np
    import matplotlib.pyplot as plt
    x = np.linspace(-3, 3, 100)
    y = x**2
    print("Pearson r:", np.corrcoef(x, y)[0, 1])
    plt.scatter(x, y)
    plt.xlabel("x")
    plt.ylabel("x squared")
    plt.show()
    ```
27. ```python
    import pandas as pd
    df = pd.DataFrame({"a": [1, 2, 3], "b": [2, 4, 5], "c": [9, 7, 6]})
    print(df.corr(numeric_only=True))
    ```
28. Compute both coefficients with and without the influential point and compare; Spearman is often less sensitive to magnitude outliers because it uses ranks, though outliers can still affect ranks.
29. High correlation among predictors can make coefficient estimates unstable and harder to interpret; investigate multicollinearity using the model context.
30. Strength describes magnitude of association; significance depends on sample size and a statistical model. A weak correlation can be significant in a huge sample.
31. A strong positive linear association in the observed data; this alone does not imply causation.
32. A weak negative linear association; inspect the plot, sample size, uncertainty, and possible nonlinear patterns.
33. Check missing values, types/units, duplicates, outliers, and whether paired rows correspond to the same observations.
34. When many pairs are tested, some apparently strong relationships may arise by chance; consider validation and multiple-testing control.
35. Answers vary. Define the variables, expected direction, chosen method, plot/checks, and limitations such as confounding, nonlinearity, sampling bias, or outliers.
