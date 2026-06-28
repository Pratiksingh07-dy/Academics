# 🧩 Clustering Algorithms (K-Means & Hierarchical)

> **Clustering** is an unsupervised learning technique that groups similar data points together into clusters, without using any predefined labels.

---

## 🎯 Why Do We Need Clustering?

🔴 Most real-world data doesn't come with labels telling us what group it belongs to

🔴 We often want to discover hidden structure or natural groupings in data

🔴 Different clustering algorithms work better for different shapes/sizes of data

### Example

```text
Task                                    | Clustering Use Case
-------------------------------------------------------------
Grouping customers by spending habits      | Customer Segmentation
Grouping news articles by topic              | Document Clustering
Grouping genes with similar expression         | Bioinformatics
Detecting unusual network traffic                | Anomaly Detection
```

---

# 🧠 The Clustering Roadmap

```text
Clustering Algorithms
 ↓
 ├── K-Means Clustering        → Partition-based, requires choosing K
 └── Hierarchical Clustering      → Tree-based, builds a dendrogram
```

---

# 1️⃣ K-Means Clustering

### Definition

> K-Means is a **partition-based** clustering algorithm that divides data into K clusters, where each data point belongs to the cluster with the nearest mean (centroid).

### How K-Means Works — Step by Step

```text
Step 1: Choose the number of clusters, K
Step 2: Randomly initialize K centroids (cluster centers)
Step 3: Assign each data point to its nearest centroid
Step 4: Recalculate each centroid as the mean of all points assigned to it
Step 5: Repeat Steps 3-4 until centroids no longer move significantly (convergence)
```

### Visual Idea

```text
Iteration 1:                Iteration 2:               Final:
  ●     ○                     ●●    ○○                   ●●●   ○○○
    ✕(centroid)         →        ✕            →             ✕
  ●     ○                     ●●    ○○                   ●●●   ○○○
       (centroids move closer to actual cluster centers each iteration)
```

### Example

```text
Customer Data: Annual Income, Spending Score

K=3 Clusters formed:
Cluster 1: High Income, High Spending  → "Premium Customers"
Cluster 2: Low Income, Low Spending     → "Budget Customers"
Cluster 3: High Income, Low Spending      → "Cautious Spenders"
```

### Choosing K — The Elbow Method

```text
Plot: Within-Cluster Sum of Squares (WCSS) vs Number of Clusters (K)

WCSS
  │╲
  │ ╲
  │  ╲___
  │      ╲____           ← "Elbow" point — best K
  │           ╲__________
  └──────────────────────── K
    1   2   3   4   5   6
```

> The "elbow" — where the rate of decrease sharply changes — indicates the optimal K.

### Interview Shortcut

> **K-Means = pick K centroids, assign points, recompute centroids, repeat until stable.**

---

# 2️⃣ Advantages & Limitations of K-Means

| Advantages | Limitations |
| -------------- | ---------------- |
| Simple and fast to compute | Must specify K in advance |
| Scales well to large datasets | Sensitive to initial centroid placement |
| Works well with spherical, evenly-sized clusters | Struggles with non-spherical or unevenly sized clusters |
| Easy to interpret results | Sensitive to outliers (they pull centroids off-center) |

### Interview Shortcut

> **K-Means works great on round, evenly-sized clusters — struggles with irregular shapes and outliers.**

---

# 3️⃣ Hierarchical Clustering

### Definition

> Hierarchical Clustering builds a **tree-like structure of nested clusters** (called a dendrogram) by either progressively merging smaller clusters into larger ones, or splitting a large cluster into smaller ones.

### Two Approaches

**🔹 Agglomerative (Bottom-Up)** — Most Common
```text
Step 1: Start with each data point as its own individual cluster
Step 2: Merge the two closest clusters into one
Step 3: Repeat until only one cluster remains (or desired number reached)
```

**🔹 Divisive (Top-Down)**
```text
Step 1: Start with all data points in a single cluster
Step 2: Split the cluster into two based on dissimilarity
Step 3: Repeat recursively until each point is its own cluster (or desired number reached)
```

### The Dendrogram

> A tree diagram that shows the order and distance at which clusters were merged (or split), allowing you to choose the number of clusters by "cutting" the tree at a certain height.

```text
Distance
   │
   │        ┌──────┴──────┐
   │     ┌──┴──┐       ┌──┴──┐
   │   ┌─┴─┐  ┌┴┐     ┌┴┐  ┌─┴─┐
   │   A   B  C D     E F  G   H
   └─────────────────────────────── Data Points

Cutting the tree at a certain height
determines how many final clusters you get.
```

