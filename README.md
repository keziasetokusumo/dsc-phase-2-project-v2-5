# Home Values in Kings County

Reducing information asymmetry in home prices

![rowan-heuvel-bjej8BY1JYQ-unsplash](https://user-images.githubusercontent.com/111642763/223301762-394731ca-4447-4c2f-a6fd-c9c8fef6dd33.jpg)

## Overview and Problem Statement
The focus for this project is to use data from the Kings County House Sales dataset to create a regression model that projects the sale price of properties in the area.

The scope of this project includes data cleaning, initial exploratory analysis, model improvement with additional or transformed variables, final recommendation, and overall evaluation.

By the end of this project, the goal is to derive a model that identifies the relevant features which account for price variations in homes within the region. The model should act as a guide for property buyers and sellers in the area so they can pinpoint attributes of a home that they should emphasize when negotiating prices.

## The Data
The data folder in the repository contains the file kc_house_data.csv, which is the King County House Sales dataset. The folder also contains a description of the columns in the dataset. Below is a list of the columns' names and a preview of the first few rows in the dataset:

<img width="1003" alt="Screen Shot 2023-03-04 at 4 24 33 PM" src="https://user-images.githubusercontent.com/111642763/222929311-6be712a1-9d57-4aba-91e4-4b6b94f03d0c.png">

<img width="1095" alt="Screen Shot 2023-03-04 at 6 13 39 PM" src="https://user-images.githubusercontent.com/111642763/222932863-70c7cdf6-0b98-4258-8539-fb0dc4810d5b.png">

## Methods
Analysis of home prices in Kings County begins with a baseline regression model. Using the correlation function, an initial predictor is selected. In this case, a baseline regression model is run to show the relationship between home prices ("price") and living area ("sqft_living"). Data cleaning and centering takes place at this stage. After the parameters from the baseline results are evaluated, another numerical predictor is added to create a second model. After two models with only numerical predictors have been run, a third and fourth model containing categorical variables is created to improve the analysis. Throughout the creation of the four models, different variables were tested to determine which ones were statistically significant. The final model builds on the fourth regression (a combination of numerical and categorical predictors) by including interaction terms. The variables in the fourth model are tested for interaction effects, and statistically significant terms are incorporated to construct the fifth and final model.

## Results

### Baseline Model

A correlation heatmap is first generated to identify which variable might serve as a good independent variable for the baseline model:

<img width="872" alt="Screen Shot 2023-03-06 at 9 21 27 PM" src="https://user-images.githubusercontent.com/111642763/223302941-f4bde8db-dbf2-4714-97ab-c6e48555a48c.png">

Based on the heatmap, a baseline model showing the relationship between "sqft_living" and "price" is run, as these two variables have the highest correlation (0.64). A baseline model that centers "sqft_living" around its mean is created. The summary of the results and a basic visualization are shown below.

<img width="667" alt="Screen Shot 2023-03-06 at 9 24 01 PM" src="https://user-images.githubusercontent.com/111642763/223303635-6e5961f4-2b3c-482d-b268-9b24e8beed31.png">

The following interpretations can be made from the summary:
* 40.5% of the variation in price can be explained by the baseline model
* The p-values for the parameters "const" and "sqft_living" are less than 0.05, indicating their statistical significance
* The overall model also has a p-value < 0.05; hence, it can be interpreted to be statistically significant
* The coefficient value for "sqft_living" suggests that a unit increase in SF living area corresponds to an increase in home price by ~$609
* The "const" value suggests that we can expect a home with average living area to sell for ~$1.22M

A graph displaying "sqft_living" along the x-axis and "price" along the y-axis is shown below:

<img width="661" alt="Screen Shot 2023-03-06 at 9 27 19 PM" src="https://user-images.githubusercontent.com/111642763/223303749-9a1c879c-26a8-4597-872f-8e3b1ba585fd.png">

Note that living area data has been centered around its mean, hence the negative values on the x-axis in the graph above.

### Second Model (Numerical Predictors Only)
To account for other variables that may impact the price of homes in Kings County, a second predictor is considered. Referencing the original correlation heatmap, "bathrooms" is selected for the next run to create a multiple regression model. The "bathrooms" column is also centered around the mean to make the data more interpretable later. The below dataframe is passed into statsmodels to generate a new summary table:

<img width="242" alt="Screen Shot 2023-03-08 at 9 11 27 PM" src="https://user-images.githubusercontent.com/111642763/223897809-40f15d11-a0f9-483c-8e2b-284a45905acb.png">

<img width="673" alt="Screen Shot 2023-03-08 at 9 12 34 PM" src="https://user-images.githubusercontent.com/111642763/223897961-1b9ea412-b5a9-48ea-a1f4-7eaa2fbf9364.png">

In terms of the R-squared value, the second model is only slightly better than the first, with 41% of the variation in price accounted for. Similar to the baseline model, a home with an average living area and average number of bathrooms can expect to have a price tag of ~$1.22M. The new parameter value for "sqft_living" shows that under these conditions, an additional unit increase in SF living area increases price by ~$530. Additionally, the parameter for "bathrooms" conveys that an additional bathroom in the home raises the value by ~$143K.

### Third Model (Categorical Predictor)
During the early exploration stage, only numerical predictors were considered. The third model builds on the second by incorporating the categorical variable "grade" to assess the impact of a home's rating on its price. To avoid multicollinearity, we perform one-hot-encoding and drop the column "grade_7 Average". A home grade of 7 becomes the reference category. Results are shown below:

<img width="747" alt="Screen Shot 2023-03-11 at 9 57 17 AM" src="https://user-images.githubusercontent.com/111642763/224491521-cfa1c63b-a7b5-466e-b3f7-2ef7dfeee3a3.png">

Homes with grades 2 to 6 inclusive are not statistically significant, as p-values are <0.05. This suggests that for homes of substandard to low average quality, there is an insignificant impact on the selling price, which makes sense in this scenario.

Since we dropped grade_7 Average and used it as the reference category, the model indicates that a home with an average grade, average living area, and average number of bathrooms sells for around $927K (the constant). Additionally, a unit increase in "sqft_living" and "bathrooms" is expected to increase the value of the average home by $250 and $1.38M, respectively.

Overall, the third model accounts for 51% of the variance in price.

### Fourth Model (Remaining Categorical Predictors
