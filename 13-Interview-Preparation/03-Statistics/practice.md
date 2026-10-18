# Statistics Interview Practice

Attempt each question before checking `solution.md`. For numerical questions, show the formula and intermediate steps. For applied questions, explain assumptions and limitations.

## Part A — Conceptual questions

1. When would you prefer the median over the mean?
2. Explain variance, standard deviation, and standard error. How are they different?
3. What is the difference between a population parameter and a sample statistic?
4. Explain conditional probability, independence, and Bayes' theorem.
5. What does the Central Limit Theorem say, and what does it not say?
6. Explain a sampling distribution and a standard error.
7. What is a confidence interval? Interpret a 95% confidence procedure correctly.
8. What is a p-value? List two incorrect interpretations.
9. Explain Type I error, Type II error, and statistical power.
10. How do sample size and effect size affect power?
11. What is the difference between statistical and practical significance?
12. When would you use a t-test rather than a z-test?
13. Compare an independent t-test and a paired t-test.
14. When might you use a chi-square test or Fisher's exact test?
15. Explain Pearson and Spearman correlation.
16. Why does correlation not establish causation? Give a confounding example.
17. What assumptions and diagnostics matter in linear regression?
18. What is multicollinearity, and why can it matter?
19. Why do multiple hypothesis tests increase the chance of false positives?
20. What is an A/B test sample-ratio mismatch, and why investigate it?
21. What is Simpson's paradox?
22. Explain selection bias, survivorship bias, and non-response bias.
23. What is a bootstrap, and when is it useful?
24. How would you explain a statistically significant but tiny product effect to a non-technical stakeholder?

## Part B — Calculations

1. For values `[2, 4, 4, 4, 5, 5, 7, 9]`, calculate the mean, median, range, and mode.
2. A sample has \(n=100\), sample standard deviation \(s=20\), and mean \(\bar{x}=50\). Calculate the standard error of the mean.
3. A fair coin is tossed three times. What is the probability of exactly two heads?
4. A condition affects 1% of a population. A test has 90% sensitivity and 95% specificity. Find the probability that a randomly selected person who tests positive actually has the condition.
5. A sample mean is 120, and its standard error is 4. Using an approximate 95% normal critical value of 1.96, calculate the confidence interval.
6. A conversion rate increases from 4% to 5%. Calculate the absolute percentage-point change and relative lift.
7. In a test of \(H_0\), the p-value is 0.03 and the significance threshold is 0.05. State the conventional decision and what cannot be concluded.
8. A test has \(\alpha=0.05\), and the null is actually false. What kind of error occurs if the test fails to reject the null?

## Part C — Applied scenarios

1. A dataset has a long right tail and a few very high values. Which center and spread statistics would you report, and why?
2. A product team tests a new checkout flow. Define a primary metric, guardrail metrics, randomization unit, and a basic analysis plan.
3. You compare before/after measurements for the same users. Which test design may fit, and what assumption should you check?
4. You compare average order values from two independent groups with different variances. Which test is a reasonable starting point?
5. A result is significant with a huge sample but the lift is only 0.05%. What would you evaluate before recommending launch?
6. An experiment has 50/50 planned allocation but observed assignment is 70/30. What do you investigate before reading the outcome?
7. A model has high accuracy on a 99%-negative dataset. Why may accuracy be misleading, and what metrics could help?
8. A feature correlates strongly with an outcome. List checks you would perform before claiming it is predictive or causal.

## Part D — Python coding exercises

Use Python 3. NumPy/SciPy/pandas may be used where appropriate; explain what each library call computes.

1. Write a function that returns mean and median without using NumPy.
2. Calculate sample variance and sample standard deviation using the \(n-1\) denominator.
3. Simulate 10,000 coin-toss experiments with 3 tosses and estimate the probability of exactly 2 heads.
4. Use SciPy to perform a Welch two-sample t-test on two independent numeric samples. State the null hypothesis.
5. Compute Pearson and Spearman correlation for two numeric arrays and explain why their values may differ.
6. Given a list of binary outcomes (1 = conversion, 0 = no conversion), calculate the observed conversion rate.
7. Implement a bootstrap confidence interval for the sample mean using resampling with replacement.
8. Create a function that calculates precision, recall, and F1 from true labels and predicted labels for a binary classifier, without using scikit-learn.

## Part E — Mock interview prompts

- Explain p-values to a product manager in under one minute.
- Design an A/B test for a recommendation feature, including metrics, randomization, duration, and risks.
- A/B results are inconclusive. What would you do next?
- How would you detect whether an experiment's randomization worked as intended?
- Describe one situation where the mean is misleading.
- Explain the difference between predictive modeling and causal inference.
