# Distributions

A practical introduction to probability distributions for data science.

## Learning objectives

By the end of this module, you should be able to:

- Explain what a probability distribution describes.
- Distinguish discrete and continuous random variables.
- Interpret PMF, PDF, CDF, expected value, and variance.
- Recognize common distributions and choose a reasonable model for a problem.
- Simulate samples and visualize distributions using Python.

## 1. What is a probability distribution?

A probability distribution describes the possible values of a random variable and how probability is assigned to those values.

A **random variable** maps outcomes of a random process to numerical values.

- **Discrete random variable:** takes countable values, such as the number of defective items.
- **Continuous random variable:** can take any value in an interval, such as a measured height or waiting time.

### Probability mass function (PMF)

For a discrete random variable \(X\), the PMF is

\[
p_X(x) = P(X=x)
\]

Properties: \(p_X(x)\geq 0\), and the probabilities across all possible values sum to 1.

### Probability density function (PDF)

For a continuous random variable, a PDF \(f_X(x)\) describes density. Probabilities are areas:

\[
P(a \leq X \leq b)=\int_a^b f_X(x)\,dx
\]

A PDF value is not generally the probability of one exact value. For a continuous variable, \(P(X=x)=0\).

### Cumulative distribution function (CDF)

For either type of random variable,

\[
F_X(x)=P(X\leq x)
\]

The CDF is non-decreasing, ranges from 0 to 1, and can be used to calculate interval probabilities.

## 2. Expected value and variance

The **expected value** is the long-run average:

- Discrete: \(E[X]=\sum_x x\,p_X(x)\)
- Continuous: \(E[X]=\int_{-\infty}^{\infty} x f_X(x)\,dx\)

Variance measures spread around the mean:

\[
\operatorname{Var}(X)=E[(X-\mu)^2]=E[X^2]-E[X]^2
\]

Standard deviation is \(\sigma=\sqrt{\operatorname{Var}(X)}\).

## 3. Common distributions

### Bernoulli distribution

One trial with two outcomes, usually success (1) and failure (0). If \(P(X=1)=p\), then:

- Mean: \(p\)
- Variance: \(p(1-p)\)

Example: whether one email is spam.

### Binomial distribution

Counts successes in a fixed number \(n\) of independent Bernoulli trials with the same success probability \(p\).

\[
P(X=k)=\binom{n}{k}p^k(1-p)^{n-k},\quad k=0,\ldots,n
\]

- Mean: \(np\)
- Variance: \(np(1-p)\)

Example: number of heads in 10 fair coin tosses.

Assumptions: fixed number of trials, independent trials, two outcomes per trial, constant probability of success.

### Geometric distribution

Counts trials up to and including the first success (in the common NumPy convention, support starts at 1).

\[
P(X=k)=(1-p)^{k-1}p,\quad k=1,2,\ldots
\]

- Mean: \(1/p\)
- Variance: \((1-p)/p^2\)

Example: number of attempts until the first successful login test, assuming independent trials and constant success probability.

### Poisson distribution

Models counts of events in a fixed interval when events occur independently at a constant average rate \(\lambda\).

\[
P(X=k)=\frac{e^{-\lambda}\lambda^k}{k!},\quad k=0,1,\ldots
\]

- Mean: \(\lambda\)
- Variance: \(\lambda\)

Example: number of support tickets arriving per hour, when the assumptions are plausible.

### Uniform distribution

For a continuous uniform distribution on \([a,b]\), all equal-length subintervals have equal probability.

- PDF: \(f(x)=1/(b-a)\) for \(a\leq x\leq b\)
- Mean: \((a+b)/2\)
- Variance: \((b-a)^2/12\)

Example: simulated random values between 0 and 1.

### Normal distribution

A symmetric, bell-shaped continuous distribution defined by mean \(\mu\) and standard deviation \(\sigma>0\).

\[
f(x)=\frac{1}{\sigma\sqrt{2\pi}}
\exp\left(-\frac{(x-\mu)^2}{2\sigma^2}\right)
\]

- Mean: \(\mu\)
- Variance: \(\sigma^2\)

The empirical 68–95–99.7 rule says approximately 68%, 95%, and 99.7% of values lie within 1, 2, and 3 standard deviations of the mean for a normal distribution.

### Exponential distribution

Models non-negative waiting times between events in a Poisson process.

- Rate parameter: \(\lambda>0\)
- PDF: \(f(x)=\lambda e^{-\lambda x}\), \(x\geq0\)
- Mean: \(1/\lambda\)
- Variance: \(1/\lambda^2\)

In NumPy, `np.random.exponential(scale=...)` uses `scale = 1 / rate`.

## 4. Quick comparison

| Distribution | Type | Typical question | Parameters |
|---|---|---|---|
| Bernoulli | Discrete | Did one event succeed? | \(p\) |
| Binomial | Discrete | How many successes in \(n\) trials? | \(n,p\) |
| Geometric | Discrete | How many trials until first success? | \(p\) |
| Poisson | Discrete | How many events in an interval? | \(\lambda\) |
| Uniform | Continuous | Value equally likely across a range? | \(a,b\) |
| Normal | Continuous | Symmetric variation around a mean? | \(\mu,\sigma\) |
| Exponential | Continuous | How long until an event? | \(\lambda\) or scale |

## 5. Python with NumPy and Matplotlib

```python
import numpy as np
import matplotlib.pyplot as plt

rng = np.random.default_rng(42)
samples = rng.normal(loc=50, scale=10, size=1000)

plt.hist(samples, bins=30, edgecolor="black")
plt.xlabel("Value")
plt.ylabel("Frequency")
plt.title("Simulated normal distribution")
plt.show()
```

Reproducibility: use a fixed random seed when you need repeatable examples. Prefer `np.random.default_rng(seed)` for new NumPy code.

## 6. How to choose a distribution

1. Identify the random variable and whether it is discrete or continuous.
2. Clarify what is being counted or measured and the time/space interval.
3. Check the distribution's assumptions; do not choose only because a histogram looks familiar.
4. Estimate parameters from context or data.
5. Compare the model with observed data using plots and suitable goodness-of-fit methods.
6. Explain uncertainty and limitations.

## 7. Best practices and common mistakes

- A distribution is a model, not proof that real data follow it.
- Do not confuse PDF height with probability; continuous probabilities are areas.
- Do not assume independence or a constant event rate without evidence.
- Keep the parameterization clear, especially rate versus scale for the exponential distribution.
- Use simulations to build intuition, not as a substitute for checking assumptions.
- A histogram depends on bin choice; use it alongside summaries and other plots.
- Separate the theoretical distribution from a finite random sample, which naturally varies.

## 8. Practice workflow

Use `practice.md` for exercises, `examples.ipynb` for runnable demonstrations, and `solutions.md` for reference answers. Try the questions before checking the solutions.
