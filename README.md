# Home Values in King County

Reducing information asymmetry in home prices

![rowan-heuvel-bjej8BY1JYQ-unsplash](https://user-images.githubusercontent.com/111642763/223301762-394731ca-4447-4c2f-a6fd-c9c8fef6dd33.jpg)

## Overview and Problem Statement
The focus for this project is to use data from the King County House Sales dataset to create a regression model that projects the sale price of properties in the area.

The scope of this project includes data cleaning, initial exploratory analysis, model improvement with additional or transformed variables, final recommendation, and overall evaluation.

By the end of this project, the goal is to derive a model that identifies the relevant features which account for price variations in homes within the region. The model should act as a guide for property buyers and sellers in the area so they can pinpoint attributes of a home that they should emphasize when negotiating prices.

## The Data
The data folder in the repository contains the file kc_house_data.csv, which is the King County House Sales dataset. The folder also contains a description of the columns in the dataset. Below is a list of the columns' names and a preview of the first few rows in the dataset:

<img width="1003" alt="Screen Shot 2023-03-04 at 4 24 33 PM" src="https://user-images.githubusercontent.com/111642763/222929311-6be712a1-9d57-4aba-91e4-4b6b94f03d0c.png">

<img width="1095" alt="Screen Shot 2023-03-04 at 6 13 39 PM" src="https://user-images.githubusercontent.com/111642763/222932863-70c7cdf6-0b98-4258-8539-fb0dc4810d5b.png">

## Methods
Analysis of home prices in King County begins with a baseline regression model. Using the correlation function, an initial predictor is selected. In this case, a baseline regression model is run to show the relationship between home prices ("price") and living area ("sqft_living"). Data cleaning takes place at this stage to exclude homes built before 1980, and centering is performed to make the values of "sqft_living" and "bathrooms" around its mean. After the parameters from the baseline results are evaluated, another numerical predictor is added to create a second model. After two models with only numerical predictors have been run, a third and fourth model containing categorical variables is created to improve the analysis. Throughout the creation of the four models, different variables were tested to determine which ones were statistically significant. The final model builds on the fourth regression (a combination of numerical and categorical predictors) by including interaction terms. The variables in the fourth model are tested for interaction effects, and statistically significant terms are incorporated to construct the fifth and final model.

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
During the early exploration stage, only numerical predictors were considered. The third model builds on the second by incrementally incorporating categorical variables, starting with "grade", to assess the impact of a home's rating on its price. To avoid multicollinearity, we perform one-hot-encoding and drop the column "grade_7 Average". A home grade of 7 becomes the reference category. Results are shown below:

<img width="750" alt="Screen Shot 2023-03-11 at 10 05 43 AM" src="https://user-images.githubusercontent.com/111642763/224491940-699e7522-2176-4746-b621-5bb51ec082ce.png">

Homes with grades 2 to 6 inclusive are not statistically significant, as p-values are <0.05. This suggests that for homes of substandard to low average quality, there is an insignificant impact on the selling price, which makes sense in this scenario.

Since we dropped grade_7 Average and used it as the reference category, the model indicates that a home with an average grade, average living area, and average number of bathrooms sells for around $927K (the constant). Additionally, a unit increase in "sqft_living" and "bathrooms" is expected to increase the value of the average home by $250 and $138K, respectively.

Overall, the third model accounts for 51% of the variance in price.

### Fourth Model (Remaining Categorical Predictors)
Other categorical variables that were deemed relevant during the initial data exploration are included in the fourth model. We have utilized "waterfront" and "sewer_system". Similar to the third model, one-hot-encoding is done and one dummy column is excluded to avoid multicollinearity.

In this run, we have dropped the variables that did not make a statistically significant difference on home price. The summary printout is as follows:

<img width="839" alt="Screen Shot 2023-03-11 at 10 24 30 AM" src="https://user-images.githubusercontent.com/111642763/224492853-29ce25d4-1eac-4030-9c18-4835c64397a1.png">

The fourth model accounts for ~54% of the variance in sale price and models against a reference home with the following features:
* Average living area
* Average number of bathrooms
* Home grade of 7 (average)
* No waterfront
* A public sewer system

Based on the constant coefficient, a home with the aforementioned features can be expected to have a sale price around $941K. With the additional variables, homes with restricted sewer systems lack a statistically significant impact. Of the additional variables, "waterfront_YES" and "sewer_system_PRIVATE" have p-values < 0.05, and they are associated with a $1.45M and -$150K change in sale price, respectively.

### Summarizing the four models
Iterating through the four regressions have shown us that certain variables do not create a statistically significant difference on home prices in King County. Through multiple linear regression and one-hot-encoding with categories, the predictors that do not contribute to the model tend to be ones that are less favorable in a home.

To recap, the variables with p-values > 0.05 are the ones representing grades 2-6 and restricted sewer systems. To better understand this observation, two seaborn visualizations showing the relationship between "price" and "sqft_living" with hues based on "grade" and "sewer_system" were created:

<img width="932" alt="Screen Shot 2023-03-11 at 11 13 42 AM" src="https://user-images.githubusercontent.com/111642763/224495292-0d5564e7-e18a-4d1e-a178-68f81d6fe86e.png">

Starting with the left graph, we can interpret that the darker hues tend to be more spread out and the lighter hues are clustered around the left/bottom. Lighter hues have low home grades, which we previously concluded has minimal impact on the model run.

Looking at the right graph, we can also see that points on the graph primarily represent public and private sewer systems that are not restricted.

Overall, we can see how the less favorable variables of low home grades and restricted sewer systems have a minimal impact on price when compared to a home with average features. This logical conclusion can be drawn from the regression models and visualizations above.

To refresh, below is a dataframe summarizing all the regression models that have been constructed thus far:

<img width="607" alt="Screen Shot 2023-03-11 at 11 25 33 AM" src="https://user-images.githubusercontent.com/111642763/224495862-9f43175a-a6bd-4e35-a481-7e5640f080f5.png">

### Final Model
A final model is constructed using the original four regressions to create interaction terms with the relevant predictors. The original numerical predictor "sqft_living" is used to create an interaction term with the remaining dummy variable from "waterfront". A new dataframe is created, and a final table showing the statistically significant term and variables are shown below:

<img width="302" alt="Screen Shot 2023-03-11 at 7 59 08 PM" src="https://user-images.githubusercontent.com/111642763/224518550-f1439a29-4423-423a-b58e-ff17bef52c41.png">

Lastly, the summary table of the final model is generated:

<img width="731" alt="Screen Shot 2023-03-11 at 7 59 04 PM" src="https://user-images.githubusercontent.com/111642763/224518556-228fbff8-d60c-41ea-955c-d028b25f3516.png">

A combination of numerical measures, categorical variables, and the creation of an interaction term improves the R-squared to ~55%.

## Conclusion
Homebuyers and homesellers who are looking for price guidance can refer to the regression models to determine how a given property compares to the average home in King County. Compared to properties with standard features (average bathrooms, average area, average rating), high home grades have a statistically significant impact on price. Additionally, adjusting variables within a home that has certain "nice-to-have" amenities, such as a waterfront, has a greater impact on value. This is shown through the interaction term "sqft_living x waterfront_YES", which indicates that a unit increase in "sqft_living" for an average-sized home with a waterfront adds ~$538 instead of just $243. When looking to market a home or to determine a reasonable price range, sellers and buyers can refer to what has been outlined through the regression analyses.

## Future Considerations
* Using variables like "lat" and "long" to visualize the geographical distribution of homes would improve the model, as location is arguably the most important aspect of real estate
* Leveraging population and socioeconomic data can provide insight into the typical homebuyer in the King County region
* Normalizing the data can better standardize the model's independent variables

## Additional Information
For detailed copy of the data analysis process and related code, please refer to the [Jupyter Notebook](https://github.com/keziasetokusumo/p2_project/blob/main/KC_Analysis_Notebook.ipynb). A summary of findings can also be found in this deliverable.

## Data Sources
This analysis uses sales data of homes within King County. The region has a comprehensive database of properties in the area, and public information can be found [here](https://kingcounty.gov/services/data.aspx).

## Repository Structure
```
├── data
├── images  
├── .gitignore                           
├── KC_Analysis_Notebook.ipynb                                      
├── README.md    
└──
```
