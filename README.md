In this notebook, we are predicting the hourly demand for bike rentals—specifically, the number of bikes rented each hour (the "cnt" column in the dataset). For example: How many bikes will be rented at 8 AM on a rainy Monday in February? How many bikes will be rented at 5 PM on a sunny summer holiday?

The problem being solved is a regression task: building a machine learning model to forecast bike rental demand based on features like weather, time, and calendar information. The goal is to monitor the model's performance over time and detect any changes (drift) in the data or target patterns that could affect prediction accuracy.

The Bike Sharing Dataset (from the UCI Machine Learning Repository) contains detailed records of a bike rental service in a city, with data collected hourly. Each row tells you how many bikes were rented in a given hour, along with information like the date, time, weather, temperature, humidity, whether it was a holiday, and more.

We check accuracy by:<br>
- Training the model on historical data (where we know the actual number of rentals).
- Testing the model on new data (where we also know the actual rentals, but the model hasn't seen this data before).
- Comparing predictions to real values using metrics like MAE (Mean Absolute Error), RMSE, and visualizations (e.g., predicted vs. actual plots).
- Monitoring for drift to catch when the model starts making worse predictions because the world has changed.

In summary, check model's accuracy by comparing its predictions to real historical data. Exm: "How many bikes will be rented on a hot summer afternoon?"

## Step-by-step: 
### 1. Loading the Data
We load a file (hour.csv) that contains hourly records of bike rentals, weather, and time info.

This data is the foundation for teaching our model how different factors (like weather, time, holidays) affect bike rentals.

### 2. Preparing the Data
We combine date and hour columns to create a proper timestamp for each row.

This makes it easier to analyze and split the data by time periods (like weeks or months).

### 3. Defining the Problem
We decide the “target” (cnt column) is what We want to predict: the total number of bikes rented each hour.

This is the main business question—how many bikes will be needed at any given hour?

### 4. Splitting the Data
We split the data into “reference” (January) and “current” (February) periods.

We train our model on January (so it learns patterns), then test it on February (to see if it can predict new, unseen data).

### 5. Training the Model
We use a Random Forest model (works well with tabular data like ours. It can handle both numerical and categorical features) to learn the relationship between features (weather, time, etc.) and bike rentals.

This model can make predictions about future bike demand, helping the service plan ahead.

### 6. Making Predictions
We use the trained model to predict bike rentals for both the training period and the new period.

This lets We compare the model’s predictions to the actual numbers and see how well it’s doing.

### 7. Monitoring Model Performance
We use the Evidently library to create dashboards and reports:<br>
Regression Performance: How accurate are the predictions?<br>
Target Drift: Are the actual rental numbers changing in unexpected ways?<br>
Data Drift: Are the input features (like weather) changing compared to what the model saw during training?<br>

Even a good model can become outdated if the world changes (e.g., sudden weather shifts, new holidays, or changes in user behavior). Monitoring helps We catch problems early, so We can retrain or adjust our model before it starts making bad predictions.

### 8. Weekly Monitoring
We check the model’s performance and for drift every week in February.

This is like regular health checkups for our model—making sure it’s still reliable as time goes on.

### In summary
- We build a prediction model for bike rentals.
- We set up automated monitoring to catch problems early.
- We use this to ensure our predictions stay accurate, even as real-world conditions change.
- This approach is essential for any real-world AI system—not just building it, but also making sure it keeps working well over time.