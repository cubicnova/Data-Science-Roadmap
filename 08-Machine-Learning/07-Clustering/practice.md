# Practice — Clustering

Try the tasks before opening `solution.md`. Use synthetic or approved data. Do not interpret cluster IDs as ordinal rankings.

## Beginner: fundamentals

1. Define clustering and distinguish it from classification.
2. Give three useful applications of clustering.
3. Explain why feature scaling matters for Euclidean-distance clustering.
4. Define a centroid and a medoid.
5. Describe K-Means assignment and update steps.
6. What is inertia? Why does lower inertia alone not identify the best K?
7. Explain the elbow method and one limitation.
8. Define the silhouette coefficient and its range.
9. What does a negative silhouette value suggest?
10. Explain the meaning of DBSCAN noise label `-1`.
11. Compare K-Means, agglomerative clustering, and DBSCAN.
12. Why should arbitrary numeric IDs not be used as customer features?

## Intermediate: implementation

13. Generate 2D blobs with `make_blobs` and visualize them.
14. Fit K-Means with K=3 and show centroids.
15. Compare K-Means K=2 through K=8 using inertia.
16. Calculate silhouette scores for K=2 through K=8.
17. Create an elbow plot and explain your selected candidate K.
18. Change K-Means `random_state` and discuss initialization stability.
19. Standardize two features with different units and explain the effect.
20. Fit `AgglomerativeClustering(n_clusters=3)`.
21. Compare Ward and complete linkage.
22. Fit DBSCAN on `make_moons` and plot its labels.
23. Change `eps` and `min_samples` and report noise counts.
24. Count clusters excluding DBSCAN noise.
25. Compute Davies–Bouldin and Calinski–Harabasz scores for K-Means.
26. Explain why silhouette can be misleading for curved clusters.
27. Profile each cluster with group means in original units.
28. Build a preprocessing `Pipeline` with imputation, scaling, and K-Means.
29. Explain why `DBSCAN` has no standard `predict` method for unseen points.
30. Describe how to evaluate clustering without ground-truth labels.

## Advanced: interpretation and design

31. Discuss how outliers affect K-Means centroids.
32. Explain why high-dimensional distances can be difficult to interpret.
33. Compare K-Means and DBSCAN on two-moons data.
34. Discuss how to choose a distance metric for mixed data types.
35. Explain cluster stability under repeated random samples.
36. Identify potential privacy or fairness risks in customer segmentation.
37. Design a monitoring plan for changing segment sizes.
38. Explain why a good silhouette score may still produce useless segments.

## Mini-project: customer segmentation

39. Create a synthetic customer dataset with annual spending, visits, and average order value.
    - Inspect distributions, missing values, and extreme observations.
    - Select relevant features and explain why.
    - Impute and scale numeric variables.
    - Compare K-Means for K=2 through K=7.
    - Fit an agglomerative model and DBSCAN for comparison.
    - Report inertia (where applicable), silhouette, and noise proportion.
    - Profile each group using original units.
    - Propose one potential business action per meaningful group.
    - Explain three limitations and a monitoring plan.

40. Write a one-page segmentation report covering purpose, data, preprocessing, algorithm choices, diagnostics, segment profiles, risks, and next steps.

## Rubric (100 points)

| Area | Points |
|---|---:|
| Problem framing and feature choice | 15 |
| Data inspection and preparation | 15 |
| K-Means and hyperparameter exploration | 20 |
| Hierarchical and density-based comparison | 15 |
| Validation and stability | 15 |
| Segment profiling and communication | 15 |
| Reproducibility | 5 |

## Reflection

Which clusters would be actionable? What changes if features are not scaled? Could noise represent valuable customers? How will you tell whether clusters remain useful next quarter?
