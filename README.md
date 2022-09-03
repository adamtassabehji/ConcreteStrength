# ConcreteStrength

Concrete compressive strength tests are among the most common forms of testing conducted in the construction industry.

On behalf of a client relying on an AutoML solution, I was tasked with building a regression to improve their modelling capability. Due to the sensitivity/privacy of this engagement, exploratory data analysis and the final variables used in the model have been excluded (this is just a prototype).

### Data Cleaning & Preparation

- Dropped rows missing the target variable
- Dropped columns missing more than 20% of values
- Transformed categorical columns using target encoding
- Replaced outliers with the 3% and 97% percentiles
- Used KNN imputer to deal with order missing values (after scaling the data)


### Feature Engineering 

Polynomial features are created by raising existing features to an exponent.

For example, if a dataset had one input feature X, then a polynomial feature would be the addition of a new feature (column) where values were calculated by squaring the values in X, e.g. X^2. This process can be repeated for each input variable in the dataset, creating a transformed version of each. It will also explore the iteractions of different variables by multiplying them

A total of 465 features were engineered.

### Feature Selection

Initially a univarite test was conducted to select the 100 variables with the highest predictive power(filter: Kbest), followed by a multivariate test (wrapper: forward step linear regression). 

With so few records, I wanted to have atleast 10 records per variable to ensure robustness and prevent overfitting.

![Screenshot 2022-09-03 at 17 52 01](https://user-images.githubusercontent.com/56136026/188280732-9f5bdbc3-7c0c-44a8-a46f-2ec8a2502eb4.png)

![Screenshot 2022-09-03 at 17 52 07](https://user-images.githubusercontent.com/56136026/188280736-79c54847-08a1-45cb-9c23-394d17843ed8.png)

### Modelling

I visualized both train and test scores to validate that non-linear models would overfit with this dataset (Only 163 records)

**LinearRegression** fits a linear model with coefficients to minimize the residual sum of squares between the observed targets in the dataset, and the targets predicted by the linear approximation.

**Least-angle regression (LARS)** is a regression algorithm for high-dimensional data, At each step, it finds the feature most correlated with the target. When there are multiple features having equal correlation, instead of continuing along the same feature, it proceeds in a direction equiangular between the features

**Ridge regression** addresses some of the problems of Ordinary Least Squares (OLS) by imposing a penalty on the size of the coefficients. The ridge coefficients minimize a penalized residual sum of squares

**Bayesian regression** techniques can be used to include regularization parameters in the estimation procedure: the regularization parameter is not set in a hard sense but tuned to the data at hand. The advantages of Bayesian Regression are:It adapts to the data at hand and It can be used to include regularization parameters in the estimation procedure. Due to the Bayesian framework, the weights found are slightly different to the ones found by Ordinary Least Squares. However, Bayesian Ridge Regression is more robust to ill-posed problems.

**The Automatic Relevance Determination (as ARDRegression)** is a kind of linear model which is very similar to the Bayesian Ridge Regression, but that leads to smaller coefficients.

**The HuberRegressor** is different to Ridge because it applies a linear loss to samples that are classified as outliers.

![Screenshot 2022-09-03 at 17 52 21](https://user-images.githubusercontent.com/56136026/188280704-e374eebe-0382-4948-8f17-f17529a96dd3.png)

I also used methods such as stacking and voting. 


![Screenshot 2022-09-03 at 18 24 14](https://user-images.githubusercontent.com/56136026/188281750-c4647578-0916-4129-8a44-dab8b4101006.png)




### Results
The best performing algorithm was stacking.

![Screenshot 2022-09-03 at 17 52 58](https://user-images.githubusercontent.com/56136026/188280717-acb3a83e-2329-4b15-9992-e91ee41d8c13.png)


**Linear Regression R2:**

Median= 0.823, Average = 0.812


**Linear Regression RMSE:**

Median = 6.862, Average = 6.808 MPa

**Stacking R2:**

Median = 0.827, Average = 0.816

**Stacking RMSE:**

Median = 6.797, Average = 6.868 


![Screenshot 2022-09-03 at 17 53 20](https://user-images.githubusercontent.com/56136026/188280719-9b0cd698-3d6c-417a-886f-90f5c0d0b8c2.png)
