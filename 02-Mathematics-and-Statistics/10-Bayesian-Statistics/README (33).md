# Bayesian Statistics

## 1. Overview

Bayesian statistics is a framework for updating beliefs about unknown quantities as new evidence becomes available. It combines prior information with observed data to produce a posterior distribution.

Bayesian reasoning is useful when evidence arrives sequentially, when prior domain knowledge matters, or when uncertainty itself needs to be quantified.

## 2. Bayes' theorem

For hypotheses/events \(H\) and evidence \(E\), where \(P(E)>0\):

\[
P(H\mid E)=\frac{P(E\mid H)P(H)}{P(E)}
\]

- **Prior** \(P(H)\): belief before observing the evidence.
- **Likelihood** \(P(E\mid H)\): probability of the evidence under the hypothesis.
- **Evidence / marginal likelihood** \(P(E)\): overall probability of the evidence.
- **Posterior** \(P(H\mid E)\): updated belief after observing the evidence.

For mutually exclusive and exhaustive hypotheses \(H_1,\ldots,H_k\):

\[
P(E)=\sum_{i=1}^{k}P(E\mid H_i)P(H_i)
\]

### Example: a diagnostic test

Suppose a condition affects 1% of a population. A test has 90% sensitivity and a 5% false-positive rate.

\[
P(D\mid +)=\frac{0.90(0.01)}{0.90(0.01)+0.05(0.99)}
\approx 0.154
\]

Although the test is reasonably sensitive, the probability of the condition after a positive result is about 15.4% in this example because the condition is uncommon. This is a simplified illustration, not medical advice.

## 3. Odds form of Bayes' theorem

Posterior odds equal prior odds multiplied by the likelihood ratio:

\[
\frac{P(H\mid E)}{P(\neg H\mid E)}
=
\frac{P(H)}{P(\neg H)}
\times
\frac{P(E\mid H)}{P(E\mid \neg H)}
\]

The likelihood ratio measures how much more likely the evidence is under \(H\) than under its alternative.

## 4. Bayesian inference for a coin

Let \(\theta\) be the unknown probability of heads. A common prior is the Beta distribution:

\[
\theta\sim\operatorname{Beta}(\alpha,\beta)
\]

After observing \(h\) heads and \(t\) tails, with conditionally independent tosses:

\[
\theta\mid data\sim\operatorname{Beta}(\alpha+h,\beta+t)
\]

The Beta prior is conjugate to the Bernoulli/Binomial likelihood, meaning the posterior belongs to the same family.

The posterior mean is:

\[
E[\theta\mid data]=\frac{\alpha+h}{\alpha+\beta+h+t}
\]

Example: with a uniform prior \(\operatorname{Beta}(1,1)\), 7 heads and 3 tails produce \(\operatorname{Beta}(8,4)\), whose posterior mean is \(8/12\approx0.667\).

## 5. Common probability models

| Model | Typical use | Parameters |
|---|---|---|
| Bernoulli | One binary outcome | Probability \(\theta\) |
| Binomial | Number of successes in fixed trials | \(n,\theta\) |
| Beta | Probability between 0 and 1 | \(\alpha,\beta\) |
| Normal | Continuous measurements | Mean \(\mu\), variance \(\sigma^2\) |
| Poisson | Counts over a fixed interval | Rate \(\lambda\) |
| Gamma | Positive rate/scale quantities | Shape and rate/scale, by convention |

Always state the parameterization. In this module, a Gamma prior for a Poisson rate uses **shape** \(a\) and **rate** \(b\), with density proportional to \(\lambda^{a-1}e^{-b\lambda}\).

## 6. Prior choices

- **Informative prior:** encodes substantial relevant prior knowledge.
- **Weakly informative prior:** regularizes implausible values without dominating reasonable data.
- **Diffuse prior:** spreads mass broadly, but is not automatically objective or harmless.
- **Conjugate prior:** yields a posterior in the same distribution family.
- **Improper prior:** integrates to infinity; it requires special care and can produce an improper posterior.

Prior choice should be justified by subject knowledge, previous data, scientific constraints, or sensitivity analysis. A prior is not simply a guess; it is an explicit assumption.

## 7. Posterior predictive distribution

Predictions account for both uncertainty in model parameters and variability in future observations:

\[
p(y_{new}\mid data)=\int p(y_{new}\mid\theta)p(\theta\mid data)\,d\theta
\]

For the Beta-Binomial setup, if \(\theta\mid data\sim\operatorname{Beta}(\alpha',\beta')\), the probability that the next Bernoulli outcome is a success equals the posterior mean \(\alpha'/(\alpha'+\beta')\).

## 8. Credible intervals vs. confidence intervals

A 95% Bayesian credible interval contains 95% of the posterior probability for the parameter under the chosen model and prior. A frequentist 95% confidence procedure has a long-run coverage interpretation under repeated sampling. The two intervals may look similar numerically, but their interpretations differ.

A credible interval may be:
- **Equal-tailed:** excludes 2.5% from each tail for a 95% interval.
- **Highest posterior density (HPD):** a shortest/high-density region under suitable conditions; may be disjoint for multimodal posteriors.

## 9. Bayesian decision theory

A posterior distribution can be combined with a loss function to choose an action. A Bayes action minimizes posterior expected loss:

\[
a^*=\arg\min_a E[L(a,\theta)\mid data]
\]

The best action depends on the consequences of errors, not only on the most probable parameter value.

## 10. Computational approaches

- **Analytical conjugate updates:** closed-form solutions for simple models.
- **Grid approximation:** evaluate unnormalized posterior weights on a parameter grid, then normalize.
- **Monte Carlo sampling:** approximate posterior summaries from representative draws.
- **MCMC:** Markov chain Monte Carlo methods for complex posterior distributions.
- **Variational inference:** optimize an approximate posterior for speed, while checking approximation quality.

For MCMC, assess trace plots, convergence diagnostics, effective sample size, and mixing. More iterations alone do not guarantee reliable inference.

## 11. Model checking and good practice

1. Define the estimand, likelihood, and assumptions.
2. Choose and document the prior before inspecting results where feasible.
3. Check prior predictive implications.
4. Fit the model and inspect posterior summaries and intervals.
5. Perform posterior predictive checks.
6. Test sensitivity to plausible alternative priors and model assumptions.
7. Report uncertainty and limitations; do not present a posterior as assumption-free fact.
8. Avoid interpreting correlation as causation without a suitable causal design.

## 12. Python tools

Core exercises use Python's standard library and NumPy. Optional libraries include SciPy, pandas, Matplotlib, PyMC, and ArviZ. Install optional packages in a virtual environment when needed:

```bash
python -m venv .venv
# Windows: .venv\Scripts\activate
# macOS/Linux: source .venv/bin/activate
python -m pip install numpy scipy matplotlib pymc arviz
```

## 13. Learning checklist

- [ ] Explain prior, likelihood, evidence, and posterior.
- [ ] Apply Bayes' theorem to two or more hypotheses.
- [ ] Update a Beta prior with Bernoulli data.
- [ ] Compute and interpret a credible interval.
- [ ] Distinguish a credible interval from a confidence interval.
- [ ] Explain posterior predictive uncertainty.
- [ ] Run a sensitivity analysis and discuss model limitations.
