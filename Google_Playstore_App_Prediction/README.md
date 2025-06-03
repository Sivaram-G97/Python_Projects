# Google Play Store Apps Prediction & Analysis

## Overview

This project focuses on analyzing a dataset of Google Play Store applications with the goal of understanding key factors influencing app success and potentially building predictive models. The dataset contains a rich set of features for various apps, allowing for comprehensive data exploration, statistical analysis, and machine learning tasks.

The primary objectives of this project include:

* **Exploratory Data Analysis (EDA):** Gain insights into the distribution and relationships between different app attributes.
* **Data Cleaning and Preprocessing:** Handle missing values, inconsistent data formats, and categorical variable encoding.
* **Predictive Modeling:** Develop models to predict key metrics such as `Rating`, `Installs`, or other relevant success indicators.
* **Insights and Recommendations:** Derive actionable insights for app developers, marketers, or investors based on the analysis.

## Dataset

This dataset consists of around **10k records** sourced from the Google Play Store. It was originally **scraped from the Google Play Store using Python** and includes data up to **August 2018**.

The dataset includes the following columns:

* **`App`**: Name of the application
* **`Category`**: Category the app belongs to (e.g., 'ART_AND_DESIGN', 'GAME', 'FAMILY')
* **`Rating`**: Overall user rating of the app
* **`Reviews`**: Number of user reviews for the app
* **`Size`**: Size of the app
* **`Installs`**: Number of user downloads/installs for the app
* **`Type`**: Paid or Free
* **`Price`**: Price of the app
* **`Content Rating`**: Age group the app is targeted at - Children / Mature 21+ / Adult
* **`Genres`**: An app can belong to multiple genres (apart from its main category). For example, a musical family game will belong to Music, Game, Family genres.
* **`Last Updated`**: Date when the app was last updated on Play Store
* **`Current Ver`**: Current version of the app available on Play Store
* **`Android Ver`**: Minimum required Android version

The dataset contains missing values, inconsistent formats, and needs careful preprocessing before analysis and modeling.

## Project Structure

This repository typically contains the following files:

* **`google_apps_prediction.ipynb`:** The Jupyter Notebook containing all the Python code for data loading, preprocessing, EDA, modeling, and visualization.
* **`README.md`:** This file, providing an overview of the project.
* **`data`:** A directory to store the raw dataset.

## Libraries Used

The following Python libraries are expected to be used in this analysis:

* **pandas:** For data manipulation and analysis.
* **numpy:** For numerical computations.
* **matplotlib:** For basic plotting and visualization.
* **seaborn:** For enhanced statistical visualizations.
* **scikit-learn (sklearn):** For machine learning tasks (e.g., data preprocessing, model selection, model training, evaluation).
       * **`train_test_split`:** For splitting datasets into training and testing sets to evaluate model performance effectively.
       * **`LinearRegression`**: For implementing linear regression models, a fundamental algorithm for predicting continuous target variables.
       * **`mean_squared_error`:** For evaluating the performance of regression models by calculating the average of the squared differences between predicted and actual values.
       * **`r2_score`:** For evaluating regression model performance, representing the proportion of the variance in the dependent variable that is predictable from the independent variables.
* **joblib:** For efficiently saving and loading Python objects, especially large NumPy arrays or scikit-learn models. This is crucial for persisting trained models so they can be reused without retraining.

## Analysis and Prediction Plan

The project will follow these general steps:

### 1. Data Loading and Initial Inspection

* Load the data file using pandas.
* Check for null values in the data.
* Get the number of null values for each column.

### 2. Data Cleaning and Preprocessing

* **Deduplication:** Duplicate app entries were strategically handled by prioritizing non-null ratings and concrete size values over missing or varying entries.
* **Handling Null Values:** Drop records with nulls in any of the columns.
* Standardizing Datatypes: Variables seem to have incorrect type and inconsistent formatting. We need to fix them:
    * **`Size` column:** Has sizes in Kb as well as Mb. To analyze, we'll need to convert these to numeric.
        * Extract the numeric value from the column.
        * Multiply the value by 1,000, if size is mentioned in Mb.
    * **`Reviews` field:** Is a numeric field that is loaded as a string field. Convert it to numeric (int/float).
    * **`Installs` field:** Is currently stored as a string and has values like `1,000,000+`.
        * Treat `1,000,000+` as `1,000,000`.
        * Remove `'+'` and `','` from the field, convert it to integer.
    * **`Price` field:** Is a string and has `'$'` symbol. Remove `'$'` sign, and convert it to numeric.

### 3. Sanity Checks

* **`Rating`**: Average rating should be between 1 and 5 as only these values are allowed on the Play Store. Drop the rows that have a value outside this range.
* **`Reviews`**: Should not be more than `Installs` as only those who installed can review the app. If there are any such records, drop them.
* **For free apps (type = "Free")**, the price should not be > 0. Drop any such rows.

