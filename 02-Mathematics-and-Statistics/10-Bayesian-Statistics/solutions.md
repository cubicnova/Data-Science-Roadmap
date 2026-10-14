# Bayesian Statistics — Reference Solutions

Answers are illustrative. Equivalent correct reasoning and code are acceptable.

## A. Concepts and vocabulary

1. **Prior:** distribution before current data. **Likelihood:** probability model for data given a parameter/hypothesis. **Evidence:** marginal probability of observed data. **Posterior:** updated distribution after data.
2. \(P(H\mid E)=P(E\mid H)P(H)/P(E)\).
3. The prior expresses assumptions before data; the likelihood describes how compatible each parameter value is with the observations.
4. An informative prior contributes substantial specific knowledge; a weakly informative prior mainly discourages implausible extremes while allowing data to update it.
5. A conjugate prior produces a posterior in the same distribution family as the prior.
6. A credible interval has a stated posterior probability conditional on model and prior. A confidence interval is a procedure with long-run coverage under repeated sampling.
7. It predicts future observations by averaging over posterior parameter uncertainty and outcome noise.
8. The prior may be uncertain; posterior conclusions can be sensitive to prior assumptions, especially with sparse data.
9. \(P(E\mid H)/P(E\mid\neg H)\).
10. Base rates matter. Even a good test can have many false positives when the condition is rare.

## B. Calculations

11. \(P(E)=0.8(0.2)+0.1(0.8)=0.24\). Posterior \(=0.16/0.24=2/3\approx0.667\).
12. \(P(D\mid +)=0.95(0.02)/[0.95(0.02)+0.08(0.98)]\approx0.195\), or about 19.5%.
13. Unnormalized weights are \(0.60(0.20)=0.12\) and \(0.40(0.70)=0.28\); total \(0.40\). Posteriors: 0.30 and 0.70.
14. Posterior Beta(7,5); mean \(7/12\approx0.5833\).
15. Posterior Beta(10,15); mean \(10/25=0.40\).
16. Predictive success probability \(4/(4+4)=0.5\).
17. Posterior shape \(=2+10=12\), rate \(=1+4=5\); posterior Gamma(12,5).
18. The central interval from the 5th to 95th posterior percentiles contains 90% of posterior probability under the model and prior.
19. Updated odds \(=3:1\times2=6:1\); probability \(=6/(6+1)=6/7\approx0.857\).
20. Need the decision context, consequences/losses, alternative hypotheses, data quality, model assumptions, and relevant uncertainty.

## C. Python practice — reference implementations

21–23. Binary-hypothesis update:

```python
def posterior_probability(prior, p_e_given_h, p_e_given_not_h):
    values = (prior, p_e_given_h, p_e_given_not_h)
    if any(not 0 <= x <= 1 for x in values):
        raise ValueError("All probabilities must be between 0 and 1.")
    evidence = p_e_given_h * prior + p_e_given_not_h * (1 - prior)
    if evidence == 0:
        raise ValueError("Evidence has zero probability under both cases.")
    return p_e_given_h * prior / evidence

print(posterior_probability(0.20, 0.80, 0.10))
# 0.6666666666666666
```

24. Beta update:

```python
def beta_update(alpha, beta, successes, failures):
    if alpha <= 0 or beta <= 0:
        raise ValueError("Beta prior parameters must be positive.")
    if successes < 0 or failures < 0:
        raise ValueError("Counts cannot be negative.")
    return alpha + successes, beta + failures
```

25. Posterior Beta(8,4), mean \(8/12\approx0.6667\).

26–28. Grid approximation:

```python
import numpy as np

theta = np.linspace(0.01, 0.99, 500)
successes, failures = 5, 2
weights = theta**successes * (1 - theta)**failures  # uniform prior
weights = weights / weights.sum()
approx_mean = np.sum(theta * weights)
print(weights.sum(), approx_mean)
```

The normalized weights sum to approximately 1. This is a discrete grid approximation, so results depend slightly on grid resolution and endpoints.

29–30. Simulation:

```python
import numpy as np

rng = np.random.default_rng(42)
draws = rng.beta(8, 4, size=200_000)
interval = np.quantile(draws, [0.025, 0.975])
print("Simulated mean:", draws.mean())
print("95% interval:", interval)
print("Exact mean:", 8 / 12)
```

The simulation mean should be close to 0.6667; exact simulation results vary with the random seed and sample size.

31. With Beta(2,2), 12 successes and 8 failures yield Beta(14,10); predictive probability \(14/24\approx0.5833\).

32. Optional density plot:

```python
import numpy as np
import matplotlib.pyplot as plt
from math import gamma

x = np.linspace(0.001, 0.999, 500)
a, b = 8, 4
beta_fn = gamma(a) * gamma(b) / gamma(a + b)
density = x**(a - 1) * (1 - x)**(b - 1) / beta_fn
plt.plot(x, density)
plt.xlabel("Success probability")
plt.ylabel("Posterior density")
plt.title("Beta(8, 4) posterior")
plt.show()
```

33. Beta(1,1) is a uniform prior; Beta(10,10) is more concentrated around 0.5. With the same data, the more concentrated prior generally pulls the posterior more toward 0.5, particularly with modest sample sizes.
34. One example is to compare Beta(1,1), Beta(2,2), and Beta(10,10) prior updates for the same observed successes and failures, then compare posterior means and intervals.
35. A strong report clearly labels assumptions, reports posterior mean and credible interval, gives a posterior predictive probability, compares at least one alternative prior, and discusses limitations such as independence assumptions, small sample size, and prior sensitivity.
