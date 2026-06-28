# 📍 K-Nearest Neighbors (KNN)

> **K-Nearest Neighbors** is a simple, intuitive supervised learning algorithm that classifies (or predicts) a new data point based on the majority class (or average value) of its **K closest neighbors** in the training data.

---

## 🎯 Why Is KNN Useful?

🔴 Some patterns don't have a clean mathematical formula — they're just "similar things behave similarly"

🔴 Many real-world classification problems are based on proximity/similarity

🔴 Sometimes you want a simple, interpretable model with no complex training phase

### Example

```text
Real-life analogy:
"If most of my 5 closest neighbors vote for Candidate A,
I'll probably vote for Candidate A too."

This is exactly how KNN classifies a new data point —
by looking at its closest "neighbors" in the data.
```

---

# 🧠 The KNN Roadmap

```text
K-Nearest Neighbors
 ↓
 ├── Choose K                  → Number of neighbors to consider
 ├── Calculate Distance           → Measure similarity to all training points
 ├── Find K Nearest Points          → Identify the closest K data points
 └── Vote / Average                    → Classification (majority vote) or Regression (average)
```

---

# 1️⃣ How KNN Works

### Definition

> KNN is a **lazy learning** algorithm — it doesn't build a model during training. Instead, it stores the entire training dataset and makes predictions at the time of query by comparing the new point to all stored points.

### Step-by-Step Process

```text
Step 1: Choose a value for K (e.g., K = 3)
Step 2: Calculate the distance from the new point to every training point
Step 3: Select the K training points with the smallest distance
Step 4: For Classification → Take a majority vote among the K neighbors
        For Regression     → Take the average value of the K neighbors
Step 5: Assign that result to the new data point
```

### Example — Classification

```text
New Point: ?

Nearest 5 Neighbors (K=5):
  Cat, Cat, Dog, Cat, Dog

Majority Vote: Cat (3 votes) vs Dog (2 votes)
→ Prediction: Cat
```

### Interview Shortcut

> **KNN = "Lazy Learner." No training phase — just stores data and compares at prediction time.**

---

# 2️⃣ Distance Metrics

### Definition

> KNN relies on a distance metric to determine how "close" two data points are. The choice of metric can significantly affect results.

### Euclidean Distance (Most Common)

```text
d(x, y) = √[ (x₁-y₁)² + (x₂-y₂)² + ... + (xₙ-yₙ)² ]
```

### Manhattan Distance

```text
d(x, y) = |x₁-y₁| + |x₂-y₂| + ... + |xₙ-yₙ|

(Sum of absolute differences — think "city block" distance)
```

### Minkowski Distance

```text
A generalized formula that becomes Euclidean (p=2)
or Manhattan (p=1) depending on the chosen value of p.
```

### Interview Shortcut

> **Euclidean = straight-line distance. Manhattan = grid/city-block distance. Both measure "closeness" differently.**

---

# 3️⃣ Choosing the Right Value of K

### Definition

> The value of K (number of neighbors considered) is a critical hyperparameter that directly affects how the model behaves — too small or too large can both cause problems.

### Effect of K

```text
Small K (e.g., K=1)
✔ Captures fine detail in the data
✘ Very sensitive to noise and outliers
✘ Leads to overfitting (high variance)

Large K (e.g., K=50)
✔ Smoother decision boundary, less sensitive to noise
✘ May ignore important local patterns
✘ Leads to underfitting (high bias)
```

### Visual Idea

```text
K=1: Decision boundary is jagged, follows every data point closely
     ╱╲╱╲╱╲╱╲

K=15: Decision boundary is smooth, generalizes better
     ‾‾‾‾‾‾‾‾
```

### How to Choose K

```text
✔ Use Cross-Validation to test multiple K values
✔ Common practice: try odd values (avoids ties in binary classification)
✔ Plot Error Rate vs K and pick the "elbow point"
```

### Interview Shortcut

> **Small K = overfitting risk. Large K = underfitting risk. Use cross-validation to find the sweet spot.**

---

# 4️⃣ Why Feature Scaling Matters for KNN

### The Problem

```text
Feature A: Age (range: 18-80)
Feature B: Income (range: 10,000-1,000,000)

Without scaling, Income differences will completely dominate
the distance calculation, making Age almost irrelevant —
even if Age is actually more predictive.
```

