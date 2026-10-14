# Reference Solutions: Sampling

Answers are examples; equivalent explanations and correct code are acceptable.

1. Population: all units of interest. Sample: observed subset. Sampling frame: list/source used to select units. Parameter: population quantity. Statistic: quantity computed from a sample.
2. A parameter describes the population; a statistic is calculated from sample data and may estimate that parameter.
3. The entity selected at a sampling stage; for example, a student selected for a survey.
4. Select units randomly from the frame, typically with equal selection probabilities. Useful when a suitable frame exists.
5. Choose a random start and then every \(k\)-th unit. Check for periodic patterns that align with the interval.
6. Split the population into relevant strata and sample within each; this can ensure important subgroups are represented.
7. Select clusters, then observe all or some units within them. It can reduce travel/collection costs; within-cluster similarity can increase variance.
8. Select at multiple levels, such as districts, then schools, then students.
9. Probability designs use known, non-zero inclusion probabilities; non-probability designs do not.
10. Recruit easy-to-reach participants. It is vulnerable to selection bias and may not generalize.
11. Select participants based on criteria or expertise. It may suit qualitative or specialist research, but population inference is limited.
12. Quota sampling fills category targets without necessarily random selection within groups; stratified random sampling randomly samples within each stratum.
13. Participants recruit other participants. It can help reach hidden populations, but referrals can create network bias.
14. Different random samples from the same population produce different statistics.
15. Bias is systematic distortion from selection or measurement. A larger sample can estimate a biased target more precisely without fixing the distortion.
16. An online-only survey may exclude people without reliable internet access.
17. Examples: nonresponse, measurement error, coverage error, data-entry error, and processing error.
18. An outdated or incomplete frame can omit eligible units or include ineligible ones, distorting selection.
19. Mean \(=(2+4+6+8)/4=5\).
20. \(s^2=\sum(x_i-\bar{x})^2/(n-1)\). The \(n-1\) correction makes sample variance unbiased under independent random sampling from a population with finite variance.
21. \(SE=s/\sqrt{n}=12/\sqrt{36}=2\).
22. Standard error is divided by \(\sqrt{4}=2\), so it is approximately halved.
23. Across repeated samples under the assumptions, about 95% of intervals constructed by the procedure would contain the fixed population parameter.
24. A t-critical value is generally used when population standard deviation is unknown, especially for small samples from a normal population.
25. Margin of error, confidence level, variability, population/design characteristics, design effect, response rate, cost, and feasibility.
26. \(n\approx(1.96\times10/2)^2=96.04\), so round up to **97**.
27. ```python
    import numpy as np
    rng = np.random.default_rng(42)
    sample = rng.choice(np.arange(1, 51), size=8, replace=False)
    print(sample)
    ```
28. ```python
    import numpy as np
    rng = np.random.default_rng(42)
    records = list(range(100))
    sample = rng.choice(records, size=12, replace=False)
    print(sample)
    ```
29. ```python
    import numpy as np
    rng = np.random.default_rng(42)
    group_a = np.repeat("A", 80)
    group_b = np.repeat("B", 20)
    a_sample = rng.choice(group_a, size=10, replace=False)
    b_sample = rng.choice(group_b, size=10, replace=False)
    print(a_sample, b_sample)
    ```
30. ```python
    import numpy as np
    import matplotlib.pyplot as plt
    rng = np.random.default_rng(42)
    population = rng.normal(loc=50, scale=12, size=10000)
    means = [rng.choice(population, size=30, replace=False).mean()
             for _ in range(500)]
    plt.hist(means, bins=25, edgecolor="black")
    plt.xlabel("Sample mean")
    plt.ylabel("Frequency")
    plt.title("Sampling distribution of the mean")
    plt.show()
    ```
31. For comparable independent samples, standard error generally decreases as n increases, approximately proportional to \(1/\sqrt{n}\). Sample means cluster around the population mean.
32. ```python
    import numpy as np
    def sample_summary(values):
        x = np.asarray(values, dtype=float)
        n = len(x)
        if n == 0:
            raise ValueError("Sample must not be empty")
        sd = float(x.std(ddof=1)) if n > 1 else float("nan")
        return {
            "size": n, "mean": float(x.mean()), "sample_std": sd,
            "standard_error": float(sd / np.sqrt(n)) if n > 1 else float("nan")
        }
    ```
33. Followers are self-selected and may differ from non-followers in demographics, interests, engagement, and buying behavior.
34. Stratified random sampling by department is a reasonable start. Randomly select students within departments; allocate proportionally or ensure minimum subgroup sizes, then use weights if allocation differs from population proportions.
35. Answers vary. Define the population and frame, justify the method, and identify likely coverage, nonresponse, or selection biases.
