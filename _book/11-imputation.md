# Imputation (Missing Data)

Imputation is a statistical procedure where you replace missing data with *some values*

-   Unit imputation = single data point

-   Item imputation = single feature value

Imputation is usually seen as the illegitimate child of statistical analysis. Several reasons that contribute to this negative views could be:

(1) Peopled hardly do imputation correctly (which will introduce bias to your estimates)
(2) Imputation can only be applied to a small range of problems correctly

If you have missing data on $y$ (dependent variable), you probability would not be able to do any imputation appropriately. However, if you have certain type of missing data (e.g., non-random missing data) in the $x$'s variable (independent variables), then you can still salvage your collected data points with imputation.

We also need to talk why you would want to do imputation in the first place. If your purpose is inference/ explanation (valid statistical inference not optimal point prediction), then imputation would not offer much help [@Rubin_1996]. However, if your purpose is prediction, you would want your standard error to be reduced by including information (non-missing data) on other variables of a data point. Then imputation could be the tool that you're looking for.

For most software packages, it will use listwise deletion or casewise deletion to have complete case analysis (analysis with only observations with all information). Not until recently that statistician can propose some methods that are a bit better than listwise deletion which are maximum likelihood and multiple imputation.

"Judging the quality of missing data procedures by their ability to recreate the individual missing values (according to hit rate, mean square error, etc) does not lead to choosing procedures that result in valid inference", [@Rubin_1996]

Missing data can make it more challenging to big datasets.

## Assumptions

### Missing Completely at Random (MCAR) {#missing-completely-at-random-mcar}

Missing Completely at Random, MCAR, means there is no relationship between the missingness of the data and any values, observed or missing. Those missing data points are a random subset of the data. There is nothing systematic going on that makes some data more likely to be missing than others.

The probability of missing data on a variable is unrelated to the value of it or to the values of any other variables in the data set.

**Note**: the "missingness" on Y can be correlated with the "missingness" on X We can compare the value of other variables for the observations with missing data, and observations without missing data. If we reject the t-test for mean difference, we can say there is evidence that the data are not MCAR. But we cannot say that our data are MCAR if we fail to reject the t-test.

-   the propensity for a data point to be missing is completely random.
-   There's no relationship between whether a data point is missing and any values in the data set, missing or observed.
-   The missing data are just a random subset of the data.

### Missing at Random (MAR) {#missing-at-random-mar}

Missing at Random, MAR, means there is a systematic relationship between the propensity of missing values and the observed data, but not the missing data. Whether an observation is missing has nothing to do with the missing values, but it does have to do with the values of an individual's observed variables. So, for example, if men are more likely to tell you their weight than women, weight is MAR.

MAR is weaker than MCAR

$$
P(Y_{missing}|Y,X)= P(Y_{missing}|X)
$$

The probability of Y missing given Y and X equal to the probability of of Y missing given X. However, it is impossible to provide evidence to the MAR condition.

-   the propensity for a data point to be missing is not related to the missing data, but it is related to some of the observed data. In another word, there is a systematic relationship between the propensity of missing values and the observed data, but not the missing data.

    -   For example, if men are more likely to tell you their weight than women, weight is MAR

-   MAR requires that the cause of the missing data is unrelated to the missing values but may be related to the observed values of other variables.

-   MAR means that the missing values are related to observed values on other variables. As an example of CD missing data, missing income data may be unrelated to the actual income values but are related to education. Perhaps people with more education are less likely to reveal their income than those with less education

### Ignorable

The missing data mechanism is ignorable when