### 4. Univariate Analysis & Outlier Treatment

* **Boxplot for Price**: Most of the apps in playstore are free. But if we think about the Price of usual apps on Playstore, $200 Seems to be a very high number so we will drop the columns above it.
  
* **Boxplot for Reviews**: While most apps receive very few reviews, a small fraction of highly popular apps accumulate an enormous number of reviews. To prevent outliers from skewing the model, app entries with less than 1,000 reviews or more than 2,000,000 reviews are removed.
  
* **Histogram for Installs**: Similar to the 'Reviews' analysis, most apps achieve very limited install numbers, while a small number of highly successful apps account for a massive number of installs. Some apps have very high number of Installs and very low number of install, these apps will skew our model. We have dropped the coulumns with more than 5,00,00,000 Installs and less than 1,000 Intalls.
  
* **Histogram for Rating**: Apps in this dataset generally receive positive feedback from users, with a strong preference for ratings above 4.0. Apps with lower ratings are much less common.
  
* **Histogram for Size**: App market is dominated by smaller-sized applications, suggesting either a preference for lighter apps, design considerations for efficient downloads/storage, or perhaps a larger volume of simpler utility apps. Larger apps exist but are far less common.

### 5. Bivariate Analysis

Let's look at how the available predictors relate to the variable of interest, i.e., our target variable `Rating`. Make Joint plots (for numeric features) and box plots (for character features) to assess the relations between rating and the other features.

* **Jointplot for Rating vs. Price** : A heavy concentration of points around Price = 0 indicates that most apps are free or very cheap. Free apps and paid apps can both have high or low ratings. There's no clear trend where increasing price leads to higher/lower ratings.
  
* **Jointplot for Rating vs. Size**: Most apps are clustered toward the lower size range, particularly below 20,000 KB (~20 MB). The scatterplot is widely spread out with no clear upward or downward trend. This suggests that app size doesn't significantly influence user rating.

* **Jointplot for Rating vs. Reviews**: Apps with ratings between 4.0 and 5.0 are found throughout the x-axis (number of reviews). This indicates that good ratings are not limited to high-review-count apps. Also, popular apps are generally well-rated, or that poorly rated apps don’t get many users/reviews.
  
* **Boxplot for Rating vs. Content Rating**: 'Content Rating' appears to be an informative feature for predicting 'Rating'. The distinct distributions, especially for 'Adults only 18+', suggest that this categorical variable provides valuable signals.
  
* **Boxplot for Ratings vs. Category**: Across almost all categories, median ratings are above 4.0, indicating that users generally rate apps highly. This suggests user satisfaction is relatively strong in general on the Play Store. Some categories consistently perform better, while others are more inconsistent.
  
* **Boxplot for Ratings vs. Genres**: 'Genres' are likely to be an important feature for predicting 'Rating'. The distinct rating distributions across genres suggest that knowing an app's genre provides strong clues about its typical rating.

### 6. Data Preprocessing

For the steps below, we create a copy of the dataframe to make all the edits. Name it **`inp1`**.

* `Reviews` and `Installs` have some values that are still relatively very high. Before building a linear regression model, we need to reduce the skew. Apply log transformation (`np.log1p`) to `Reviews` and `Installs`.
* Drop columns `App`, `Size`, `Type`, `Price`, `Last Updated`, `Current Ver`, and `Android Ver`. These variables are not useful for our task.
* Get dummy columns for `Category`, `Genres`, and `Content Rating`. This needs to be done as the models do not understand categorical data, and all data should be numeric. Dummy encoding is one way to convert character fields to numeric. Name of dataframe should be **`inp2`**.

### 7. Model Building

* Apply 70-30 split. Name the new dataframes `df_train` and `df_test`.
* Separate the dataframes into `X_train`, `y_train`, `X_test`, and `y_test`.
* We use linear regression as the technique.
   
### 8. Model Persistance

The final trained Linear Regression model is saved to linear_model.sav using the joblib library. This allows for the model to be efficiently loaded and reused for future predictions without the need for retraining, ensuring convenience and consistency.

## How to Run the Code

1.  **Clone the repository:**
    ```bash
    git clone https://github.com/Sivaram-G97/Python_Projects.git
    cd Python_Projects/Google_Playstore_App_Prediction
    ```
2.  **Place the dataset:** Google_Playstore_App_Prediction/googleplaystore.csv
3.  **Install the required libraries:**
    ```bash
    pip install -r requirements.txt
    ```
4.  **Run the Jupyter Notebook:**
    ```bash
    jupyter notebook Google_Playstore_App_Prediction/App_Rating_Prediction.ipynb
    ```
    This will open the notebook in your web browser, and you can then execute the cells.

## Citation

If you use this dataset anywhere in your work, kindly cite as below:
L. Gupta, "Google Play Store Apps," Feb 2019. [Online]. Available: https://www.kaggle.com/lava18/google-play-store-apps

---
