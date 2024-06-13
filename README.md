# Statistical Analysis of Breakfast Recipes 

This is a data science project for DSC 80 at UCSD. 
Author: Yujie Kang

## Introduction

### General
This project focused on the analysis of breakfast recipes, aiming to uncover distinctive features and patterns that differentiate them from other types of recipes. Breakfast is often regarded as the most important meal of the day, providing essential nutrients and energy required to start the day efficiently. Understanding what makes a breakfast recipe unique can provide insights into dietary habits and potentially guide healthier eating choices. So I'm wondering if **breakfast recipes are different from other recipes in the nutrition facts and time complexity**. The project aims to offer valuable insights into the composition and uniqueness of breakfast recipes, contributing to broader nutritional studies and practical applications in meal planning and dietary recommendations.

To get the information about the recipes, I gathered two datasets `recipes` and `interactions` from [food.com](https://www.food.com/). These two datasets was originally scraped and used by the authors of [Generating Personalized Recipes from Historical User Preferences](https://cseweb.ucsd.edu/~jmcauley/pdfs/emnlp19c.pdf) paper.

### Columns
The first dataset, `recipe`, contains _83782_ rows and _10_ columns, which contains _83782_ unique recipes and _10_ different types of information about each unique recipe, the informations are as follows:

| Column          | Description                                                                                                                                                    |
| --------------- | -------------------------------------------------------------------------------------------------------------------------------------------------------------- |
| `name`          | Recipe name                                                                                                                                                    |
| `id`            | Recipe ID                                                                                                                                                      |
| `minutes`       | Minutes to prepare recipe                                                                                                                                      |
| `contributor_id`| User ID who submitted this recipe                                                                                                                               |
| `submitted`     | Date recipe was submitted                                                                                                                                      |
| `tags`          | Food.com tags for recipe                                                                                                                                       |
| `nutrition`     | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `n_steps`       | Number of steps in recipe                                                                                                                                      |
| `steps`         | Text for recipe steps, in order                                                                                                                                |
| `description`   | User-provided description                                                                                                                                      |

<br>
The second dataset, `interactions`, contains user's interaction with the recipes, including the following information:

| Column      | Description             |
| ----------- | ----------------------- |
| `user_id`   | User ID                 |
| `recipe_id` | Recipe ID               |
| `date`      | Date of interaction     |
| `rating`    | Rating given            |
| `review`    | Review text             |

## Data Cleaning and Exploratory Data Analysis

### Data Cleaning

1. The recipe and interaction datasets were left-merged to create a comprehensive dataset containing both recipe details and user interactions.
2. There were many 0s in the rating column. Since the lowest possible rating is 1, these 0 values were converted to NaN to accurately reflect missing or invalid ratings.
3. I checked the `n_steps` and `n_ingredients` columns to ensure that no steps or ingredients have a count of 0.
4. A new column `mean_rating` was created to provide a representative average rating for each recipe.
5. I examined the data types and appropriate conversions were made: the `submitted` and `date` columns were converted to datetime format. The `tags`, `nutrition`, `steps`, and `ingredients` columns were converted to lists, as they were originally stored as strings but represented list-like data. 
6. An `is_breakfast` column was added to the dataset to identify and classify breakfast recipes.
7. The values in the `nutrition` column were split into separate columns for easier analysis: `calories`, `total_fat`, `sugar`, `sodium`, `protein`, `saturated_fat`, and `carbohydrates`.

The final dataframe has these columns and their types are listed at the right:

| Column          | Data Type                                    |
| --------------- | -------------------------------------------- |
| `name`          | `str`                                        |
| `minutes`       | `int64`                                      |
| `contributor_id`| `int64`                                      |
| `submitted`     | `Timestamp`                                  |
| `tags`          | `list`                                       |
| `n_steps`       | `int64`                                      |
| `steps`         | `list`                                       |
| `description`   | `str`                                        |
| `ingredients`   | `list`                                       |
| `n_ingredients` | `int64`                                      |
| `user_id`       | `float64`                                    |
| `recipe_id`     | `float64`                                    |
| `date`          | `Timestamp`                                  |
| `rating`        | `float64`                                    |
| `review`        | `str`                                        |
| `mean_rating`   | `float64`                                    |
| `is_breakfast`  | `bool`                                       |
| `calories`      | `float64`                                    |
| `total_fat`     | `float64`                                    |
| `sugar`         | `float64`                                    |
| `sodium`        | `float64`                                    |
| `protein`       | `float64`                                    |
| `saturated_fat` | `float64`                                    |
| `carbohydrates` | `float64`                                    |

The first 3 rows of the cleaned dafaframe looks like this:

| name                                | minutes | contributor_id | submitted   | ...   | sodium | protein |saturated_fat  |carbohydrates |
|-------------------------------------|---------|----------------|-------------|-------|--------|---------|---------------|---------------|
| 1 brownies in the world best ever   | 40      | 985201         | 2008-10-27  | ...   | 3.0    | 3.0     | 19.0          | 6.0           |
| 1 in canada chocolate chip cookies  | 45      | 1848091        | 2011-04-11  | ...   | 22.0   | 13.0    | 51.0          | 26.0          |
| 412 broccoli casserole              | 40      | 50969          | 2008-05-30  | ...   | 32.0   | 22.0    | 36.0          | 3.0           |

### Univariate Analysis

<iframe
  src="assets/distribution_of_rating.html"
  width="800"
  height="600"
  frameborder="0"
></iframe>