### Linkage Methods (How Distance Between Clusters Is Measured)

```text
Single Linkage    → Distance between the closest pair of points in two clusters
Complete Linkage    → Distance between the farthest pair of points in two clusters
Average Linkage       → Average distance between all pairs of points in two clusters
Ward's Method            → Minimizes the increase in total within-cluster variance
```

### Interview Shortcut

> **Hierarchical Clustering = builds a dendrogram by merging (or splitting) clusters step by step. No need to predefine K upfront.**

---

# 4️⃣ Advantages & Limitations of Hierarchical Clustering

| Advantages | Limitations |
| -------------- | ---------------- |
| No need to predefine the number of clusters | Computationally expensive for large datasets |
| Produces an interpretable dendrogram | Once a merge/split is made, it cannot be undone |
| Can capture nested cluster structures | Sensitive to noise and outliers |

### Interview Shortcut

> **Hierarchical Clustering gives flexibility (no fixed K) but doesn't scale well to huge datasets.**

---

# ⚖️ K-Means vs Hierarchical Clustering

| Feature | K-Means | Hierarchical Clustering |
| -------- | --------- | --------------------------- |
| Need to specify K upfront? | Yes | No (decide later by cutting dendrogram) |
| Scalability | Good for large datasets | Poor for large datasets |
| Output | Flat set of clusters | Nested tree (dendrogram) |
| Cluster Shape Handled | Spherical, evenly-sized | Can capture more complex/nested structures |
| Reversibility | Can reassign points each iteration | Merges/splits are permanent (greedy) |

---

# 📌 Quick Revision

| Concept | Core Idea |
| --------- | ----------- |
| K-Means | Partition data into K clusters using centroids |
| Elbow Method | Technique to choose the optimal K |
| Agglomerative Clustering | Bottom-up — merge closest clusters step by step |
| Divisive Clustering | Top-down — split large cluster into smaller ones |
| Dendrogram | Tree diagram showing merge/split order and distances |
| Linkage Methods | Single, Complete, Average, Ward's — define inter-cluster distance |

---

# 🎤 Viva Questions

### What is Clustering and why is it used?

> Clustering is an unsupervised learning technique that groups similar data points together without predefined labels, used to discover hidden structure or natural groupings in data.

### How does K-Means Clustering work?

> It selects K initial centroids, assigns each data point to its nearest centroid, then recalculates centroids as the mean of assigned points, repeating this process until centroids stabilize.

### How do you choose the optimal value of K in K-Means?

> Using the Elbow Method — plotting the Within-Cluster Sum of Squares (WCSS) against different values of K and selecting the K at the "elbow point" where the rate of decrease sharply changes.

### What is a major limitation of K-Means Clustering?

> It requires specifying the number of clusters (K) in advance, and it struggles with non-spherical or unevenly sized clusters, as well as being sensitive to outliers and initial centroid placement.

### What is Hierarchical Clustering?

> A clustering technique that builds a tree-like structure of nested clusters (a dendrogram) by progressively merging (agglomerative) or splitting (divisive) clusters.

### What is the difference between Agglomerative and Divisive Hierarchical Clustering?

> Agglomerative is a bottom-up approach that starts with individual points and merges the closest clusters together. Divisive is a top-down approach that starts with one large cluster and recursively splits it into smaller ones.

### What is a Dendrogram?

> A tree diagram that visually represents the order and distance at which clusters were merged or split, allowing the number of final clusters to be chosen by cutting the tree at a specific height.

### Name the different linkage methods used in Hierarchical Clustering.

> Single Linkage, Complete Linkage, Average Linkage, and Ward's Method — each defining the distance between clusters differently.

### What is a key advantage of Hierarchical Clustering over K-Means?

> Hierarchical Clustering doesn't require specifying the number of clusters in advance — you can decide later by choosing where to cut the dendrogram.

### Why is K-Means generally preferred for very large datasets compared to Hierarchical Clustering?

> Because K-Means is computationally more efficient and scales better with large datasets, while Hierarchical Clustering becomes computationally expensive as dataset size grows.

---

## 🏆 One-Line Summary

```text
K-Means              → Partition data into K clusters using centroids

Elbow Method            → Technique to find optimal K

Agglomerative Clustering   → Bottom-up, merge closest clusters

Divisive Clustering           → Top-down, split large cluster

Dendrogram                       → Tree diagram of merge/split history
```

---

## References

1. Tom M. Mitchell — *Machine Learning*, McGraw-Hill
2. Aurélien Géron — *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*, 2nd Edition, O'Reilly

---

<div align="center">

### ⭐ Star this repository if it helped you learn Machine Learning!

</div>
