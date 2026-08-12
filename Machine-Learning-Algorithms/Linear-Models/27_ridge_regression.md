# Ridge Regression

## Definition

Ridge Regression is a regularized version of Linear Regression.

It adds a penalty to large coefficients to reduce model complexity and help control overfitting.

```text
Ridge Regression
=
Linear Regression
+
L2 Regularization
```

---

# 1. Why Ridge Regression?

In normal Linear Regression, coefficients can sometimes become very large.

This can happen when:

- Features are highly correlated
- The model is sensitive to the training data
- The model is overfitting

Ridge Regression controls large coefficients by adding a penalty.

---

# 2. Linear Regression vs Ridge Regression

### Linear Regression

```text
Minimize Prediction Error
```

### Ridge Regression

```text
Minimize Prediction Error
+
Penalize Large Coefficients
```

The goal is to maintain good predictions while keeping coefficients smaller.

---

# 3. L2 Regularization

Ridge Regression uses **L2 Regularization**.

The penalty is based on the squared values of the coefficients.

Conceptually:

```text
Ridge Loss
=
Squared Error
+
λ × Σ(coefficient²)
```

The coefficient values are squared before being added to the penalty.

---

# 4. Lambda (λ) / Alpha

The regularization strength controls how strongly large coefficients are penalized.

In Machine Learning libraries such as Scikit-Learn, this parameter is commonly called `alpha`.

```text
Small alpha
→ Weak regularization

Large alpha
→ Strong regularization
```

### Important

```text
alpha = 0
```

makes Ridge equivalent to ordinary Linear Regression from the regularization perspective.

---

# 5. Coefficient Shrinkage

Ridge Regression reduces the size of coefficients.

Example:

```text
Before Ridge:

Feature A → 20
Feature B → 15
Feature C → -18
```

After Ridge:

```text
Feature A → 12
Feature B → 9
Feature C → -10
```

The coefficients have been shrunk toward zero.

### Important

Ridge usually does **not** force coefficients exactly to zero.

This is different from Lasso Regression.

---

# 6. Overfitting

## Definition

Overfitting occurs when a model learns the training data too closely and performs poorly on unseen data.

Example:

```text
Training Performance → Very High
Test Performance     → Poor
```

Ridge can help reduce overfitting by limiting excessively large coefficients.

```text
Large Coefficients
       ↓
Ridge Penalty
       ↓
Smaller Coefficients
       ↓
Simpler / More Stable Model
```

Regularization can improve generalization, although the appropriate strength must be selected using validation.

---

# 7. Multicollinearity

## Definition

Multicollinearity occurs when input features are strongly linearly related to each other.

Example:

```text
House Area
Covered Area
Number of Rooms
```

Some of these features may contain overlapping information.

This can make ordinary Linear Regression coefficients unstable.

Ridge Regression can help stabilize the model by shrinking coefficients.

---

# 8. Effect of Alpha

### Low Alpha

```text
Weak Penalty
↓
Coefficients remain relatively large
↓
Model behaves more like Linear Regression
```

### High Alpha

```text
Strong Penalty
↓
Coefficients shrink more
↓
Model becomes more regularized
```

Very large regularization can cause underfitting.

Therefore, alpha should be selected carefully.

---

# 9. Ridge Regression Equation

A simplified representation is:

```text
y = b₀ + b₁x₁ + b₂x₂ + ... + bₙxₙ
```

Ridge learns these coefficients while also minimizing an L2 penalty.

Conceptually:

```text
Loss =
Prediction Error
+
alpha × Sum of Squared Coefficients
```

---

# 10. Ridge Regression with Scikit-Learn

```python
from sklearn.linear_model import Ridge

model = Ridge(
    alpha=1.0
)

model.fit(
    X_train,
    y_train
)

y_pred = model.predict(
    X_test
)
```

---

# 11. Changing Alpha

Different alpha values can be tested:

