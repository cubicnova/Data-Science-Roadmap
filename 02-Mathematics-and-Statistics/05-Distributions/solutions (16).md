# Reference Solutions: Distributions

These are sample answers. Coding approaches and written explanations may vary.

1. A probability distribution describes possible values of a random variable and the probabilities or density associated with them. A random variable maps outcomes to numbers.
2. Discrete variables take countable values (e.g., number of defects); continuous variables are measured on a continuum (e.g., response time).
3. A PMF gives \(P(X=x)\) for a discrete variable. Values are non-negative and sum to 1 across the support.
4. A PDF describes probability density. Probability over an interval is the area under the curve; a single point has probability zero for a continuous variable.
5. \(F_X(x)=P(X\leq x)\).
6. Expected value is the long-run average over repeated observations.
7. Standard deviation is the square root of variance.
8. Let \(X=1\) for heads and \(X=0\) for tails. For a fair coin, \(p=0.5\).
9. Fixed trial count, independent trials, two outcomes per trial, and constant success probability.
10. \(n=8\), \(p=0.5\).
11. Mean \(np=3\); variance \(np(1-p)=2.1\).
12. It models the number of trials up to and including the first success; here support begins at 1.
13. \(E[X]=1/p=5\) trials.
14. For example, number of support tickets arriving in one hour, if arrivals are approximately independent and the rate is stable.
15. Both mean and variance equal \(\lambda=4\).
16. PDF \(1/(8-2)=1/6\) on [2, 8]; mean 5; variance \((8-2)^2/12=3\).
17. \(\mu\) is the mean; \(\sigma\) is the standard deviation.
18. Approximately 68% within 1σ, 95% within 2σ, and 99.7% within 3σ of the mean.
19. A non-negative waiting time, such as time between events in a Poisson process.
20. Mean waiting time \(1/\lambda=2\) minutes.
21. NumPy's `scale` is the mean waiting time and equals \(1/\lambda\), not the rate itself.
22. (a) discrete, (b) continuous, (c) discrete, (d) continuous.
23. Binomial, because it counts successes in a fixed number of independent trials with constant success probability.
24. Poisson, if events are independent and the average rate is approximately constant over the interval.
25. Normal. Check the data shape, outliers, dependence, and whether the model is appropriate for the measurement process.
26. Example:
    ```python
    import numpy as np
    rng = np.random.default_rng(42)
    x = rng.binomial(n=1, p=0.25, size=500)
    print(x.mean())  # observed success rate
    ```
27. Example:
    ```python
    import numpy as np
    rng = np.random.default_rng(42)
    x = rng.binomial(n=12, p=0.4, size=1000)
    print(x.mean())  # should be near 4.8
    ```
28. Example:
    ```python
    import numpy as np
    rng = np.random.default_rng(42)
    x = rng.poisson(lam=3, size=1000)
    print("sample mean:", x.mean())
    print("sample variance:", x.var())
    # Both should be around 3, with sampling variation.
    ```
29. Example:
    ```python
    import numpy as np
    rng = np.random.default_rng(42)
    x = rng.uniform(0, 1, size=1000)
    print(x.mean())  # should be near 0.5
    ```
30. Example:
    ```python
    import numpy as np
    import matplotlib.pyplot as plt
    rng = np.random.default_rng(42)
    x = rng.normal(loc=10, scale=2, size=1000)
    plt.hist(x, bins=30, edgecolor="black")
    plt.xlabel("Value")
    plt.ylabel("Frequency")
    plt.title("Normal samples")
    plt.show()
    ```
31. Example:
    ```python
    import numpy as np
    rng = np.random.default_rng(42)
    x = rng.exponential(scale=2, size=1000)
    print(x.mean())  # should be near 2
    ```
32. Example:
    ```python
    import numpy as np
    def summarize(sample):
        x = np.asarray(sample, dtype=float)
        return {"mean": float(x.mean()), "variance": float(x.var())}
    print(summarize([1, 2, 3, 4]))
    ```
33. Histogram shape depends on sample size and bin choice; different distributions can look similar in finite samples. Use assumptions, summaries, plots, and formal checks as appropriate.
34. Check whether the rate is roughly stable over the hour and whether arrivals are reasonably independent; also investigate clustering or seasonality.
35. Answers vary. A good response defines the variable, names a plausible distribution and its parameters, explains assumptions, and states a limitation.
