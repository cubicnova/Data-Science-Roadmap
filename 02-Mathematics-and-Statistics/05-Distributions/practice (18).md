# Practice Questions: Distributions

Work through the questions before consulting `solutions.md`. For coding questions, use Python, NumPy, and Matplotlib where appropriate.

1. Define a probability distribution and a random variable.
2. Explain the difference between a discrete and a continuous random variable, with one example of each.
3. What is a PMF? State two properties of a valid PMF.
4. What is a PDF, and why is a PDF value not usually the probability of an exact value?
5. Define the cumulative distribution function (CDF).
6. Explain expected value in plain language.
7. How are variance and standard deviation related?
8. A fair coin is tossed once. Define a Bernoulli random variable and give its success probability.
9. State the assumptions of a binomial distribution.
10. A fair coin is tossed 8 times. What are n and p for the number of heads?
11. For X ~ Binomial(n=10, p=0.3), calculate its mean and variance.
12. What does a geometric distribution model? State the convention used for its support in this module.
13. If a success probability is 0.2, what is the expected number of trials until the first success under the geometric model?
14. Describe a practical situation that might be modeled with a Poisson distribution.
15. If the average rate is 4 events per hour, what are the mean and variance of the corresponding Poisson count?
16. Give the PDF, mean, and variance of a uniform distribution on [2, 8].
17. What do the parameters μ and σ represent in a normal distribution?
18. State the 68–95–99.7 rule for a normal distribution.
19. What kind of quantity can an exponential distribution model?
20. If an exponential distribution has rate λ=0.5 per minute, what is its mean waiting time?
21. Explain the difference between rate and scale in NumPy's exponential sampler.
22. Classify these variables as discrete or continuous: (a) number of login failures, (b) request latency in milliseconds, (c) number of defects, (d) temperature.
23. Which distribution could model the number of successes in 20 independent trials with constant success probability? Explain.
24. Which distribution could model the number of arrivals in a fixed time interval under a constant independent rate? Explain.
25. Which distribution is a reasonable starting model for symmetric measurements clustered around a mean? What should you check?
26. Use NumPy to generate 500 Bernoulli trials with p=0.25 and report the observed success rate.
27. Use NumPy to simulate 1,000 binomial outcomes with n=12 and p=0.4. Compute the sample mean.
28. Simulate 1,000 Poisson counts with λ=3 and compare sample mean and variance with the theoretical value.
29. Generate 1,000 uniform samples on [0, 1] and check whether their sample mean is close to 0.5.
30. Generate 1,000 normal samples with mean 10 and standard deviation 2. Plot a histogram.
31. Generate 1,000 exponential samples with scale 2 and compare the sample mean with the expected mean.
32. Write a function that accepts a sample and returns its mean and variance.
33. Explain why a histogram alone cannot prove that data follow a particular distribution.
34. Describe two assumptions you would check before using a Poisson model for hourly support tickets.
35. Choose one real-world variable and propose a distribution for it. Explain the variable, parameters, assumptions, and one limitation.
