# Descriptive Statistics for Data Science

Descriptive statistics summarizes and describes the main features of a dataset. It helps analysts understand typical values, spread, shape, and unusual observations before building models or drawing conclusions.

## Learning objectives

- Distinguish population and sample summaries.
- Calculate mean, median, mode, range, variance, and standard deviation.
- Understand quartiles, percentiles, and the interquartile range.
- Summarize categorical data with counts and proportions.
- Interpret skewness, outliers, and correlation.
- Compute descriptive statistics using Python and pandas.

## 1. Population and sample

A **population** is the complete group of interest. A **sample** is a subset observed from that population. Descriptive statistics can summarize either, but the formulas for variance differ depending on whether the data are treated as the full population or a sample.

## 2. Measures of central tendency

### Mean

The arithmetic mean is the sum of observations divided by the number of observations:

\[
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
\]

The mean uses every value and can be affected by extreme observations.

### Median

The median is the middle value after sorting. For an even number of observations, it is usually the average of the two central values. It is often more robust than the mean when data contain outliers.

### Mode

The mode is the most frequent value or category. A dataset may have one mode, several modes, or no unique mode depending on the values and convention used.

Example for `[2, 3, 3, 4, 18]`: mean = 6, median = 3, and mode = 3. The large value 18 raises the mean.

## 3. Measures of spread

### Range

\[
\text{Range}=\max(x)-\min(x)
\]

The range is easy to interpret but depends only on the minimum and maximum.

### Variance

Population variance:

\[
\sigma^2=\frac{1}{N}\sum_{i=1}^{N}(x_i-\mu)^2
\]

Sample variance:

\[
s^2=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2
\]

The sample formula uses \(n-1\) in the denominator and is commonly used to estimate population variance from a sample.

### Standard deviation

Standard deviation is the square root of variance. It is expressed in the same units as the original data. A larger standard deviation indicates greater spread around the mean, although it does not describe every feature of a distribution.

## 4. Quartiles, percentiles, and IQR

Quartiles divide ordered data into four parts:
- \(Q_1\): approximately the 25th percentile.
- \(Q_2\): the median, or 50th percentile.
- \(Q_3\): approximately the 75th percentile.

The interquartile range is:

\[
IQR=Q_3-Q_1
\]

Different software and textbooks may use different percentile interpolation conventions, especially for small datasets. State the method when exact quartile values matter.

### A common outlier rule

The 1.5×IQR rule flags values below \(Q_1-1.5IQR\) or above \(Q_3+1.5IQR\) as potential outliers. This is a diagnostic flag, not proof that a value is erroneous; investigate context before removing observations.

## 5. Frequency and proportions

For categorical data, useful summaries include:
- Frequency: the count for each category.
- Relative frequency: category count divided by total count.
- Percentage: relative frequency multiplied by 100.

A bar chart can compare category counts. A histogram is commonly used to explore the distribution of numerical data.

## 6. Distribution shape

- **Symmetric:** left and right sides are broadly similar.
- **Right-skewed:** a longer tail extends toward larger values; the mean may exceed the median.
- **Left-skewed:** a longer tail extends toward smaller values; the mean may be below the median.
- **Multimodal:** more than one peak may be present.

These are general tendencies, not strict rules for every dataset.

## 7. Correlation

Correlation summarizes the strength and direction of a relationship between variables. Pearson's correlation coefficient \(r\) ranges from -1 to 1 and measures linear association.

- Near +1: strong positive linear association.
- Near -1: strong negative linear association.
- Near 0: little or no linear association, though a nonlinear relationship may still exist.

Correlation does not establish causation. Outliers can substantially affect Pearson correlation.

## 8. Descriptive statistics with Python

```python
import numpy as np
import pandas as pd

values = [2, 3, 3, 4, 18]
series = pd.Series(values)

print("Mean:", series.mean())
print("Median:", series.median())
print("Mode(s):", series.mode().tolist())
print("Range:", series.max() - series.min())
print("Sample variance:", series.var())
print("Sample standard deviation:", series.std())
print(series.describe())
```

Pandas `Series.var()` and `Series.std()` use `ddof=1` by default, which corresponds to the sample variance and standard deviation. NumPy functions default to `ddof=0`, which computes population variance and standard deviation.

## 9. Missing data and data quality

Before summarizing data:
- Inspect missing values and decide whether to report, exclude, or impute them.
- Check data types and units.
- Identify duplicate records.
- Review extreme values for possible errors and valid rare cases.
- Avoid comparing statistics across groups without checking sample sizes and measurement definitions.

## 10. Best practices

- Report sample size alongside summary statistics.
- Use median and IQR for skewed data or when outliers dominate the mean and standard deviation.
- Use mean and standard deviation when they suit the distribution and question.
- Visualize distributions instead of relying only on a single number.
- Make the variance convention and percentile method explicit when needed.
- Avoid treating an outlier flag as automatic permission to delete a record.
- Remember that descriptive statistics describe observed data; they do not alone prove causal claims.

## Quick reference

| Statistic | What it describes |
|---|---|
| Mean | Arithmetic average |
| Median | Middle ordered value |
| Mode | Most frequent value/category |
| Range | Maximum minus minimum |
| Variance | Average squared deviation, with population/sample conventions |
| Standard deviation | Square root of variance |
| IQR | \(Q_3-Q_1\), spread of the middle half |
| Relative frequency | Category count divided by total |
| Pearson correlation | Direction and strength of linear association |

## Summary

Descriptive statistics turns raw data into interpretable summaries. Combine measures of center and spread with visualizations and data-quality checks to understand a dataset responsibly.
