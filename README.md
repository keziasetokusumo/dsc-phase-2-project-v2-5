# Home Values in King County

Reducing information asymmetry in home prices

![rowan-heuvel-bjej8BY1JYQ-unsplash](https://user-images.githubusercontent.com/111642763/223301762-394731ca-4447-4c2f-a6fd-c9c8fef6dd33.jpg)

## Overview and Problem Statement
This project focuses on leveraging data from the [King County House Sales dataset](https://info.kingcounty.gov/assessor/DataDownload/default.aspx) to create a regression model that prices the average home within the region.

The scope of this project includes data cleaning, initial exploratory analysis, model improvement with additional or transformed variables, final recommendation, and overall evaluation.

The goal is to derive a model that helps the general public determine how a certain property should sell compared to the typical home. The model should act as a guide for property buyers and sellers in the area so they can pinpoint attributes of a home that they should emphasize when negotiating prices.

## The Data
The data folder in the repository contains the file `kc_house_data.csv`, which is the King County House Sales dataset. The folder also contains a description of the columns in the dataset. Below is a list of the columns' names and a preview of the first few rows in the dataset:

* `id` - Unique identifier for a house
* `date` - Date house was sold
* `price` - Sale price (prediction target)
* `bedrooms` - Number of bedrooms
* `bathrooms` - Number of bathrooms
* `sqft_living` - Square footage of living space in the home
* `sqft_lot` - Square footage of the lot
* `floors` - Number of floors (levels) in house
* `waterfront` - Whether the house is on a waterfront
* `greenbelt` - Whether the house is adjacent to a green belt
* `nuisance` - Whether the house has traffic noise or other recorded nuisances
* `view` - Quality of view from house
* `condition` - How good the overall condition of the house is. Related to maintenance of house.
* `grade` - Overall grade of the house. Related to the construction and design of the house.
* `heat_source` - Heat source for the house
* `sewer_system` - Sewer system for the house
* `sqft_above` - Square footage of house apart from basement
* `sqft_basement` - Square footage of the basement
* `sqft_garage` - Square footage of garage space
* `sqft_patio` - Square footage of outdoor porch or deck space
* `yr_built` - Year when house was built
* `yr_renovated` - Year when house was renovated
* `address` - The street address
* `lat` - Latitude coordinate
* `long` - Longitude coordinate


<img width="1095" alt="Screen Shot 2023-03-04 at 6 13 39 PM" src="https://user-images.githubusercontent.com/111642763/222932863-70c7cdf6-0b98-4258-8539-fb0dc4810d5b.png">

## Methods
Analysis of home prices in King County begins with a baseline regression model. Using the correlation function, an initial predictor is selected. In this case, a baseline regression model is run to show the relationship between home prices (`price`) and living area (`sqft_living`). Data cleaning takes place at this stage to exclude homes built before 1980, and centering is performed to make the values of `sqft_living` and `bathrooms` around its mean. After the parameters from the baseline results are evaluated, another numerical predictor is added to create a second model. After two models with only numerical predictors have been run, a third and fourth model containing categorical variables is created to improve the analysis. Throughout the creation of the four models, different variables were tested to determine which ones were statistically significant. The final model builds on the fourth regression (a combination of numerical and categorical predictors) by including interaction terms. The variables in the fourth model are tested for interaction effects, and statistically significant terms are incorporated to construct the fifth and final model.

## Results

### Baseline Model

A correlation heatmap is first generated to identify which variable might serve as a good independent variable for the baseline model:

<img width="872" alt="Screen Shot 2023-03-06 at 9 21 27 PM" src="https://user-images.githubusercontent.com/111642763/223302941-f4bde8db-dbf2-4714-97ab-c6e48555a48c.png">

Based on the heatmap, a baseline model showing the relationship between `sqft_living` and `price` is run, as these two variables have the highest correlation (0.64). A baseline model that centers `sqft_living` around its mean is created. The summary of the results and a basic visualization are shown below.

<img width="667" alt="Screen Shot 2023-03-06 at 9 24 01 PM" src="https://user-images.githubusercontent.com/111642763/223303635-6e5961f4-2b3c-482d-b268-9b24e8beed31.png">

The following interpretations can be made from the summary:
* 40.5% of the variation in price can be explained by the baseline model
* The p-values for the parameters `const` and `sqft_living` are less than 0.05, indicating their statistical significance
* The overall model also has a p-value < 0.05; hence, it can be interpreted to be statistically significant
* The coefficient value for `sqft_living` suggests that a unit increase in SF living area corresponds to an increase in home price by ~$609
* The `const` value suggests that we can expect a home with average living area to sell for ~$1.22M

A graph displaying `sqft_living` along the x-axis and `price` along the y-axis is shown below:

<img width="661" alt="Screen Shot 2023-03-06 at 9 27 19 PM" src="https://user-images.githubusercontent.com/111642763/223303749-9a1c879c-26a8-4597-872f-8e3b1ba585fd.png">

Note that living area data has been centered around its mean, hence the negative values on the x-axis in the graph above.

### Second Model (Multiple Regression Model)
To account for other variables that may impact the price of homes in Kings County, a second predictor is considered. Referencing the original correlation heatmap, `bathrooms` is a discrete predictor selected for the multiple regression model. Its data is also centered. The below dataframe is passed into statsmodels to generate a new summary table:

<img width="242" alt="Screen Shot 2023-03-08 at 9 11 27 PM" src="https://user-images.githubusercontent.com/111642763/223897809-40f15d11-a0f9-483c-8e2b-284a45905acb.png">

<img width="673" alt="Screen Shot 2023-03-08 at 9 12 34 PM" src="https://user-images.githubusercontent.com/111642763/223897961-1b9ea412-b5a9-48ea-a1f4-7eaa2fbf9364.png">

In terms of the R-squared value, the second model is only slightly better than the first, with 41% of the variation in price accounted for. Similar to the baseline model, a home with an average living area and average number of bathrooms can expect to have a price tag of ~$1.22M. The new parameter value for `sqft_living` shows that under these conditions, an additional unit increase in SF living area increases price by ~$530. Additionally, the parameter for `bathrooms` conveys that an additional bathroom in the home raises the value by ~$143K.

Note that these observations are in reference to average/median values, as the data has been centered.

### Third Model (Multiple Regression Model Cont.)
The third model builds on the second by incorporating `grade`, to assess the impact of a home's rating on its price. `grade` is another discrete variable in the form of a string that we hypothesize has an influence on price levels. Since its column is not numerical, we need to map its values with the corresponding rating number.

Once mapping has been done with dictionaries, we can visualize the impact of `grade` on `price`:

<img width="580" alt="Screen Shot 2023-03-13 at 7 07 42 AM" src="https://user-images.githubusercontent.com/111642763/224684741-f5cb6d31-c060-4aca-b4d8-6dacb5fc4b6f.png">

`grade` data is then centered and passed through statsmodels to generate the summary below:

<img width="599" alt="Screen Shot 2023-03-13 at 6 55 01 AM" src="https://user-images.githubusercontent.com/111642763/224681789-fd35f10d-1309-4013-b2b9-b23840ede069.png">

Our new model is better at accounting for price variance than the previous two. Now we can asssess the impact of `grade` on price and we can infer from the results that when an increase in rating to the average home leads to a rise in price of ~$317K.

### Fourth Model (Categorical Predictors)
Categorical variables that we expect to be relevant are included in the fourth model. We have utilized `waterfront` and `view`. We dummify these categories, conduct one-hot-encoding, and drop one column per category to avoid multicollinearity.

The summary printout is as follows:

<img width="631" alt="Screen Shot 2023-03-13 at 7 25 30 AM" src="https://user-images.githubusercontent.com/111642763/224688599-a1f2483a-e374-4396-8311-473b3ec78343.png">

The fourth model accounts for 51.1% of the variance in sale price and models against a reference home with the following features:
* Average living area
* Average number of bathrooms
* Average grade
* Average views
* No waterfront

Based on the constant coefficient, a home with the aforementioned features can be expected to have a sale price around $1.13 Million. With the additional variables, homes with good views or no views lack a statistically significant impact. Of the additional variables, fair and excellent views have p-values < 0.05, and they are associated with a $313M and $982K change in sale price, respectively.

To summarize all the regression models that we have run so far, refer to this table below:

<img width="492" alt="Screen Shot 2023-03-13 at 7 23 29 AM" src="https://user-images.githubusercontent.com/111642763/224878833-2553e933-67a5-43b3-a4c4-4349bc1646c4.png">

### Final Model (Interaction Term)
A final model is constructed using the original four regressions to create an interaction term. The original numerical predictor `sqft_living` is used to create an interaction term with the remaining dummy variable from `waterfront`. A new dataframe is created, and a final table showing the statistically significant term and variables are shown below:

<img width="289" alt="Screen Shot 2023-03-13 at 7 31 46 AM" src="https://user-images.githubusercontent.com/111642763/224690037-9ddd9f80-bff3-4491-9136-265257aa0bf5.png">

Considering that these variables are statistically significant, we pass them through statsmodels to create the final summary output:

<img width="735" alt="Screen Shot 2023-03-13 at 7 22 32 AM" src="https://user-images.githubusercontent.com/111642763/224690119-1d5c9fd0-13aa-4351-ba63-0613caedcdc3.png">

A combination of numerical measures, categorical variables, and an interaction term improves the R-squared to 52.4%.

## Conclusion
Homebuyers and homesellers who are looking for price guidance can refer to the regression models to determine how a given property compares to the average home in King County. The model prices the typical home with standard features at ~$1.2 Million. Additionally, adjusting variables within a home that has certain "nice-to-have" amenities, such as a waterfront, has a greater impact on value. This is shown through the interaction term `sqft_living x waterfront_YES`, which indicates that a unit increase in `sqft_living` for an average-sized home with a waterfront adds ~$629 instead of just $269. When looking to market a home or to determine a reasonable price range, sellers and buyers can refer to what has been outlined through the regression analyses.

## Future Considerations
* Using variables like `lat` and `long` to visualize the geographical distribution of homes would improve the model, as location is arguably the most important aspect of real estate
* Leveraging population and socioeconomic data can provide insight into the typical homebuyer in the King County region

## Additional Information
For detailed copy of the data analysis process and related code, please refer to the [Jupyter Notebook](https://github.com/keziasetokusumo/p2_project/blob/main/KC_Analysis_Notebook.ipynb). A summary of findings can also be found in this [deliverable](https://github.com/keziasetokusumo/p2_project/blob/main/p2_project_deliverable.pdf).

## Data Sources
This analysis uses sales data of homes within King County. The region has a comprehensive database of properties in the area, and public information can be found [here](https://kingcounty.gov/services/data.aspx).

## Repository Structure
```
├── data
├── images  
├── .gitignore                           
├── KC_Analysis_Notebook.ipynb                                      
├── README.md
├── p2_project_deliverable.pdf
└──
```
