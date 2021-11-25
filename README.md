# Predicting Weekly Sales for Walmart
## Authors
* Claire Brekken
* Rachel Brynsvold
* Asher Khan
## Description
In this project we try to predict the future weekly sales based on the historical sales data from 45 Walmart stores. Each store has multiple departments and our goal is to predict future sales for each department. 
## Technical Details
### Preprocessing
The first step in our project was to remove noise from the data. When we look at the sales pattern of a department, we can see they are very similar across stores, therefore we are able to apply SVD to reduce the noise. We apply SVD on each department and choose the eight principal components. We aggregate the principal components for each department into a dataframe and perform the rest of the analysis on this clean data.
### Models and Tuning
After using SVD to pick the principal components, we use linear regression to predict the future weekly sales. One model we first created used the most recent week’s Sales as the prediction value for future weeks, but testing and intuition reveals this is not an accurate model. Using only the most recent week as a feature ignores seasonal shopping trends and sales such as Black Friday and Christmas shopping. Just because a store had high sales on Christmas week does not mean the store will have high sales the week after. It most likely has lower sales, because people are no longer Christmas shopping, and most sales have ended. 

Using this logic, we decided to use the sales history from the same week of the previous year. For example, if we are trying to predict the sales for week 43 of year 2021, then we will use the sales history from week 43 of year 2020. This helps take into consideration seasonal shopping trends and sales, and the impact these trends and sales have on the surrounding weeks. This model is far more accurate than the first model, but using only the past year as a feature significantly limits our model. Using store sales data from a year that experienced a recession will not act as a good predictor for a year that no longer has a recession, and vice versa. 

Since store sales can vary week to week and year to year, we landed on using a linear regression model that uses the week number and year as features. Since we learned that the same week number across different years can have similar Sales from our previous modeling attempt, and because there is a set number of week numbers in the data set, we decided to treat the week number as a categorical feature in the model. On the other hand, we had to treat the year as a numerical feature because the prediction set will have years that were not seen in the training set. This final model, along with the data cleaned with SVD, is what we decided to use for our predictions. 

### Findings
One important thing we discovered is our model is only good at predicting the future weekly sales after performing SVD on the dataset. The mean WMAE for our model without SVD on the dataset is 1659.709, while the WMAE for our model with SVD on the dataset is 1608.776. We can conclude that this reduction in WMAE is due to the reduction of noise in the data.  Departments 25 and 31 were selected for visual illustration:
![alt text](https://github.com/asher657/Walmart-Weekly-Sales/blob/main/NoiseReduction.png?raw=true)
Interestingly, we do not see a big visual difference between the ‘with SVD’ and ‘without SVD’ charts for either department.  However, this relatively small difference in numerical values of Weekly_Sales has a meaningful effect on our WMAE value.

### Results
Run on a Lenovo P330, 3.70GHz, 16GB Memory
| Fold | Accuracy (WAE) |
| ---  | ---------------|
|1|1941.581|
|2|1363.462|
|3|1382.497|
|4|1527.280|
|5|2310.469|
|6|1635.783|
|7|1682.747|
|8|1399.604|
|9|1418.078|
|10|1426.258|
|Average|1608.776|
Total Run Time: 667.69908 minutes


