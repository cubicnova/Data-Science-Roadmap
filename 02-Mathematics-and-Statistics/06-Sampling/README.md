# Sampling

A practical introduction to selecting observations from a population and using samples to learn about that population.

## Learning objectives
- Explain population, sample, sampling frame, parameter, and statistic.
- Distinguish probability and non-probability sampling.
- Describe simple random, systematic, stratified, cluster, convenience, quota, purposive, and snowball sampling.
- Explain sampling bias, sampling variability, standard error, confidence intervals, and sample-size planning.
- Use Python to draw samples and visualize sampling distributions.

## 1. Core terminology
- **Population:** the complete set of units of interest.
- **Sample:** a subset observed from the population.
- **Sampling frame:** the operational list/source used to select units.
- **Parameter:** a numerical property of a population, such as population mean \(\mu\).
- **Statistic:** a quantity calculated from a sample, such as sample mean \(\bar{x}\).
- **Sampling unit:** the entity selected at a sampling stage.
- **Sampling variability:** natural differences among statistics from different samples.

A statistic is often used to estimate an unknown parameter. Its quality depends on the selection method, sample size, measurement quality, and assumptions.

## 2. Sampling methods

### Probability sampling
Each eligible unit has a known, non-zero selection probability. This supports design-based estimates of uncertainty.
- **Simple random sampling (SRS):** select units randomly, often with equal probability.
- **Systematic sampling:** choose a random start and then select every \(k\)-th unit. Periodicity in the frame can cause bias.
- **Stratified sampling:** divide the population into relevant strata, then sample within each stratum. This helps ensure subgroup representation.
- **Cluster sampling:** randomly select clusters, then observe all units or sample within selected clusters. It can reduce collection cost, but within-cluster similarity can increase variance.
- **Multistage sampling:** select units at several levels, such as regions, schools, then students.

### Non-probability sampling
Selection probabilities are unknown or not determined by a random design.
- **Convenience sampling:** recruit easy-to-reach units.
- **Purposive/judgment sampling:** select units based on expertise or characteristics.
- **Quota sampling:** fill category targets without necessarily selecting randomly within categories.
- **Snowball sampling:** participants refer other participants.

These methods can be useful for exploration or hard-to-reach populations, but generalization requires caution.

## 3. Sampling bias and other errors
**Sampling variability/error** is random variation between a sample statistic and the population parameter. It usually decreases with larger samples under suitable designs.

**Sampling bias** is systematic distortion caused by selection or the frame, such as excluding people without internet access from an online-only survey. A larger sample does not automatically remove bias.

Other survey errors include nonresponse, measurement error, coverage error, data-entry error, and processing mistakes.

To reduce bias:
1. Define the target population precisely.
2. Use a suitable, current sampling frame.
3. Use a probability design when probability inference is needed.
4. Track nonresponse and compare respondents with the target population where possible.
5. Use neutral questions and consistent measurements.
6. Document exclusions, replacements, and weighting adjustments.

## 4. Estimation and standard error
For observations \(x_1,\ldots,x_n\), the sample mean is

\[
\bar{x}=\frac{1}{n}\sum_{i=1}^{n}x_i
\]

Sample variance is

\[
s^2=\frac{1}{n-1}\sum_{i=1}^{n}(x_i-\bar{x})^2
\]

For an independent random sample, the estimated standard error of the sample mean is

\[
SE(\bar{x})=\frac{s}{\sqrt{n}}
\]

Larger samples usually lower standard error at a rate proportional to \(1/\sqrt{n}\), assuming comparable designs and variability. Quadrupling sample size approximately halves standard error.

## 5. Confidence intervals
A common approximate interval for a population mean with a sufficiently large independent sample is

\[
\bar{x}\pm z^*\frac{s}{\sqrt{n}}
\]

For a 95% normal-based interval, \(z^*\approx1.96\). For small samples from a normal population with unknown standard deviation, a t-critical value is generally more appropriate.

A 95% confidence procedure means that across repeated samples under the model, about 95% of intervals made by the procedure would contain the fixed parameter. It does not mean there is a 95% probability that the fixed parameter lies in the already-computed interval.

These formulas do not automatically account for clustering, stratification, unequal inclusion probabilities, or nonresponse.

## 6. Sample-size planning
For estimating a population mean with margin of error \(E\), standard deviation estimate \(\sigma\), and normal critical value \(z^*\),

\[
n\approx\left(\frac{z^*\sigma}{E}\right)^2
\]

Round up. For a population proportion, a common planning approximation is

\[
n\approx\frac{(z^*)^2p(1-p)}{E^2}
\]

If \(p\) is unknown, \(p=0.5\) is conservative for this formula. Precision target, variability, design effect, expected response rate, and cost all matter.

## 7. Python sampling example
```python
import numpy as np

rng = np.random.default_rng(42)
population = np.arange(1, 101)
sample = rng.choice(population, size=10, replace=False)
print(sample)
```

Stratified sampling can be implemented by selecting separately from each group. For real data, preserve the design and inclusion probabilities rather than automatically treating all data as a simple random sample.

## 8. Best practices
- Keep the target population and unit of analysis explicit.
- Do not call a sample representative just because it is large.
- Separate sampling variability from selection bias and measurement error.
- Avoid duplicates when sampling without replacement.
- Set a seed for reproducible teaching examples.
- Report sample size, selection method, exclusions, response rate, and uncertainty.
- Do not overstate what a convenience sample says about a broader population.

Use `practice.md` for exercises, `examples.ipynb` for runnable demonstrations, and `solutions.md` for reference answers.
