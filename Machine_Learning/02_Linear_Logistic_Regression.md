# 📈 Linear & Logistic Regression

> **Regression algorithms** are supervised learning techniques used to model the relationship between input features and an output — predicting either a continuous value (Linear) or a class probability (Logistic).

---

## 🎯 Why These Two First?

Both are the foundation of supervised learning:

🔴 **Linear Regression** — predicts a **number** (continuous output)

🔴 **Logistic Regression** — predicts a **category** (despite the name "regression")

### Example

```text
Task                              | Output Type        | Algorithm
---------------------------------------------------------------------
Predict house price                | Continuous value     | Linear Regression
Predict if email is spam           | Class (0/1)           | Logistic Regression
Predict temperature tomorrow       | Continuous value     | Linear Regression
Predict if a patient has disease   | Class (Yes/No)         | Logistic Regression
```

---

# 🧠 The Two Regression Pillars

```text
Regression
 ↓
 ├── Linear Regression    → Predicts continuous values
 └── Logistic Regression  → Predicts probabilities/classes
```

---

# 1️⃣ Linear Regression

### Definition

> Linear Regression models the relationship between a dependent variable (Y) and one or more independent variables (X) by fitting a **straight line** through the data.

### Rules

✔ Output is continuous (real-valued)

✔ Assumes a linear relationship between X and Y

✔ Minimizes the difference between predicted and actual values

✔ Sensitive to outliers

### The Equation

```text
Simple Linear Regression:
        Y = mX + c

Multiple Linear Regression:
        Y = b0 + b1X1 + b2X2 + ... + bnXn
```

| Term | Meaning |
| ----- | -------- |
| `Y` | Predicted output (dependent variable) |
| `X` | Input feature (independent variable) |
| `m` / `b1, b2...` | Slope / Coefficients (weight of each feature) |
| `c` / `b0` | Intercept (value of Y when X = 0) |

### How It Works

```text
Data Points → Fit Best Straight Line → Minimize Error → Predict New Y
```

### Visual Idea

```text
  Y
  │           ●
  │        ●
  │     ●  ___________ (best fit line)
  │  ●
  │●
  └──────────────────── X
```

> The line is positioned to minimize the total distance between itself and all data points.

### Cost Function — Mean Squared Error (MSE)

```text
MSE = (1/n) × Σ (Actual - Predicted)²
```

> The goal of training is to find `m` and `c` that **minimize MSE**, usually using **Gradient Descent**.

### Example

| Hours Studied (X) | Marks Scored (Y) |
| ------------------- | ------------------- |
| 1 | 35 |
| 3 | 55 |
| 5 | 75 |
| 7 | 95 |

> Model learns: `Y = 10X + 25` → Predicting for 4 hours: `Y = 65`

### Types of Linear Regression

**🔹 Simple Linear Regression** — One independent variable
```text
Salary = f(Years of Experience)
```

**🔹 Multiple Linear Regression** — Multiple independent variables
```text
House Price = f(Area, Bedrooms, Location)
```

### Assumptions of Linear Regression

```text
Linearity        → Relationship between X and Y is linear
Independence     → Observations are independent of each other
Homoscedasticity → Constant variance of errors
No Multicollinearity → Independent variables are not highly correlated
Normality        → Residuals (errors) are normally distributed
```

### Real-World Applications

```text
House Price Prediction
Sales Forecasting
Stock Price Trend Estimation
Salary Prediction based on Experience
```

### Interview Shortcut

> **Linear Regression = Fits a straight line to predict a continuous numeric value.**

---

# 2️⃣ Logistic Regression

### Definition

> Logistic Regression is a classification algorithm that predicts the **probability** of an outcome belonging to a particular class, using an **S-shaped (Sigmoid) curve** instead of a straight line.

### Rules

✔ Output is a probability between 0 and 1

✔ Used for classification, not regression of continuous values

✔ Uses the Sigmoid function to squash values into a (0,1) range

✔ A threshold (commonly 0.5) decides the final class

### The Sigmoid Function

```text
σ(z) = 1 / (1 + e^(-z))

where z = b0 + b1X1 + b2X2 + ... + bnXn
```

### Visual Idea

