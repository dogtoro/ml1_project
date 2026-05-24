# Project: Used Car Price Prediction
**Course:** Machine Learning 1 (ML1)

##  Project Overview
This project builds a Machine Learning model to predict the selling price of used cars based on the `used_cars.csv` dataset. The core algorithm utilized is the **Random Forest Regressor**. Throughout this project, the standard Data Science workflow (CRISP-DM) was applied, from cleaning raw data and feature extraction to training and evaluating the model.




Detailed Workflow Explanation (Cell by Cell)


## Cell 1: Exploratory Data Analysis (EDA) & Data Cleaning
Objective: Convert raw data into a standard format and handle Missing Values.
Steps taken:

Removed special characters ($, ,,  mi.) from the price and milage columns, then cast the data type to float for mathematical computations.

Handling Missing Values:

fuel_type: Filled with the most frequent value (Mode).

accident: Filled with 'None reported' (Assuming no accident if it was not recorded).

clean_title: Filled with 'Yes' (The vast majority of cars have clean titles).

Visualization (EDA): Plotted the initial distribution of car prices and a scatter plot showing the relationship between Mileage and Price.


## Cell 2: Feature Engineering & Outliers
Objective: Create more meaningful variables and remove noise to help the model learn more effectively.

Steps taken:

Handling Outliers: Kept only cars priced between $1,000 and $100,000. This helps the model focus on making accurate predictions for the mainstream and mid-range segments, removing supercars or incorrectly priced entries that could distort the model.

Feature Engineering: Created a car_age column by subtracting the model_year from the current year (2024). This variable reflects actual wear and tear much better than a plain production year. The model_year column was subsequently dropped.

Data Encoding (Label Encoding): Converted all categorical columns (text data such as brand, fuel_type, transmission, etc.) into numerical values using Scikit-learn's LabelEncoder.


## Cell 3: Modeling & Evaluation
Objective: Discover the underlying patterns between the input features and car prices.
Steps taken:

Separated the dataset into independent variables X (features) and the dependent variable y (target: price).
Split the dataset into two parts: 80% for training (Train) and 20% for testing (Test).
Trained the Random Forest Regressor algorithm using 100 decision trees (n_estimators=100).
Result: The model achieved an R-squared ($R^2$) score of approximately 0.83, meaning it can explain over 83% of the variance in car prices. The Mean Absolute Error (MAE) is roughly $6,200.
##  Algorithm Model: Random Forest Regressor

The project utilizes the **Random Forest Regressor** algorithm. This is an Ensemble Learning algorithm that provides high accuracy and is exceptionally well-suited for tabular data.

### 1. How It Works
Instead of building a single mathematical equation, Random Forest creates a "committee" consisting of hundreds of smaller models called **Decision Trees**:
*   **Building the "Forest":** The model in this project is configured with 100 decision trees (`n_estimators=100`).
*   **Randomness to Prevent Overfitting:** Each tree is trained on a random subset of the data (Bootstrapping) and is only allowed to consider a random subset of features when making a splitting decision. This forces the trees to learn independent patterns, avoiding rote memorization (overfitting).
*   **Aggregation:** When a new car's data is inputted, all 100 trees make their individual predictions. The final price returned by the model is the **average (mean)** of the predictions from all 100 trees.

### 2. Why Choose Random Forest?
*   **No Scaling Needed:** Columns like `milage` (reaching hundreds of thousands) and `car_age` (ranging from 1 to 20) have a massive difference in scale. Random Forest splits data based on conditional branching (e.g., `milage` > 50,000), so it is completely unaffected by the magnitude of the variables.
*   **Handles Non-linear Relationships Well:** The depreciation of a car's value is not a steady straight line over the years. Random Forest is highly capable of capturing sharp price drops in the early years or price stabilization in later years.
*   **Provides Feature Importance:** The algorithm automatically calculates the weight of each feature (e.g., pointing out that `milage` accounts for over 40% of the importance in pricing a car). This makes explaining the model to end-users (Explainable AI) transparent and straightforward.


## Cell 4: Visualizing Results
Objective: Visually illustrate the model's performance and extract actionable insights.
Steps taken:

Chart 1 (Actual vs. Predicted): Demonstrates that the model performs extremely accurately in the sub-$40,000 segment (where data points closely hug the ideal diagonal line). The deviation gradually increases for luxury vehicles over $60,000.

Chart 2 (Feature Importance): Extracts and ranks the importance level of each feature. The results clearly indicate that Mileage is the most significant determinant of a used car's price, followed by the Engine type and Car age.