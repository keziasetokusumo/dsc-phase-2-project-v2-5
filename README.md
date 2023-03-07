# Home Values in Kings County

Reducing information asymmetry in home prices

![rowan-heuvel-bjej8BY1JYQ-unsplash](https://user-images.githubusercontent.com/111642763/223301762-394731ca-4447-4c2f-a6fd-c9c8fef6dd33.jpg)

### Overview and Problem Statement
The focus for this project is to use data from the Kings County House Sales dataset to create a regression model that projects the sale price of properties in the area.

The scope of this project includes data cleaning, initial exploratory analysis, model improvement with additional or transformed variables, final recommendation, and overall evaluation.

By the end of this project, the goal is to derive a model that identifies the relevant features which account for price variations in homes within the region. The model should act as a guide for property buyers and sellers in the area so they can pinpoint attributes of a home that they should emphasize when negotiating prices.

### The Data
The data folder in the repository contains the file kc_house_data.csv, which is the King County House Sales dataset. The folder also contains a description of the columns in the dataset. Below is a list of the columns' names and a preview of the first few rows in the dataset:

<img width="1003" alt="Screen Shot 2023-03-04 at 4 24 33 PM" src="https://user-images.githubusercontent.com/111642763/222929311-6be712a1-9d57-4aba-91e4-4b6b94f03d0c.png">

<img width="1095" alt="Screen Shot 2023-03-04 at 6 13 39 PM" src="https://user-images.githubusercontent.com/111642763/222932863-70c7cdf6-0b98-4258-8539-fb0dc4810d5b.png">

### Methods
Analysis of home prices in Kings County begins with a baseline regression model. Using the correlation function, an initial predictor is selected. In this case, a baseline regression model is run to show the relationship between home prices ('price') and living area ('sqft_living'). The parameters from the baseline results are evaluated and a second numerical predictor is added to create a second model. After two models with only numerical predictors have been run, a third and fourth model containing categorical variables is created to improve the analysis. Throughout the creation of the four models, different variables were tested to determine which ones were statistically significant. The final model builds on the fourth regression (a combination of numerical and categorical predictors) by including interaction terms. The variables in the fourth model are tested for interaction effects, and statistically significant terms are incorporated to construct the fifth and final model.

### Results
