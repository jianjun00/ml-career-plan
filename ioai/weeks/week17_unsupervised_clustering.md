# Week 17 — Unsupervised Learning & Dimensionality Reduction
## Phase 2: Deep Learning Core (Week 17 of 18)

---

## Theory (4h)

- **K-Means**: algorithm, k-selection (elbow method, silhouette), limitations
- **DBSCAN**: density-based clusters, eps/min_samples, handles noise
- **Hierarchical clustering**: dendrogram, linkage methods
- **PCA revisited**: explained variance ratio, choosing components
- **t-SNE**: perplexity, high-dimensional to 2D, what it preserves vs. distorts
- **UMAP**: faster than t-SNE, better global structure

---

## Tudor Musat's IOAI 2025 Gold Medal Approach ("Antique Painting")

This task had a hidden spiral cluster structure. The winning approach:

```python
# Step 1: PCA to 2D — visualize immediately
from sklearn.decomposition import PCA
from sklearn.manifold import TSNE
import umap

pca = PCA(n_components=2)
X_pca = pca.fit_transform(X)
plt.scatter(X_pca[:,0], X_pca[:,1], c=y, cmap='tab10')
# Look for structure: curves, spirals, clusters

# Step 2: t-SNE refinement (slower but better local structure)
tsne = TSNE(n_components=2, perplexity=30, random_state=42)
X_tsne = tsne.fit_transform(X)

# Step 3: UMAP (faster, better for large datasets)
reducer = umap.UMAP(n_components=2, random_state=42)
X_umap = reducer.fit_transform(X)

# Step 4: once structure visible, apply appropriate clustering
from sklearn.cluster import KMeans, DBSCAN
km = KMeans(n_clusters=k, random_state=42)
labels = km.fit_predict(X_pca)

# DBSCAN for non-spherical clusters (like spirals):
db = DBSCAN(eps=0.5, min_samples=5)
labels = db.fit_predict(X_pca)
```

## Silhouette score for k selection
```python
from sklearn.metrics import silhouette_score
scores = [silhouette_score(X, KMeans(n_clusters=k).fit_predict(X)) for k in range(2,11)]
best_k = range(2,11)[np.argmax(scores)]
```

---

## Practice (8h)

- Reproduce Tudor Musat's gold medal approach: PCA → visualize spiral cluster → manual class separation → t-SNE refinement
- Apply UMAP to a high-dimensional dataset; compare with PCA and t-SNE
- Practice IOAI 2025 "Antique Painting" task from [IOAI 2025 GitHub](https://github.com/IOAI-official/IOAI-2025)

---

## Deliverable

`week17_unsupervised_clustering.ipynb` — PCA/t-SNE/UMAP comparison + clustering on real dataset

---

*Part of the IOAI Gold Medal 52-Week Training Program — Phase 2: Deep Learning Core*