```python
alphas = [0.01, 0.1, 1, 10, 100]
```

A model can be trained using each value and evaluated on validation data.

The goal is to find a suitable regularization strength.

---

# 12. Ridge and Feature Scaling

Ridge Regression applies a penalty to coefficients.

Because coefficient magnitudes depend on feature scales, scaling numerical features is generally important before applying Ridge.

Example:

```text
Age
20–80

Income
20,000–500,000
```

These features are on very different scales.

A common workflow is:

```text
Data
 ↓
Train/Test Split
 ↓
Feature Scaling
 ↓
Ridge Regression
 ↓
Evaluation
```

A `Pipeline` is useful because it keeps preprocessing and modeling together and helps prevent data leakage.

---

# 13. Advantages

- Helps reduce overfitting
- Controls large coefficients
- Can improve model stability
- Useful with multicollinearity
- Works well when many features contribute to the target
- Retains all features in the model

---

# 14. Limitations

- Does not normally perform feature selection
- Coefficients are shrunk but usually remain non-zero
- Requires choosing an appropriate alpha
- Strong regularization can cause underfitting
- Feature scaling is generally important
- Does not automatically solve every form of overfitting

---

# 15. Ridge vs Linear Regression

| Linear Regression | Ridge Regression |
|---|---|
| No regularization | Uses L2 regularization |
| Can produce large coefficients | Shrinks coefficients |
| More sensitive to multicollinearity | More stable with multicollinearity |
| Can overfit | Can help control overfitting |
| No alpha parameter | Uses alpha |

---

# 16. Ridge vs Lasso

| Ridge | Lasso |
|---|---|
| L2 Regularization | L1 Regularization |
| Uses squared coefficients | Uses absolute coefficient values |
| Shrinks coefficients | Can shrink some coefficients exactly to zero |
| Usually keeps all features | Can perform feature selection |

---

# 17. Evaluation

Ridge Regression can be evaluated using the same Regression metrics used for Linear Regression:

- MAE
- MSE
- RMSE
- R² Score

The best model should be selected based on validation/test performance and the requirements of the problem.

---

# 18. Common Mistakes

- Using Ridge without considering feature scaling
- Choosing an extremely large alpha without validation
- Assuming higher regularization is always better
- Assuming Ridge automatically removes irrelevant features
- Evaluating only on training data
- Ignoring multicollinearity
- Comparing models using different train/test splits

---

# 19. Basic Workflow

```text
Prepare Data
     ↓
Split Data
     ↓
Scale Features
     ↓
Choose Alpha
     ↓
Train Ridge Model
     ↓
Make Predictions
     ↓
Evaluate
     ↓
Tune Alpha if Needed
```

---

# Key Points

- Ridge Regression is Linear Regression with L2 regularization.
- It penalizes large coefficients.
- The regularization parameter is commonly called `alpha`.
- Small alpha means weak regularization.
- Large alpha means strong regularization.
- Ridge shrinks coefficients toward zero.
- Ridge usually does not make coefficients exactly zero.
- Ridge can help control overfitting.
- Ridge can improve stability when features are highly correlated.
- Feature scaling is generally important before Ridge.
- Alpha should be selected using validation or cross-validation.
- Ridge can be evaluated using MAE, MSE, RMSE, and R².

---

# Quick Recap

```text
Ridge
= Linear Regression + L2 Regularization

L2
= Squared Coefficient Penalty

alpha
= Regularization Strength

Small alpha
→ Less penalty

Large alpha
→ More penalty

Ridge
→ Shrinks coefficients

Ridge usually
→ Does not make coefficients exactly zero

Main Uses
→ Reduce overfitting
→ Handle multicollinearity
→ Improve model stability
```

---

# Scikit-Learn Syntax

```python
from sklearn.linear_model import Ridge

model = Ridge(alpha=1.0)

model.fit(X_train, y_train)

y_pred = model.predict(X_test)
```