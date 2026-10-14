# Bayesian Statistics — Practice Questions

Complete the questions in order. Show formulas, calculations, code, and interpretation where requested.

## A. Concepts and vocabulary

1. Define prior, likelihood, evidence, and posterior in your own words.
2. State Bayes' theorem and explain each term.
3. Explain why a posterior distribution depends on both the prior and the likelihood.
4. Distinguish an informative prior from a weakly informative prior.
5. What does it mean for a prior to be conjugate to a likelihood?
6. Explain the difference between a credible interval and a confidence interval.
7. What is a posterior predictive distribution, and why does it include parameter uncertainty?
8. Give two reasons to perform prior sensitivity analysis.
9. What is a likelihood ratio?
10. Explain why a positive test result does not always mean a high probability of disease.

## B. Calculations

11. A hypothesis has prior probability 0.20. Evidence occurs with probability 0.80 if the hypothesis is true and 0.10 otherwise. Calculate the posterior probability.
12. A rare condition has prevalence 0.02, test sensitivity 0.95, and false-positive rate 0.08. Find \(P(D\mid +)\).
13. Two hypotheses have priors 0.60 and 0.40. Their likelihoods for observed evidence are 0.20 and 0.70. Calculate the normalized posterior probabilities.
14. A Beta(1,1) prior is updated with 6 successes and 4 failures. State the posterior distribution and posterior mean.
15. A Beta(2,3) prior is updated with 8 successes and 12 failures. Find the posterior parameters and posterior mean.
16. A Beta(4,4) posterior describes a Bernoulli success probability. Calculate the posterior predictive probability of success on the next trial.
17. A Gamma(shape=2, rate=1) prior is used for a Poisson rate. Three observation periods have counts totaling 10 and total exposure 4 units. Find the posterior shape and rate.
18. Explain what a 90% equal-tailed credible interval means.
19. If posterior odds are 3:1 and the likelihood ratio for new evidence is 2, find the updated odds and probability.
20. A model assigns posterior probability 0.7 to a hypothesis. What additional information is needed before making a decision based on that value?

## C. Python practice

21. Write a Python function that computes the posterior probability for a binary hypothesis from prior, sensitivity \(P(E\mid H)\), and false-alarm probability \(P(E\mid \neg H)\).
22. Validate your function's inputs and handle invalid probabilities.
23. Use the function to reproduce Question 11.
24. Write a function to update Beta prior parameters from success and failure counts.
25. Compute the posterior mean for a Beta(1,1) prior after 7 successes and 3 failures.
26. Create a grid from 0.01 to 0.99 for a Bernoulli probability and compute an unnormalized posterior for 5 successes and 2 failures under a uniform prior.
27. Normalize the grid posterior weights and verify that they sum to approximately 1.
28. Compute an approximate posterior mean from the normalized grid weights.
29. Simulate posterior draws from Beta(8,4) and estimate a 95% equal-tailed credible interval.
30. Compare the simulated posterior mean with the exact Beta posterior mean.
31. Calculate the posterior predictive probability of success after 12 successes and 8 failures with a Beta(2,2) prior.
32. Plot a Beta posterior density for Beta(8,4) using NumPy and Matplotlib, if Matplotlib is installed.
33. Compare posteriors from Beta(1,1) and Beta(10,10) priors after the same data. Explain the difference.
34. Implement a simple prior sensitivity analysis using at least three Beta priors and a fixed dataset.
35. Write a short report describing the prior, likelihood, posterior, interval estimate, predictive result, and limitations for a coin-bias experiment.

## Suggested submission format

For calculation questions, show the equation and result. For programming questions, include runnable code, sample output, and a short interpretation. Do not use the exercises to make real medical, financial, or other high-stakes decisions.
