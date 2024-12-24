# Recipe Analytics: Linking Recipe Complexity and User Ratings for Culinary Insights 

Authors: Preethi Manne & Abhi Sathvika Goriparthy

## Introduction
The dataset being analyzed contains detailed information about recipes from a popular online platform, offering a rich repository of culinary data. Each recipe is described by various attributes, including the number of steps required, the ingredients involved, the time needed to prepare, and its average rating by users. This dataset allows us to delve into user preferences and explore what factors make a recipe more appealing.

### Project Focus:
Our project focuses on answering this central question:
**"What is the relationship between the number of steps in a recipe and its rating?"**
This question is intriguing because it sheds light on how complexity in recipe preparation might influence user satisfaction. Does a simple recipe garner more favorable reviews, or do users appreciate the effort and detail of a complex recipe? Answering this can benefit chefs, culinary websites, and anyone looking to optimize recipe creation for higher ratings.

### Why It Matters
This question has practical implications for both recipe creators and consumers:
Recipe Creators: Insights from this analysis can help content creators design recipes that align with user preferences, maximizing engagement and positive feedback.
Consumers: Understanding these patterns helps readers find recipes that are more likely to meet their expectations, whether they value simplicity or complexity.

### Dataset Overview
For our analysis, we used two datasets, recipes and interactions. 

Relevant columns:
- n_steps: The number of steps required to prepare the recipe. This reflects the recipe's complexity.
- rating_x: The user rating of the recipe, a measure of its popularity and perceived quality.
- minutes: The total preparation time for the recipe, providing context to its complexity.

### Datasets Used
- Recipes: 83782 rows, 12 columns
- Interactions: 731927 rows, 5 columns

