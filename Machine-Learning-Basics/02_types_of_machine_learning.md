# Types of Machine Learning

## Introduction

Machine Learning is mainly divided into three types based on how a model learns from data.

- Supervised Learning
- Unsupervised Learning
- Reinforcement Learning

Each type is designed to solve different kinds of problems.

---

# Supervised Learning

## Definition

Supervised Learning is a type of Machine Learning in which the model is trained using input data along with the correct output (labels). The model learns the relationship between inputs and outputs and uses this knowledge to make predictions on new data.

## Workflow

```text
Input Data + Correct Output
          ↓
      Train Model
          ↓
Learn Patterns
          ↓
Predict New Output
```

## Examples

- Spam Email Detection
- House Price Prediction
- Disease Prediction
- Student Result Prediction
- Loan Approval

## Types of Supervised Learning

### Classification

Classification predicts a category or class.

**Examples**

- Spam / Not Spam
- Yes / No
- Cat / Dog

### Regression

Regression predicts a continuous numerical value.

**Examples**

- House Price
- Temperature
- Salary Prediction

---

# Unsupervised Learning

## Definition

Unsupervised Learning is a type of Machine Learning in which only input data is provided. The model does not receive correct answers and tries to discover hidden patterns or relationships in the data.

## Workflow

```text
Input Data
     ↓
Find Hidden Patterns
     ↓
Create Groups
```

## Examples

- Customer Segmentation
- Market Basket Analysis
- Recommendation Systems
- Pattern Discovery
- Data Compression

---

# Reinforcement Learning

## Definition

Reinforcement Learning is a type of Machine Learning in which an agent learns by interacting with an environment. The agent receives rewards for correct actions and penalties for incorrect actions, gradually learning the best strategy.

## Workflow

```text
Take Action
     ↓
Receive Reward / Penalty
     ↓
Improve Strategy
     ↓
Repeat
```

## Examples

- Self-Driving Cars
- Robotics
- Chess Playing AI
- Video Games
- Trading Systems

---

# Comparison of Machine Learning Types

| Feature | Supervised | Unsupervised | Reinforcement |
|---------|------------|--------------|----------------|
| Labels Available | Yes | No | No |
| Learns From | Correct Outputs | Hidden Patterns | Rewards and Penalties |
| Main Goal | Prediction | Pattern Discovery | Decision Making |
| Example | Spam Detection | Customer Segmentation | Chess AI |

---

# When to Use Each Type

## Use Supervised Learning When

- Historical labeled data is available.
- The goal is prediction.
- The expected output is known.

## Use Unsupervised Learning When

- Labels are not available.
- Hidden patterns need to be discovered.
- Data grouping is required.

## Use Reinforcement Learning When

- The model must learn through experience.
- Decisions are made step by step.
- Rewards and penalties are available.

---

# Key Differences

- Supervised Learning uses labeled data.
- Unsupervised Learning uses unlabeled data.
- Reinforcement Learning learns through interaction with the environment.
- Classification and Regression are part of Supervised Learning.
- Clustering is commonly used in Unsupervised Learning.

---

# Key Points

- Machine Learning has three major learning approaches.
- Choosing the correct learning type depends on the problem.
- Supervised Learning is the most commonly used approach in real-world applications.
- Unsupervised Learning helps discover hidden structures in data.
- Reinforcement Learning is widely used in robotics, gaming, and autonomous systems.

---

# Quick Summary

```text
Machine Learning
│
├── Supervised Learning
│   ├── Classification
│   └── Regression
│
├── Unsupervised Learning
│   └── Clustering & Pattern Discovery
│
└── Reinforcement Learning
    └── Reward-Based Learning
```