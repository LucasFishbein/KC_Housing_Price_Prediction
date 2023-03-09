# King County, WA Home Sale Price Predictor and Recommendations to Maximize Home Value
**Created by Lucas Fishbein**

![istockphoto-1171946250-612x612](https://user-images.githubusercontent.com/117129342/223840005-89ba4712-5409-4d23-a58c-80bd294b445b.jpg) 
[Photo Source: Istockphoto.com](istockphoto.com/illustrations/house-construction-frame)



# Getting Started with this Repository

To Run this code, download the following data files from the link below and place both files in a folder you will create named "/data" (case-sensitive) inside this repository.

Download: King County Home Sale 2021-2022 From: https://github.com/LayFish21/dsc-phase-2-project-v2-5/tree/main/data

# Overview and Business Problem


The Stakeholders of interest will be a residental home construction team within King County, WA. Their main focus will be procuring land and then constructing residental buildings for the purpose of selling them to turn a profit. These companies will want to know what features are most imporant in maximizing home values upon their sale in order to dictate where they buy land and to what standards they should build them.

The purpose of this data analysis is to build a model that will predict the price of constructed homes for a builder located in Kings County, WA. From this model, the most vaulable features of a home will be identified and from there recomendations can be given to the building team for choosing locations and making construction decisions that will maximize the value of a home.


# Data Analysis Quick Results


### Question 1: What factors coorelate most with the sale price of a home?
  
        1. Square footage of space inside the home
        2. Square footage of house apart from basement
        3. Number Of Bathrooms
        4. Price per Square Foot of space inside the house            
        5. Number of Bedrooms

### Question 2: Can we build a model to predict home prices?

Yes, an OLS multilinear regression model has been built in which every feature has a significant relationship with price and accounts for 55% of the variance in the sales price, the average error of this model is about half a standard deviation. This model is a moderately good predictor and can be used to understand trends as they relate to price but should not be used as an absolute predictor.


### Question 3:  Action steps a home builder can take on in order to best increase their home's value upon sale.

#### Choosing locations to Build Upon

Choose plots with a "Fair" view but no better, next to a greenbelt.

#### Home Construction
Choose between a "Good","Better" and "Very Good"  conistruction grade. 
#### Home Presentation and Sale
Each step up in condition will net about a $50,000 increase in value.

Aim to sell homes in the months of April or May to maximize the home sale price.


# Database Understanding

A database of 30,155 home sales within King County, WA during the years 2021-2022 has been sourced from [King County Assessor Data Download](https://info.kingcounty.gov/assessor/DataDownload/default.aspx) to build this predictive model, this data was choosen as it is the most recent and relevant data available within the stakeholder's area of interest. 

The `address`, `lat`, and `long` fields have been retrieved using a third-party [geocoding API](https://docs.mapbox.com/api/search/geocoding/). 
to build this predictive model, this data was choosen as it is the most recent and relevant data available within the stakeholder's county of interest.

There are 25 home features included in this data set, with "price" being treated as the target variable. The definition of each of these features can be found at [King County Glossary](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r).

# Data Cleaning and Preparation

A number of features were engineered using the present data including the Month of sale, does the home have a patio,  does the home have a patio and price per square foot

The data was culled to better fit the business question at hand, homes with prices above $2,275,075 and indoor square footage above 4,420 were considered possible outliers based on [interquartile range method](https://online.stat.psu.edu/stat200/lesson/3/3.2) Homes without a single bathroom or bedroom were also excluded as these were not considered to be residential homes. Homes with construction grades 1-4, which are considered not up to code via [King County Glossary of Terms](https://info.kingcounty.gov/assessor/esales/Glossary.aspx?type=r) were also removed as homes should be built to code.

In order to use Categorical Variables in the regression modeling, they were converted into numerical codings via one-hot encoding with a reference value being dropped from each variable.

Any remaining variables that did not appply directly to the business problem or were not within a home builders control were removed from the dataset, to keep the model relevant to our goal.

The features were also check for colinearity and any colinear pairs had the feature with the lowest correlation to price dropped.

# Data Analysis and Modeling

The cleaned dataset contained 23,512 entries and 17 features in total, this dataset was used first to build a OLS simple linear regression containing only one predictor variable. From there, OLS Multilinear regression models were created by iterating on the simple linear model and adding more predictor variables in order to strengthen the model.

A model's strength was dictated largely by its adjusted R-Squared value, its overall significance, the coefficients significance with price and its ability to fall in line with the assumptions of linear regressions.

Models were tested for linearity, normality and homoscedasticity to be sure they followed the [linear regression assumptions](https://www.statology.org/linear-regression-assumptions/).

Predictor Variables were added or removed from the model based on if their relationship with price was significant and their effect on the model's ability to follow the assumptions of linear regressions. 

The square footage inside the house and square footage of the garage features were log transformed in order to make these variables more normal, to help the model remain normal.


# Final Model for Predicting Home Price

## Final Model Outline:
![Screenshot 2023-03-08 at 2 41 11 PM](https://user-images.githubusercontent.com/117129342/223826661-0c809b37-4508-4b0f-92f6-87d9ddb95279.png)

![Screenshot 2023-03-08 at 2 40 09 PM](https://user-images.githubusercontent.com/117129342/223826807-8831be31-ed1c-47e8-a53d-9bc43b9a50e4.png)

## Interpreting Final Model


* Our model explains about 55% of the variance in home price throughout this dataset.
* The overall model is significant based on a 0.05 alpha level.
* All of the coefficeints relationship's to price are significant, in other words a relationship between the feature and price exists 
* This model passes the Linearity and Homoscedasticity Assumptions
* Model has a Low condition number, meaning multicollinearity between features is very unlikely
* While the Residuals are not completely normal, they are fairly normal as can been seen in the Q-Q plot and histogram of residuals, while breaking this assumption is not ideal, it is acceptable in a model of this nature.
* The average error of this model is about half of a standard deviation in price, or about 200,000 dollars


### Interpreting Model Predictors

#### GreenBelt:
>Being adjacent to a greenbelt provided an increase in overall value of about $126,000.

#### Nuisance:
>The homes having reported traffic noise or some other nuisance was assocaited with an overall increase of $36,000

#### View:
>Having a view overall increased the value of a home but the quality of the view did not relate to home price increase in the most intuiative way. A fair view, the lowest quality of view, increased the value of a home by about \$106,000 compared with no view, which was greater than the increase for an average view. Excellent views still added the most value by a small margine when compared with no view at a $108,000 increase.

####  Construction Grade:
> The value associated with an increase in construction grade basically acted as expected with higher grades fetching higher prices. The breakdown of price increases for each grade when compared with a Fair graded house was \$52,000 for an Average grade, \$175,000 for Good, \$381,000 for Better, \$585,000 for Very Good, \$565,000 for Luxury amd \$757,000 for Mansion grade construction 

#### Number of Bathrooms:
> For each additional bathroom added to a house it will add about $28,000 in value to the home.


#### Sqft of living space:
> sqft_living was log transformed and therefore the interpretation of this of metric is that a 1% increase in sqft inside the home is associated with an increase in value of 480,000/100 or about $4,800 in home value  

#### Sqft of Garage Space:
>sqft_garage was log transformed and therefore the interpretation of this of metric is that a 1% increase in sqft of the garage is associated with a decrease in value of 12,000/100 or about $1,200 in home value  

#### Condition:
>As expected as the condition of a home increases, so does the value. An increase from a condition of poor to average resulted in a \\$60,000, poor to good was \$103,000, and poor to excellent was $155,000

#### Month sold:
> When comparing to a sale in janurary, the months that are associated with the highest increase in sales value are March, April and May with an associated increase of \$61,000, \$77,000 and \$ 95,000 respectively


# Conclusions and Recommendations


### Choosing locations to Build Upon

When looking for land we are looking for the cheapest property that will provide great home value, for this we recommend searching for a "Fair" view but no better, next to a greenbelt as these will raise the value of a home on average about \$106,000 and \$126,000 respectively.

### Home Construction

Increasing the construction grade will generally increase its value significantly but will also raise investment costs, for this reason we recommend choosing between a "Good","Better" and "Very Good" grade. We suggest running a cost/benefit analysis and choosing which of these options works best for your situation. 

### Home Presentation and Sale

When placing the home up for sale we recommend you spend a little extra to make sure it is in good condition as each step up in condition will net you around $50,000 in value, this could be as simple as cleaning and repainting and would provided a great return on investment.

We recommend trying to sell homes in the months of April or May to maximize the home sale price, choosing these months could net you around $80,000 more dollars when compared to selling the same house in another month.

# For More Information
See the full analysis in the [Jupyter Notebook](https://github.com/LayFish21/KC_Housing_Price_Prediction/blob/main/KC_Housing_Price_Predictor.ipynb) 

For additional info, contact Lucas Fishbein at FishbeinLucas@gmail.com

## Repository Structure

```
├── .gitignore
├── CONTRIBUTING.md
├── KC_Housing_Price_Prediction.ipynb
├── LICENSE.md
└── README.md
```