| Column             | Description                                                                                                                                                                                       |
| :----------------- | :------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------ |
| `'name'`           | Recipe name                                                                                                                                                                                       |
| `'id'`             | Recipe ID                                                                                                                                                                                         |
| `'minutes'`        | Minutes to prepare recipe                                                                                                                                                                         |
| `'contributor_id'` | User ID who submitted this recipe                                                                                                                                                                 |
| `'submitted'`      | Date recipe was submitted                                                                                                                                                                         |
| `'tags'`           | Food.com tags for recipe                                                                                                                                                                          |
| `'nutrition'`      | Nutrition information in the form [calories (#), total fat (PDV), sugar (PDV), sodium (PDV), protein (PDV), saturated fat (PDV), carbohydrates (PDV)]; PDV stands for “percentage of daily value” |
| `'n_steps'`        | Number of steps in recipe                                                                                                                                                                         |
| `'steps'`          | Text for recipe steps, in order                                                                                                                                                                   |
| `'description'`    | User-provided description                                                                                                                                                                         |
| `'ingredients'`    | Text for recipe ingredients                                                                                                                                                                       |
| `'n_ingredients'`  | Number of ingredients in recipe                                                                                                                                                                   |

         

| Column        | Description         |
| :------------ | :------------------ |
| `'user_id'`   | User ID             |
| `'recipe_id'` | Recipe ID           |
| `'date'`      | Date of interaction |
| `'rating'`    | Rating given        |
| `'review'`    | Review text         |


## Data Cleaning and Exploratory Data Analysis

We conducted the following data cleaning steps:

1. Left merge the recipes and interactions datasets on id and recipe_id.

   - The purpose of this step is to helps match the unique recipes with their rating and review. We did a left merge specifically to get all the recipes in the recipes dataset.

1. Checked data types of all the columns.

   - The purpose of this step is to help evaluate what data cleaning steps and data type conversion are needed.
   - | Column             | Description |
     | :----------------- | :---------- |
     | `'name'`           | object      |
     | `'id'`             | int64       |
     | `'minutes'`        | int64       |
     | `'contributor_id'` | int64       |
     | `'submitted'`      | object      |
     | `'tags'`           | object      |
     | `'nutrition'`      | object      |
     | `'n_steps'`        | int64       |
     | `'steps'`          | object      |
     | `'description'`    | object      |
     | `'ingredients'`    | object      |
     | `'n_ingredients'`  | int64       |
     | `'user_id'`        | float64     |
     | `'recipe_id'`      | float64     |
     | `'date'`           | object      |
     | `'rating'`         | float64     |
     | `'review'`         | object      |

1. Filled all ratings of 0 with np.nan.

   - Rating is typicaly on a scale from 1 to 5. 1 represents the lowest rating, while 5 represents the highest rating. Leaving missing ratings as is would affect the average rating column. While computing the average, the denominator would be higher, while the numerator would just add a 0, not changing it. So this would make the average smaller than it should be. Therfore, we repalce all the ratings of 0 with np.nan.

1. Added column `'average_rating'` containing average rating per recipe.

   - A recipe's overall rating is determined by averaging the ratings provided by multiple users, offering a more comprehensive evaluation of its quality.
  
1. Dropped duplicate values of the recipe name
   -  The purpose of this step is to ensure that there is only one average rating listed per recipe.
  


### Result of cleaned dataframe 

| name                                 |     id |   minutes |   contributor_id | submitted   |   n_steps |   n_ingredients |   recipe_id | date       |   avg_rating | Steps_Bin   |
|:-------------------------------------|-------:|----------:|-----------------:|:------------|----------:|----------------:|------------:|:-----------|-------------:|:------------|
| 0 carb   0 cal gummy worms           | 283618 |        45 |           134414 | 2008-02-02  |        15 |               3 |      283618 | 2008-04-25 |      4.75    | 10-20       |
| 0 point ice cream  only 1 ingredient | 480558 |       125 |          2283828 | 2012-06-05  |         5 |               3 |      480558 | 2012-09-09 |      5       | 0-10        |
| 0 point soup   ww                    | 391705 |        55 |            39835 | 2009-09-24  |         5 |              14 |      391705 | 2009-10-20 |      4.77778 | 0-10        |
| 0 point soup  crock pot              | 416980 |       305 |            39835 | 2010-03-18  |         2 |              11 |      416980 | 2010-03-30 |      5       | 0-10        |
| 007  martini                         | 429524 |         5 |           599450 | 2010-06-12  |         4 |               4 |      429524 | 2012-08-20 |      5       | 0-10        |



### Univariate Graph Analysis

<iframe src="assets/n-steps-distribution.html" width="800" height="600" frameborder="0"></iframe>

Analysis: This histogram displays the distribution of number of steps in recipes, showing a significant concentration of ratings near 10-15 steps. The trend indicates that most entities have fewer steps, while recipes with many steps are relatively rare, suggesting a right skew in the data.

### Bivariate Graph Analysis

<iframe src="assets/n-steps-bivariate-distribution.html" width="800" height="600" frameborder="0"></iframe>

Analysis: The graph shows that the recipes with ratings 3 and 4 have lower number of steps, while the recipes with ratings 1, 2, and 5 have higher number of steps. This suggests that typically recipes with fewer number of steps tend to have higher ratings and vice versa. 

### Interesting Aggregates

| Steps_Bin   |   Average_Rating_Mean |   Count |   Rating_StdDev |
|:------------|----------------------:|--------:|----------------:|
| 0-10        |               4.62423 |   51793 |        0.632202 |
| 10-20       |               4.6232  |   26641 |        0.65224  |
| 20-30       |               4.63654 |    4199 |        0.671727 |
| 30-40       |               4.68632 |     716 |        0.637707 |


This pivot table summarizes the data by grouping the "Number_of_Steps" column into bins (e.g., "0-10", "10-20", etc.) and calculating aggregate statistics for each bin. Here's the significance of each column:

- **Average_Rating_Mean**: Shows the average rating for each step range, providing insight into how ratings vary across different activity levels.
- **Count**: Indicates the number of entries (or observations) in each bin, reflecting the distribution of data across step ranges.
- **Rating_StdDev**: Displays the standard deviation of ratings within each bin, highlighting the variability or consistency of ratings for a given step range.

This table shows the relationships between step levels and ratings, such as whether higher step counts are associated with better ratings and if certain bins have more consistent feedback.



## Assessment of Missingness

- ‘Rating’ is a column that is Not Missing At Random (NMAR). This is because if people felt really strongly about a specific recipe, they are more likely to report their rating. On the other hand, if people do not have a strong opinion about a recipe, they are more likely to not share their rating. Therefore, the rating column is dependent on its own value. 
- Additional data that we can collect to make the missingness of ‘rating’ MAR is the popularity of the recipe. For example, we can collect the number of views for that recipe. The higher the number of views, the more likely people have tried and rated a recipe and vice versa. Therefore, the missingness of ‘rating” is dependent on an entirely different column, “popularity of recipe”.
  


### Rating on n_steps:

- **Null Hypothesis**: Rating is not MAR dependent on the n_steps column.

- **Alternative Hypothesis**: Rating is MAR dependent on the n_steps column.

- **Test Statistic**: Difference in means

- **Results**: The results show a p-value of 0.0, which is less than the significance level at 0.05. Since the p-value is extremely small, we can reject the null hypothesis, so Rating is MAR dependent on the n_steps column.


<iframe src="assets/nsteps-missing-dependency.html" width="800" height="600" frameborder="0"></iframe>



### Rating on minutes

- **Null Hypothesis**: Rating is not MAR dependent on the minutes column.

- **Alternative Hypothesis**: Rating is MAR dependent on the minutes column.

- **Test Statistic**: Difference in means

- **Results**: The results show a p-value of 0.06, which is greater than the significance level at 0.05. Since the p-value is greater, we can fail to reject the null hypothesis of, so Rating is not MAR dependent on the minutes column.


<iframe src="assets/minutes-missing-dependency.html" width="800" height="600" frameborder="0"></iframe>



## Hypothesis Testing

- **Null Hypothesis**: There is no association between the number of steps and the average rating of a recipe.

- **Alternative Hypothesis** : There is an association between the number of steps and the average rating of a recipe.

- **Test Statistic**: Difference in means

- **Significance Level**: 0.05
  
- **Result**: The null hypothesis was rejected with a statistically significant result (p-value = 0.0), indicating an association. However, the observed difference in means (-0.0197) was very small, suggesting the association is weak or negligible.

- **Justification**: We chose the difference in means between the two groups as our test statistic because we are interested in comparing the central tendencies of the ratings for recipes with fewer steps (0-20) versus those with more steps (20-40). The observed difference in means is −0.0197. A permutation test is appropriate in this case because we do not have any information about the population distribution, and we aim to assess whether the observed difference could have occurred by chance under the null hypothesis. The practical purpose of this analysis is to help people make informed decisions when selecting recipes. If a recipe has a high number of steps, its rating could influence whether someone decides to try it. For instance, if a complex recipe with many steps has a high rating, people might feel more motivated to follow it, trusting that the effort is worthwhile. Conversely, if the rating is low, they may opt for simpler alternatives. Understanding this relationship can guide users toward recipes that best match their preferences and willingness to invest time.



## Framing a Prediction Problem 
Prediction Problem and Type:
- The prediction problem is multiclass classification. We are building a model to predict a categorical variable (rating_x), which represents a rating on a scale (e.g., 1 to 5). This makes the problem a multiclass classification task because there are more than two possible classes (ratings 1 through 5).

Response Variable:
- The response variable (i.e., the variable being predicted) is rating_x. This variable represents a rating on a scale, such as user ratings for a product, which can take values from 1 to 5.

- We chose rating_x as the response variable because it represents the outcome we want to predict based on the features n_steps and minutes. These features could correspond to data such as steps taken and time spent on an activity, and we want to predict how that activity is rated. This is a typical use case for predicting user ratings based on quantitative metrics.

#### NOTE: 

- We transitioned from a KNN classifier to a Random Forest Classifier, which is better suited for predicting categorical outcomes.

Evaluation Metric:
- The metric used to evaluate the model is accuracy. We chose accuracy because it is a simple and interpretable metric that measures the proportion of correct predictions out of all predictions made. Accuracy is a natural fit for multiclass classification tasks as it measures the proportion of correct predictions relative to the total number of predictions. Since the problem involves predicting one of several discrete classes, accuracy provides a straightforward measure of the model's overall performance.


## Baseline Model 

Model Type:
- The model we are using is a KNN Classifier model. This model aims to predict a categorical target variable, specifically the variable rating_x, based on the input features n_steps and minutes. 

Features in the Model:

- n_steps (Quantitative): This is a quantitative feature representing the number of steps taken during an activity. It is a continuous numerical variable.

- minutes (Quantitative): This is another quantitative feature representing the number of minutes spent on the activity. It is also a continuous numerical variable.

- There are no ordinal or nominal features in the current model. Both features are quantitative, meaning they are continuous and represent measurable quantities. Since these are numerical features, no encoding (such as one-hot encoding or label encoding) is necessary.

Model Fitting:
- The model is trained using KNN Classifier. The process involves:

Training Data: We split the dataset into training and test sets using a 80-20 split. The model is trained on the training data using the features n_steps and minutes to predict the target variable rating_x.

Test Data: After training, the model is used to predict the ratings for the test set, and we evaluate the model's performance using appropriate metrics.

Performance Metrics:

Model Evaluation:
Test Accuracy: 0.4729 
- This value represents the model's performance on unseen data (test set). The value of 0.4729 is a relatively low accuracy score, indicating the baseline is not able to identify the underlying patterns of the dataset.

- The value of 0.4729 is good, but could be better. Right now, the model is not able to predict 53% of the data points correctly. Although this is a good starting point, this metric could definitely improve!



## Final Model 

In the final model, we made several additions to the feature set, which were designed to improve the model’s performance based on the context of the data generating process:

Nutrition Features:

- Calories, Sugar: These features were extracted from the nutrition column, which originally contained a list of nutritional values for each record. By parsing through the nutritions column, we extracted the specific calorie and sugar values in each recipe. In the real world, calories and sugar would be considered two separate factors when people rate the recipe. Therefore, we also treated calories and sugar as two separate features that could train our model. By separating out specific nutrients (e.g., calories, sugar), we introduce more specific and interpretable features into the model. We think that calories and sugar improved our model's performance as it has a direct influence on recipe ratings. For example, people tend to give recipes with more calories and sugar a higher rating as the recipe might taste better than a healthier alternative.
  
Fat, Protein, Sodium, Carbs, Fiber: These additional nutrition-related features can potentially add value by offering a more comprehensive understanding of the data. For the final model, we focused on a subset of these (calories and sugar) for simplicity and interpretability.

Minutes and N_steps:

- We think that minutes and n_steps improved our model's performance as it has a direct influence on recipe ratings. For example, recipes that take longer to prepare might have a lower rating as people prefer and quick and easy recipes. Similarly, this pattern applies to the number of steps a recipe has as well, with more stepped recipes receiving lower ratings. 

- Log Transformation: The minutes feature was log-transformed using FunctionTransformer(np.log1p). This transformation helps handle skewness in the data where the original distribution of minutes might have outliers or heavy tails. 
  
- Justification: Log transformation is commonly used for skewed data because it reduces the impact of extreme values, making the model more stable and improving its predictive accuracy. In this case, long-duration activities may have disproportionate effects on the target variable, and applying the log transformation makes the data more normally distributed.

Scaling of Features:

- Standard Scaling: Quantitative features such as n_steps, calories, and sugar were standardized using StandardScaler to have zero mean and unit variance.

- Justification: Feature scaling helps ensure that all features contribute equally to the model, especially when using distance-based algorithms. Standardizing the features prevents one feature with a larger range from dominating others and thus improves model performance.

Model Description

- For the final model, we chose a Random Forest Classifier, an ensemble learning method that uses multiple decision trees to make predictions. The model aggregates the predictions from several trees to improve accuracy and reduce overfitting. Random forests are robust, handle non-linear relationships well, and provide feature importance, which can be useful for interpretation.

Hyperparameter Tuning:
- We used GridSearchCV to tune the hyperparameters of the Random Forest model, and found the best combination of parameters that maximized the model’s performance. We did 3 fold cross-validations during our GridSearchCV. The following hyperparameters were optimized:

- n_estimators: 100 
- max_depth: 30
- min_samples_split: 2
- min_samples_leaf: 1 
- max_features: 'sqrt'


Final Model Performance

- Accuracy: The accuracy for our final model is 0.731, which is a 26% increase in accuracy, indicating a great improvement from our baseline model.
  
Improvement Over Baseline Model:

- Better Handling: In a KNN Classifier, a single noisy point in the dataset can significantly affect predictions because it relies directly on the closest neighbors. On the other hand, Random Forest Classifier combines multiple decision trees, each trained on a random subset of data and features, which averages out errors and reduces sensitivity to noise. This is one of the factors that might explain the drastic improvement from our baseline to our final model.
  

- Feature Engineering: The addition of nutrition-related features and the transformation of the minutes feature likely contributed to a better understanding of the data, allowing the Random Forest model to make more informed predictions.

- Hyperparameter Tuning: By fine-tuning the Random Forest hyperparameters, we were able to improve the model's generalization and accuracy, making it more suitable for the prediction task.


Conclusion
- In summary, the Random Forest Classifier with feature engineering and hyperparameter optimization performed significantly better than the baseline KNN Classifier model. The additional features, transformations, and careful tuning of hyperparameters helped the model capture the underlying patterns in the data more effectively, leading to improved performance. The use of GridSearchCV to optimize the model's parameters ensured that we were using the best possible configuration for the given data.



## Fairness Analysis

- In this fairness analysis, we want to examine whether our model performs differently for two groups in our dataset. The groups we will compare are based on the number of steps taken, which may represent physical activity levels, potentially influencing the rating (rating_x) given to the items in the dataset.

Group Definition
- Group X: Recipes which have fewer steps (n_steps < median).
- Group Y: Recipes which have more steps (n_steps >= median).
  
- The choice of this grouping is based on the assumption that recipes with more steps might be rated differently than those with lower number of steps. For instance, the amount of effort a recipe requires could correlate with different preferences, which we want to examine in terms of fairness.

Evaluation Metric
- We will use accuracy as our evaluation metric. Since the target variable rating_x is categorical and we are working with a classification model, accuracy is an appropriate metric to compare across different groups. Accuracy represents the proportion of correct predictions, and a lower accuracy for one group would suggest that the model is unfairly biased against that group.

Hypotheses
- Null Hypothesis (H₀): The model performs equally for both groups (Group X and Group Y). Any differences in accuracy between these two groups are due to random chance.
  
- Alternative Hypothesis (H₁): The model performs differently for the two groups. Specifically, we hypothesize that the model's accuracy is significantly lower for Group X (fewer steps) compared to Group Y (more steps).
  
Test Statistic
- Our test statistic will be the difference in accuracy between Group X and Group Y. We will use a permutation test to determine whether the observed difference in accuracy is statistically significant.

Significance Level
- We will use a significance level of α = 0.05. If the p-value is less than 0.05, we will reject the null hypothesis, indicating that there is a significant difference in the model's performance between the two groups.

**Conclusion**
- P-value: 0.844
- Since 0.844 > 0.05, we fail to reject the null hypothesis. The model's accuracy does not differ significantly between Group X, recipes which have fewer steps and Group Y, recipes which have more steps.
