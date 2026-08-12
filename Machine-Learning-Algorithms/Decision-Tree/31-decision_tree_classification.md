# Decision Tree Classification

## Definition

A Decision Tree is a supervised machine learning algorithm that makes predictions by asking a series of questions and splitting data into smaller groups.

For classification, the final prediction is a class/category.

Examples:
- Pass / Fail
- Stay / Leave
- Spam / Not Spam

---

## Decision Tree Basics

A Decision Tree contains:

- Root Node → Starting point of the tree
- Internal Node → A decision/question
- Branch → Result of a decision
- Leaf Node → Final prediction

Example:

```text
Age < 30?
├── Yes → Stay
└── No → Leave
```

The tree keeps splitting data until it reaches a final prediction.

---

## Entropy

Entropy measures the impurity or uncertainty in a dataset.

- Entropy = 0 → Completely pure
- Higher entropy → More mixed classes

Formula:

```text
Entropy = -Σ p(x) log₂ p(x)
```

If a node contains only one class, its entropy is 0.

---

## Gini Impurity

Gini Impurity also measures the impurity of a node.

Formula:

```text
Gini = 1 - Σ p(x)²
```

Interpretation:

- Gini = 0 → Pure node
- Higher Gini → More mixed classes

Decision Trees can use Gini or Entropy to evaluate splits.

---

## Information Gain

Information Gain measures how much a split reduces uncertainty.

```text
Information Gain
= Parent Impurity - Weighted Child Impurity
```

A good split produces a larger reduction in impurity.

Therefore, the tree prefers splits with higher Information Gain.

---

## Categorical Decision Trees

Categorical features contain categories or labels.

Examples:

- Department → HR, IT, Sales
- Gender → Male, Female
- City → Lahore, Karachi, Islamabad

Categorical features generally need encoding before being passed to the standard scikit-learn Decision Tree implementation.

---

## Numerical Decision Trees

Numerical features contain numbers.

Examples:

- Age
- Salary
- Experience
- Area

The tree commonly creates threshold-based splits.

Examples:

```text
Age < 30
Salary >= 70000
```

The tree evaluates possible thresholds and chooses useful splits according to its criterion.

---

## Mixed Decision Trees

A dataset can contain both numerical and categorical features.

Example:

```text
Age          → Numerical
Salary       → Numerical
Experience   → Numerical
Department   → Categorical
Gender       → Categorical
```

Categorical features generally need encoding before being used with scikit-learn Decision Trees.

---

# Hyperparameters

## criterion

`criterion` determines how the tree evaluates the quality of a split.

Common classification criteria include:

```python
criterion="gini"
criterion="entropy"
criterion="log_loss"
```

- Gini → Uses Gini Impurity
- Entropy → Uses Entropy / Information Gain
- Log Loss → Uses logarithmic loss

Example:

```python
DecisionTreeClassifier(criterion="gini")
```

`criterion` is used during training to choose splits; it is not a final evaluation metric.

---

## max_depth

Controls the maximum depth of the tree.

```python
DecisionTreeClassifier(max_depth=5)
```

A smaller value creates a simpler tree.

A very large value can make the tree overly complex and increase overfitting risk.

---

## min_samples_split

Specifies the minimum number of samples required to split an internal node.

Example:

```python
DecisionTreeClassifier(min_samples_split=10)
```

A node with fewer than 10 samples will not be split.

---

## min_samples_leaf

Specifies the minimum number of samples required in a leaf node.

Example:

```python
DecisionTreeClassifier(min_samples_leaf=5)
```

This helps prevent very small leaves and can reduce overfitting.

---

## max_features

Controls the maximum number of features considered when looking for a split.

Example:

```python
DecisionTreeClassifier(max_features=5)
```

Common options include:

```python
None
"sqrt"
"log2"
```

`None` means all available features can be considered.

---

## max_leaf_nodes

Limits the maximum number of leaf nodes.

Example:

```python
DecisionTreeClassifier(max_leaf_nodes=10)
```

This can be used to control tree complexity.

---

## min_impurity_decrease

A split is made only when it provides at least the specified improvement in impurity.

Example:

```python
DecisionTreeClassifier(min_impurity_decrease=0.01)
```

Higher values can prevent unnecessary splits.

---

## splitter

Controls the split selection strategy.

### best

Chooses the best split from available options.

```python
splitter="best"
```

### random

Chooses a split randomly from available options.

```python
splitter="random"
```

`best` is the default option for Decision Tree classifiers.

---

## random_state

Controls randomness so results can be reproduced.

Example:

```python
DecisionTreeClassifier(random_state=42)
```

`42` is not a special ML number. Any fixed integer can be used.

The important part is using the same value when reproducibility is required.

---

## class_weight

Controls the importance given to different classes.

This is especially useful for imbalanced classification datasets.

Example:

```python
DecisionTreeClassifier(class_weight="balanced")
```

`balanced` automatically gives more importance to minority classes.

Manual weights can also be provided:

```python
DecisionTreeClassifier(
    class_weight={
        0: 1,
        1: 3
    }
)
```

---

# Overfitting and Underfitting

## Underfitting

Underfitting happens when the model is too simple to learn the important patterns in the data.

```text
Training performance → Poor
Testing performance  → Poor
```

Example:

A Decision Tree with a very small `max_depth`.

---

## Overfitting

Overfitting happens when the model learns the training data too closely, including unnecessary details and noise.

```text
Training performance → Very high
Testing performance  → Lower
```

Example:

A Decision Tree with excessive depth.

---

## Controlling Overfitting

Useful hyperparameters include:

- `max_depth`
- `min_samples_split`
- `min_samples_leaf`
- `max_leaf_nodes`
- `min_impurity_decrease`

The goal is to create a tree that learns useful patterns without memorizing the training data.

---

# DecisionTreeClassifier

Scikit-learn provides `DecisionTreeClassifier` for classification problems.

```python
from sklearn.tree import DecisionTreeClassifier

model = DecisionTreeClassifier(
    criterion="gini",
    max_depth=5,
    random_state=42
)

model.fit(X_train, y_train)

y_pred = model.predict(X_test)
```

---

# Key Points

- Decision Trees make predictions using a sequence of decisions.
- Classification Trees predict categories/classes.
- Entropy and Gini measure impurity.
- Information Gain measures improvement from a split.
- Numerical features are commonly split using thresholds.
- Categorical features generally need encoding in scikit-learn.
- `max_depth` controls tree depth.
- `min_samples_split` controls when a node can split.
- `min_samples_leaf` controls minimum samples in leaves.
- `max_features` controls features considered for a split.
- `max_leaf_nodes` limits the number of leaves.
- `min_impurity_decrease` prevents weak splits.
- `splitter` controls split selection strategy.
- `random_state` helps make results reproducible.
- `class_weight` helps with imbalanced classes.
- Very simple trees can underfit.
- Very complex trees can overfit.
