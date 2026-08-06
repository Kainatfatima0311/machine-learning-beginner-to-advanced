# Model Evaluation

## Definition

Model Evaluation is the process of measuring the performance of a trained Machine Learning model using unseen testing data.

---

# Purpose

The goal of Model Evaluation is to determine how well the model performs on new data.

---

# Evaluation Workflow

```text
Train Model
      ↓
Testing Data
      ↓
Generate Predictions
      ↓
Compare with Actual Values
      ↓
Measure Performance
```

---

# Why Model Evaluation is Important

- Measures model performance.
- Detects overfitting and underfitting.
- Helps compare different models.
- Determines whether the model is ready for deployment.

---

# Testing Data

Testing data is not used during training.

It is only used to evaluate the trained model.

---

# Common Evaluation Metrics

## Classification

- Accuracy
- Precision
- Recall
- F1 Score
- ROC-AUC

## Regression

- MAE
- MSE
- RMSE
- R² Score

---

# Characteristics of a Good Model

- High predictive performance
- Good generalization
- Stable results
- Reliable predictions

---

# Key Points

- Model Evaluation comes after Model Training.
- Testing data should never be used for training.
- Different problems require different evaluation metrics.
- A model should perform well on unseen data, not only on training data.