(1) The data are [MAR](#missing-at-random-mar)
(2) the parameters in the function of the missing data process are unrelated to the parameters (of interest) that need to be estimated.

In this case, you actually don't need to model the missing data mechanisms unless you would like to improve on your accuracy, in which case you still need to be very rigorous about your approach to improve efficiency in your parameters.

### Nonignorable

Missing Not at Random, MNAR, means there is a relationship between the propensity of a value to be missing and its values.

Example: people with the lowest education are missing on education or the sickest people are most likely to drop out of the study.

MNAR is called [Nonignorable] because the missing data mechanism itself has to be modeled as you deal with the missing data. You have to include some model for why the data are missing and what the likely values are.

Hence, in the case of nonignorable, the data are not MAR. Then, your parameters of interest will be biased if you do not model the missing data mechanism. One of the most widely used approach for nonignorable missing data is [@Heckman_1976]

-   Another name: Missing Not at Random (MNAR): there is a relationship between the propensity of a value to be missing and its values

    -   For example, people with low education will be less likely to report it.\

-   We need to model why the data are missing and what the likely values are.

-   the missing data mechanism is related to the missing values\

-   It commonly occurs when people do not want to reveal something very personal or unpopular about themselves\

-   Complete case analysis can give highly biased results for NI missing data. If proportionally more low and moderate income individuals are left in the sample because high income people are missing, an estimate of the mean income will be lower than the actual population mean.

## Solutions to Missing data

### Listwise Deletion

Also known as complete case deletion only where you only retain cases with complete data for all features.

Advantages:

-   Can be applied to any statistical test (SEM, multi-level regression, etc.)

-   In the case of MCAR, both the parameters estimates and its standard errors are unbiased.

-   In the case of MAR among independent variables (not depend on the values of dependent variables), then listwise deletion parameter estimates can still be unbiased. [@Little_1992] For example, you have a model $y=\beta_{0}+\beta_1X_1 + \beta_2X_2 +\epsilon$ if the probability of missing data on X1 is independent of Y, but dependent on the value of X1 and X2, then the model estimates are still unbiased.

    -   The missing data mechanism the depends on the values of the independent variables are the same as stratified sampling. And stratified sampling does not bias your estimates
    -   In the case of logistic regression, if the probability of missing data on any variable depends on the value of the dependent variable, but independent of the value of the independent variables, then the listwise deletion will yield biased intercept estimate, but consistent estimates of the slope and their standard errors [@Vach_1994]. However, logistic regression will still fail if the probability of missing data is dependent on both the value of the dependent and independent variables.
    -   Under regression analysis, listwise deletion is more robust than maximum likelihood and multiple imputation when MAR assumption is violated.

Disadvantages:

-   It will yield a larger standard errors than other more sophisticated methods discussed later.
-   If the data are not MCAR, but MAR, then your listwise deletion can yield biased estimates.
-   In other cases than regression analysis, other sophisticated methods can yield better estimates compared to listwise deletion.

### Pairwise Deletion

This method could only be used in the case of linear models such as linear regression, factor analysis, or SEM. The premise of this method based on that the coefficient estimates are calculated based on the means, standard deviations, and correlation matrix. Compared to listwise deletion, we still utilized as many correlation between variables as possible to compute the correlation matrix.

Advantages:

-   If the true missing data mechanism is MCAR, pair wise deletion will yield consistent estimates, and unbiased in large samples

-   Compared to listwise deletion: [@Glasser_1964]

    -   If the correlation among variables are low, pairwise deletion is more efficient estimates than listwise
    -   If the correlations among variables are high, listwise deletion is more efficient than pairwise.

Disadvantages:

-   If the data mechanism is MAR, pairwise deletion will yield biased estimates.
-   In small sample, sometimes covariance matrix might not be positive definite, which means coefficients estimates cannot be calculated.

**Note**: You need to read carefully on how your software specify the sample size because it will alter the standard errors.

### Dummy Variable Adjustment

Also known as Missing Indicator Method or Proxy Variable

Add another variable in the database to indicate whether a value is missing.

Create 2 variables

```{=tex}
\begin{equation}
D=
\begin{cases}
1 & \text{data on X are missing} \\
0 & \text{otherwise}\\
\end{cases}
\end{equation}
```
```{=tex}
\begin{equation}
X^* = 
\begin{cases}
X & \text{data are available} \\
c & \text{data are missing}\\
\end{cases}
\end{equation}
```
**Note**: A typical choice for c is usually the mean of X

Interpretation:

-   Coefficient of D is the the difference in the expected value of Y between the group with data and the group without data on X.
-   Coefficient of X\* is the effect of the group with data on Y

Disadvantages:

-   This method yields bias estimates of the coefficient even in the case of MCAR [@Jones_1996]

### Imputation

#### Mean, Mode, Median Imputation

-   Bad:

    -   Mean imputation does not preserve the relationships among variables
    -   Mean imputation leads to An Underestimate of Standard Errors → you're making Type I errors without realizing it.
    -   Biased estimates of variances and covariances [@Haitovsky_1968]
    -   In high-dimensions, mean substitution cannot account for dependence structure among features.

#### Maximum Likelihood {#maximum-likelihood}

When missing data are MAR and monotonic (such as in the case of panel studies), ML can be adequately in estimating coefficients.

Monotonic means that if you are missing data on X1, then that observation also has missing data on all other variables that come after it.

ML can generally handle linear models, log-linear model, but beyond that, ML still lacks both theory and software to implement.

##### Expectation-Maximization Algorithm (EM Algorithm)

An iterative process:

(1) Other variables are used to impute a value (Expectation).
(2) Check whether the value is most likely (Maximization).
(3) If not, it re-imputes a more likely value.

You start your regression with your estimates based on either listwise deletion or pairwise deletion. After regressing missing variables on available variables, you obtain a regression model. Plug the missing data back into the original model, with modified variances and covariances For example, if you have missing data on $X_{ij}$ you would regress it on available data of $X_{i(j)}$, then plug the expected value of $X_{ij}$ back with its $X_{ij}^2$ turn into $X_{ij}^2 + s_{j(j)}^2$ where $s_{j(j)}^2$ stands for the residual variance from regressing $X_{ij}$ on $X_{i(j)}$ With the new estimated model, you rerun the process until the estimates converge.

Advantages:

(1) easy to use
(2) preserves the relationship with other variables (important if you use Factor Analysis or Linear Regression later on), but best in the case of Factor Analysis, which doesn't require standard error of individuals item.

Disadvantages:

(1) Standard errors of the coefficients are incorrect (biased usually downward - underestimate)
(2) Models with overidentification, the estimates will not be efficient

##### Direct ML (raw maximum likelihood)

Advantages

(1) efficient estimates and correct standard errors.

Disadvantages:

(1) Hard to implements

#### Multiple Imputation

MI is designed to use "the Bayesian model-based approach to *create* procedures, and the frequentist (randomization-based approach) to *evaluate* procedures". [@Rubin_1996]

MI estimates have the same properties as [ML](#maximum-likelihood) when the data is [MAR](#missing-at-random-mar)

-   Consistent
-   Asymptotically efficient
-   Asymptotically normal

MI can be applied to any type of model, unlike [Maximum Likelihood](#maximum-likelihood) that is only limited to a small set of models.

A drawback of MI is that it will produce slightly different estimates every time you run it. To avoid such problem, you can set seed when doing your analysis to ensure its reproducibility.

##### Single Random Imputation

Random draws form the residual distribution of each imputed variable and add those random numbers to the imputed values.

For example, if we have missing data on X, and it's MCAR, then

(1) regress X on Y ([Listwise Deletion] method) to get its residual distribution.

(2) For every missing value on X, we substitute with $\tilde{x_i}=\hat{x_i} + \rho u_i$ where

    -   $u_i$ is a random draw from a standard normal distribution
    -   $x_i$ is the predicted value from the regression of X and Y
    -   $\rho$ is the standard deviation of the residual distribution of X regressed on Y.

However, the model you run with the imputed data still thinks that your data are collected, not imputed, which leads your standard error estimates to be too low and test statistics too high.

To address this problem, we need to repeat the imputation process which leads us to repeated imputation or multiple random imputation.

##### Repeated Imputation

"Repeated imputations are draws from the posterior predictive distribution of the missing values under a specific model , a particular Bayesian model for both the data and the missing mechanism".[@Rubin_1996]

Repeated imputation, also known as, multiple random imputation, allows us to have multiple "completed" data sets. The variability across imputations will adjust the standard errors upward.

The estimate of the standard error of $\bar{r}$ (mean correlation estimates between X and Y) is $$
SE(\bar{r})=\sqrt{\frac{1}{M}\sum_{k}s_k^2+ (1+\frac{1}{M})(\frac{1}{M-1})\sum_{k}(r_k-\bar{r})^2}
$$ where M is the number of replications, $r_k$ is the the correlation in replication k, $s_k$ is the estimated standard error in replication k.

However, this method still considers the parameter in predicting $\tilde{x}$ is still fixed, which means we assume that we are using the true parameters to predict $\tilde{x}$. To overcome this challenge, we need to introduce variability into our model for $\tilde{x}$ by treating the parameters as a random variables and use Bayesian posterior distribution of the parameters to predict the parameters.

However, if your sample is large and the proportion of missing data is small, the extra Bayesian step might not be necessary. If your sample is small or the proportion of missing data is large, the extra Bayesian step is necessary.

Two algorithms to get random draws of the regression parameters from its posterior distribution:

-   [Data Augmentation]
-   Sampling importance/resampling (SIR)

Authors have argued for SIR superiority due to its computer time [@King_2001]

###### Data Augmentation

Steps for data augmentation:

(1) Choose starting values for the parameters (e.g., for multivariate normal, choose means and covariance matrix). These values can come from previous values, expert knowledge, or from listwise deletion or pairwise deletion or EM estimation.
(2) Based on the current values of means and covariances calculate the coefficients estimates for the equation that variable with missing data is regressed on all other variables (or variables that you think will help predict the missing values, could also be variables that are not in the final estimation model)
(3) Use the estimates in step (2) to predict values for missing values. For each predicted value, add a random error from the residual normal distribution for that variable.
(4) From the "complete" data set, recalculate the means and covariance matrix. And take a random draw from the posterior distribution of the means and covariances with Jeffreys' prior.
(5) Using the random draw from step (4), repeat step (2) to (4) until the means and covariances stabilize (converged).

The iterative process allows us to get random draws from the joint posterior distribution of both data nd parameters, given the observed data.

Rules of thumb regarding convergence:

-   The higher the proportion of missing, the more iterations
-   the rate of convergence for EM algorithm should be the minimum threshold for DA.
-   You can also check if your distribution has been converged by diagnostic statistics Can check [Bayesian Diagnostics](https://bookdown.org/mike/bayesian_analysis/diag.html) for some introduction.

Types of chains

1.  **Parallel**: Run a separate chain of iterations for each of data set. Different starting values are encouraged. For example, one could use bootstrap to generate different data set with replacement, and for each data set, calculate the starting values by EM estimates.

    -   Pro: Run faster, and less likely to have dependence in the resulting data sets.
    -   Con: Sometimes it will not converge

2.  **Sequential** one long chain of data augmentation cycles. After burn-in and thinning, you will have to data sets

    -   Pro: Converged to the true posterior distribution is more likely.
    -   Con: The resulting data sets are likely to be dependent. Remedies can be thinning and burn-in.

**Note on Non-normal or categorical data** The normal-based methods still work well, but you will need to do some transformation. For example,

-   If the data is skewed, then log-transform, then impute, the exponentiate to have the missing data back to its original metric.
-   If the data is proportion, logit-transform, impute, then de-transform the missing data.

If you want to impute non-linear relationship, such as interaction between 2 variables and 1 variable is categorical. You can do separate imputation for different levels of that variable separately, then combined for the final analysis.

-   If all variables that have missing data are categorical, then **unrestricted multinomial model** or **log-linear model** is recommended.
-   If a single categorical variable, **logistic (logit) regression** would be sufficient.

#### Nonparametric/ Semiparametric Methods

##### Hot Deck Imputation

-   Used by U.S. Census Bureau for public datasets
-   approximate Bayesian bootstrap
-   A randomly chosen value from an individual in the sample who has similar values on other variables. In other words, find all the sample subjects who are similar on other variables, then randomly choose one of their values on the missing variable.

When we have $n_1$ cases with complete data on Y and $n_0$ cases with missing data on Y

-   Step 1: From $n_1$, take a random sample (with replacement) of $n_1$ cases
-   Step 2: From the retrieved sample take a random sample (with replacement) of $n_0$ cases
-   Step 3: Assign the $n_0$ cases in step 2 to $n_0$ missing data cases.
-   Step 4: Repeat the process for every variable.
-   Step 5: For multiple imputation, repeat the four steps multiple times.

Note:

-   If we skip step 1, it reduce variability for estimating standard errors.

-   Good:

    -   Constrained to only possible values.
    -   Since the value is picked at random, it adds some variability, which might come in handy when calculating standard errors.

-   Challenge: how can you define "similar" here.

##### Cold Deck Imputation

Contrary to Hot Deck, Cold Deck choose value systematically from an observation that has similar values on other variables, which remove the random variation that we want.

Donor samples of "cold-deck" imputation come from a different data set.

##### Predictive Mean Matching

Steps:

1.  Regress Y on X (matrix of covariates) for the $n_1$ (i.e., non-missing cases) to get coefficients $b$ (a $k \times 1$ vector) and residual variance estimates $s^2$
2.  Draw randomly from the posterior predictive distribution of the residual variance (assuming a noninformative prior) by calculating $\frac{(n_1-k)s^2}{\chi^2}$, where $\chi^2$ is a random draw from a $\chi^2_{n_1-k}$ and let $s^2_{[1]}$ be an i-th random draw
3.  Randomly draw from the posterior distribution of the coefficients $b$, by drawing from $MVN(b, s^2_{[1]}(X'X)^{-1})$, where X is an $n_1 \times k$ matrix of X values. Then we have $b_{1}$
4.  Using step 1, we can calculate standardized residuals for $n_1$ cases: $e_i = \frac{y_i - bx_i}{\sqrt{s^2(1-k/n_1)}}$
5.  Randomly draw a sample (with replacement) of $n_0$ from the $n_1$ residuals in step 4
6.  With $n_0$ cases, we can calculate imputed values of Y: $y_i = b_{[1]}x_i + s_{[1]}e_i$ where $e_i$ are taken from step 5, and $b_{[1]}$ taken from step 3, and $s_{[1]}$ taken from step 2.
7.  Repeat steps 2 through 6 except for step 4.

Notes:

-   can be used for multiple variables where each variable is imputed using all other variables as predictor.
-   can also be used for heteroskedasticity in imputed values.

Example from [Statistics Globle](https://statisticsglobe.com/predictive-mean-matching-imputation-method/)


```r
set.seed(918273)                                # Seed
N <- 3000                                       # Sample size
y <- round(runif(N, -10, 10))                   # Target variable Y
x1 <- y + round(runif(N, 0, 50))                # Auxiliary variable 1
x2 <- round(y + 0.25 * x1 + rnorm(N, - 3, 15))  # Auxiliary variable 2
x3 <- round(0.1 * x1 + rpois(N, 2))             # Auxiliary variable 3
x4 <- as.factor(round(0.02 * y + runif(N)))     # Auxiliary variable 4 (categorical variable)
y[rbinom(N, 1, 0.2) == 1] <- NA                 # Insert 20% missing data in Y
data <- data.frame(y, x1, x2, x3, x4)           # Store data in dataset
head(data)                                      # First 6 rows of our data
#>    y x1  x2 x3 x4
#> 1  8 38  -3  6  1
#> 2  1 50  -9  5  0
#> 3  5 43  20  5  1
#> 4 NA  9  13  3  0
#> 5 -4 40 -10  6  0
#> 6 NA 29  -6  5  1

library("mice")                                 # Load mice package

##### Impute data via predictive mean matching (single imputation)#####
 
imp_single <- mice(data, m = 1, method = "pmm") # Impute missing values
#> 
#>  iter imp variable
#>   1   1  y
#>   2   1  y
#>   3   1  y
#>   4   1  y
#>   5   1  y
data_imp_single <- complete(imp_single)         # Store imputed data
# head(data_imp_single)

# SInce single imputation underestiamtes stnadard errors, we use multiple imputaiton

##### Predictive mean matching (multiple imputation)#####
 
imp_multi <- mice(data, m = 5, method = "pmm")  # Impute missing values multiple times
#> 
#>  iter imp variable
#>   1   1  y
#>   1   2  y
#>   1   3  y
#>   1   4  y
#>   1   5  y
#>   2   1  y
#>   2   2  y
#>   2   3  y
#>   2   4  y
#>   2   5  y
#>   3   1  y
#>   3   2  y
#>   3   3  y
#>   3   4  y
#>   3   5  y
#>   4   1  y
#>   4   2  y
#>   4   3  y
#>   4   4  y
#>   4   5  y
#>   5   1  y
#>   5   2  y
#>   5   3  y
#>   5   4  y
#>   5   5  y
data_imp_multi_all <- complete(imp_multi,       # Store multiply imputed data
                           "repeated",
                           include = TRUE)
 
data_imp_multi <- data.frame(                   # Combine imputed Y and X1-X4 (for convenience)
  data_imp_multi_all[ , 1:6], data[, 2:5])
head(data_imp_multi)                            # First 6 rows of our multiply imputed data
#>   y.0 y.1 y.2 y.3 y.4 y.5 x1  x2 x3 x4
#> 1   8   8   8   8   8   8 38  -3  6  1
#> 2   1   1   1   1   1   1 50  -9  5  0
#> 3   5   5   5   5   5   5 43  20  5  1
#> 4  NA  -6  -4  -4  -1  -3  9  13  3  0
#> 5  -4  -4  -4  -4  -4  -4 40 -10  6  0
#> 6  NA  -8   5  -4   1   4 29  -6  5  1
```

Example from [UCLA Statistical Consulting](https://stats.idre.ucla.edu/r/faq/how-do-i-perform-multiple-imputation-using-predictive-mean-matching-in-r/) [@newtest]


```r
library(mice)
library(VIM)
library(lattice)
library(ggplot2)
## set observations to NA
anscombe <- within(anscombe, {
    y1[1:3] <- NA
    y4[3:5] <- NA
})
## view
head(anscombe)
#>   x1 x2 x3 x4   y1   y2    y3   y4
#> 1 10 10 10  8   NA 9.14  7.46 6.58
#> 2  8  8  8  8   NA 8.14  6.77 5.76
#> 3 13 13 13  8   NA 8.74 12.74   NA
#> 4  9  9  9  8 8.81 8.77  7.11   NA
#> 5 11 11 11  8 8.33 9.26  7.81   NA
#> 6 14 14 14  8 9.96 8.10  8.84 7.04

## check missing data patterns
md.pattern(anscombe)
```

<img src="11-imputation_files/figure-html/unnamed-chunk-2-1.png" width="90%" style="display: block; margin: auto;" />

```
#>   x1 x2 x3 x4 y2 y3 y1 y4  
#> 6  1  1  1  1  1  1  1  1 0
#> 2  1  1  1  1  1  1  1  0 1
#> 2  1  1  1  1  1  1  0  1 1
#> 1  1  1  1  1  1  1  0  0 2
#>    0  0  0  0  0  0  3  3 6

## Number of observations per patterns for all pairs of variables
p <- md.pairs(anscombe)
p # rr = number of observations where both pairs of values are observed
#> $rr
#>    x1 x2 x3 x4 y1 y2 y3 y4
#> x1 11 11 11 11  8 11 11  8
#> x2 11 11 11 11  8 11 11  8
#> x3 11 11 11 11  8 11 11  8
#> x4 11 11 11 11  8 11 11  8
#> y1  8  8  8  8  8  8  8  6
#> y2 11 11 11 11  8 11 11  8
#> y3 11 11 11 11  8 11 11  8
#> y4  8  8  8  8  6  8  8  8
#> 
#> $rm
#>    x1 x2 x3 x4 y1 y2 y3 y4
#> x1  0  0  0  0  3  0  0  3
#> x2  0  0  0  0  3  0  0  3
#> x3  0  0  0  0  3  0  0  3
#> x4  0  0  0  0  3  0  0  3
#> y1  0  0  0  0  0  0  0  2
#> y2  0  0  0  0  3  0  0  3
#> y3  0  0  0  0  3  0  0  3
#> y4  0  0  0  0  2  0  0  0
#> 
#> $mr
#>    x1 x2 x3 x4 y1 y2 y3 y4
#> x1  0  0  0  0  0  0  0  0
#> x2  0  0  0  0  0  0  0  0
#> x3  0  0  0  0  0  0  0  0
#> x4  0  0  0  0  0  0  0  0
#> y1  3  3  3  3  0  3  3  2
#> y2  0  0  0  0  0  0  0  0
#> y3  0  0  0  0  0  0  0  0
#> y4  3  3  3  3  2  3  3  0
#> 
#> $mm
#>    x1 x2 x3 x4 y1 y2 y3 y4
#> x1  0  0  0  0  0  0  0  0
#> x2  0  0  0  0  0  0  0  0
#> x3  0  0  0  0  0  0  0  0
#> x4  0  0  0  0  0  0  0  0
#> y1  0  0  0  0  3  0  0  1
#> y2  0  0  0  0  0  0  0  0
#> y3  0  0  0  0  0  0  0  0
#> y4  0  0  0  0  1  0  0  3
# rm = the number of observations where both variables are missing values
# mr = the number of observations where the first variable’s value (e.g. the row variable) is observed and second (or column) variable is missing
# mm = the number of observations where the second variable’s value (e.g. the col variable) is observed and first (or row) variable is missing

## Margin plot of y1 and y4
marginplot(anscombe[c(5, 8)], col = c("blue", "red", "orange"))
```

<img src="11-imputation_files/figure-html/unnamed-chunk-2-2.png" width="90%" style="display: block; margin: auto;" />

```r

## 5 imputations for all missing values
imp1 <- mice(anscombe, m = 5)
#> 
#>  iter imp variable
#>   1   1  y1  y4
#>   1   2  y1  y4
#>   1   3  y1  y4
#>   1   4  y1  y4
#>   1   5  y1  y4
#>   2   1  y1  y4
#>   2   2  y1  y4
#>   2   3  y1  y4
#>   2   4  y1  y4
#>   2   5  y1  y4
#>   3   1  y1  y4
#>   3   2  y1  y4
#>   3   3  y1  y4
#>   3   4  y1  y4
#>   3   5  y1  y4
#>   4   1  y1  y4
#>   4   2  y1  y4
#>   4   3  y1  y4
#>   4   4  y1  y4
#>   4   5  y1  y4
#>   5   1  y1  y4
#>   5   2  y1  y4
#>   5   3  y1  y4
#>   5   4  y1  y4
#>   5   5  y1  y4

## linear regression for each imputed data set - 5 regression are run
fitm <- with(imp1, lm(y1 ~ y4 + x1))
summary(fitm)
#> # A tibble: 15 x 6
#>    term        estimate std.error statistic p.value  nobs
#>    <chr>          <dbl>     <dbl>     <dbl>   <dbl> <int>
#>  1 (Intercept)    6.44      1.77       3.63 0.00664    11
#>  2 y4            -0.381     0.162     -2.35 0.0468     11
#>  3 x1             0.458     0.101      4.56 0.00186    11
#>  4 (Intercept)    6.17      2.02       3.06 0.0156     11
#>  5 y4            -0.351     0.183     -1.92 0.0914     11
#>  6 x1             0.443     0.117      3.79 0.00535    11
#>  7 (Intercept)    6.47      2.02       3.21 0.0125     11
#>  8 y4            -0.360     0.186     -1.93 0.0893     11
#>  9 x1             0.430     0.114      3.77 0.00549    11
#> 10 (Intercept)    6.51      2.63       2.48 0.0384     11
#> 11 y4            -0.358     0.242     -1.48 0.178      11
#> 12 x1             0.433     0.149      2.91 0.0195     11
#> 13 (Intercept)    6.15      2.37       2.60 0.0318     11
#> 14 y4            -0.362     0.219     -1.65 0.137      11
#> 15 x1             0.478     0.138      3.46 0.00855    11

## pool coefficients and standard errors across all 5 regression models
pool(fitm)
#> Class: mipo    m = 5 
#>          term m   estimate       ubar            b          t dfcom       df
#> 1 (Intercept) 5  6.3487398 4.76263432 0.0300416474 4.79868430     8 6.495687
#> 2          y4 5 -0.3623970 0.04025189 0.0001255017 0.04040250     8 6.520908
#> 3          x1 5  0.4485592 0.01560878 0.0003990336 0.01608762     8 6.341712
#>           riv      lambda       fmi
#> 1 0.007569335 0.007512471 0.2165521
#> 2 0.003741490 0.003727544 0.2130085
#> 3 0.030677620 0.029764516 0.2374856

## output parameter estimates
summary(pool(fitm))
#>          term   estimate std.error statistic       df    p.value
#> 1 (Intercept)  6.3487398 2.1905899  2.898187 6.495687 0.02504369
#> 2          y4 -0.3623970 0.2010037 -1.802937 6.520908 0.11751237
#> 3          x1  0.4485592 0.1268370  3.536502 6.341712 0.01119463
```

##### Stochastic Imputation

`Regression imputation + random residual = Stochastic Imputation`

Most multiple imputation is based off of some form of stochastic regression imputation.

Good:

-   Has all the advantage of [Regression Imputation]
-   and also has the random components

Bad:

-   might lead to implausible values (e.g. negative values)
-   can't handle heteroskadastic data

**Note**\
Multiple Imputation usually based on some form of stochastic regression imputation.


```r
# Income data
 
set.seed(91919)                              # Set seed
N <- 1000                                    # Sample size
 
income <- round(rnorm(N, 0, 500))            # Create some synthetic income data
income[income < 0] <- income[income < 0] * (- 1)
 
x1 <- income + rnorm(N, 1000, 1500)          # Auxiliary variables
x2 <- income + rnorm(N, - 5000, 2000)
 
income[rbinom(N, 1, 0.1) == 1] <- NA         # Create 10% missingness in income
 
data_inc_miss <- data.frame(income, x1, x2)
```

Single stochastic regression imputation


```r
imp_inc_sri <- mice(data_inc_miss, method = "norm.nob", m = 1)
#> 
#>  iter imp variable
#>   1   1  income
#>   2   1  income
#>   3   1  income
#>   4   1  income
#>   5   1  income
data_inc_sri <- complete(imp_inc_sri)
```

Single predictive mean matching


```r
imp_inc_pmm <- mice(data_inc_miss, method = "pmm", m = 1)
#> 
#>  iter imp variable
#>   1   1  income
#>   2   1  income
#>   3   1  income
#>   4   1  income
#>   5   1  income
data_inc_pmm <- complete(imp_inc_pmm)
```

Stochastic regression imputation contains negative values


```r
data_inc_sri$income[data_inc_sri$income < 0]
#> [1]  -66.055957  -96.980053  -28.921432   -4.175686  -54.480798  -27.207102
#> [7] -143.603500  -80.960488
data_inc_pmm$income[data_inc_pmm$income < 0] # No values below 0
#> numeric(0)
```

Proof for heteroskadastic data


```r
# Heteroscedastic data
 
set.seed(654654)                             # Set seed
N <- 1:5000                                  # Sample size
 
a <- 0
b <- 1
sigma2 <- N^2
eps <- rnorm(N, mean = 0, sd = sqrt(sigma2))
 
y <- a + b * N + eps                         # Heteroscedastic variable
x <- 30 * N + rnorm(N[length(N)], 1000, 200) # Correlated variable
 
y[rbinom(N[length(N)], 1, 0.3) == 1] <- NA   # 30% missings
 
data_het_miss <- data.frame(y, x)
```

Single stochastic regression imputation


```r
imp_het_sri <- mice(data_het_miss, method = "norm.nob", m = 1)
#> 
#>  iter imp variable
#>   1   1  y
#>   2   1  y
#>   3   1  y
#>   4   1  y
#>   5   1  y
data_het_sri <- complete(imp_het_sri)
```

Single predictive mean matching


```r
imp_het_pmm <- mice(data_het_miss, method = "pmm", m = 1)
#> 
#>  iter imp variable
#>   1   1  y
#>   2   1  y
#>   3   1  y
#>   4   1  y
#>   5   1  y
data_het_pmm <- complete(imp_het_pmm)
```

Comparison between predictive mean matching and stochastic regression imputation


```r
par(mfrow = c(1, 2))                              # Both plots in one graphic
 
plot(x[!is.na(data_het_sri$y)],                   # Plot of observed values
     data_het_sri$y[!is.na(data_het_sri$y)],
     main = "",
     xlab = "X", ylab = "Y")
points(x[is.na(y)], data_het_sri$y[is.na(y)],     # Plot of missing values
       col = "red")
title("Stochastic Regression Imputation",         # Title of plot
      line = 0.5)
abline(lm(y ~ x, data_het_sri),                   # Regression line
       col = "#1b98e0", lwd = 2.5)
legend("topleft",                                 # Legend
       c("Observed Values", "Imputed Values", "Regression Y ~ X"),
       pch = c(1, 1, NA),
       lty = c(NA, NA, 1),
       col = c("black", "red", "#1b98e0"))
 
plot(x[!is.na(data_het_pmm$y)],                   # Plot of observed values
     data_het_pmm$y[!is.na(data_het_pmm$y)],
     main = "",
     xlab = "X", ylab = "Y")
points(x[is.na(y)], data_het_pmm$y[is.na(y)],     # Plot of missing values
       col = "red")
title("Predictive Mean Matching",                 # Title of plot
      line = 0.5)
abline(lm(y ~ x, data_het_pmm),
       col = "#1b98e0", lwd = 2.5)
legend("topleft",                                 # Legend
       c("Observed Values", "Imputed Values", "Regression Y ~ X"),
       pch = c(1, 1, NA),
       lty = c(NA, NA, 1),
       col = c("black", "red", "#1b98e0"))
 
mtext("Imputation of Heteroscedastic Data",       # Main title of plot
      side = 3, line = - 1.5, outer = TRUE, cex = 2)
```

<img src="11-imputation_files/figure-html/unnamed-chunk-10-1.png" width="90%" style="display: block; margin: auto;" />

#### Regression Imputation

Also known as conditional mean imputation Missing value is based (regress) on other variables.

-   Good:

    -   Maintain the relationship with other variables (i.e., preserve dependence structure among features, unlike \@ref(mean-mode-median-imputation)).

    -   If the data are MCAR, least-squares coefficients estimates will be consistent, and approximately unbiased in large samples [@Gourieroux_1981]

        -   Can have improvement on efficiency by using weighted least squares [@Beale_1975] or generalized least squares [@Gourieroux_1981].

-   Bad:

    -   No variability left. treated data as if they were collected.
    -   Underestimate the standard errors and overestimate test statistics

#### Interpolation and Extrapolation

An estimated value from other observations from the same individual. It usually only works in longitudinal data.

#### K-nearest neighbor (KNN) imputation

The above methods are model-based imputation (regression).\
This is an example of neighbor-based imputation (K-nearest neighbor).

For every observation that needs to be imputed, the algorithm identifies 'k' closest observations based on some types distance (e.g., Euclidean) and computes the weighted average (weighted based on distance) of these 'k' obs.

For a discrete variable, it uses the most frequent value among the k nearest neighbors.

-   Distance metrics: Hamming distance.

For a continuous variable, it uses the mean or mode.

-   Distance metrics:

    -   Euclidean
    -   Mahalanobis
    -   Manhattan

#### Bayesian Ridge regression implementation

#### Matrix Completion

Impute items missing at random while accounting for dependence between features by using principal components, which is known as **matrix completion** [@james2013, Sec 12.3]

Consider an $n \times p$ feature matrix, $\mathbf{X}$, with element $x_{ij}$, some of which are missing.

Similar to \@ref(principal-components), we can approximate the matrix $\mathbf{X}$ in terms of its leading PCs.

We consider the $M$ principal components that optimize

$$
\underset{\mathbf{A} \in R^{n \times M}, \mathbf{B} \in R^{p \times M}}{\operatorname{min}} \{ \sum_{(i,j) \in \cal{O}} (x_{ij} - \sum_{m=1}^M a_{im}b_{jm})^2 \} 
$$

where $\cal{O}$ is the set of all observed pairs indices $(i,j)$, a subset of the possible $n \times p$ pairs

Once this minimization is solved,

-   One can impute a missing observation, $x_{ij}$, with $\hat{x}_{ij} = \sum_{m=1}^M \hat{a}_{im}\hat{b}_{jm}$ where $\hat{a}_{im}, \hat{b}_{jm}$ are the $(i,m)$ and $(j.m)$ elements, respectively, of the matrices $\hat{\mathbf{A}}$ and $\hat{\mathbf{B}}$ from the minimization, and

-   One can approximately recover the $M$ principal component scores and loadings, as we did when the data were complete

The challenge here is to solve this minimization problem: the eigen-decomposition non longer applies (as in \@ref(principal-components)

Hence, we have to use iterative algorithm [@james2013 Alg 12.1]

1.  Create a complete data matrix $\tilde{\mathbf{X}}$ of dimension $n \times p$ of which the $(i,j)$ element equals

$$
\tilde{x}_{ij} =
\begin{cases}
x_{ij} & \text{if } (i,j) \in \cal{O} \\
\bar{x}_{j} & \text{if } (i,j) \notin \cal{O}
\end{cases}
$$

where $\bar{x}_j$ is the average of the observed values for the $j$th variable in the incomplete data matrix $\mathbf{X}$

$\cal{O}$ indexes the observations that are observed in $\mathbf{X}$

2.  Repeat these 3 steps until some objectives are met

a\. Solve

$$
\underset{\mathbf{A} \in R^{n \times M}, \mathbf{B} \in R^{p \times M}}{\operatorname{min}} \{ \sum_{(i,j) \in \cal{O}} (x_{ij} - \sum_{m=1}^M a_{im}b_{jm})^2 \} 
$$

by computing the principal components of $\tilde{\mathbf{X}}$

b\. For each element $(i,j) \notin \cal{O}$, set $\tilde{x}_{ij} \leftarrow \sum_{m=1}^M \hat{a}_{im}\hat{b}_{jm}$

c\. Compute the objective

$$
\sum_{(i,j \in \cal{O})} (x_{ij} - \sum_{m=1}^M \hat{a}_{im} \hat{b}_{jm})^2
$$

3.  Return the estimated missing entries $\tilde{x}_{ij}, (i,j) \notin \cal{O}$

<br>

### Other methods

-   For panel data, or clustered data, use `pan` package by Schafer (1997)

## Criteria for Choosing an Effective Approach

Criteria for an ideal technique in treating missing data:

1.  Unbiased parameter estimates
2.  Adequate power
3.  Accurate standard errors (p-values, confidence intervals)

The Multiple Imputation and Full Information Maximum Likelihood are the the most ideal candidate. Single imputation will generally lead to underestimation of standard errors.

## Another Perspective

Model bias can arisen from various factors including:

-   Imputation method
-   Missing data mechanism ([MCAR](#missing-completely-at-random-mcar) vs. [MAR](#missing-at-random-mar))
-   Proportion of the missing data
-   Information available in the data set

Since the imputed observations are themselves estimates, their values have corresponding random error. But when you put in that estimate as a data point, your software doesn't know that. So it overlooks the extra source of error, resulting in too-small standard errors and too-small p-values. So multiple imputation comes up with multiple estimates.

Because multiple imputation have a random component, the multiple estimates are slightly different. This re-introduces some variation that your software can incorporate in order to give your model accurate estimates of standard error. Multiple imputation was a huge breakthrough in statistics about 20 years ago. It solves a lot of problems with missing data (though, unfortunately not all) and if done well, leads to unbiased parameter estimates and accurate standard errors. If your rate of missing data is very, very small (2-3%) it doesn't matter what technique you use.

Remember that there are three goals of multiple imputation, or any missing data technique:

-   Unbiased parameter estimates in the final analysis (regression coefficients, group means, odds ratios, etc.)
-   accurate standard errors of those parameter estimates, and therefore, accurate p-values in the analysis
-   adequate power to find meaningful parameter values significant.

Hence,

1.  Don't round off imputations for dummy variables. Many common imputation techniques, like MCMC, require normally distributed variables. Suggestions for imputing categorical variables were to dummy code them, impute them, then round off imputed values to 0 or 1. Recent research, however, has found that rounding off imputed values actually leads to biased parameter estimates in the analysis model. You actually get better results by leaving the imputed values at impossible values, even though it's counter-intuitive.

2.  Don't transform skewed variables. Likewise, when you transform a variable to meet normality assumptions before imputing, you not only are changing the distribution of that variable but the relationship between that variable and the others you use to impute. Doing so can lead to imputing outliers, creating more bias than just imputing the skewed variable.

3.  Use more imputations. The advice for years has been that 5-10 imputations are adequate. And while this is true for unbiasedness, you can get inconsistent results if you run the multiple imputation more than once. [@Bodner_2008] recommends having as many imputations as the percentage of missing data. Since running more imputations isn't any more work for the data analyst, there's no reason not to.

4.  Create multiplicative terms before imputing. When the analysis model contains a multiplicative term, like an interaction term or a quadratic, create the multiplicative terms first, then impute. Imputing first, and then creating the multiplicative terms actually biases the regression parameters of the multiplicative term [@von_Hippel_2009]

## Diagnosing the Mechanism

### MAR vs. MNAR

The only true way to distinguish between MNAR and MAR is to measure some of that missing data.

It's a common practice among professional surveyors to, for example, follow-up on a paper survey with phone calls to a group of the non-respondents and ask a few key survey items. This allows you to compare respondents to non-respondents.

If their responses on those key items differ by very much, that's good evidence that the data are MNAR.

However in most missing data situations, we can't get a hold of the missing data. So while we can't test it directly, we can examine patterns in the data get an idea of what's the most likely mechanism.

The first thing in diagnosing randomness of the missing data is to use your substantive scientific knowledge of the data and your field. The more sensitive the issue, the less likely people are to tell you. They're not going to tell you as much about their cocaine usage as they are about their phone usage.

Likewise, many fields have common research situations in which non-ignorable data is common. Educate yourself in your field's literature.

### MCAR vs. MAR

There is a very useful test for MCAR, Little's test.

A second technique is to create dummy variables for whether a variable is missing.

1 = missing 0 = observed

You can then run t-tests and chi-square tests between this variable and other variables in the data set to see if the missingness on this variable is related to the values of other variables.

For example, if women really are less likely to tell you their weight than men, a chi-square test will tell you that the percentage of missing data on the weight variable is higher for women than men.

## Application


```r
library(visdat)
library(naniar)
library(ggplot2)
vis_miss()


ggplot(data, aes(x, y)) + 
    geom_miss_point() + 
    facet_wrap(~ group)

gg_miss_var(data, facet = group)

gg_miss_upset(data)

gg_miss_fct(x = variable1, fct = variable2)

```

Read more on [The Missing Book by Nicholas Tierney & Allison Horst](https://tmb.njtierney.com/)

How many imputation:

**Usually 5**. (unless you have extremely high portion of missing, in which case you probably need to check your data again)

According to Rubin, the relative efficiency of an estimate based on m imputations to infinity imputation is approximately

$$
(1+\frac{\lambda}{m})^{-1}
$$

where $\lambda$ is the rate of missing data

Example 50% of missing data means an estimate based on 5 imputation has standard deviation that is only 5% wider compared to an estimate based on infinity imputation\
($\sqrt{1+0.5/5}=1.049$)


```r
library(missForest)

#load data
data <- iris

#Generate 10% missing values at Random
set.seed(1)
iris.mis <- prodNA(iris, noNA = 0.1)

#remove categorical variables
iris.mis.cat <- iris.mis
iris.mis <- subset(iris.mis, select = -c(Species))
```

### Imputation with mean / median / mode


```r
# whole data set
e1071::impute(iris.mis, what = "mean") # replace with mean
e1071::impute(iris.mis, what = "median") # replace with median

# by variables
Hmisc::impute(iris.mis$Sepal.Length, mean)  # mean
Hmisc::impute(iris.mis$Sepal.Length, median)  # median
Hmisc::impute(iris.mis$Sepal.Length, 0)  # replace specific number
```

check accurary


```r
library(DMwR)
actuals <- iris$Sepal.Width[is.na(iris.mis$Sepal.Width)]
predicteds <- rep(mean(iris$Sepal.Width, na.rm=T), length(actuals))
regr.eval(actuals, predicteds)
#>       mae       mse      rmse      mape 
#> 0.2870303 0.1301598 0.3607767 0.1021485
```

### KNN


```r
library(DMwR)
# iris.mis[,!names(iris.mis) %in% c("Sepal.Length")] 
# data should be this line. But since knn cant work with 3 or less variables, we need to use at least 4 variables. 

# knn is not appropriate for categorical variables
knnOutput <-
    knnImputation(data = iris.mis.cat, 
                  #k = 10, 
                  meth = "median" # could use "median" or "weighAvg"
                  )  # should exclude the dependent variable: Sepal.Length
anyNA(knnOutput)
#> [1] FALSE
```


```r
library(DMwR)
actuals <- iris$Sepal.Width[is.na(iris.mis$Sepal.Width)]
predicteds <- knnOutput[is.na(iris.mis$Sepal.Width), "Sepal.Width"]
regr.eval(actuals, predicteds)
#>       mae       mse      rmse      mape 
#> 0.2318182 0.1038636 0.3222788 0.0823571
```

Compared to mape (mean absolute percentage error) of mean imputation, we see almost always see improvements.

### rpart

For categorical (factor) variables, rpart can handle


```r
library(rpart)
class_mod <- rpart(Species ~ . - Sepal.Length, data=iris.mis.cat[!is.na(iris.mis.cat$Species), ], method="class", na.action=na.omit)  # since Species is a factor, and exclude dependent variable "Sepal.Length"

anova_mod <- rpart(Sepal.Width ~ . - Sepal.Length, data=iris.mis[!is.na(iris.mis$Sepal.Width), ], method="anova", na.action=na.omit)  # since Sepal.Width is numeric.
species_pred <- predict(class_mod, iris.mis.cat[is.na(iris.mis.cat$Species), ])
width_pred <- predict(anova_mod, iris.mis[is.na(iris.mis$Sepal.Width), ])
```

### MICE (Multivariate Imputation via Chained Equations) {#mice-multivariate-imputation-via-chained-equations}

Assumption: data are [MAR](#missing-at-random-mar)

It imputes data per variable by specifying an imputation model for each variable

**Example**

We have $X_1, X_2,..,X_k$. If $X_1$ has missing data, then it is regressed on the rest of the variables. Same procedure applies if $X_2$ has missing data. Then, predicted values are used in place of missing values.

By default,

-   **Continuous variables** use linear regression.
-   **Categorical Variables** use logistic regression.

Methods in [MICE](#mice-multivariate-imputation-via-chained-equations):

-   PMM (Predictive Mean Matching) -- For numeric variables
-   logreg(Logistic Regression) -- For Binary Variables( with 2 levels)
-   polyreg(Bayesian polytomous regression) -- For Factor Variables (\>= 2 levels)
-   Proportional odds model (ordered, \>= 2 levels)


```r
# load package
library(mice)
library(VIM)

# check missing values
md.pattern(iris.mis)
```

<img src="11-imputation_files/figure-html/unnamed-chunk-18-1.png" width="90%" style="display: block; margin: auto;" />

```
#>     Sepal.Width Sepal.Length Petal.Length Petal.Width   
#> 100           1            1            1           1  0
#> 15            1            1            1           0  1
#> 8             1            1            0           1  1
#> 2             1            1            0           0  2
#> 11            1            0            1           1  1
#> 1             1            0            1           0  2
#> 1             1            0            0           1  2
#> 1             1            0            0           0  3
#> 7             0            1            1           1  1
#> 3             0            1            0           1  2
#> 1             0            0            1           1  2
#>              11           15           15          19 60

#plot the missing values
aggr(iris.mis, col=mdc(1:2), numbers=TRUE, sortVars=TRUE, labels=names(iris.mis), cex.axis=.7, gap=3, ylab=c("Proportion of missingness","Missingness Pattern"))
```

<img src="11-imputation_files/figure-html/unnamed-chunk-18-2.png" width="90%" style="display: block; margin: auto;" />

```
#> 
#>  Variables sorted by number of missings: 
#>      Variable      Count
#>   Petal.Width 0.12666667
#>  Sepal.Length 0.10000000
#>  Petal.Length 0.10000000
#>   Sepal.Width 0.07333333


mice_plot <- aggr(iris.mis, col=c('navyblue','yellow'),
                    numbers=TRUE, sortVars=TRUE,
                    labels=names(iris.mis), cex.axis=.7,
                    gap=3, ylab=c("Missing data","Pattern"))
```

<img src="11-imputation_files/figure-html/unnamed-chunk-18-3.png" width="90%" style="display: block; margin: auto;" />

```
#> 
#>  Variables sorted by number of missings: 
#>      Variable      Count
#>   Petal.Width 0.12666667
#>  Sepal.Length 0.10000000
#>  Petal.Length 0.10000000
#>   Sepal.Width 0.07333333
```

Impute Data


```r
imputed_Data <-
    mice(
        iris.mis,
        m = 5, # number of imputed datasets
        maxit = 50, # number of iterations taken to impute missing values
        method = 'pmm', # method used in imputation. Here, we used predictive mean matching
        # other methods can be 
        # "pmm": Predictive mean matching
        # "midastouch" : weighted predictive mean matching
        # "sample": Random sample from observed values
        # "cart": classification and regression trees
        # "rf": random forest imputations.
        # "2lonly.pmm": Level-2 class predictive mean matching
        # Other methods based on whether variables are (1) numeric, (2) binary, (3) ordered, (4), unordered
        seed = 500
    )
```


```r
summary(imputed_Data)
#> Class: mids
#> Number of multiple imputations:  5 
#> Imputation methods:
#> Sepal.Length  Sepal.Width Petal.Length  Petal.Width 
#>        "pmm"        "pmm"        "pmm"        "pmm" 
#> PredictorMatrix:
#>              Sepal.Length Sepal.Width Petal.Length Petal.Width
#> Sepal.Length            0           1            1           1
#> Sepal.Width             1           0            1           1
#> Petal.Length            1           1            0           1
#> Petal.Width             1           1            1           0

#make a density plot
densityplot(imputed_Data)
```

<img src="11-imputation_files/figure-html/unnamed-chunk-20-1.png" width="90%" style="display: block; margin: auto;" />

```r
#the red (imputed values) should be similar to the blue (observed)
```

Check imputed dataset


```r
# 1st dataset 
completeData <- complete(imputed_Data,1)

# 2nd dataset
complete(imputed_Data,2)
```

Regression model using imputed datasets


```r
# regression model
fit <- with(data = imputed_Data, exp = lm(Sepal.Width ~ Sepal.Length + Petal.Width)) 

#combine results of all 5 models
combine <- pool(fit)
summary(combine)
#>           term   estimate  std.error statistic       df      p.value
#> 1  (Intercept)  1.8963130 0.32453912  5.843095 131.0856 3.838556e-08
#> 2 Sepal.Length  0.2974293 0.06679204  4.453066 130.2103 1.802241e-05
#> 3  Petal.Width -0.4811603 0.07376809 -6.522608 108.8253 2.243032e-09
```

### Amelia

-   Use bootstrap based EMB algorithm (faster and robust to impute many variables including cross sectional, time series data etc)
-   Use parallel imputation feature using multicore CPUs.

Assumptions

-   All variables follow Multivariate Normal Distribution (MVN). Hence, this package works best when data is MVN, or transformation to normality.
-   Missing data is [Missing at Random (MAR)](#missing-at-random-mar)

Steps:

1.  m bootstrap samples and applies EMB algorithm to each sample. Then we have m different estimates of mean and variances.
2.  the first set of estimates are used to impute first set of missing values using regression, then second set of estimates are used for second set and so on.

However, [Amelia] is different from [MICE](#mice-multivariate-imputation-via-chained-equations)

-   MICE imputes data on variable by variable basis whereas MVN uses a joint modeling approach based on multivariate normal distribution.
-   MICE can handle different types of variables while the variables in MVN need to be normally distributed or transformed to approximate normality.
-   MICE can manage imputation of variables defined on a subset of data whereas MVN cannot.


```r
library(Amelia)
data("iris")
#seed 10% missing values
iris.mis <- prodNA(iris, noNA = 0.1)

# idvars – keep all ID variables and other variables which you don’t want to impute
# noms – keep nominal variables here

#specify columns and run amelia
amelia_fit <- amelia(iris.mis, m=5, parallel = "multicore", noms = "Species")
#> -- Imputation 1 --
#> 
#>   1  2  3  4  5  6  7  8
#> 
#> -- Imputation 2 --
#> 
#>   1  2  3  4  5  6  7  8  9 10
#> 
#> -- Imputation 3 --
#> 
#>   1  2  3  4  5  6
#> 
#> -- Imputation 4 --
#> 
#>   1  2  3  4  5  6  7  8
#> 
#> -- Imputation 5 --
#> 
#>   1  2  3  4  5  6  7

# access imputed outputs
# amelia_fit$imputations[[1]]
```

### missForest

-   an implementation of random forest algorithm (a non parametric imputation method applicable to various variable types). Hence, no assumption about function form of f. Instead, it tries to estimate f such that it can be as close to the data points as possible.
-   builds a random forest model for each variable. Then it uses the model to predict missing values in the variable with the help of observed values.
-   It yields out of bag imputation error estimate. Moreover, it provides high level of control on imputation process.
-   Since bagging works well on categorical variable too, we don't need to remove them here.


```r
library(missForest)
#impute missing values, using all parameters as default values
iris.imp <- missForest(iris.mis)
#>   missForest iteration 1 in progress...done!
#>   missForest iteration 2 in progress...done!
#>   missForest iteration 3 in progress...done!
#>   missForest iteration 4 in progress...done!
#>   missForest iteration 5 in progress...done!
#>   missForest iteration 6 in progress...done!
#>   missForest iteration 7 in progress...done!
# check imputed values
# iris.imp$ximp


# check imputation error
# NRMSE is normalized mean squared error. It is used to represent error derived from imputing continuous values. 
# PFC (proportion of falsely classified) is used to represent error derived from imputing categorical values.
iris.imp$OOBerror
#>      NRMSE        PFC 
#> 0.14004198 0.03053435

#comparing actual data accuracy
iris.err <- mixError(iris.imp$ximp, iris.mis, iris)
iris.err
#>      NRMSE        PFC 
#> 0.11567322 0.05263158
```

This means categorical variables are imputed with 5% error and continuous variables are imputed with 14% error.

This can be improved by tuning the values of `mtry` and `ntree` parameter.

-   `mtry` refers to the number of variables being randomly sampled at each split.
-   `ntree` refers to number of trees to grow in the forest.

### Hmisc

-   `impute()` function imputes missing value using user defined statistical method (mean, max, mean). It's default is median.\
-   `aregImpute()` allows mean imputation using additive regression, bootstrapping, and predictive mean matching.

1.  In bootstrapping, different bootstrap resamples are used for each of multiple imputations. Then, a flexible additive model (non parametric regression method) is fitted on samples taken with replacements from original data and missing values (acts as dependent variable) are predicted using non-missing values (independent variable).\
2.  it uses predictive mean matching (default) to impute missing values. Predictive mean matching works well for continuous and categorical (binary & multi-level) without the need for computing residuals and maximum likelihood fit.

**Note**

-   For predicting categorical variables, Fisher's optimum scoring method is used.
-   `Hmisc` automatically recognizes the variables types and uses bootstrap sample and predictive mean matching to impute missing values.
-   `missForest` can outperform `Hmisc` if the observed variables have sufficient information.

Assumption

-   linearity in the variables being predicted.


```r
library(Hmisc)
# impute with mean value
iris.mis$imputed_age <- with(iris.mis, impute(Sepal.Length, mean))

# impute with random value
iris.mis$imputed_age2 <- with(iris.mis, impute(Sepal.Length, 'random'))

# could also use min, max, median to impute missing value

# using argImpute
impute_arg <- aregImpute(~ Sepal.Length + Sepal.Width + Petal.Length + Petal.Width +
Species, data = iris.mis, n.impute = 5) # argImpute() automatically identifies the variable type and treats them accordingly.
#> Iteration 1 Iteration 2 Iteration 3 Iteration 4 Iteration 5 Iteration 6 Iteration 7 Iteration 8 
impute_arg # R-squares are for predicted missing values.
#> 
#> Multiple Imputation using Bootstrap and PMM
#> 
#> aregImpute(formula = ~Sepal.Length + Sepal.Width + Petal.Length + 
#>     Petal.Width + Species, data = iris.mis, n.impute = 5)
#> 
#> n: 150 	p: 5 	Imputations: 5  	nk: 3 
#> 
#> Number of NAs:
#> Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
#>           14           14           13           15           19 
#> 
#>              type d.f.
#> Sepal.Length    s    2
#> Sepal.Width     s    2
#> Petal.Length    s    2
#> Petal.Width     s    2
#> Species         c    2
#> 
#> Transformation of Target Variables Forced to be Linear
#> 
#> R-squares for Predicting Non-Missing Values for Each Variable
#> Using Last Imputations of Predictors
#> Sepal.Length  Sepal.Width Petal.Length  Petal.Width      Species 
#>        0.884        0.606        0.983        0.955        0.989

# check imputed variable Sepal.Length
impute_arg$imputed$Sepal.Length
#>     [,1] [,2] [,3] [,4] [,5]
#> 3    5.0  5.0  4.7  4.8  5.4
#> 25   5.4  5.0  4.8  5.1  5.0
#> 49   5.1  5.1  5.0  5.1  5.0
#> 56   6.5  5.8  6.0  6.9  6.3
#> 60   6.1  6.0  5.5  5.6  5.7
#> 81   5.5  5.7  5.6  5.2  5.7
#> 83   5.7  5.5  5.7  5.6  5.5
#> 104  6.4  6.5  6.7  6.4  6.7
#> 108  6.3  7.2  7.7  7.7  7.7
#> 121  7.2  6.3  6.3  6.4  6.5
#> 127  5.6  6.3  6.0  6.3  6.3
#> 133  6.8  6.7  5.9  6.4  6.9
#> 148  5.7  5.8  6.8  6.4  6.1
#> 150  6.1  6.4  6.7  6.3  5.9
```

### mi

1.  allows graphical diagnostics of imputation models and convergence of imputation process.
2.  uses Bayesian version of regression models to handle issue of separation.
3.  automatically detects irregularities in data (e.g., high collinearity among variables).
4.  adds noise to imputation process to solve the problem of additive constraints.


```r
library(mi)
# default values of parameters
# 1. rand.imp.method as “bootstrap”
# 2. n.imp (number of multiple imputations) as 3
# 3. n.iter ( number of iterations) as 30
mi_data <- mi(iris.mis, seed = 335)
summary(mi_data)
```
