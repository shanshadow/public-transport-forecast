# Technical Report: Forecasting Algorithm Selection

## 1. Selected Algorithm: LightGBM (Light Gradient Boosting Machine)

For the task of forecasting five separate transport services, we have chosen to train five independent **LightGBM Regressor** models.

LightGBM is a high-performance gradient boosting framework based on decision trees. It is an ensemble method, meaning it builds multiple models (decision trees) in a sequential, stage-wise fashion. Each new tree corrects the errors of the previous ones, "boosting" the overall accuracy.

## 2. Rationale for Choosing LightGBM

This problem is a multivariate time series forecast. While classical models like SARIMA or Prophet are options, a tree-based machine learning model like LightGBM was chosen for several key reasons:

* **Feature-Rich Modeling:** This approach allows us to "transform" the time series problem into a regression problem. We can engineer a rich set of features (like day of week, month, holidays, lag values) that a traditional model would struggle with.
* **Handling Non-Linearity:** LightGBM excels at capturing complex, non-linear relationships. For example, it can learn that the `is_school_holiday` feature has zero impact on `Light Rail` but a 100% impact on the `School` service.
* **Performance:** It is exceptionally fast to train and efficient at prediction, even with hundreds of thousands of data points and many features.
* **Robustness:** It is less prone to the influence of outliers compared to linear models and naturally handles the "on/off" nature of the `School` and `Peak Service` data.

## 3. Modeling Strategy: Five Independent Models

Instead of a single complex model that tries to predict all five targets at once (a multi-output model), we adopted a simpler and often more effective strategy:

**Train 5 separate `LGBMRegressor` models.**
1.  Model 1: (Features) -> Predicts `Local Route`
2.  Model 2: (Features) -> Predicts `Light Rail`
3.  Model 3: (Features) -> Predicts `Peak Service`
4.  Model 4: (Features) -> Predicts `Rapid Route`
5.  Model 5: (Features) -> Predicts `School`

This allows each model to optimize its internal parameters and feature importance specifically for the unique patterns of its target variable.

## 4. Key Model Parameters

The following are the most critical hyperparameters for a LightGBM model. These would be tuned using a validation set (e.g., via `GridSearchCV`) to find the optimal combination.

* `objective = 'regression_l2'`: Specifies the loss function. 'L2' (or MSE) is used to optimize for Root Mean Squared Error (RMSE).
* `n_estimators`: The number of decision trees to build. A higher number generally improves accuracy but can lead to overfitting.
* `learning_rate`: A small value (e.g., 0.05) that controls how much each tree "corrects" the previous one. It's a trade-off with `n_estimators`.
* `max_depth`: The maximum depth of any individual tree. This is a key parameter to control model complexity and prevent overfitting.
* `num_leaves`: The total number of leaves in a single tree. This is the main complexity-control parameter in LightGBM.
* `colsample_bytree`: The fraction of features (columns) to be randomly selected for building each tree, which helps prevent overfitting.