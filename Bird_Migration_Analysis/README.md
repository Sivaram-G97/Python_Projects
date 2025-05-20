# Bird Migration Data Analysis

## Overview

This project analyzes a synthetic dataset simulating the migratory behavior of various bird species across global regions. The dataset contains 10,000 records, each representing a single bird tagged with a tracking device. It includes detailed information such as flight distance, speed, altitude, weather conditions, tagging information, and migration outcomes (success, interruption, nesting success).

The data was entirely synthetically generated using randomized yet realistic values based on known ranges from ornithological studies. This makes it ideal for practicing data analysis and visualization techniques without privacy concerns or real-world data access restrictions.

This project explores several aspects of the bird migration data, aiming to answer questions such as:

* Do Certain species migrate in larger flocks?
* How does weather impact nesting success?
* What conditions lead to migration interruptions?
* What are the geospatial patterns of migration start and end locations?


## Dataset

The dataset used in this project is a synthetic bird migration dataset containing 10,000 records and over 40 columns. Key features include:

* **Migration Information:** Start and end latitudes and longitudes, flight distance, speed, altitude, migration start month, migration success.
* **Weather Conditions:** Temperature, wind speed, humidity, pressure, visibility, weather condition (categorical).
* **Tagging Information:** Tag ID, tag battery level.
* **Nesting Information:** Nesting success.
* **Interruption Information:** Migration interrupted (yes/no), interrupted reason.
* **Bird Characteristics:** Species.

## Project Structure

This repository contains the following files:

* **`bird_migration_analysis.ipynb`:** The Jupyter Notebook containing the Python code for data analysis, visualization, and modeling.
* **`README.md`:** This file, providing an overview of the project.
* **`data/`:** A directory to store the dataset
  
## Libraries Used

The following Python libraries were used in this analysis:

* **pandas:** For data manipulation and analysis.
* **numpy:** For numerical computations.
* **matplotlib:** For basic plotting and visualization.
* **seaborn:** For enhanced statistical visualizations.
* **geopandas:** For geospatial data analysis and mapping.
* **shapely:** For creating geometric objects.
* **scipy.stats:** For statistical tests (e.g., t-tests, ANOVA, Chi-squared).
* **statsmodels:** For statistical modeling (e.g., ANOVA, Multinomial Logistic Regression).
* **scikit-learn (sklearn):** For machine learning tasks (e.g., clustering, preprocessing).

## Key Findings (Summary of your project's results)

* **Birds which migrate in large flocks:** Geese display a clear peak in the larger flock size range (around 350-500), suggesting that when Geese migrate in flocks, they typically form larger groups compared to most other species in this selection.
  
* **Weather and Nesting Success:**
  1. The **Chi-squared test** between 'Weather_Condition' and 'Nesting_Success' yielded a p-value of 0.2545. This p-value is greater than the common significance level of 0.05. Therefore, we fail to reject the null hypothesis. This indicates that there is no statistically significant association between the categorical weather condition (Clear, Foggy, Rainy, Stormy, Windy) and nesting success in the dataset. The observed distribution of successful and unsuccessful nests across the different weather conditions is not significantly different from what we would expect if there were no relationship between them.
  2. **Based on these ANOVA results**, for each of the numerical weather variables tested individually, there is no statistically significant evidence to suggest that the mean weather condition was different for successful nesting events compared to unsuccessful nesting events in the dataset. As the p-value is higher than 0.05. There is no strong statistical evidence to conclude that weather, as represented by these variables, has a significant impact on nesting success.
     
* **Migration Interruptions:** The primary conditions leading to recorded migration interruptions appear to be adverse weather (storms), physical injury to the birds, and encounters with predators. Loss of tracking signal is also noted as a reason, but less frequently than the other three.
  
* **Geospatial Mapping:** Geospatial mapping of start and end locations revealed the general migratory patterns across different latitudes and longitudes.

## Credits

This project utilizes a synthetic dataset designed for educational and analytical purposes.

---
