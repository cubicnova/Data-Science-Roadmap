# Statistics Cheat Sheet

A practical, beginner-friendly reference for statistics in data science, machine learning, and data analytics. This cheat sheet covers essential statistical concepts, formulas, probability distributions, sampling techniques, hypothesis testing, confidence intervals, correlation, regression, and Python implementations.

**Repository:** [CubicNova Data Science Roadmap](https://github.com/cubicnova/Data-Science-Roadmap)

---

## Table of Contents

1. [Introduction to Statistics](#1-introduction-to-statistics)
2. [Types of Statistics](#2-types-of-statistics)
3. [Types of Data and Measurement Scales](#3-types-of-data-and-measurement-scales)
4. [Population and Sample](#4-population-and-sample)
5. [Measures of Central Tendency](#5-measures-of-central-tendency)
6. [Measures of Dispersion](#6-measures-of-dispersion)
7. [Percentiles, Quartiles, and IQR](#7-percentiles-quartiles-and-iqr)
8. [Probability Fundamentals](#8-probability-fundamentals)
9. [Conditional Probability and Bayes' Theorem](#9-conditional-probability-and-bayes-theorem)
10. [Random Variables and Expected Value](#10-random-variables-and-expected-value)
11. [Probability Distributions](#11-probability-distributions)
12. [Sampling Techniques](#12-sampling-techniques)
13. [Sampling Distributions and the Central Limit Theorem](#13-sampling-distributions-and-the-central-limit-theorem)
14. [Standard Error and Confidence Intervals](#14-standard-error-and-confidence-intervals)
15. [Hypothesis Testing](#15-hypothesis-testing)
16. [Common Statistical Tests](#16-common-statistical-tests)
17. [Correlation and Covariance](#17-correlation-and-covariance)
18. [Regression Analysis](#18-regression-analysis)
19. [Outlier Detection](#19-outlier-detection)
20. [Exploratory Data Analysis](#20-exploratory-data-analysis)
21. [Statistical Methods in Machine Learning](#21-statistical-methods-in-machine-learning)
22. [Python Libraries for Statistics](#22-python-libraries-for-statistics)
23. [Common Statistical Mistakes](#23-common-statistical-mistakes)
24. [Practice Exercises](#24-practice-exercises)
25. [Quick Formula Reference](#25-quick-formula-reference)
26. [Official Resources](#26-official-resources)

---

## 1. Introduction to Statistics

Statistics is the science of collecting, organizing, analyzing, interpreting, and communicating data.

### Applications in data science

- Understanding datasets and identifying patterns.
- Summarizing distributions and variability.
- Estimating population characteristics from samples.
- Testing hypotheses and comparing groups.
- Measuring relationships between variables.
- Quantifying uncertainty in estimates.
- Supporting machine learning model development and evaluation.
- Making evidence-based business decisions.

### Example

Suppose a company wants to understand customer spending.

```python
import numpy as np

spending = np.array([200, 250, 300, 350, 900])

print("Mean:", np.mean(spending))
print("Median:", np.median(spending))
print("Standard deviation:", np.std(spending, ddof=1))
```

The mean is sensitive to the unusually high value of 900, whereas the median is more resistant to that value.

**Key principle:** Statistics helps distinguish patterns supported by data from conclusions that remain uncertain.

---

## 2. Types of Statistics

### 2.1 Descriptive statistics

Descriptive statistics summarize the observed data.

Examples:

- Mean, median, and mode.
- Minimum and maximum.
- Variance and standard deviation.
- Quartiles and percentiles.
- Tables, histograms, and box plots.

### 2.2 Inferential statistics

Inferential statistics use sample data to estimate population characteristics or evaluate hypotheses.

Examples:

- Confidence intervals.
- Hypothesis tests.
- Regression inference.
- Analysis of variance (ANOVA).
- Bootstrap estimation.

### Comparison

| Feature | Descriptive statistics | Inferential statistics |
|---|---|---|
| Purpose | Summarize observed data | Draw conclusions beyond observed data |
| Typical output | Mean, median, standard deviation | Estimates, confidence intervals, p-values |
| Uncertainty | May summarize observed variation | Explicitly evaluates sampling uncertainty |
| Example | Average salary in a dataset | Estimate average salary in a wider population |

### Statistical workflow

1. Define the question.
2. Identify the target population and variables.
3. Collect representative, reliable data.
4. Inspect and clean the data.
5. Summarize distributions.
6. Select an appropriate statistical method.
7. Check assumptions.
8. Interpret the results in context.
9. Communicate uncertainty and limitations.

---

## 3. Types of Data and Measurement Scales

Understanding data types helps determine which summaries, charts, and statistical tests are appropriate.

### 3.1 Categorical data

Categorical variables represent groups or labels.

**Nominal:** Categories without an inherent order.

Examples: department, city, product category.

**Ordinal:** Categories with an order, but unequal or unknown distances between levels.

Examples: satisfaction ratings, education levels, severity categories.

### 3.2 Numerical data

**Discrete:** Countable values, often integers.

Examples: number of purchases, number of defects.

**Continuous:** Measurements that can take values across a range.

Examples: height, weight, time, temperature.

### 3.3 Measurement scales

| Scale | Meaning | Example |
|---|---|---|
| Nominal | Categories without order | Blood group |
| Ordinal | Ordered categories | Satisfaction rating |
| Interval | Equal intervals, no meaningful absolute zero | Temperature in °C |
| Ratio | Equal intervals and meaningful zero | Weight, distance, duration |

### 3.4 Choosing a summary

| Data type | Useful summaries |
|---|---|
| Nominal | Counts, proportions, mode |
| Ordinal | Counts, proportions, median, percentiles |
| Numerical | Mean, median, variance, standard deviation, quantiles |

Treat ordinal categories carefully: calculating a mean from coded labels is not automatically meaningful.

---

## 4. Population and Sample

### Population

The complete set of individuals or observations relevant to a research question.

### Sample

A subset of the population used to estimate population characteristics.

### Important notation

| Symbol | Meaning |
|---|---|
| \(N\) | Population size |
| \(n\) | Sample size |
| \(\mu\) | Population mean |
| \(\bar{x}\) | Sample mean |
| \(\sigma^2\) | Population variance |
| \(s^2\) | Sample variance |
| \(p\) | Population proportion |
| \(\hat{p}\) | Sample proportion |

### Population versus sample

| Feature | Population | Sample |
|---|---|---|
| Mean | \(\mu\) | \(\bar{x}\) |
| Standard deviation | \(\sigma\) | \(s\) |
| Size | \(N\) | \(n\) |
| Role | Describes the full population | Provides evidence about the population |

### Calculate a sample mean

For observations \(x_1, x_2, \ldots, x_n\):

\[
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
\]

Python:

```python
import numpy as np

data = np.array([10, 20, 30, 40, 50])

sample_mean = np.mean(data)

print(sample_mean)
```

### Sampling bias

A large sample is not necessarily representative. Biased recruitment, nonresponse, undercoverage, and measurement errors can distort estimates even when the sample size is large.

---

## 5. Measures of Central Tendency

Central tendency describes the center of a distribution.

### 5.1 Mean

The arithmetic mean is the sum of the observations divided by their count.

\[
\bar{x}=\frac{\sum_{i=1}^{n}x_i}{n}
\]

Example:

\[
\bar{x}=\frac{10+20+30+40+50}{5}=30
\]

Python:

```python
import numpy as np

data = [10, 20, 30, 40, 50]

print(np.mean(data))
```

**Use when:** The mean is a meaningful summary and extreme observations do not make it misleading.

### 5.2 Median

The median is the middle observation after sorting the data. For an even number of observations, it is the average of the two middle values.

```python
data = [10, 20, 30, 40, 50, 100]

print(np.median(data))
```

The median is more robust to extreme values than the mean.

### 5.3 Mode

The mode is the most frequently occurring value or category.

```python
from scipy import stats

data = [10, 20, 20, 30, 40]

print(stats.mode(data, keepdims=False))
```

For categorical data, Pandas is also convenient:

```python
import pandas as pd

categories = pd.Series(["IT", "HR", "IT", "Finance", "IT"])

print(categories.mode())
```

A distribution may have more than one mode or no uniquely useful mode.

### Comparison

| Measure | Strength | Limitation |
|---|---|---|
| Mean | Uses all numerical values | Sensitive to extreme values |
| Median | Robust to extreme values | Does not reflect every value's magnitude |
| Mode | Works with categorical data | May not be unique or informative |

### 5.4 Weighted mean

Use a weighted mean when observations contribute unequally.

\[
\bar{x}_w=\frac{\sum_{i=1}^{n}w_ix_i}{\sum_{i=1}^{n}w_i}
\]

```python
values = np.array([70, 80, 90])
weights = np.array([1, 2, 3])

weighted_mean = np.average(values, weights=weights)

print(weighted_mean)
```

### 5.5 Geometric mean

For positive values, the geometric mean is:

\[
G=\left(\prod_{i=1}^{n}x_i\right)^{1/n}
\]

It is useful for multiplicative growth rates and certain ratio-based data.

```python
from scipy.stats import gmean

growth_factors = [1.05, 1.10, 0.98]

print(gmean(growth_factors))
```

The example returns the geometric mean of the growth factors, not their percentage changes.

---

## 6. Measures of Dispersion

Dispersion describes how much observations vary.

### 6.1 Range

\[
\text{Range}=x_{\max}-x_{\min}
\]

```python
data = np.array([10, 20, 30, 40, 50])

print(np.max(data) - np.min(data))
```

### 6.2 Population variance

\[
\sigma^2=\frac{\sum_{i=1}^{N}(x_i-\mu)^2}{N}
\]

Population variance divides by \(N\).

```python
np.var(data, ddof=0)
```

### 6.3 Sample variance

\[
s^2=\frac{\sum_{i=1}^{n}(x_i-\bar{x})^2}{n-1}
\]

Sample variance commonly divides by \(n-1\) when estimating population variance from an independent sample.

```python
np.var(data, ddof=1)
```

### 6.4 Standard deviation

Standard deviation is the square root of variance.

\[
s=\sqrt{s^2}
\]

```python
np.std(data, ddof=1)
```

Standard deviation is expressed in the original units of the variable.

### 6.5 Mean absolute deviation

The mean absolute deviation around the mean is:

\[
\text{MAD}_{\text{mean}}=\frac{1}{n}\sum_{i=1}^{n}|x_i-\bar{x}|
\]

```python
np.mean(np.abs(data - np.mean(data)))
```

### 6.6 Coefficient of variation

For a positive, nonzero mean:

\[
CV=\frac{s}{\bar{x}}\times100\%
\]

```python
mean = np.mean(data)
std = np.std(data, ddof=1)

if mean != 0:
    cv = std / abs(mean) * 100
    print(cv)
```

The coefficient of variation is most interpretable for ratio-scale data with a meaningful, positive mean. It can be misleading when the mean is close to zero or negative.

---

## 7. Percentiles, Quartiles, and IQR

### 7.1 Percentiles

The \(p\)th percentile is a value below which approximately \(p\%\) of observations fall, depending on the quantile convention used.

```python
data = np.array([10, 20, 30, 40, 50, 60, 70, 80])

print(np.percentile(data, 25))
print(np.percentile(data, 50))
print(np.percentile(data, 75))
```

### 7.2 Quartiles

- \(Q_1\): 25th percentile.
- \(Q_2\): 50th percentile, or median.
- \(Q_3\): 75th percentile.

### 7.3 Interquartile range

\[
IQR=Q_3-Q_1
\]

```python
q1 = np.percentile(data, 25)
q3 = np.percentile(data, 75)

iqr = q3 - q1

print(iqr)
```

### 7.4 IQR-based outlier fences

Lower fence:

\[
L=Q_1-1.5(IQR)
\]

Upper fence:

\[
U=Q_3+1.5(IQR)
\]

```python
lower_fence = q1 - 1.5 * iqr
upper_fence = q3 + 1.5 * iqr

outliers = data[
    (data < lower_fence) |
    (data > upper_fence)
]

print(outliers)
```

These fences flag potential outliers. They do not establish that an observation is erroneous.

### 7.5 Five-number summary

A five-number summary contains:

1. Minimum.
2. First quartile.
3. Median.
4. Third quartile.
5. Maximum.

```python
np.percentile(data, [0, 25, 50, 75, 100])
```

The five-number summary is useful for comparing distributions and constructing box plots.

---

## 8. Probability Fundamentals

Probability quantifies uncertainty about events.

### 8.1 Probability range

For an event \(A\):

\[
0\le P(A)\le1
\]

- \(P(A)=0\): impossible under the specified model.
- \(P(A)=1\): certain under the specified model.

### 8.2 Complement rule

\[
P(A^c)=1-P(A)
\]

Example: If the probability of success is 0.8, the probability of failure is 0.2.

### 8.3 Addition rule

For two events:

\[
P(A\cup B)=P(A)+P(B)-P(A\cap B)
\]

If the events are mutually exclusive:

\[
P(A\cup B)=P(A)+P(B)
\]

### 8.4 Multiplication rule

\[
P(A\cap B)=P(A)P(B\mid

