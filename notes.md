- RegressionPreset   #Create a regression performance report using Evidently's pre-built template
- TargetDriftPreset  #Check if the actual bike demand patterns changed
- DataDriftPreset    #Data drift detects when the input features (temperature, humidity, etc.) are different from what our model learned during training. Exmp: This checks if the weather patterns in February are different from January

- Numerical features (e.g., temperature, humidity) contain continuous values that help the model learn quantitative relationships.
- Categorical features (e.g., season, holiday) represent groups or categories that influence the target but are not measured on a numeric scale.

- Linear Regression (Predict a number): Use when your target variable is continuous (e.g., predicting bike count, price, temperature).
- Logistic Regression (Predict a category; especially two classes): Use when your target variable is categorical/binary (e.g., yes/no, 0/1, spam/not spam).

- We use RandomForestRegressor to predict a number (bike rentals per hour). Random Forest works well when:
    - Data has both numerical and categorical features
    - Relationships are complex or non-linear
    - You want to reduce overfitting compared to a single decision tree

- Using DataDriftPreset, we get 57% drift. 57% drift is bad (lower, at least <20% is better. <10% is stable and idea) for the model—especially since the drifted columns (temp, atemp, hum, windspeed) are key features for predicting bike demand. Investigate 
    - Drift: Check if the drift is due to seasonal changes, unusual weather, or data quality issues.
    - Retrain the Model: 
        - Update the training data to include more recent samples (e.g., add February data). 
        - Retrain the Random Forest model on the new data so it learns the updated relationships.
    - Monitor Performance: 
        - Use Evidently reports to compare new predictions and error metrics (MAE, RMSE, MAPE) after retraining. 
        - Continue weekly monitoring for further drift.
    - Consider Feature Engineering: If drift persists, review feature selection or transformation (e.g., normalize, add new features).


- Check for model performance metrics (MAE, RMSE, MAPE). Even with some drift, if your error metrics are low, the model may still be good.

- In this notebook, we used time-based splitting rather random split (train/va/test). Becuase our problem is time series regression, predicting future bike demand based on past data. We use random split with mixes data from all time periods, fine for static datasets, but not for time-dependent problems. In time series, you must respect the chronological order: train on the past, test on the future. Otherwise, you risk "leakage" (using future info to predict the past).

- In random split, Validation data helps you tune model hyperparameters (e.g., number of trees, max depth) without touching the test set.

- In time series, we add new data but for random splitting (not time based) problem, we Collect new data (e.g., new samples from production or recent months), combine the new data with your old data (or just use the new data if you want the model to focus on recent patterns), randomly split again into train, validation, and test sets and finally retrain your model on the new train set, tune on validation, and evaluate on the new test set
So : Time-based: Add new time periods, train on past, test on future. Random split: Add new data, randomly split all, retrain and test.

- Feedback loop: means using model predictions and their real-world outcomes to improve the model automatically (e.g., online learning, active learning).

- Data drift: The distribution of the input data changes (e.g., temperature values are higher).

- Concept drift means the relationship between your input features and the target variable (cnt, bike rentals) changes over time. e.g., temperature used to increase rentals, now it decreases them
    - detect concept drift:
        - Monitor model error metrics (MAE, RMSE, MAPE) over time.
        - Use tools like Evidently’s TargetDriftPreset to check if the relationship between features and target has changed.
        - Compare feature-target correlations between reference and current data.