# Logistic Regression

## Definition

Logistic Regression is a supervised machine learning algorithm mainly used for **classification problems**.

Instead of directly predicting a continuous value, Logistic Regression predicts a **probability** and then converts that probability into a class.

Example:

```text
Patient
   ↓
Features
   ↓
Logistic Regression
   ↓
Probability
   ↓
Threshold
   ↓
Class 0 or Class 1
```

---

## 1. Logistic Regression vs Linear Regression

### Linear Regression

Linear Regression predicts continuous numerical values.

Example:

```text
House Size → House Price

1000 sq ft → Rs. 5 million
1500 sq ft → Rs. 7 million
2000 sq ft → Rs. 9 million
```

It answers:

```text
"How much?"
```

### Logistic Regression

Logistic Regression is mainly used to predict classes.

Example:

```text
Patient → Heart Failure?

0 → No
1 → Yes
```

It answers:

```text
"Which class?"
```

---

## 2. Classification

Classification means assigning an observation to a category or class.

Examples:

```text
Email → Spam / Not Spam

Patient → Disease / No Disease

Transaction → Fraud / Not Fraud

Student → Pass / Fail
```

---

## 3. Probability

Logistic Regression first produces a probability between `0` and `1`.

Example:

```text
0.10 → 10%
0.40 → 40%
0.75 → 75%
0.95 → 95%
```

For binary classification, this probability usually represents the probability of Class 1.

Example:

```text
P(Heart Failure = 1) = 0.82
```

This means the model estimates an 82% probability of Class 1.

---

## 4. Sigmoid Function

The Sigmoid Function converts any real-valued input into a value between `0` and `1`.

Formula:

```text
σ(z) = 1 / (1 + e⁻ᶻ)
```

The output is always between:

```text
0 < σ(z) < 1
```

### Example

```text
z = -2
→ probability ≈ 0.12

z = 0
→ probability = 0.50

z = 2
→ probability ≈ 0.88
```

The sigmoid curve looks like an S-shape.

---

## 5. How Logistic Regression Works

The basic workflow is:

```text
Input Features
      ↓
Linear Combination
      ↓
Sigmoid Function
      ↓
Probability
      ↓
Classification Threshold
      ↓
Predicted Class
```

The linear part can be represented as:

```text
z = b₀ + b₁x₁ + b₂x₂ + ... + bₙxₙ
```

Then:

```text
Probability = σ(z)
```

---

## 6. Classification Threshold

A threshold converts probability into a class.

A common threshold is `0.5`.

```text
Probability < 0.5
→ Class 0

Probability ≥ 0.5
→ Class 1
```

Example:

```text
0.20 → Class 0
0.40 → Class 0
0.50 → Class 1
0.80 → Class 1
```

The threshold can be changed depending on the problem.

---

## 7. Physical Example

Imagine a doctor predicting whether a patient has a disease.

The model predicts:

```text
Patient A → 0.12
Patient B → 0.43
Patient C → 0.87
```

Using a threshold of `0.5`:

```text
Patient A → Class 0
Patient B → Class 0
Patient C → Class 1
```

So the model first predicts a probability and then converts it into a class.

---

# Log Loss

## 8. Definition

Log Loss, also called **Logarithmic Loss** or **Cross-Entropy Loss**, measures how well predicted probabilities match the actual classes.

Lower Log Loss is better.

It strongly penalizes predictions that are **wrong and confident**.

---

## 9. Log Loss Example

Suppose the actual class is:

```text
Actual = 1
```

### Prediction A

```text
Probability = 0.95
```

Very confident and correct.

```text
→ Low Log Loss
```

### Prediction B

```text
Probability = 0.55
```

Correct but uncertain.

```text
→ Higher Log Loss
```

### Prediction C

```text
Probability = 0.05
```

Very confident but wrong.

```text
→ Very high Log Loss
```

---

## 10. Binary Log Loss Formula

```text
Log Loss =
-[y log(p) + (1-y) log(1-p)]
```

Where:

