# Probability for Data Science

Probability provides a mathematical way to describe uncertainty. It is used in statistical inference, experimentation, forecasting, classification, and machine learning.

## Learning objectives

- Understand experiments, outcomes, sample spaces, and events.
- Calculate probabilities using equally likely outcomes.
- Apply complement, addition, and multiplication rules.
- Distinguish independent and mutually exclusive events.
- Understand conditional probability and Bayes' theorem.
- Work with random variables, expected value, and variance.
- Calculate simple probabilities with Python.

## 1. Experiments, outcomes, and sample spaces

A **random experiment** is a process whose outcome is uncertain before it occurs. An **outcome** is one possible result, and the **sample space** is the set of all possible outcomes.

Example: for one fair six-sided die, the sample space is \(S=\{1,2,3,4,5,6\}\). An **event** is a subset of the sample space, such as rolling an even number: \(A=\{2,4,6\}\).

## 2. Probability of equally likely outcomes

When all outcomes are equally likely,

\[
P(A)=\frac{\text{number of outcomes in }A}{\text{number of outcomes in }S}
\]

For a fair die, \(P(\text{even})=3/6=1/2\).

A probability is between 0 and 1. Probability 0 means impossible under the model; probability 1 means certain under the model.

## 3. Complement rule

The complement \(A^c\) means that event \(A\) does not occur.

\[
P(A^c)=1-P(A)
\]

If the probability of rain is 0.3, the probability of no rain (under the same event definition) is 0.7.

## 4. Addition rule

For two events \(A\) and \(B\),

\[
P(A\cup B)=P(A)+P(B)-P(A\cap B)
\]

The intersection term is subtracted because outcomes in both events would otherwise be counted twice.

If \(A\) and \(B\) are mutually exclusive, then \(P(A\cap B)=0\), so \(P(A\cup B)=P(A)+P(B)\).

**Mutually exclusive** means they cannot occur together in the same trial. It does not mean independent.

## 5. Multiplication rule and independence

For two events,

\[
P(A\cap B)=P(A)P(B\mid A)
\]

If \(A\) and \(B\) are independent, then \(P(B\mid A)=P(B)\), so:

\[
P(A\cap B)=P(A)P(B)
\]

Example: two independent fair coin tosses have probability \(1/2 \times 1/2=1/4\) of both landing heads.

## 6. Conditional probability

Conditional probability describes the probability of \(A\) given that \(B\) has occurred:

\[
P(A\mid B)=\frac{P(A\cap B)}{P(B)}, \quad P(B)>0
\]

Conditioning changes the information available and can change the probability. For example, given that a randomly selected die outcome is greater than 3, the possible outcomes are \(\{4,5,6\}\); the probability it is even is \(2/3\).

## 7. Bayes' theorem

Bayes' theorem reverses a conditional probability:

\[
P(A\mid B)=\frac{P(B\mid A)P(A)}{P(B)}
\]

For a partition into \(A\) and \(A^c\),

\[
P(B)=P(B\mid A)P(A)+P(B\mid A^c)P(A^c)
\]

Example context: a medical test may be accurate, but the probability that a person has a condition after a positive test depends on the condition's prevalence as well as the test's sensitivity and false-positive rate. This is why base rates matter.

## 8. Random variables and distributions

A **random variable** maps outcomes to numerical values.

- **Discrete random variable:** countable possible values, such as the number of heads in three coin tosses.
- **Continuous random variable:** values over a continuum, such as a measured time or height.

A probability mass function (PMF) gives probabilities for discrete values. A probability density function (PDF) describes a continuous distribution; probabilities are areas under the density curve, and a single exact value has probability zero in a continuous model.

## 9. Expected value and variance

For a discrete random variable \(X\),

\[
E[X]=\sum_x xP(X=x)
\]

Expected value is a long-run average, not necessarily a value that must occur in one trial.

Variance measures spread around the mean:

\[
\operatorname{Var}(X)=E[(X-E[X])^2]
\]

An equivalent formula is \(\operatorname{Var}(X)=E[X^2]-(E[X])^2\). Standard deviation is the square root of variance.

## 10. Common distributions

- **Bernoulli:** one trial with outcomes 0 and 1, with success probability \(p\).
- **Binomial:** number of successes in \(n\) independent Bernoulli trials with the same success probability \(p\).
- **Uniform discrete:** each value in a finite set has equal probability.
- **Normal:** a continuous, symmetric, bell-shaped distribution described by mean and standard deviation.

A binomial model requires a fixed number of trials, independent trials, two outcome categories per trial, and constant success probability.

## 11. Probability with Python

```python
# Fair die: probability of an even result
sample_space = {1, 2, 3, 4, 5, 6}
even = {2, 4, 6}
p_even = len(even) / len(sample_space)
print(p_even)  # 0.5

# Complement
p_rain = 0.3
print(1 - p_rain)  # 0.7

# Expected value for a discrete distribution
values = [0, 1, 2]
probabilities = [0.25, 0.5, 0.25]
expected_value = sum(x * p for x, p in zip(values, probabilities))
print(expected_value)  # 1.0
```

For simulation, use a pseudorandom generator such as `random` or NumPy's `default_rng`. Simulation estimates probability from repeated trials; estimates vary due to sampling noise.

## 12. Best practices

- State the experiment and event clearly before calculating.
- Check that probabilities lie between 0 and 1 and that a complete distribution sums to 1.
- Do not confuse independent events with mutually exclusive events.
- Keep the conditioning event explicit in conditional probability.
- Consider base rates when interpreting test results.
- Distinguish theoretical probability from simulation estimates.
- State assumptions such as fairness, independence, and equal likelihood.

## Quick reference

| Rule | Formula |
|---|---|
| Equally likely outcomes | \(P(A)=|A|/|S|\) |
| Complement | \(P(A^c)=1-P(A)\) |
| Addition | \(P(A\cup B)=P(A)+P(B)-P(A\cap B)\) |
| Conditional probability | \(P(A\mid B)=P(A\cap B)/P(B)\) |
| Independent intersection | \(P(A\cap B)=P(A)P(B)\) |
| Bayes' theorem | \(P(A\mid B)=P(B\mid A)P(A)/P(B)\) |
| Expected value | \(E[X]=\sum_x xP(X=x)\) |

## Summary

Probability quantifies uncertainty through events, conditional information, and distributions. These ideas underpin statistical reasoning and many machine-learning models.
