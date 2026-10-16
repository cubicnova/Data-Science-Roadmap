# 07 — Clustering

Clustering is an **unsupervised learning** approach that groups observations using feature similarity without requiring target labels. It is useful for exploratory customer segmentation, pattern discovery, image grouping, and anomaly investigation.

## Learning objectives

- Distinguish clustering from classification and understand when clusters are meaningful.
- Prepare and scale numerical features before distance-based clustering.
- Train K-Means, agglomerative hierarchical clustering, and DBSCAN.
- Select candidate cluster counts with elbow plots and silhouette analysis.
- Interpret inertia, silhouette score, and Davies–Bouldin index.
- Read dendrogram concepts, linkage choices, density parameters, and noise labels.
- Profile segments using original feature units and evaluate business usefulness.
- Recognize limitations: arbitrary cluster IDs, unstable solutions, sensitive distance metrics, and spurious patterns.

## Algorithms

| Algorithm | Main idea | Key parameters | Important trade-off |
|---|---|---|---|
| K-Means | Minimize squared distances to cluster centroids | `n_clusters`, `n_init`, `random_state` | Prefers compact, roughly spherical groups; sensitive to scaling and outliers |
| Agglomerative | Merge closest groups using a linkage criterion | `n_clusters`, `linkage` | Hierarchy can be useful; memory/computation grow on large datasets |
| DBSCAN | Connect dense neighborhoods and mark sparse points as noise | `eps`, `min_samples` | Handles irregular shapes and noise; sensitive to scale and varying density |

## Core concepts

### Distances and scaling
Euclidean distance is sensitive to feature units. If one column is in rupees and another in counts, the larger numeric scale can dominate. Fit a `StandardScaler` or another justified transformation on the intended fitting data. For mixed numeric and categorical inputs, choose encodings and distances carefully; Euclidean distance on arbitrary category codes is misleading.

### K-Means
K-Means alternates between assigning points to centroids and updating those centroids. `inertia_` is the within-cluster sum of squared distances. Inertia always decreases or stays equal as K increases, so do not simply choose the smallest inertia.

### Elbow and silhouette
The elbow method searches for diminishing improvements in inertia, but an obvious elbow may not exist. The silhouette coefficient compares within-cluster cohesion to separation from neighboring clusters. It ranges from -1 to 1 and is defined only when there are at least two clusters and fewer clusters than samples. Higher scores are not proof of useful business segments.

### Hierarchical clustering
Agglomerative clustering produces nested merges. Single, complete, average, and Ward linkage define different merge criteria. Ward linkage requires Euclidean distance and tends to favor compact clusters.

### DBSCAN
`eps` is a neighborhood radius and `min_samples` is a density threshold. Points labeled `-1` are noise, not a regular cluster. DBSCAN does not require K beforehand and can find non-convex groups, but may struggle with varying density.

### Cluster evaluation
- **Silhouette score:** larger is generally better when defined; consider geometry and noise handling.
- **Davies–Bouldin index:** smaller is generally better; requires at least two clusters.
- **Calinski–Harabasz score:** larger generally indicates better separated/compact groups.
- **External metrics (ARI, NMI):** use only if independent reference labels exist, never to claim unsupervised discovery from training labels.
- **Stability and utility:** compare seeds, samples, and time periods; validate interpretability and operational value.

## End-to-end workflow

1. Define the decision and why grouping is useful.
2. Inspect missing values, duplicates, units, skew, outliers, and sampling bias.
3. Select relevant, available features; exclude identifiers and known outcomes.
4. Fit preprocessing and clustering without using target labels.
5. Compare K or density settings using diagnostics and visual inspection.
6. Profile groups in original units and check stability.
7. Explain limitations, privacy/fairness concerns, and actionability.
8. Monitor segment size and feature drift if used operationally.

## Setup

Python 3.10+ recommended:

```bash
python -m venv .venv
# Windows
.venv\Scripts\activate
# macOS/Linux
source .venv/bin/activate
python -m pip install numpy pandas matplotlib scikit-learn jupyter
jupyter notebook
```

Run `examples.ipynb` from top to bottom. All data are generated locally.

## Files

- `README.md`: theory and workflow
- `examples.ipynb`: runnable demonstrations
- `practice.md`: 40 progressive questions and a mini-project
- `solution.md`: reference answers and code

## Checklist

- [ ] Features and distance metric have a defensible meaning.
- [ ] Scaling and missing-value treatment are documented.
- [ ] K or density settings were compared, not assumed.
- [ ] Noise points are treated separately from clusters.
- [ ] Segment profiles are interpreted in original units.
- [ ] Stability, fairness, privacy, and business usefulness are considered.
- [ ] Cluster IDs are not treated as ordered or permanent labels.

## References

- [scikit-learn clustering user guide](https://scikit-learn.org/stable/modules/clustering.html)
- [KMeans API](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.KMeans.html)
- [DBSCAN API](https://scikit-learn.org/stable/modules/generated/sklearn.cluster.DBSCAN.html)
- [Silhouette score API](https://scikit-learn.org/stable/modules/generated/sklearn.metrics.silhouette_score.html)