- `y` = actual class
- `p` = predicted probability of Class 1

---

## 11. When Actual Class is 1

If:

```text
y = 1
```

then:

```text
Log Loss = -log(p)
```

Therefore:

```text
Actual = 1
Prediction = 0.99
→ Very low loss

Actual = 1
Prediction = 0.01
→ Very high loss
```

---

## 12. When Actual Class is 0

If:

```text
y = 0
```

then:

```text
Log Loss = -log(1-p)
```

Therefore:

```text
Actual = 0
Prediction = 0.01
→ Very low loss

Actual = 0
Prediction = 0.99
→ Very high loss
```

---

# Binary Logistic Regression

## 13. Definition

Binary Logistic Regression is used when there are **two possible classes**.

Examples:

```text
Spam / Not Spam

Disease / No Disease

Pass / Fail

Yes / No
```

Usually the classes are represented as:

```text
0 → Class 0
1 → Class 1
```

---

## 14. Example

Heart failure prediction:

```text
0 → No heart failure
1 → Heart failure
```

The model might predict:

```text
Patient A → 0.12
Patient B → 0.78
```

Using a threshold of `0.5`:

```text
Patient A → 0
Patient B → 1
```

---

# Multinomial Logistic Regression

## 15. Definition

Multinomial Logistic Regression is used when there are **more than two classes** and the classes are not naturally ordered.

Example:

```text
Fruit Classification

0 → Apple
1 → Banana
2 → Orange
```

Another example:

```text
Transport

0 → Car
1 → Bus
2 → Train
```

The model predicts probabilities for multiple classes.

Example:

```text
Apple  → 0.10
Banana → 0.70
Orange → 0.20
```

The class with the highest probability becomes the prediction.

```text
Prediction → Banana
```

---

# Ordinal Logistic Regression

## 16. Definition

Ordinal Logistic Regression is used when classes have a **natural order**.

Example:

```text
Customer Satisfaction

1 → Poor
2 → Average
3 → Good
4 → Excellent
```

The categories have an order:

```text
Poor < Average < Good < Excellent
```

Another example:

```text
Education Level

High School
Bachelor
Master
PhD
```

The categories are ordered, so ordinal classification is appropriate.

---

## 17. Multinomial vs Ordinal

### Multinomial

Classes have no natural order.

```text
Apple
Banana
Orange
```

There is no meaningful:

```text
Apple < Banana < Orange
```

### Ordinal

Classes have a natural order.

```text
Poor
Average
Good
Excellent
```

---

# Regularized Logistic Regression

## 18. Definition

Regularization helps prevent Logistic Regression from becoming too complex and overfitting the training data.

Common regularization methods include:

- L1 Regularization
- L2 Regularization

---

## 19. L1 Regularization

L1 regularization is associated with Lasso.

It can make some coefficients exactly zero.

```text
Feature A → 0.52
Feature B → 0
Feature C → 0.18
```

This can provide feature selection.

---

## 20. L2 Regularization

L2 regularization is associated with Ridge.

It shrinks coefficients toward zero but generally does not make them exactly zero.

```text
Feature A → 0.52
Feature B → 0.03
Feature C → 0.18
```

---

## 21. Why Regularization?

Without regularization:

```text
Complex Model
     ↓
Training Error ↓
     ↓
Overfitting Risk ↑
```

With appropriate regularization:

```text
Regularization
     ↓
Smaller Coefficients
     ↓
Simpler Model
     ↓
Better Generalization
```

Too much regularization can cause underfitting.

---

# Logistic Regression Parameters

## 22. Important Parameters

### `C`

In scikit-learn Logistic Regression, `C` controls the inverse of regularization strength.

```text
Small C
→ Stronger regularization

Large C
→ Weaker regularization
```

This is opposite in interpretation to the `alpha` used by Ridge/Lasso.

---

### `penalty`

Specifies the type of regularization.

Common options include:

```text
l1
l2
elasticnet
```

The exact available options depend on the selected solver.

---

### `solver`

The solver is the optimization algorithm used to train the model.

