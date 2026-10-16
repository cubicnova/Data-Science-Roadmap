# Reference Solutions — Clustering

These are illustrative answers; the best approach depends on geometry, business context, and feature quality.

## Beginner answers (1–12)

1. Clustering groups similar unlabeled observations; classification learns to predict known classes.
2. Examples: customer segmentation, image grouping, exploratory anomaly detection.
3. A high-magnitude column can dominate distance unless appropriately scaled.
4. A centroid is a coordinate-wise mean; a medoid is an observed representative point minimizing dissimilarity.
5. Assign each observation to its nearest centroid, then recompute centroids; repeat until stopping criteria.
6. Inertia is the sum of squared within-cluster distances. It generally decreases as K grows, including for over-fragmented groups.
7. The elbow is a bend in inertia-vs-K; many datasets have no clear bend.
8. Silhouette compares average distance within the assigned cluster to the nearest alternative cluster; it ranges from -1 to 1.
9. A negative value suggests an observation may be closer to another cluster than its assigned one.
10. `-1` marks a point as noise in DBSCAN, not a normal cluster.
11. K-Means finds centroid-based compact groups; agglomerative merges groups hierarchically; DBSCAN discovers density-connected groups and noise.
12. IDs are arbitrary identifiers, not numerical rankings; their values can change between fits.

## Intermediate answers (13–30)

### 13–17. K-Means, elbow, and silhouette

```python
import numpy as np
import pandas as pd
import matplotlib.pyplot as plt
from sklearn.datasets import make_blobs
from sklearn.cluster import KMeans
from sklearn.metrics import silhouette_score

X, _ = make_blobs(n_samples=450, centers=3, cluster_std=0.8, random_state=42)
plt.scatter(X[:, 0], X[:, 1], s=12)
plt.show()

model = KMeans(n_clusters=3, n_init=10, random_state=42).fit(X)
plt.scatter(X[:, 0], X[:, 1], c=model.labels_, s=12)
plt.scatter(model.cluster_centers_[:, 0], model.cluster_centers_[:, 1],
            marker="X", s=180, c="black")
plt.show()

rows = []
for k in range(2, 9):
    m = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X)
    rows.append({"k": k, "inertia": m.inertia_,
                 "silhouette": silhouette_score(X, m.labels_)})
scores = pd.DataFrame(rows)
print(scores)
scores.plot(x="k", y=["inertia", "silhouette"], subplots=True)
plt.show()
```

Use the elbow and silhouette together with stability and interpretability; neither proves a unique true K.

### 18. Initialization
Fit K-Means using different seeds and compare inertia, silhouette, and co-membership. Label integers can permute even when partitions are equivalent.

### 19. Scaling

```python
from sklearn.preprocessing import StandardScaler
X_scaled = StandardScaler().fit_transform(X)
```

For real pipelines, fit preprocessing on the fitting sample and apply the same fitted transformation to future samples.

### 20–21. Hierarchical clustering

```python
from sklearn.cluster import AgglomerativeClustering
ward = AgglomerativeClustering(n_clusters=3, linkage="ward").fit_predict(X)
complete = AgglomerativeClustering(n_clusters=3, linkage="complete").fit_predict(X)
```

Ward minimizes increases in within-cluster variance and uses Euclidean distance. Complete linkage uses the farthest pairwise distance when merging groups.

### 22–24. DBSCAN and noise

```python
from sklearn.datasets import make_moons
from sklearn.cluster import DBSCAN

X_moons, _ = make_moons(n_samples=450, noise=0.06, random_state=42)
labels = DBSCAN(eps=0.20, min_samples=5).fit_predict(X_moons)
n_clusters = len(set(labels) - {-1})
n_noise = int(np.sum(labels == -1))
print(n_clusters, n_noise)
plt.scatter(X_moons[:, 0], X_moons[:, 1], c=labels, s=12)
plt.show()
```

Try `eps` values such as 0.10, 0.20, and 0.35; changes in cluster/noise counts depend on the dataset scale.

### 25. Additional internal metrics

```python
from sklearn.metrics import davies_bouldin_score, calinski_harabasz_score
print(davies_bouldin_score(X, model.labels_))
print(calinski_harabasz_score(X, model.labels_))
```

Lower Davies–Bouldin and higher Calinski–Harabasz are generally preferred, but compare with domain usefulness.

### 26. Curved clusters
Silhouette based on Euclidean separation can prefer compact convex groups even when density-connected curved groups are meaningful.

### 27. Profile original-unit features

