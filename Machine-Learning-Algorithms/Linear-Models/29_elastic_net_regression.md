# Elastic Net Regression

## Definition

Elastic Net Regression is a regularized version of Linear Regression that combines **L1 (Lasso)** and **L2 (Ridge)** regularization.

```text
Elastic Net
=
Linear Regression
+
L1 Regularization
+
L2 Regularization
```

It can both **shrink coefficients** and **make some coefficients exactly zero**.

---

## 1. Why Elastic Net Regression?

Ridge and Lasso dono ke apne advantages hain.

### Ridge

Ridge controls large coefficients but usually keeps all features.

### Lasso

Lasso can make some coefficients exactly zero and perform feature selection.

### Elastic Net

Elastic Net combines both approaches.

```text
Ridge
→ Coefficient Shrinkage

Lasso
→ Feature Selection

Elastic Net
→ Shrinkage + Feature Selection
```

---

## 2. L1 Regularization

L1 regularization comes from Lasso.

It uses the absolute values of coefficients.

```text
L1 Penalty
=
Σ|coefficient|
```

L1 regularization can push some coefficients exactly to zero.

---

## 3. L2 Regularization

L2 regularization comes from Ridge.

It uses the squared values of coefficients.

```text
L2 Penalty
=
Σ(coefficient²)
```

L2 regularization strongly controls large coefficients.

---

## 4. Elastic Net Penalty

Elastic Net combines both penalties.

Conceptually:

```text
Elastic Net Loss
=
Squared Error
+
L1 Penalty
+
L2 Penalty
```

Therefore:

```text
Elastic Net
=
Lasso + Ridge
```

---

## 5. Alpha

`alpha` controls the overall strength of regularization.

```text
Small alpha
→ Weak regularization

Large alpha
→ Strong regularization
```

When alpha becomes very large, coefficients can become heavily restricted and the model may underfit.

---

## 6. L1 Ratio

Elastic Net has another important parameter called `l1_ratio`.

It controls the balance between L1 and L2 regularization.

```text
l1_ratio = 1
→ Mostly / entirely L1
→ Lasso behavior

l1_ratio = 0
→ Mostly / entirely L2
→ Ridge behavior
```

For example:

```text
l1_ratio = 0.5
→ Balanced combination of L1 and L2
```

---

## 7. Physical Example

Imagine you have two tools.

```text
Ridge
→ Controls how large coefficients become

Lasso
→ Removes unnecessary features
```

Elastic Net combines both tools.

```text
Elastic Net
→ Controls coefficients
+
→ Can remove unnecessary features
```

---

## 8. Coefficient Shrinkage

Elastic Net can reduce the size of coefficients.

Example:

```text
Before:

Feature A → 10
Feature B → 5
Feature C → 2
```

After Elastic Net:

```text
Feature A → 6
Feature B → 3
Feature C → 0
```

Feature A and B were shrunk, while Feature C became zero.

---

## 9. Feature Selection

Because Elastic Net includes L1 regularization, some coefficients can become exactly zero.

Example:

```text
Age              → 0.42
Income           → 0.21
Noise Feature    → 0
Random Feature   → 0
```

The features with zero coefficients are excluded from the fitted linear prediction.

Therefore, Elastic Net can perform feature selection.

---

## 10. Ridge vs Lasso vs Elastic Net

| Ridge | Lasso | Elastic Net |
|---|---|---|
| L2 | L1 | L1 + L2 |
| Shrinks coefficients | Shrinks coefficients | Shrinks coefficients |
| Usually keeps features | Can remove features | Can remove features |
| No exact feature selection usually | Feature selection | Feature selection |
| Good for correlated features | Useful for sparse models | Useful for correlated + sparse features |

---

## 11. `alpha` vs `l1_ratio`

These two parameters have different jobs.

### Alpha

Controls **how strong the overall regularization is**.

```text
alpha ↑
→ Stronger regularization
```

### l1_ratio

Controls **which type of regularization dominates**.

```text
l1_ratio ↑
→ More L1 / Lasso influence

l1_ratio ↓
→ More L2 / Ridge influence
```

---

## 12. Example Parameter Combinations

```python
ElasticNet(alpha=1.0, l1_ratio=0.5)
```