### The Solution

```text
✔ Normalize or Standardize all features before applying KNN
✔ Ensures every feature contributes fairly to the distance calculation
```

### Interview Shortcut

> **KNN is distance-based, so feature scaling is mandatory — unscaled features distort the results.**

---

# 5️⃣ KNN for Regression

### Definition

> KNN can also be used for regression tasks — instead of taking a majority vote, it takes the **average** (or weighted average) of the K nearest neighbors' target values.

### Example

```text
New Point: House with certain features

5 Nearest Neighbors' Prices: $200K, $210K, $195K, $205K, $220K

Predicted Price = Average = $206K
```

### Interview Shortcut

> **KNN Classification = majority vote. KNN Regression = average of neighbor values.**

---

# ⚖️ Advantages vs Disadvantages

| Advantages | Disadvantages |
| -------------- | ------------------ |
| Simple to understand and implement | Slow at prediction time (must compare to all points) |
| No training phase required | Sensitive to irrelevant/unscaled features |
| Naturally handles multi-class problems | Requires choosing the right K |
| Works well with small, clean datasets | Struggles with high-dimensional data (curse of dimensionality) |

---

# 📌 Quick Revision

| Concept | Core Idea |
| --------- | ----------- |
| KNN | Classifies based on majority vote of K closest neighbors |
| Lazy Learning | No training phase — comparisons happen at prediction time |
| Euclidean Distance | Straight-line distance between points |
| Manhattan Distance | Grid-based, sum of absolute differences |
| Small K | Overfitting risk (sensitive to noise) |
| Large K | Underfitting risk (oversimplified boundary) |
| Feature Scaling | Mandatory — KNN is purely distance-based |

---

# 🎤 Viva Questions

### What is K-Nearest Neighbors (KNN)?

> A supervised learning algorithm that classifies or predicts a new data point based on the majority class or average value of its K closest neighbors in the training data.

### Why is KNN called a "Lazy Learner"?

> Because it doesn't build a model during a training phase — it simply stores the training data and performs all the computation (distance calculation) at the time of prediction.

### What distance metrics are commonly used in KNN?

> Euclidean Distance (straight-line distance) and Manhattan Distance (sum of absolute differences, like city-block distance) are the most common, with Minkowski Distance as a generalized form of both.

### What happens if K is too small?

> The model becomes overly sensitive to noise and outliers in the data, leading to overfitting and a jagged, overly specific decision boundary.

### What happens if K is too large?

> The model's decision boundary becomes overly smooth, potentially ignoring important local patterns, leading to underfitting.

### Why is feature scaling important for KNN?

> Because KNN relies entirely on distance calculations, and features with larger numeric ranges would dominate the distance metric, distorting results unless all features are scaled to a comparable range.

### How does KNN work for regression problems?

> Instead of taking a majority vote among the K nearest neighbors (as in classification), KNN regression takes the average (or weighted average) of the target values of the K nearest neighbors.

### What is a major disadvantage of KNN with large datasets?

> KNN can be slow at prediction time because it must calculate the distance from the new point to every single point in the training dataset.

### How do you choose the optimal value of K?

> By using cross-validation to test multiple values of K, plotting the error rate against K, and selecting the value at the "elbow point" where error is minimized without overfitting.

### Why might odd values of K be preferred in binary classification?

> To avoid ties in the majority vote — with an even K, there's a chance of an equal split between the two classes, making the prediction ambiguous.

---

## 🏆 One-Line Summary

```text
KNN              → Classifies based on majority vote of K nearest neighbors

Lazy Learning      → No training phase, all computation at prediction time

Distance Metrics     → Euclidean (straight-line) or Manhattan (grid-based)

Small K                → Overfitting risk

Large K                   → Underfitting risk

Feature Scaling              → Mandatory for fair distance calculation
```

---

## References

1. Tom M. Mitchell — *Machine Learning*, McGraw-Hill
2. Aurélien Géron — *Hands-On Machine Learning with Scikit-Learn, Keras, and TensorFlow*, 2nd Edition, O'Reilly

---

<div align="center">

### ⭐ Star this repository if it helped you learn Machine Learning!

</div>
