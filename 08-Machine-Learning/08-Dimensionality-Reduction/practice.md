# Practice — Dimensionality Reduction

Try each task before consulting `solution.md`. Use synthetic or approved data. Treat dimensionality reduction as a modeling choice that must be evaluated, not an automatic improvement.

## Beginner: fundamentals
1. Define dimensionality reduction and list three reasons to use it.
2. Explain the difference between feature selection and feature extraction.
3. What is the curse of dimensionality?
4. Describe PCA in plain language.
5. What is a principal component?
6. Explain explained variance ratio and cumulative explained variance.
7. Why might scaling be important before PCA?
8. Why is scaling not always automatically appropriate?
9. Compare PCA and t-SNE.
10. Why should t-SNE plots not be interpreted as faithful global maps?
11. What is UMAP commonly used for?
12. Why is TruncatedSVD often useful for sparse text matrices?

## Intermediate: implementation
13. Generate a dataset with correlated features and inspect its correlation matrix.
14. Standardize numeric data and fit PCA with two components.
15. Plot the first two principal components.
16. Plot individual and cumulative explained variance.
17. Find the minimum number of PCA components needed to exceed 90% explained variance.
18. Compare PCA with and without standardization on features with different units.
19. Fit t-SNE to a small synthetic dataset and visualize the embedding.
20. Change t-SNE perplexity and random seed; describe changes.
21. Explain why t-SNE perplexity must be less than the number of samples.
22. Generate sparse-like nonnegative data and apply `TruncatedSVD`.
23. Apply `GaussianRandomProjection` and compare input/output dimensions.
24. Build a pipeline that imputes, scales, applies PCA, and fits logistic regression.
25. Compare cross-validation performance of a classifier with and without PCA.
26. Explain why PCA must be fit inside each cross-validation fold.
27. Describe how you would reduce TF-IDF text features.
28. Explain why components may be difficult to interpret in business terms.

## Advanced: evaluation and trade-offs
29. Create a dataset where a low-variance feature strongly predicts the target. Explain why PCA might remove useful information.
30. Compare PCA and random projection on computation, interpretability, and distance preservation.
31. Explain how high dimensionality can affect distance-based clustering.
32. Describe a method to test whether an embedding is stable across random seeds.
33. Explain how to evaluate a dimensionality reducer for a supervised task without leaking test information.
34. Discuss privacy and fairness risks when using embeddings for customer or user data.
35. Explain why a visually separated t-SNE plot does not establish statistically meaningful clusters.
36. Propose a monitoring plan if an embedding is used in a deployed model.

## Mini-project: reduce dimensions and compare models
37. Use `make_classification` to generate a dataset with correlated/redundant features.
    - Inspect shape, feature scales, and correlations.
    - Create a train/test split.
    - Build a baseline classifier.
    - Compare a scaled logistic-regression pipeline with and without PCA.
    - Plot cumulative explained variance.
    - Try a few component counts using cross-validation on training data only.
    - Evaluate the selected model once on held-out test data.
    - Explain any performance change and limitations.
38. Use a dataset with at least 10 numeric features to create a 2D PCA visualization and a t-SNE visualization. Compare what each plot communicates and list at least three cautions.

## Rubric (100 points)
| Area | Points |
|---|---:|
| Problem framing and data inspection | 10 |
| Correct preprocessing and split strategy | 20 |
| PCA and explained variance | 20 |
| Nonlinear visualization comparison | 15 |
| Downstream model evaluation | 20 |
| Interpretation and limitations | 10 |
| Reproducibility | 5 |

## Reflection
Did dimensionality reduction improve the downstream task? What information might have been lost? Are your conclusions based on predictive validation or only on a visualization?