```text
  1 │            ________
    │          /
0.5 │ - - - - / - - - - -   ← decision threshold
    │       /
  0 │_____/
    └──────────────────── z
```

> Any output ≥ 0.5 → Class 1 (e.g., "Spam")
> Any output < 0.5 → Class 0 (e.g., "Not Spam")

### How It Works

```text
Input (X) → Linear Combination (z) → Sigmoid(z) → Probability → Apply Threshold → Class
```

### Example

| Hours Studied (X) | Pass (1) / Fail (0) |
| ------------------- | ---------------------- |
| 1 | 0 |
| 2 | 0 |
| 5 | 1 |
| 8 | 1 |

> Model predicts: P(Pass | Hours=6) = 0.81 → Classified as **Pass**

### Types of Logistic Regression

**🔹 Binary Logistic Regression** — Two classes
```text
Spam / Not Spam
Pass / Fail
```

**🔹 Multinomial Logistic Regression** — More than two unordered classes
```text
Fruit type: Apple / Banana / Mango
```

**🔹 Ordinal Logistic Regression** — More than two ordered classes
```text
Rating: Low / Medium / High
```

### Cost Function — Log Loss (Cross-Entropy)

```text
Cost = -(1/n) × Σ [ Y×log(Ŷ) + (1-Y)×log(1-Ŷ) ]
```

> MSE isn't used here because it creates a non-convex curve for classification — Log Loss ensures smooth optimization via Gradient Descent.

### Real-World Applications

```text
Spam Email Detection
Disease Diagnosis (Yes/No)
Customer Churn Prediction
Credit Card Fraud Detection
```

### Interview Shortcut

> **Logistic Regression = Uses Sigmoid function to predict probability, then classifies based on a threshold.**

---

# ⚖️ Linear vs Logistic Regression

| Feature | Linear Regression | Logistic Regression |
| -------- | -------------------- | ----------------------- |
| Output Type | Continuous value | Probability (0 to 1) → Class |
| Used For | Regression | Classification |
| Function Used | Straight line equation | Sigmoid function |
| Cost Function | Mean Squared Error (MSE) | Log Loss (Cross-Entropy) |
| Graph Shape | Straight line | S-shaped curve |
| Example | Predicting price | Predicting spam/not spam |

---

# 📌 Quick Revision

| Algorithm | Core Idea | Output |
| ----------- | ----------- | -------- |
| Linear Regression | Fit a straight line | Continuous number |
| Logistic Regression | Fit an S-curve (sigmoid) | Probability → Class |

---

# 🎤 Viva Questions

### What is Linear Regression?

> A supervised learning algorithm that models the relationship between independent and dependent variables using a straight line to predict continuous values.

### What is Logistic Regression?

> A classification algorithm that predicts the probability of a data point belonging to a class using the sigmoid function.

### Why is Logistic Regression called "regression" if it's used for classification?

> Because it uses a regression-style linear equation internally (`z = b0 + b1X...`) before applying the sigmoid function to convert it into a probability for classification.

### What is the Sigmoid Function?

> A mathematical function that maps any real value into a range between 0 and 1, shaped like an "S" curve, used to convert linear output into probabilities.

### Why can't we use Linear Regression for classification problems?

> Because its output is unbounded (can be any real number) and doesn't represent probabilities, and using MSE on classification creates a non-convex cost function that's hard to optimize.

### What is the Cost Function used in Linear Regression?

> Mean Squared Error (MSE) — the average squared difference between actual and predicted values.

### What is the Cost Function used in Logistic Regression?

> Log Loss / Cross-Entropy Loss — penalizes confident wrong predictions more heavily than MSE would.

### What is Gradient Descent?

> An optimization algorithm that iteratively adjusts model parameters (weights) to minimize the cost function.

### What is the decision threshold in Logistic Regression?

> A cutoff value (commonly 0.5) used to convert predicted probability into a final class label.

### What is Multicollinearity?

> A situation where independent variables are highly correlated with each other, which can distort coefficient estimates in Linear Regression.

---

## 🏆 One-Line Summary

```text
Linear Regression     → Straight line → Predicts continuous values → MSE

Logistic Regression   → Sigmoid curve → Predicts class probability → Log Loss
```

---

<div align="center">

### ⭐ Star this repository if it helped you learn Machine Learning!

</div>
