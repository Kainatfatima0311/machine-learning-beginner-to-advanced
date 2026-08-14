# 32 - Ensemble Learning

## What is Ensemble Learning?
Ensemble Learning is a technique where instead of relying on a single model,
the combined results of **multiple models** are used to make the final
prediction, so that it becomes more accurate and reliable. It's like asking
10 friends instead of just one, and going with the majority answer.

## Diversity of Models
Ensemble learning works well only when the models are **different** from
each other (different data, features, or algorithms). If all models think
the same way, their combined mistakes will also be the same. Diversity
reduces overall error.

## Wisdom of Crowds
Concept: individual guesses can be wrong, but the **average of many guesses**
tends to be very close to the actual value. Ensemble Learning is based on
this principle.

---

## Bagging (Bootstrap Aggregating)
- Multiple models are trained **in parallel (independently)**.
- Each model gets a **random sample (with replacement)** from the original dataset.
- Final prediction: **average/majority vote** of all models.
- Goal: **Reduce variance** (reduce overfitting).
- Example: **Random Forest** (a combination of many Decision Trees).

## Boosting
- Multiple models are trained **sequentially (one after another)**.
- Each new model gives more weight to the **errors** made by the previous model.
- Goal: **Reduce bias** (turn weak learners into strong ones).
- Popular Algorithms:
  - **AdaBoost** - gives more weight to misclassified samples
  - **Gradient Boosting** - gradually minimizes errors (residuals)
  - **XGBoost** - a fast, optimized version of Gradient Boosting
  - **LightGBM** - fast Boosting technique for large datasets
  - **CatBoost** - Boosting specially designed for categorical data
  - **Stochastic Gradient Boosting** - adds randomness to Gradient Boosting

## Bagging vs Boosting

| | Bagging | Boosting |
|---|---|---|
| Training | Parallel (independent) | Sequential (one after another) |
| Focus | Reduce variance | Reduce bias |
| Example | Random Forest | AdaBoost, XGBoost |

## Stacking
Predictions from multiple different models (e.g. Decision Tree + Logistic
Regression + KNN) are passed to a **meta-model**, which intelligently
combines them to produce the final prediction. This is a smarter approach
than simple voting.

## Voting
- **Hard Voting**: Each model predicts its own final class, and the class
  that appears most often becomes the final answer (majority vote).
- **Soft Voting**: Each model outputs a probability, and the average of
  all probabilities is taken for the final decision — more accurate since
  confidence is also considered.

## Dataset Used
`employee_data_cleaned.csv` - same dataset used in the Decision Tree notebook.
Target: `Attrition` (Yes/No)

## Workflow in this Notebook
1. Load data and apply the same preprocessing (as done in Decision Tree)
2. Train a Random Forest Classifier (Bagging example)
3. Compare Random Forest with Decision Tree
4. Check Feature Importance
5. Train AdaBoost (Boosting example)
6. Train Gradient Boosting
7. Build a Voting Classifier (Hard + Soft)
8. Compare accuracy of all models