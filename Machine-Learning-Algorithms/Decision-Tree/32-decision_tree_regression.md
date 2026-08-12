# Decision Tree Regression

## Definition

* Decision Tree Regression is a **supervised machine learning algorithm**.
* It is used for predicting **continuous numerical values**.
* It works by dividing data into smaller groups using **if-else decision rules**.
* The final prediction is made at a **leaf node**.

## Simple Example

* Suppose we want to predict house prices.
* The tree may ask:

  * Is house area ≤ 1000 sq ft?
  * Is number of bedrooms ≤ 3?
  * Is location score > 7?
* After a series of decisions, the model reaches a leaf.
* The leaf gives the predicted house price.

## How Decision Tree Regression Works

* Start with the complete training dataset.
* Find the best feature and threshold for splitting the data.
* Divide the data into smaller groups.
* Continue splitting the groups.
* Stop when a stopping condition is reached.
* Each final group becomes a **leaf**.
* The leaf provides the prediction for new observations.

## Decision Tree Structure

* **Root Node**

  * The first and top-most decision in the tree.
  * It represents the first split.

* **Internal Node**

  * A decision point inside the tree.
  * It contains another condition or split.

* **Branch**

  * Connects nodes.
  * Represents the result of a decision.

* **Leaf Node**

  * The final node.
  * Contains the predicted numerical value.

## Splitting

* Splitting means dividing the dataset into smaller groups.
* The algorithm searches for the split that gives the lowest prediction error.
* Regression trees commonly use **Mean Squared Error (MSE)** to evaluate split quality.
* A good split creates groups where target values are more similar.

## Mean Squared Error (MSE)

* MSE measures the average squared difference between actual and predicted values.
* Lower MSE generally means lower prediction error.
* Squaring the errors gives larger errors more importance.

## Leaf Prediction

* In standard Decision Tree Regression, a leaf usually predicts the **mean target value** of the training samples that reach that leaf.
* Example:

  * Target values in a leaf: 50, 60, 70
  * Prediction = 60
* Therefore, different leaves can have different predicted values.

## Important Hyperparameters

### max_depth

* Controls the maximum depth of the tree.
* Small value:

  * Simpler tree.
  * Lower risk of overfitting.
  * Can cause underfitting.
* Large value:

  * More complex tree.
  * Can capture more patterns.
  * Higher risk of overfitting.

### min_samples_split

* Specifies the minimum number of samples required to split an internal node.
* Higher value makes the tree simpler.
* Helps control overfitting.

### min_samples_leaf

* Specifies the minimum number of samples required in a leaf.
* Higher values create larger leaves.
* Can make predictions more stable.
* Helps reduce overfitting.

### max_features

* Controls how many features are considered when searching for a split.
* Can help control model complexity.

### criterion

* Determines how the quality of a split is measured.
* Common regression criteria include:

  * Squared Error
  * Absolute Error
  * Friedman MSE
  * Poisson

## Overfitting

* Decision Trees can easily overfit if they become too deep.
* An overfitted tree learns noise and very specific patterns from the training data.
* It may perform extremely well on training data but poorly on unseen data.

### Signs of Overfitting

* Very good training performance.
* Much worse testing performance.
* Tree has excessive depth or too many branches.

### Controlling Overfitting

* Reduce `max_depth`.
* Increase `min_samples_split`.
* Increase `min_samples_leaf`.
* Limit the number of features considered.
* Use pruning techniques.
* Evaluate performance on unseen testing data.

## Underfitting

* Underfitting happens when the tree is too simple.
* The model fails to learn important patterns.
* Both training and testing performance may be poor.

## Feature Scaling

* Decision Tree Regression generally **does not require feature scaling**.
* Standardization or normalization is usually unnecessary.
* This is because trees make decisions using feature thresholds rather than distances or feature magnitudes.
* Example:

  * Age: 18–80
  * Salary: 20,000–500,000
* A Decision Tree can still split these features without scaling.

## Decision Tree Regression vs Linear Regression

* **Decision Tree Regression**

  * Uses decision rules.
  * Can capture non-linear relationships.
  * Produces step-like predictions.
  * Usually does not require scaling.
  * Can easily overfit.

* **Linear Regression**

  * Models a linear relationship.
  * Produces a continuous linear prediction.
  * Can struggle with complex non-linear patterns.
  * Feature scaling may be useful depending on the workflow.

## Advantages

* Easy to understand.
* Easy to visualize.
* Can model non-linear relationships.
* Usually does not require feature scaling.
* Can capture complex relationships.
* Uses simple decision rules.
* Can work with different feature scales.

## Disadvantages

* Can easily overfit.
* Small changes in data can produce a different tree.
* Deep trees can become complex.
* Predictions are usually step-like rather than smooth.
* A single tree may generalize worse than ensemble models.
* Can be sensitive to noisy data.

## Model Evaluation Metrics

### MAE

* Mean Absolute Error.
* Measures the average absolute difference between actual and predicted values.
* Easy to interpret.
* Lower MAE is better.

### MSE

* Mean Squared Error.
* Measures the average squared prediction error.
* Gives more importance to large errors.
* Lower MSE is better.

### RMSE

* Root Mean Squared Error.
* Square root of MSE.
* Expressed in the same units as the target.
* Lower RMSE is better.

### R² Score

* Measures how well the model explains variation in the target.
* A value closer to 1 generally indicates better performance.
* A value around 0 means the model explains little of the variation compared with a mean baseline.
* A negative value can mean the model performs worse than that baseline.

## Decision Tree Regression Workflow

* Load the dataset.
* Understand the data.
* Perform required preprocessing.
* Select features and target.
* Split data into training and testing sets.
* Create the Decision Tree Regressor.
* Train the model.
* Make predictions.
* Evaluate the model.
* Tune hyperparameters.
* Compare training and testing performance.
* Select the final model.

## Key Points

* Decision Tree Regression is a **supervised learning algorithm**.
* It predicts **continuous numerical values**.
* It uses **if-else decision rules**.
* Data is divided using feature thresholds.
* The final groups are called **leaf nodes**.
* Leaf predictions are generally based on the mean target value.
* MSE is commonly used to evaluate regression splits.
* `max_depth` controls tree depth.
* `min_samples_split` controls when splitting can occur.
* `min_samples_leaf` controls the minimum samples in a leaf.
* Feature scaling is generally not required.
* Deep trees can cause overfitting.
* MAE, MSE, RMSE, and R² are common evaluation metrics.
* Hyperparameter tuning is important for better generalization.
* Decision Trees can capture non-linear relationships.

