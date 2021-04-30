# MPC Final Project

__Team members__
* Henri Aïdasso
* Octaviana Diaconu

<br>

## __Houses Price prediction__

Above all, we used some python librairies for purpose, listed in the __requirements.txt__ file, please make sure your environment satisfy these requirements before running the notebook. Otherwise, you may also just read the notebook's outputs.

<br>

#### __IMPORTANT !__
It's worth to underline that we created a section for all __utilities functions__ that we used in the notebook, right after our descriptive analysis of variables. Also, __the notebook contains more specific details__ of each step of our process and the noted results. Here we will just explain globally the process we followed and the end results.

<br>

### __Descriptive analysis__

From the correlation matrix we notice that :
- waterfront, view, sqft_basement, yr_renovated: have more than half of their values nulls
- waterfront is mostly null
- lat, long: have almost constants values

and there are strong correlation between those variables taken in pairs.

<br>

### __Simple Linear Regression__

We tried each single variable and compared their model's r-squared, residuals and p-value values. Then we compared the generalization error made using each one of them.
The best variables here were __sqft_living__ and __grade__. 

<br>

### __Multiple Linear Regression__

In a first time we computed the generalization error made by the model using all the 18 simple variables. That error was less than all the simple linear regressions models one.

#### __Variable selection__
We therefore used several variable selection algorithm aiming at selecting the best set of variables that make the lowest possible error in multiple linear regression. The algorithms implemented are : 

- Forward and Backward selection based on generalization error with strict stop criterion.

- Forward and Backward selection based on generalization error without stop criterion: keeping the best model

- Forward selection based on variables influence (p-value) without stop criterion (keep the best). No backward because we wanted the most significant variables together.

After applying each variables selection, we compare their generalization errors with __Mean Squared Error__ and __K-Fold Cross Validation.__

We figured at the end that we had a better selection of 15 variables found with the Forward selection based on generalization error. 

<br>

### __Multiple Nonlinear Regression__

We added in a first variable squared, then variables at power 2 and 3 and finally variables from power 2 to 6 and did the previous variables selection. 

At each step we found a better selection that decreased the generalization error by comparing __MSE__ and __K-Fold__ cross validation errors. Nevertheless we found that p-value was not at some point the best criterion for selecting our variables.

Also, having a lower generalization error did not always mean that we had a lower error on the Kaggle competition prediction, espcially when we used 90 variables (variables at power 2-5 added).

At the end the best performance (lowest error) that we found was after adding variables at powers 2 to 10
(x¹ to x¹⁰) and performing a forward variable selection based on generalization error with early stopping.
Also, predicting new values (competiton) using only the train split dataset was better with that latest model than using the whole dataset.

A summary of our results is in the table below:

- SLR : Simple Linear Regression
- MLR : Multiple Linear Regression
- MNLR: Multiple Nonlinear Regression

If p-value is not specified, then we used generalization error for the selection.


| Model                               | Gen. Error | Kaggle Error |
|-------------------------------------|:----------:|:------------:|
| SLR with sqft_living                |   6.96657  |    6.33249   |
| MLR with 18 variables               |   4.44719  |    3.54670   |
| MLR with 15 variables (gen. error)  |   4.44042  |    3.55712   |
| MNLR with 15 best and sqft_living^2 |   3.80194  |    3.06725   |
| MNLR with power 2 forward p-value   |   3.57610  |    3.24439   |
| MNLR with power 2 forward           |   3.57433  |    2.89440   |
| MNLR with power 2,3 forward         |   3.20178  |    2.57199   |
| MNLR with power 2,3 forward strict  |   3.20014  |    2.57134   |
| MNLR with power 2-5 forward strict  |   3.18872  |    2.59864   |
| MNLR with power 2-10 forward strict |   3.18157  |  __2.48896__ |


<br>
Computing started to take a long time. 
The best selection found so far is therefore the one made with variables x¹ to x¹⁰ selected in forward based on generalization error with strict stop criterion. 
<br>

__MNLR with power 2-10 forward strict__


<br><br><br>

### __Going further with XGBoost !__
There are several ML algorithms to perform linear regression aiming at decreasing the mean squared error (among over loss metrics). We choosed to give a try to the Extreme Gradient Boosting.

The results found are below :


| Model                               | Gen. Error | Kaggle Error |
|-------------------------------------|:----------:|:------------:|
|  XGBoost with features selection    |   1.30622  |  __1.40666__ |

<br><br>

We could have better performances with some data pre-processing and hyperparamters tuning but those are way out of the scope of this course.