```python
profile = pd.DataFrame(X, columns=["feature_a", "feature_b"])
profile["cluster"] = model.labels_
print(profile.groupby("cluster").agg(["mean", "median", "count"]))
```

Avoid interpreting cluster numbers as ordered rankings.

### 28. Pipeline

```python
from sklearn.pipeline import Pipeline
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.cluster import KMeans

pipeline = Pipeline([
    ("imputer", SimpleImputer(strategy="median")),
    ("scaler", StandardScaler()),
    ("cluster", KMeans(n_clusters=3, n_init=10, random_state=42)),
])
pipeline.fit(X)
```

### 29. DBSCAN inference
DBSCAN labels the fitted samples based on their density neighborhoods; scikit-learn's DBSCAN estimator does not expose a general `predict` method for new observations. A separate assignment policy would require validation.

### 30. Unlabeled evaluation
Use internal metrics, visualization, domain interpretation, robustness across samples/settings, and usefulness in downstream decisions. Do not claim a ground-truth accuracy score when labels do not exist.

## Advanced answers (31–38)

31. Outliers can pull K-Means centroids away from dense regions; investigate data quality and consider robust alternatives or density methods.
32. In high dimensions, distances can become less discriminative and irrelevant variables may dominate. Feature selection, suitable representations, and distance choice matter.
33. DBSCAN can recover curved density-connected moons; K-Means often partitions them into convex Voronoi regions.
34. Avoid arbitrary integer codes for nominal categories. Consider one-hot representations with justified weighting, Gower-style distances, or mixed-type methods, and validate the resulting similarity.
35. Resample data and refit; compare segment membership, centroid profiles, and stable business interpretations. Cluster labels may permute.
36. Sensitive attributes or proxies can yield discriminatory targeting; evaluate fairness, necessity, consent, privacy, and data minimization.
37. Track cluster sizes, distances to centroids, outlier/noise rates, feature distributions, and changes in business outcomes; set review triggers.
38. Geometrically compact groups may not correspond to meaningful or actionable segments. Validate with stakeholder decisions and observed outcomes.

## Mini-project (39) — reference workflow

```python
import numpy as np
import pandas as pd
from sklearn.cluster import KMeans, AgglomerativeClustering, DBSCAN
from sklearn.impute import SimpleImputer
from sklearn.preprocessing import StandardScaler
from sklearn.metrics import silhouette_score

rng = np.random.default_rng(42)
n = 600
segment = rng.choice(3, size=n)
df = pd.DataFrame({
    "annual_spend": rng.normal(np.array([400, 1400, 3200])[segment], 180).clip(0),
    "visits": rng.poisson(np.array([3, 9, 18])[segment]),
    "avg_order_value": rng.normal(np.array([50, 110, 170])[segment], 15).clip(0),
})
cols = ["annual_spend", "visits", "avg_order_value"]
X = StandardScaler().fit_transform(SimpleImputer(strategy="median").fit_transform(df[cols]))

scores = []
for k in range(2, 8):
    m = KMeans(n_clusters=k, n_init=10, random_state=42).fit(X)
    scores.append((k, m.inertia_, silhouette_score(X, m.labels_)))
print(pd.DataFrame(scores, columns=["k", "inertia", "silhouette"]))

km = KMeans(n_clusters=3, n_init=10, random_state=42).fit(X)
hier = AgglomerativeClustering(n_clusters=3).fit_predict(X)
db = DBSCAN(eps=0.45, min_samples=6).fit_predict(X)

df["cluster"] = km.labels_
print(df.groupby("cluster")[cols].mean())
print("DBSCAN noise share:", np.mean(db == -1))
```

The hidden synthetic `segment` variable is used only to generate data; it is **not** used to fit clustering. Interpret cluster profiles, not their arbitrary numeric IDs. If DBSCAN returns fewer than two non-noise clusters, do not force a silhouette score.

## Mini-project report (40) — suggested structure

- **Purpose:** Which business decision could benefit from grouping?
- **Data:** Source, time window, sample, permissions, limitations.
- **Features:** Definitions, units, exclusions, and missingness.
- **Preprocessing:** Imputation, scaling, outlier handling.
- **Models:** K-Means candidate K values, agglomerative settings, DBSCAN density parameters.
- **Evaluation:** Inertia where relevant, silhouette where defined, noise share, stability, and interpretability.
- **Profiles:** Cluster sizes and median/mean feature values in original units.
- **Risks:** Instability, proxies for sensitive characteristics, drift, and over-interpretation.
- **Next steps:** Validate potential actions and establish monitoring.
