# DataFrame Overview
- We have 7 X variables - 'gre_score', 'toefl_score', 'university_rating', 'sop', 'lor', 'cgpa', 'research' and y variable for prediction is - chance_of_admit
- All X variables were of data type int or float, which is acceptable data type for Linear Regression.
- There are total of 500 rows
- SOP, LOR, University Rating are in range 1 to 5, they seem to be ratings
- Serial Number is insignificant, after checking if all rows are unique serial number can be dropped
- cgpa is 10 scale grading, with max value of 9.92, and 8.56 is median cgpa.
- gre_scores are also on higher side as median gre_score is 317, with 340 as max marks
- toefl_score are also higher, with 107 being median marks.
- GRE, TOEFL marks and CGPA values suggest that good merit students are mostly applying.
- chance_of_admit is in percentage
- Each serial number is unique, there are no duplicates, Serial Number is removed from X features, as it holds no useful information regarding chance of admission.
- sop, lor, university rating have ordinal values (order matters), although these are not continuous, but can be taken as continuous for Linear Regression
- research is a categorical feature, since there are only 2 distinct values 0, 1, it can be taken as is for Linear Regression
- There are no missing values present in dataframe


# Univariate Analysis
- gre_score has shallow peak signifying few chances of outliers, and is very slight left skewness. Boxplot confirms absence of outliers. Below are overall statistics.
  - Skewness : -0.03984185809159066
  - Kurtosis : -0.7110644625938418
  - Mean : 316.472
  - Median : 317.0

- university_rating are mostly 3, followed by 2 and 4. Ratings are distinctly from 1 to 5.

- Sop is slightly left skewed, low kurtosis signifies shallow peak and few chance of outliers, and no outliers are confirmed with boxplot. Below are overall statistics.
  - Skewness : -0.22897239628779945
  - Kurtosis : -0.7057169536396795
  - Mean : 3.374
  - Median : 3.5

- lor is left skewed, low kurtosis signifies shallow peak and few chances of outliers. Boxplot shows few outliers around value 1, but the outlier can be kept as it is defined range of lor. Below are overall statistics.
  - Skewness : -0.1452903146082398
  - Kurtosis : -0.7457485105986423
  - Mean : 3.484
  - Median : 3.5

- cgpa is slightly left skewed, low kurtosis signifies shallow peak and few chances of outliers, and no outliers are confirmed with boxplot. Below are overall statistics.
  - Skewness : -0.026612517318359303
  - Kurtosis : -0.5612783980560527
  - Mean : 8.576439999999998
  - Median : 8.56

- chance_of_admit is left skewed, low kurtosis signifies shallow peak and few chances of outliers. There are few outliers observed in boxplot, but outliers are closer to the whisker and also lie in acceptable range, this outlier will not be removed. Below are overall statistics.
  - Skewness : -0.289966210041158
  - Kurtosis : -0.4546817998465431
  - Mean : 0.72174
  - Median : 0.72

- Number of students with research is slightly more than the number of students without research.
- Percentage of students with research experience are : 56.00000000000001%
- Percentage of students without research experience are : 44.0%


# Pre-Modelling Analysis (Linearity, VIF and p-values)
- Linearity condition is met, we see almost straight lines between all X and y variables
- cgpa is highly positively correlated with chance_admit, followed by gre_score and toefl_score (all are 80% or above approx), they seem to be primary variables for prediction.
- All X variables are positively correlated with chance_admit.
- VIF of all variables were identified to be lesser than 5, therefore all variables can be used for modelling.
- Overall Linear Regression Model is giving R2 adj of 81.8%, with all 7 variables
- sop and university_rating have high p_values, so these variables are not significant for our model. Their coefficients are also suggesting same.
- We will run Backward Selection of variables in next step to eliminate non-performing features.
- We designed a Backward Selection that would remove variables (one by one) with high VIFs (1st priority) and p-values (2nd priority) to get the best model.


# Linear Regression Model Evaluation
- Adj R2 of train set is 0.8184577289487925
- RMSE on Train set is: 0.0035331469389868714
- MAPE on train set is 6.855579749353616%
- Adj R2 of test set is 0.8056863883126606
- RMSE on Test Set is 0.003773020765116895
- MAPE on test set is 6.8724356583899695%

Therefore there is no overfitting or underfitting present in the model. Model is performing with adequate R2 and RMSE. 

## Normality of Errors
- Errors are left skewed, and shallow peak, suggesting few chances of outliers
- Shappiro Test is suggesting a non-Normal distribution, but this is due to some outliers on the left side, which are in acceptable range.
- Plot confirms that the overall Errors are approximating to Normal Distribution with Mean near to 0. Errors are left skewed, possibly due to some outliers. This much outliers should be acceptable (therefore the Shappiro test is not confirming Gaussian Distribution). 

Below is overall statistics.

- ShapiroResult(statistic=0.9312566782302102, pvalue=1.3099647192165179e-12)
- Skewness: -1.098089940306712
- Kurtosis: 2.560558510059634
- Mean Error: -4.2063574845485617e-16


## Heteroscedasticity
- Using GoldFeld test, p-value of 0.61 suggests that errors are almost const across all values of y_pred, so Heteroscedasticity is not present in OLS-LinearRegression model.
- ('F-statistic', 0.9592288620962849), ('p-value', 0.6139024845884469)]
- The plot of errors is also suggesting that errors are almost constant across values of y.


