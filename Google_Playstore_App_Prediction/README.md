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
* **`data/`:** A directory to store the raw dataset.

## Libraries Used

The following Python libraries are expected to be used in this analysis:

* **pandas:** For data manipulation and analysis.
* **numpy:** For numerical computations.
* **matplotlib:** For basic plotting and visualization.
* **seaborn:** For enhanced statistical visualizations.
* **scikit-learn (sklearn):** For machine learning tasks (e.g., data preprocessing, model selection, model training, evaluation).

## Analysis and Prediction Plan

The project will follow these general steps:

### 1. Data Loading and Initial Inspection

* Load the data file using pandas.
* Check for null values in the data.
* Get the number of null values for each column.

### 2. Data Cleaning and Formatting

* Drop records with nulls in any of the columns.
* Variables seem to have incorrect type and inconsistent formatting. We need to fix them:
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

### 4. Univariate Analysis

* **Boxplot for Price**:
* **Boxplot for Reviews**:
* **Histogram for Rating**:
* **Histogram for Size**:

### 5. Outlier Treatment

* **Price**: From the box plot, it seems there are some apps with very high prices. A price of $200 for an application on the Play Store is very high and suspicious!
    * Check out the records with very high price.
    * Is 200 indeed a high price?
    * Drop these as most seem to be junk apps.
* **Reviews**: Very few apps have very high numbers of reviews. These are all star apps that don't help with the analysis and, in fact, will skew it. Drop records having more than 2 million reviews.
[OBSERVATIONS & FINDINGS]

### 6. Bivariate Analysis

Let's look at how the available predictors relate to the variable of interest, i.e., our target variable `Rating`. Make scatter plots (for numeric features) and box plots (for character features) to assess the relations between rating and the other features.

* **Make scatter plot/jointplot for Rating vs. Price**
* **Make scatter plot/jointplot for Rating vs. Size**
* **Make scatter plot/jointplot for Rating vs. Reviews**
* **Make boxplot for Rating vs. Content Rating**
* **Make boxplot for Ratings vs. Category**

[OBSERVATIONS & FINDINGS]

### 7. Data Preprocessing

For the steps below, we create a copy of the dataframe to make all the edits. Name it `inp1`.

* `Reviews` and `Installs` have some values that are still relatively very high. Before building a linear regression model, we need to reduce the skew. Apply log transformation (`np.log1p`) to `Reviews` and `Installs`.
* Drop columns `App`, `Last Updated`, `Current Ver`, and `Android Ver`. These variables are not useful for our task.
* Get dummy columns for `Category`, `Genres`, and `Content Rating`. This needs to be done as the models do not understand categorical data, and all data should be numeric. Dummy encoding is one way to convert character fields to numeric. Name of dataframe should be `inp2`.

### 8. Train Test Split

* Apply 70-30 split. Name the new dataframes `df_train` and `df_test`.

### 9. Separate Dataframes

* Separate the dataframes into `X_train`, `y_train`, `X_test`, and `y_test`.

### 10. Model Building

* We use linear regression as the technique.
   
### 11. Conclusion and Insights

[OBSERVATIONS & FINDINGS]

## How to Run the Code

1.  **Clone the repository:**
    ```bash
    git clone [your_repository_url]
    cd google_apps_prediction
    ```
2.  **Place the dataset:** Download the Google Play Store dataset (e.g., `googleplaystore.csv`) and place it in the `data/` directory within this repository.
3.  **Install the required libraries:**
    ```bash
    pip install -r requirements.txt
    ```
    (You will create this `requirements.txt` file by listing all libraries you use in your notebook.)
4.  **Run the Jupyter Notebook:**
    ```bash
    jupyter notebook google_apps_prediction.ipynb
    ```
    This will open the notebook in your web browser, and you can then execute the cells.

## Potential Further Work

* Explore more advanced natural language processing (NLP) techniques on the `App` name or `Genres` for more nuanced feature extraction.
* Implement time series analysis if daily/weekly update data becomes available.
* Deploy the best-performing model as a web application.
* Conduct A/B testing simulations based on predictions.

## Citation

If you use this dataset anywhere in your work, kindly cite as below:
L. Gupta, "Google Play Store Apps," Feb 2019. [Online]. Available: https://www.kaggle.com/lava18/google-play-store-apps

---
