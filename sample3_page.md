## Kaggle Competition: Predicting Housing Prices Project Page

**Project description:**
This machine learning competitionn gives the user a dataset of house and prices, along with specific features of the houses that include quality of beds and baths, lot size, yard size, neighborhood information, year built, and so on. The goal of the competition is to build and train a model that will accurately predict the sale price of houses in a new dataset. For my model, I use a Random Forest classifier, which is a model that takes the mean value of multiple decision trees. I used this model, along with specific features that I tested based on which feature combination would provide the lowest mean absolute error with the validation data used.

### 1. Preprocessing Steps

Kaggle provides the housing data for a training set and a testing set. Some important libraries to note is the use of python pandas, as well as scikit-learn, which is an open source machine learning library. There are many features provided in the dataset to be used for modeling, however some may be more useful for the model than others. Using too many features can skew the model with irrelevant information. Therefore, I have only used a select number of features, such as the year sold, year built, lot size and area, quality of beds and baths, neighborhood information, and a few others.

```python
features = ['LotArea', 'HouseStyle', 'Street', 'Neighborhood', 'Foundation', 'BldgType', 'YearBuilt', 'YrSold', 'YearRemodAdd', 'Electrical','1stFlrSF', '2ndFlrSF', 'FullBath', 'BedroomAbvGr', 'HalfBath', 'GrLivArea', 'KitchenAbvGr', 'TotRmsAbvGrd', 'OverallCond', 'OverallQual', 'PoolArea', 'EnclosedPorch', 'OpenPorchSF', 'WoodDeckSF', 'ScreenPorch', 'Fireplaces']
```

The training data must then be split into training and validation data. Since this is a regression problem where the goal is to make a prediction, the model must first be trained with provided y values so it can learn a pattern, and then the validation data is used to see how well the model has learned to predict those y values.

There are several preprocessing steps required before the data can be fed into the model. We will first drop any categorical columns (columns that have data stored with names or labels rather than numeric values) that are not worth encoding, which might include columns that have mostly null values, or too many unique values. Then, an ordinal encoder is applied to the categorical columns that are kept. An ordinal encoder allows for the categorical data to be transformed into numerical format based on their ordinal relationship with each other. After the encoder, the data is imputed so that any remaining null values are replaced with mathematically estimated values. It is important to keep in mind that some null data may not necessarily be missing data. For example, a feature that records how big the porch of a house is may be null because that house does not have a porch.

<img src="images/kagglesc1.png?raw=true"/>

After imputation is complete, that data is finally ready for our model to be fitted with.

### 2. Random Forest Regression Model

As discussed earlier, for this regression problem  the Random Forest classifier is used. This classifier creates a set of decision trees and collects votes from the different trees to decide a final prediction. Below we have an image of a single decision tree, and then multiple decision trees that are interconnected.

<p align="center">
  <img src="images/decisiontree.png?raw=true" height="425"/>
  <br><br>
  <img src="images/randomforest.png?raw=true" height="425"/>
</p>


We define our Random Forest Regression model with n_estimators=100, which means we will use 100 decision trees in the forest. We record the accuracy of the model by noting the value of the mean absolute error.

<img src="images/kagglesc2.png?raw=true"/>

Find the full Kaggle code [here](https://www.kaggle.com/code/nnennaeze/exercise-machine-learning-competitions)
