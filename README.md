# Banking-Products-Recommendation--Multioutput-Classification
Solution to Analytics Vidhya AmExpert 2021 Hackathon


## Problem Statement
XYZ Bank is a mid-sized private bank that includes a variety of banking products, such as savings accounts, current accounts, investment products, credit products, and home loans.

The Bank wants to predict the next set of products for a set of customers to optimize their marketing and communication campaigns.

The data available in this problem contains the following information:
- User Demographic Details : Gender, Age, Vintage, Customer Category etc.
- Current Product Holdings
- Product Holding in Next 6 Months (only for Train dataset)
Here, our task is to predict the next set of products (upto 3 products) for a set of customers (Test data) based on their demographics and current product holdings.

## Data Dictionary

### Train Data
Variable | Definition
--- | ---
Customer_ID | Unique ID for the customer 
Gender | Gender of the Customer
Age | Age of the Customer (in Years)
Vintage | Vintage for the Customer (In Months)
Is_Active | Activity Index, 0 :  Less frequent customer, 1 : More frequent customer
City_Category | Encoded Category of customer's city
Customer_Category | Encoded Category of the customer
Product_Holding_B1 | Current Product Holding (Encoded)
Product_Holding_B2 | Product Holding in next six months (Encoded) - Target Column

### Test Data
Variable | Definition
--- | ---
Customer_ID | Unique ID for the customer 
Gender | Gender of the Customer
Age | Age of the Customer (in Years)
Vintage | Vintage for the Customer (In Months)
Is_Active | Activity Index, 0 :  Less frequent customer, 1 : More frequent customer
City_Category | Encoded Category of customer's city
Customer_Category | Encoded Category of the customer
Product_Holding_B1 | Current Product Holding (Encoded)

## Data Preprocessing
1. Columns Product_Holding_B1 and Product_Holding_B2  are converted to list columns with ast.literal_eval during read_csv.
2. Train and test dataframes are merged.
3. The list in the columns Product_Holding_B1 and Product_Holding_B2 is split into separate columns into a new dataframe. These two dataframes are then merged to the original dataframe. 
4. **Missing Value Imputation** – Missing values are imputed with ‘N’
5. Encoding categorical variables
- a. Categorical Columns containing demographic information of the customers are one-hot encoded. 
- b. User_ID column is encoded with LabelEncoder() as other target encoders cannot be used. 
- c. The categorical columns derived from Product_Holding_B1 and Product_Holding_B2 columns are also encoded with LabelEncoder() 
6. The top 3 products for Holding B1 and B2 are retained in the train dataset while top 3 products for Holding B1 are retained. 
In both columns Product_Holding_B1 and Product_Holding_B2, we can see instances of customers having upto 6 to 7 products. However, we will be predicting only the top 3 products for each customer. Hence, only the top 3 product columns for Holding_B1 and Holding_B2 are retained. 


## Evaluation Metric
The competition evaluation metric for the hackathon is Mean Average Precision (MAP).


## Prediction Approach
- Since this is a multilabel classification problem, the normal classifiers will not work. Hence, MultiOutputClassifier along with the classifier algorithms is used to build the model.
- Logistic Regression, Naïve Bayes, tree based algorithm like Random Forest and ensemble models such as XGBoost and LightGBM are evaluated. 
- XGBoost model is selected as the final model as it achieved the higher accuracy than other models.


## Data Postprocessing
- The predicted values are in the form of a 2D array. It has to be processed and converted to original product codes before adding it to the submission file. 
1. The array is reshaped to a 1D ,inverse transformed with LabelEncoder to original categories and reshaped to its original shape.
2. The array is converted into a dataframe. 
3. 'N' (none) values in the dataframe are converted into null values
4. The dataframe is converted to a list
5. Null values are removed from the list
6. The values are mapped to a set and the set is converted to a list.
7. This list is added to the submission file. 


## Leaderboard
- [Public Leaderboard](https://datahack.analyticsvidhya.com/contest/amexpert-2021-machine-learning-hackathon/#LeaderBoard): 8th Rank
- [Private Leaderboard](https://datahack.analyticsvidhya.com/contest/amexpert-2021-machine-learning-hackathon/#LeaderBoard): 5th Rank
