# House Sale Price Predictor: Project Overview
- Created a tool that estimates house sale prices (RMSE ~$27,000) for based to help people negotiate selling or buying a home
- Built the tool on house sale data compiled from Ames, Iowa from 2006 to 2010. The data were published by Dean De Cock in the Journal of Statistics Education
- Implemented a multivariate linear regression model with k-fold cross validation

## Code and Resources Used
**Python Version:** 3.7

**Packages:** pandas, numpy, sklearn, matplotlib

**Dataset Documentation:** https://www.tandfonline.com/doi/abs/10.1080/10691898.2011.11889627

## Data Overview
For each house listing, we got 82 features reporting information about the house and the sale. The data included numerical information (eg., square footage of the garage) and categorical information (eg., quality ratings or type of street).

## Feature Engineering
To make the features suitable for modeling, I applied several general modifications:
- Removed any column missing more than 5% of its data.
- Removed any text column missing 1 or more value
- Filled remaining missing values in numerical columns with the mode of each column

I then read the documentation and manually explored each variable in order to remove extraneous variables or information pertaining to the final sale. I made the following modifications:
- Replaced 'Yr Sol', 'Yr Built', and 'Year Remod/Add' with two new features: 'Age at Sale' and 'Yrs Since Remod'
- Removed extraneous information: 'PID' and 'Order' columns
- Removed columns leaking information about the final sale

## Feature Selection
### Categorical Features
First, I identified several numeric columns that needed to be converted to categorical: Overall condition, Overall quality, and MS SubClass. Then, I converted the remaining object type columns to categorical. I filtered out any categorical variables where more than 95% of values belonged to one category to prevent low variance from impacting the model. I then dummy coded the cateogrical variables so that each unique value within each category would be represented as a binary array.

### Numerical features
I computed the Pearson correlation coefficient between all numeric variables in order to identify high correlates with SalePrice (the target variable), and to remove or consolidate features with a high relationship to one another (redundancy).
![image](https://user-images.githubusercontent.com/97380323/173205815-c92a0aa7-773b-4548-81f5-fd0d81678d70.png)

I removed the 'Garage Cars' column as it correlated highly with 'Garage Area', and they convey mostly the same thing. Garage Area is a more robust metric, so I kept it for our model.

I filtered out any numeric variables that had less than a .4 absolute correlation with Sale Price.

### Cross Validation
The model function I implemented allows the user to choose the number of folds (k) to validate the model. The program divides the data into k folds. For each fold, it trains the model on the rest of the data and tests on that fold. For each instance of the model, the root mean squared error is computed. The RMSE is averaged across all model instances to determine model performance. With 5-fold cross validation, the model predicts house sale prices within a RMSE of ~$27,000.
