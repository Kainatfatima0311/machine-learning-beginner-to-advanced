# Lasso Regression

## Definition

Lasso Regression is a regularized version of Linear Regression that uses **L1 Regularization**.

It reduces large coefficients and can make some coefficients exactly zero.

```text
Lasso Regression
=
Linear Regression
+
L1 Regularization
```

---

# 1. Why Lasso Regression?

Normal Linear Regression can sometimes produce large coefficients.

Lasso adds a penalty to control coefficient size.

Its special advantage is:

```text
Some coefficients → Exactly 0
```

This means Lasso can perform **feature selection**.

---

# 2. L1 Regularization

Lasso uses L1 Regularization.

The penalty is based on the absolute values of coefficients.

Conceptually:

```text
Lasso Loss
=
Squared Error
+
λ × Σ|coefficient|
```

---

# 3. Alpha

In Scikit-Learn, the regularization strength is controlled using `alpha`.

```text
Small alpha
→ Weak regularization

Large alpha
→ Strong regularization
```

Very large regularization can cause underfitting.

---

# 4. Coefficient Shrinkage

Lasso reduces coefficient values and can make some exactly zero.

Example:

```text
Before:

Feature A → 10
Feature B → 5
Feature C → 2
Feature D → 0.1
```

After Lasso:

```text
Feature A → 7
Feature B → 3
Feature C → 0
Feature D → 0
```

Some coefficients can become exactly zero.

---

# 5. Feature Selection

When a coefficient becomes `0`, that feature does not contribute to the fitted linear prediction.

Therefore, Lasso can automatically perform feature selection.

Example:

```text
Age              → 0.42
Education        → 0.31
Income           → 0.15
Random Feature   → 0
Noise Feature    → 0
```

The model effectively ignores the features with zero coefficients.

---

# 6. Ridge vs Lasso

## Ridge

```text
L2 Regularization
↓
Shrinks coefficients
↓
Usually keeps all features
```

## Lasso

```text
L1 Regularization
↓
Shrinks coefficients
↓
Can make some coefficients exactly zero
↓
Feature Selection
```

---

# 7. Physical Example

Imagine a teacher wants you to solve a problem using only useful information.

You have:

```text
Study Hours
Attendance
Previous Marks
Favorite Color
Shoe Size
```

Ridge may keep all features but reduce the influence of less useful ones.

Lasso may decide:

```text
Favorite Color → 0
Shoe Size      → 0
```

So:

```text
Ridge
→ Control all coefficients

Lasso
→ Control coefficients + remove some through zero coefficients
```

---

# 8. Overfitting

Lasso can help reduce overfitting by limiting model complexity.

```text
Large Coefficients
       ↓
L1 Penalty
       ↓
Smaller Coefficients
       ↓
Simpler Model
```

Excessive regularization can cause underfitting.

---

# 9. Multicollinearity

For highly correlated features, Lasso may select one feature and reduce another related feature's coefficient to zero.

Therefore, a zero coefficient does not automatically mean that the underlying real-world variable is completely useless.

---

# 10. Feature Scaling

Feature scaling is generally important before Lasso.

Example:

```text
Age       → 20–80
Income    → 20,000–500,000
```

A common workflow is:

```text
Train/Test Split
       ↓
Feature Scaling
       ↓
Lasso Regression
       ↓
Evaluation
```

Using a `Pipeline` helps keep preprocessing and modeling together and helps prevent data leakage.

---

# 11. Scikit-Learn Syntax

```python
from sklearn.linear_model import Lasso

model = Lasso(alpha=1.0)

model.fit(
    X_train,
    y_train
)

y_pred = model.predict(
    X_test
)
```

---

# 12. Checking Selected Features

After training:

```python
model.coef_
```

can be used to inspect coefficients.

Example:

```text
Feature A → 0.52
Feature B → 0.18
Feature C → 0.00
Feature D → -0.31
```

Feature C has a zero coefficient and is therefore not contributing to the fitted linear prediction.

---

# 13. Advantages

- Helps reduce overfitting
- Controls large coefficients
- Can perform automatic feature selection
- Useful when many features are available
- Produces a simpler model when some coefficients become zero

---

# 14. Limitations

- Strong regularization can cause underfitting
- Alpha must be selected carefully
- Feature scaling is generally important
- With highly correlated features, selected features can be unstable
- Zero coefficient does not necessarily mean a real-world variable has no importance

---

# 15. Lasso vs Ridge

| Ridge | Lasso |
|---|---|
| L2 Regularization | L1 Regularization |
| Squares coefficients | Uses absolute coefficients |
| Shrinks coefficients | Shrinks coefficients |
| Usually keeps all features | Can set coefficients to zero |
| Does not usually perform feature selection | Can perform feature selection |
| Useful with multicollinearity | Useful when feature selection is desirable |

---

# 16. Evaluation

Lasso Regression can be evaluated using:

- MAE
- MSE
- RMSE
- R² Score

The regularization strength should be selected using validation or cross-validation.

---

# 17. Common Mistakes

- Assuming every zero coefficient means the feature is useless in the real world
- Using a very large alpha without validation
- Forgetting feature scaling
- Evaluating only on training data
- Assuming Lasso is always better than Ridge
- Ignoring correlated features
- Choosing alpha based only on training performance

---

# 18. Basic Workflow

```text
Prepare Data
     ↓
Split Data
     ↓
Scale Features
     ↓
Choose Alpha
     ↓
Train Lasso
     ↓
Make Predictions
     ↓
Evaluate
     ↓
Inspect Coefficients
     ↓
Identify Zero Coefficients
```

---

# Key Points

- Lasso Regression is Linear Regression with L1 regularization.
- L1 uses the absolute values of coefficients in its penalty.
- `alpha` controls regularization strength.
- Larger alpha means stronger regularization.
- Lasso shrinks coefficients toward zero.
- Some coefficients can become exactly zero.
- Zero coefficients allow Lasso to perform feature selection.
- Feature scaling is generally important.
- Too much regularization can cause underfitting.
- Lasso can be useful when a simpler feature set is desirable.
- Lasso and Ridge use different regularization penalties.

---

# Quick Recap

```text
Lasso
= Linear Regression + L1 Regularization

L1
= Absolute Coefficient Penalty

alpha
= Regularization Strength

alpha ↑
→ Stronger Penalty
→ More Shrinkage

Coefficient = 0
→ Feature is excluded from the fitted linear prediction

Main Advantage
→ Feature Selection
```

---

# Scikit-Learn Syntax

```python
from sklearn.linear_model import Lasso

model = Lasso(alpha=1.0)

model.fit(X_train, y_train)

y_pred = model.predict(X_test)
```