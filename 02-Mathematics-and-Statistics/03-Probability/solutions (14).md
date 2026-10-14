# Probability — Reference Solutions

```python
import random

# 1. {H, T}
# 2. {1, 2, 3, 4, 5, 6}
# 3. 1/6
# 4. 3/6 = 1/2
# 5. 2/6 = 1/3
# 6. 1/2
# 7. 1 - 0.27 = 0.73
# 8. Outcome: one result; event: a set of outcomes; sample space: all possible outcomes.
# 9. Sample space {HH, HT, TH, TT}; exactly one head: 2/4 = 1/2.
# 10. 1/4
# 11. 0.4 + 0.5 - 0.2 = 0.7
# 12. 0.3 + 0.25 = 0.55
# 13. 0.6 * 0.2 = 0.12
# 14. If mutually exclusive, P(A intersection B)=0; independence would require
#     P(A intersection B)=P(A)P(B), which is nonzero for nonzero probabilities.
# 15. Given {4,5,6}, even outcomes are {4,6}; probability = 2/3.
# 16. 0.12 / 0.3 = 0.4

# 17. Let D = condition, T+ = positive test.
prevalence = 0.02
sensitivity = 0.95
false_positive_rate = 0.05
p_positive = sensitivity * prevalence + false_positive_rate * (1 - prevalence)
print(p_positive)  # 0.068

# 18. Bayes' theorem: P(D | T+) = P(T+ | D) P(D) / P(T+)
p_condition_given_positive = sensitivity * prevalence / p_positive
print(p_condition_given_positive)  # about 0.2794

# 19. A variable with countable possible numeric values, e.g. number of heads.
# 20. A variable measured on a continuum, e.g. a person's height.
# 21. E[X] = 0*0.7 + 1*0.3 = 0.3
# 22. (1 + 2 + 3) / 3 = 2
# 23. p(1-p) = 0.4*0.6 = 0.24
# 24. A PMF assigns a probability to each possible value of a discrete random variable.
# 25. Fixed n; independent trials; two outcomes per trial; constant p.
# 26. (1/2)^3 = 1/8

# 27.
p_even = len([2, 4, 6]) / 6
print(p_even)  # 0.5

# 28.
values = [0, 1, 2]
probabilities = [0.25, 0.5, 0.25]
expected_value = sum(x * p for x, p in zip(values, probabilities))
print(expected_value)  # 1.0

# 29.
rng = random.Random(42)
trials = [rng.choice(["H", "T"]) for _ in range(10_000)]
print(trials.count("H") / len(trials))  # near 0.5

# 30.
rng = random.Random(42)
rolls = [rng.randint(1, 6) for _ in range(10_000)]
print(sum(roll > 4 for roll in rolls) / len(rolls))  # near 1/3

# 31.
def complement_probability(p):
    if not 0 <= p <= 1:
        raise ValueError("Probability must be between 0 and 1.")
    return 1 - p

# 32.
def union_probability(p_a, p_b, p_intersection):
    for p in (p_a, p_b, p_intersection):
        if not 0 <= p <= 1:
            raise ValueError("Probabilities must be between 0 and 1.")
    result = p_a + p_b - p_intersection
    if not 0 <= result <= 1:
        raise ValueError("Inputs are inconsistent with a valid union probability.")
    return result

# 33. The condition may be rare, so false positives among the much larger
#     unaffected group can outnumber true positives. Base rates matter.
# 34. Theoretical probability follows a model; an empirical estimate is based
#     on observed or simulated frequencies and varies from run to run.
# 35.
distribution = {0: 0.2, 1: 0.5, 2: 0.3}
assert abs(sum(distribution.values()) - 1.0) < 1e-12
```

## Challenge example

```python
import random

rng = random.Random(7)
trials = 20_000
count_sum_7 = sum(
    rng.randint(1, 6) + rng.randint(1, 6) == 7
    for _ in range(trials)
)
estimate = count_sum_7 / trials
theoretical = 6 / 36

print("Estimate:", estimate)
print("Theoretical:", theoretical)
```

The theoretical probability is \(6/36=1/6\). The simulated estimate should be close, but not necessarily identical, because a finite simulation has sampling variability.