# Ridge Model Evaluation
Below are resp metrics wrt to alpha values
- 1e-05 Adj R2 Score : 0.7740742456671815
- 1e-05 Mean Squared Error : 0.0037367390322690873
- 0.0001 Adj R2 Score : 0.7740742023433888
- 0.0001 Mean Squared Error : 0.00373673903227027
- 0.001 Adj R2 Score : 0.7740737691023771
- 0.001 Mean Squared Error : 0.0037367390323885586
- 0.01 Adj R2 Score : 0.774069436384026
- 0.01 Mean Squared Error : 0.003736739044213913
- 0.1 Adj R2 Score : 0.7740260784967263
- 0.1 Mean Squared Error : 0.0037367402232375765
- 1.0 Adj R2 Score : 0.7735895455207402
- 1.0 Mean Squared Error : 0.0037368547090133723
- 10.0 Adj R2 Score : 0.7690174364632629
- 10.0 Mean Squared Error : 0.003745640370589866
- 50 Adj R2 Score : 0.7476201550410027
- 50 Mean Squared Error : 0.003839001667733219
- 100 Adj R2 Score : 0.7206267126324437
- 100 Mean Squared Error : 0.003969747678365019
- 200 Adj R2 Score : 0.6629594510073669
- 200 Mean Squared Error : 0.004234919661857691
- 500 Adj R2 Score : 0.4362375957339466
- 500 Mean Squared Error : 0.005137619731423437
- 700 Adj R2 Score : 0.23356391893200212
- 700 Mean Squared Error : 0.005790347269605879
- 1000 Adj R2 Score : -0.15075070260799195
- 1000 Mean Squared Error : 0.006759777664071061





# Lasso Model Evaluation
Below are resp metrics wrt to alpha values
- 1e-05 Adj R2 Score : 0.7740342905754989
- 0.0001 Adj R2 Score : 0.7736696003834076
- 0.001 Adj R2 Score : 0.7699697737315414
- 0.01 Adj R2 Score : 0.7176709004124979
- 0.1 Adj R2 Score : -20.39973506123193
- 1.0 Adj R2 Score : -1.7127070431091017e+30
- 10.0 Adj R2 Score : -1.7127070431091017e+30
- 50 Adj R2 Score : -1.7127070431091017e+30
- 100 Adj R2 Score : -1.7127070431091017e+30
- 200 Adj R2 Score : -1.7127070431091017e+30
- 500 Adj R2 Score : -1.7127070431091017e+30
- 700 Adj R2 Score : -1.7127070431091017e+30
- 1000 Adj R2 Score : -1.7127070431091017e+30



# ElasticNet Model Evaluation
Below are resp metrics wrt to alpha values
- 1e-05 Adj R2 Score : 0.7740535192504759
- 0.0001 Adj R2 Score : 0.7738641396186379
- 0.001 Adj R2 Score : 0.7719758783194217
- 0.01 Adj R2 Score : 0.7494616079255482
- 0.1 Adj R2 Score : -0.2910072098734655
- 1.0 Adj R2 Score : -1.7127070431091017e+30
- 10.0 Adj R2 Score : -1.7127070431091017e+30
- 50 Adj R2 Score : -1.7127070431091017e+30
- 100 Adj R2 Score : -1.7127070431091017e+30
- 200 Adj R2 Score : -1.7127070431091017e+30
- 500 Adj R2 Score : -1.7127070431091017e+30
- 700 Adj R2 Score : -1.7127070431091017e+30
- 1000 Adj R2 Score : -1.7127070431091017e+30



## L1_ratio optimization
Below are resp metrics wrt to l1 ratio
- 1e-05 Adj R2 Score : 0.7740727096594135
- 1e-05 Mean Squared Error : 0.003736739033492709
- 0.0001 Adj R2 Score : 0.7740727062783137
- 0.0001 Mean Squared Error : 0.0037367390334947306
- 0.001 Adj R2 Score : 0.7740726728890448
- 0.001 Mean Squared Error : 0.0037367390335165582
- 0.01 Adj R2 Score : 0.7740723380125124
- 0.01 Mean Squared Error : 0.003736739033829692
- 0.1 Adj R2 Score : 0.7740689233865973
- 0.1 Mean Squared Error : 0.003736739046969901
- 1.0 Adj R2 Score : 0.7740342905754989
- 1.0 Mean Squared Error : 0.0037367401688909105


# Final Verdict
The Linear Regression (LR) model has outperformed the Ridge, Lasso, and ElasticNet models. The model is providing a satisfactory adjusted RÂ² of around 81.8% on the training set and 80.6% on the test set, which indicates a good fit and prediction accuracy. Additionally, metrics such as RMSE and MAPE show consistent results across both training and test sets, suggesting no overfitting or underfitting.

Furthermore, VIF values and p-values suggest that not all features are equally significant, and a Backward Selection approach was correctly used to remove insignificant features such as SOP and University Rating.

While Ridge, Lasso, and ElasticNet were evaluated and tuned, none of them surpassed the performance of the Linear Regression model. 


# Scope of Improvement
Using Polynomial transformation, for dropped variables might help in improving R2, by capturing information more significantly. Overall, polynomial transformations can also be explored on other selected variables that might have improved model metrics. 

Using PCA or PLS transformation on X features might also aid in capturing more information of X variables, improving overall model performance. 

Using more complex models also have a higher chance of improved model metrics.