Common solvers include:

```text
lbfgs
liblinear
saga
```

Different solvers support different penalties and problem types.

---

# Logistic Regression Evaluation

## 23. Classification Metrics

Logistic Regression can be evaluated using:

- Accuracy
- Precision
- Recall
- F1 Score
- Confusion Matrix
- ROC Curve
- AUC
- Log Loss

Different metrics are useful for different problems.

---

# Logistic Regression vs Linear Regression

## 24. Comparison

| Linear Regression | Logistic Regression |
|---|---|
| Mainly used for regression | Mainly used for classification |
| Predicts continuous values | Predicts probabilities/classes |
| Output can be any real value | Probability is between 0 and 1 |
| Uses linear relationship | Uses sigmoid to convert output to probability |
| Example: house price | Example: disease yes/no |

---

## 25. Real-Life Examples

### Linear Regression

```text
House Size
     ↓
House Price
```

### Logistic Regression

```text
Patient Features
     ↓
Disease Probability
     ↓
Disease / No Disease
```

---

# Feature Scaling

## 26. Feature Scaling

Feature scaling can be important for Logistic Regression, especially when regularization is used.

Example:

```text
Age              → 20–80
Platelets        → 100000–500000
Serum Sodium     → 120–150
```

Different scales can affect the optimization and regularization.

A common workflow is:

```text
Train/Test Split
       ↓
Feature Scaling
       ↓
Logistic Regression
       ↓
Prediction
       ↓
Evaluation
```

A `Pipeline` can be used to safely combine scaling and Logistic Regression.

---

# Logistic Regression Workflow

## 27. Complete Workflow

```text
Collect Data
     ↓
Clean Data
     ↓
Split Data
     ↓
Scale Features
     ↓
Train Logistic Regression
     ↓
Predict Probability
     ↓
Apply Threshold
     ↓
Predict Class
     ↓
Evaluate Model
     ↓
Tune Parameters
```

---

# Key Points

- Logistic Regression is mainly used for classification.
- It predicts probabilities between 0 and 1.
- The Sigmoid Function converts the linear output into a probability.
- A threshold converts probability into a class.
- `0.5` is a common default threshold.
- Binary Logistic Regression has two classes.
- Multinomial Logistic Regression handles multiple unordered classes.
- Ordinal Logistic Regression handles multiple ordered classes.
- Log Loss evaluates predicted probabilities.
- Lower Log Loss is better.
- Wrong and confident predictions receive a large Log Loss penalty.
- L1 regularization can produce zero coefficients.
- L2 regularization shrinks coefficients.
- Regularization helps control overfitting.
- In scikit-learn, `C` controls the inverse of regularization strength.
- Feature scaling is generally useful when regularization is used.
- Logistic Regression and Linear Regression solve different types of problems.

---

# Memory Tricks

```text
Linear Regression
→ "Kitna?"

Logistic Regression
→ "Kis class mein?"
```

```text
Sigmoid
→ Probability
```

```text
Probability
→ Threshold
→ Class
```

```text
Log Loss
→ Probability ki quality
```

```text
Binary
→ 2 classes

Multinomial
→ Multiple unordered classes

Ordinal
→ Multiple ordered classes
```

```text
L1
→ Feature selection

L2
→ Coefficient shrinkage
```

---

# Quick Revision

```text
Features
   ↓
Linear Combination
   ↓
Sigmoid
   ↓
Probability
   ↓
Threshold
   ↓
Class
```

Example:

```text
Model Output
     ↓
0.82
     ↓
82% probability
     ↓
0.82 ≥ 0.50
     ↓
Class 1
```

---

# Final Takeaway

Logistic Regression converts input features into a probability using the Sigmoid Function and then uses a classification threshold to assign a class.

Its major concepts are:

```text
Logistic Regression
├── Sigmoid Function
├── Probability
├── Classification Threshold
├── Log Loss
├── Binary Classification
├── Multinomial Classification
├── Ordinal Classification
└── Regularization
    ├── L1
    └── L2
```