This gives a balanced combination of L1 and L2.

```python
ElasticNet(alpha=1.0, l1_ratio=0.8)
```

This gives more Lasso influence.

```python
ElasticNet(alpha=1.0, l1_ratio=0.2)
```

This gives more Ridge influence.

---

## 13. Scikit-Learn Syntax

```python
from sklearn.linear_model import ElasticNet

model = ElasticNet(
    alpha=1.0,
    l1_ratio=0.5
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

## 14. Feature Scaling

Feature scaling is generally important before Elastic Net.

Different feature scales can affect regularization.

A common workflow is:

```text
Train/Test Split
       ↓
Feature Scaling
       ↓
Elastic Net
       ↓
Prediction
       ↓
Evaluation
```

Using a `Pipeline` helps combine scaling and modeling safely.

---

## 15. Overfitting

Elastic Net can help reduce overfitting by controlling model complexity.

```text
Large Coefficients
       ↓
L1 + L2 Penalties
       ↓
Smaller Coefficients
       ↓
Simpler Model
```

However, very strong regularization can cause underfitting.

---

## 16. Correlated Features

Elastic Net can be useful when several features are correlated.

Lasso may select one feature and remove other correlated features.

Ridge tends to distribute the effect across correlated features.

Elastic Net combines ideas from both approaches and can provide a useful balance.

---

## 17. Evaluation

Elastic Net Regression can be evaluated using:

- MAE
- MSE
- RMSE
- R² Score

The best values of `alpha` and `l1_ratio` should ideally be selected using validation or cross-validation.

---

## 18. Advantages

- Combines Ridge and Lasso
- Controls large coefficients
- Can perform feature selection
- Useful with many features
- Can work well with correlated features
- Provides flexibility through `l1_ratio`
- Helps reduce overfitting

---

## 19. Limitations

- Has more parameters to tune than Ridge or Lasso
- Requires careful selection of `alpha`
- Requires careful selection of `l1_ratio`
- Feature scaling is generally important
- Too much regularization can cause underfitting
- Zero coefficient does not automatically mean the real-world feature is useless

---

## 20. Common Mistakes

- Confusing `alpha` with `l1_ratio`
- Thinking alpha decides Ridge vs Lasso
- Forgetting that `l1_ratio` controls the L1/L2 balance
- Using very large alpha without validation
- Forgetting feature scaling
- Evaluating only on training data
- Assuming Elastic Net is always better than Ridge or Lasso

---

## 21. Basic Workflow

```text
Prepare Data
     ↓
Split Data
     ↓
Scale Features
     ↓
Choose Alpha
     ↓
Choose L1 Ratio
     ↓
Train Elastic Net
     ↓
Make Predictions
     ↓
Evaluate Model
     ↓
Inspect Coefficients
     ↓
Tune Parameters
```

---

## 22. Quick Comparison

```text
Ridge
→ L2
→ Shrink coefficients

Lasso
→ L1
→ Shrink + Feature Selection

Elastic Net
→ L1 + L2
→ Shrink + Feature Selection
```

---

## Key Points

- Elastic Net combines L1 and L2 regularization.
- L1 comes from Lasso.
- L2 comes from Ridge.
- `alpha` controls overall regularization strength.
- `l1_ratio` controls the balance between L1 and L2.
- `l1_ratio = 1` gives Lasso-like behavior.
- `l1_ratio = 0` gives Ridge-like behavior.
- Elastic Net can shrink coefficients.
- Some coefficients can become exactly zero.
- Therefore, Elastic Net can perform feature selection.
- Feature scaling is generally important.
- Very strong regularization can cause underfitting.
- Alpha and l1_ratio should ideally be tuned using validation or cross-validation.

---

## Quick Recap

```text
Elastic Net
=
Linear Regression
+
L1 Regularization
+
L2 Regularization
```

```text
alpha
→ How strong is the regularization?

l1_ratio
→ How much L1 vs L2?
```

```text
l1_ratio = 1
→ Lasso

l1_ratio = 0
→ Ridge

l1_ratio = 0.5
→ Balanced L1 + L2
```

### Memory Trick

```text
Ridge  → Shrink
Lasso  → Select
Elastic Net → Shrink + Select
```