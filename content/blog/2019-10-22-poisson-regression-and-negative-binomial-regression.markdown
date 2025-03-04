---
title: Poisson Regression and Negative Binomial Regression
author: Hafizah Ilma
github: https://github.com/HafizahIlma
date: '2019-10-22'
slug: poisson-regression-and-neg  ative-binomial-regression
categories:
  - R
tags:
  - Machine Learning
description: ''
featured: ''
featuredalt: ''
featuredpath: ''
linktitle: ''
type: post
---



# Introduction 

Regression analysis is a set of statistical processes for estimating the relationships among variables. It includes many techniques for modeling and analyzing several variables, when the focus is on the relationship between a dependent variable and one or more independent variables (or 'predictors'). 

## For example 

1. Find out the *effect* of land area and building area on house prices in the Kuningan region (Multiple Linear Regression)


2. Find out the *effect* of allowance and GPA on cum laude predicate (yes or no) ? (Binary Logistic Regression)


3. Find out the *effect* of math score and learning program on the number of days in which students are absent from senior high school 123 ?


## Some Types of Regression 

<img src="/img/pic.png" style="display: block; margin: auto;" />


## Why OLS is not Suitable for Discrete Data ?

Let's see the extreme example below !

Suppose I have data of 20 different visitors who have shopped at store A. Then I want to predict how many times visitors arrive based on the amount of money spent on shopping at store A?

Target variable    (y) : Count of times a visitor arrived 

Predictor Variable (x) : 
Total costs (Rupiah-Indonesian) incurred for shopping at the store A


```r
storeA <- data.frame(visitor=c(2,1,1,1,2,2,2,2,2,1,1,1,1,1,2,2,2,2,2,1),
                    cost=c(52034, 83432, 80241, 52949, 55876, 70292, 30077, 69195, 60991, 46161, 57759, 30648,77724 , 73404, 36850, 42691, 64087, 84858, 70533, 62188))
head(storeA)
```

```
#>   visitor  cost
#> 1       2 52034
#> 2       1 83432
#> 3       1 80241
#> 4       1 52949
#> 5       2 55876
#> 6       2 70292
```

I am trying to build a linear regression model using the `lm` function in R.

```r
modelols1 <- lm(visitor~cost, storeA)
summary(modelols1)
```

```
#> 
#> Call:
#> lm(formula = visitor ~ cost, data = storeA)
#> 
#> Residuals:
#>     Min      1Q  Median      3Q     Max 
#> -0.6809 -0.5033  0.3316  0.4574  0.5600 
#> 
#> Coefficients:
#>               Estimate Std. Error t value Pr(>|t|)    
#> (Intercept)  1.817e+00  4.432e-01   4.100 0.000673 ***
#> cost        -4.444e-06  7.118e-06  -0.624 0.540251    
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> Residual standard error: 0.5188 on 18 degrees of freedom
#> Multiple R-squared:  0.0212,	Adjusted R-squared:  -0.03318 
#> F-statistic: 0.3898 on 1 and 18 DF,  p-value: 0.5403
```
From the summary *modelols* above, it can be seen that *modelols* has an R-squared of only 2.12% and the variable `cost` is not significant to the model.


```r
plot(storeA$cost, storeA$visitor)
abline(modelols1$coefficients[1], modelols1$coefficients[2], col="red")
```

<img src="/blog/2019-10-22-poisson-regression-and-negative-binomial-regression_files/figure-html/unnamed-chunk-4-1.png" width="672" style="display: block; margin: auto;" />

The red line in the visualization above is the line of the regression model. And the model formed is  

`$$visitor = 1.817059368-{0.000004444}\times{cost}$$` 

If Ani shopping in shop A with the amount of Rp. 422,000 : then how many times did Ani's prediction come?


```r
predict(modelols1, data.frame(cost=c(422000)))
```

```
#>           1 
#> -0.05814847
```

Based on the prediction results above, Ani came to shop A is -0.6 times. 
Does it make sense ?

# Poisson Regression

In statistics, Poisson regression is a generalized linear model form of regression analysis used to model count data and contingency tables. Poisson regression assumes the response variable Y has a Poisson distribution, and assumes the logarithm of its expected value can be modeled by a linear combination of unknown parameters. A Poisson regression model is sometimes known as a log-linear model, especially when used to model contingency tables.

Poisson regression models are generalized linear models with the logarithm as the (canonical) link function, and the Poisson distribution function as the assumed probability distribution of the response.

## Where Poisson Regression is Applicable?

*1. The target variable is taking positive integer values like :*

- Count of complete network failure in a year for a big size tech firm.

- Number of families visiting a restaurant in a giver hour. 

- Number of accident claims being made by car insurance holder in a year. 

- Expected number of credit cards a customer may have?


*2. Please note in all these cases, outcome can be:*

- 0

- 1

- 2

- 3

- ..(only positive integer outcome is posible)

*3. And most of the time dependent variable is smaller values like 0,1,2 and there are few somewhat larger values, so the average is small value *

*4. In such cases the target variabel Y has a Poisson Distribution  * 

- Numerical

- Categorical

*5. If the average is large value, then for the target variabel (y), the normal distribution is a good approximation for the Poissson distribution *

## Assumption of Poisson Regression

- The dependent variable y is count 

- Counts must be positive integers (i.e. 0,1,2,..,k)

- Counts must follow a Poisson Distribution. Therefore, the *mean = variance*. 

- Explanatory variables must be continuous, dichotomous or ordinal.

- Observation are independent of each other.


## In case of Poisson Distribution 
- The probability of dependent variable y, taking value k (i.e. count=k) is given by:

`$$P(k\:events\:in\:interval) = e^{-\lambda}\frac{\lambda^k}{k!}$$`

Where :

- \(\lambda\)  is the rate of event or average count of events per interval

- `\(e = 2.71828\)`

- `\(k! = {k}\times{(k-1)}\times{(k-2)}\times.....\times1\)`

## An Example of Poisson Distribution

`$$P(k\:events\:in\:interval) = e^{-\lambda} \frac{\lambda^k}{k!}$$`

- If defect rate for a manufacturer is 5%, then

- What is the probability that in a consignment of 100 TV

- Q : There will be no defect at all?
- A : here \(\lambda\)  is 5
- `\(P(0)=e^{-5}\)`
- If Poisson regression gives: `\(\hat{y} = ln(y) = a + bx\)`

- Then please `\(\frac{5^0}{0!}\)`
- Q : There will be one defect only?
- A : `\(P(1)=e^{-5}\frac{5^1}{1!}\)`


## Process of Poisson Regression Model Development
- If you have count as the dependent variabel y,

- `\(\hat{y} = ln(y)\)` can be taken to develop a regression model (i.e. Poisson Regression)

- MLE (Maximum Likelihood Estimation) is used to estimate the parameters of regression equation 
 understand :

  - if `\(b=0\)`, then `\(\hat{y} =a\)` i.e. ln(y)=a, hence **expected/predicted 
  `\(\hat{y} = exp(a)\)`**

  - If `\({b}\neq{0}\)` , then `\(\hat{y}\)` i.e. `\(ln(y)=a + bx\)`, hence predicted 
  
  `$$\hat{y} = exp(a+bx) = exp(a) \times {exp(bx)}$$`
  
  
  - One unit of x increase, will change y by exp(b) **times** (note : **Multiplicative** impact not the *addition* impact that happens in linear regression case).

  - If b>0, then y will become greater by exp(b) times.

  - Like if `\(b=2\)`, then y will become bigger with each unit increase of x, by exp(2) times.

  - And similary if `\(b<0\)` (like `\(b=2\)`), then y will decrease with each unit increase of x, by `\(exp(-2)\)` times. 


# Negative Binomial Regression

Negative binomial regression is a popular generalization of Poisson regression because it loosens the highly restrictive assumption that the variance is equal to the mean made by the Poisson model. The traditional negative binomial regression model, commonly known as NB2, is based on the Poisson-gamma mixture distribution. This model is popular because it models the Poisson heterogeneity with a gamma distribution.

**Basic assumption for Poisson Regression : mean=variance --> equidispersi**

- variance < mean --> Undispersion

- *variance > mean --> Overdispersion*

To overcome the Overdispersion problem, one alternative is to use *Negative Binomial Regression*.

*Negative Binomial Regression is a special regression of Poisson regression that occurs Overdispersion.*

# Study Case
**An administrator at a school 123 wants to learn about student absence behavior. Look for the effect of the number of days of absence based on the type of student program and mathematics score**



## Read Data

The first step taken is reading the data. Use the `read.dta` function from the` foreign` library. The data used comes from the link "https://stats.idre.ucla.edu/stat/stata/dae/nb_data.dta"

```r
dat <- read.dta("https://stats.idre.ucla.edu/stat/stata/dae/nb_data.dta")
head(dat)
```

```
#>     id gender math daysabs prog
#> 1 1001   male   63       4    2
#> 2 1002   male   27       4    2
#> 3 1003 female   20       2    2
#> 4 1004 female   16       3    2
#> 5 1005 female    2       3    2
#> 6 1006 female   71      13    2
```

daysabs : The number of days of absence (Discreate - Positive) 
The value of the daysabs variable cannot be negative. The dayabs variable is positive integer value

math : Math score (kontinu)

program (prog): Student program (Categorical)

  - 1 : General, 
  
  - 2 : Academic,
  
  - 3 : Vocational


Choose 3 variables to be used are math, daysabs, and prog (based on study case)

```r
dat <- dat[,c(3:5)]
dat$prog <-  factor(dat$prog, levels = 1:3, labels = c("General", "Academic", "Vocational"))
summary(dat$daysabs)
```

```
#>    Min. 1st Qu.  Median    Mean 3rd Qu.    Max. 
#>   0.000   1.000   4.000   5.955   8.000  35.000
```

## Distribution of days absent in each program.

```r
ggplot(dat, aes(daysabs, fill = prog)) + geom_histogram(binwidth = 1, breaks=1:90) + facet_grid(prog ~ 
    ., margins = TRUE, scales = "free")
```

<img src="/blog/2019-10-22-poisson-regression-and-negative-binomial-regression_files/figure-html/unnamed-chunk-8-1.png" width="672" style="display: block; margin: auto;" />
In the plots above is a distribution of each program has 314 observations and the distribution seems quite reasonable. The average of our outcome variables is much lower than the variance. It can be seen that the accumulation of data is close to 0, this condition indicates the data are overdispersion. There is no negative data. Because the number of days of absence cannot be negative.

## Build the Model

Poisson regression modeling in R uses the `glm` function, with parameters:

- formula: y~x

- family: "poisson"

- data: data

**If I want to build a model to predict daysabs without any predictor variable using Poisson Regression**

```r
model1 <- glm(daysabs ~1, family = "poisson", data = dat)
summary(model1)
```

```
#> 
#> Call:
#> glm(formula = daysabs ~ 1, family = "poisson", data = dat)
#> 
#> Deviance Residuals: 
#>     Min       1Q   Median       3Q      Max  
#> -3.4512  -2.5184  -0.8525   0.7957   8.1169  
#> 
#> Coefficients:
#>             Estimate Std. Error z value Pr(>|z|)    
#> (Intercept)  1.78430    0.02312   77.16   <2e-16 ***
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> (Dispersion parameter for poisson family taken to be 1)
#> 
#>     Null deviance: 2217.7  on 313  degrees of freedom
#> Residual deviance: 2217.7  on 313  degrees of freedom
#> AIC: 3103
#> 
#> Number of Fisher Scoring iterations: 5
```
The Coefficients intercept is 1.784. Recall, writing the Poisson regression model formula:
$$ \hat{y} = exp(a+bx) = {exp(a)}\times{exp(bx)}$$ 

Our model1 : $$ \hat{y} = exp(a) = exp(1.784)$$ 

```r
exp(model1$coefficients[1])
```

```
#> (Intercept) 
#>    5.955414
```

```r
mean(dat$daysabs)
```

```
#> [1] 5.955414
```
From the result above, exp(intercept) is mean of the target variable (daysabs)


**If I want to predict daysabs based on math scores and student programs with Poisson Regression**


```r
modelpois <- glm(daysabs ~ math + prog, family = "poisson", data = dat)
summary(modelpois)
```

```
#> 
#> Call:
#> glm(formula = daysabs ~ math + prog, family = "poisson", data = dat)
#> 
#> Deviance Residuals: 
#>     Min       1Q   Median       3Q      Max  
#> -4.2597  -2.2038  -0.9193   0.6511   7.4233  
#> 
#> Coefficients:
#>                 Estimate Std. Error z value Pr(>|z|)    
#> (Intercept)     2.651974   0.060736  43.664  < 2e-16 ***
#> math           -0.006808   0.000931  -7.313 2.62e-13 ***
#> progAcademic   -0.439897   0.056672  -7.762 8.35e-15 ***
#> progVocational -1.281364   0.077886 -16.452  < 2e-16 ***
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> (Dispersion parameter for poisson family taken to be 1)
#> 
#>     Null deviance: 2217.7  on 313  degrees of freedom
#> Residual deviance: 1774.0  on 310  degrees of freedom
#> AIC: 2665.3
#> 
#> Number of Fisher Scoring iterations: 5
```

From the summary(modelpois) above,
We get the Poisson model: $$ \hat{daysabs} = exp(2.651974-0.006808math-0.439897progAcademic-1.281364progVocational)$$ 

Intercept, math, progAcademic, and progVocational are significant to the model.

And the AIC (Akaike Information Criterion) value is 2665.3

## Equidispersion Assumption

Base assumption of Poisson regression is **Equidispersion ** : *No Overdispersion* or *No Undispersion*

Equidispersion occurs if the Dispersion parameter approaches 1. 


```r
library(DHARMa)
resp <- simulateResiduals(modelpois, refit = T)
testDispersion(resp, plot = F)
```

```
#> 
#> 	DHARMa nonparametric dispersion test via mean deviance residual
#> 	fitted vs. simulated-refitted
#> 
#> data:  resp
#> dispersion = 6.5899, p-value < 2.2e-16
#> alternative hypothesis: two.sided
```
Based on the results above, we got the *dispersion* value is 6.5899. That means our data is overdispersion (variance is greater than average). Because the dispersion value far exceeds one. Otherwise, if our dispersion value is far below one, then our data is undispersion (variance value is smaller than average).

On this occasion, I only discussed when data is overdispersion. Because generally overdispersion is more common than undispersion


```r
plot(resp)
```

<img src="/blog/2019-10-22-poisson-regression-and-negative-binomial-regression_files/figure-html/unnamed-chunk-13-1.png" width="672" style="display: block; margin: auto;" />
Look at the Q-Q plot residuals. The plot above is a residual plot whether spreads normally or not. When the residual points have followed the diagonal line, then the residuals are normally distributed.

However, Q-Q plot residuals from the Poisson regression model above show that residuals do not follow the normal distribution


## Overdispersion Assumption

Because our data has overdispersion (offense of dispersion assumption). Then the alternative is to use *Negative Binomial Regression*.

Build a Binomial Negative Regression model in R using the `glm.nb` function from the library `car`.

```r
modelnb1 <- glm.nb(daysabs ~ 1, data = dat)
summary(modelnb1)
```

```
#> 
#> Call:
#> glm.nb(formula = daysabs ~ 1, data = dat, init.theta = 0.7999836134, 
#>     link = log)
#> 
#> Deviance Residuals: 
#>     Min       1Q   Median       3Q      Max  
#> -1.8476  -1.0921  -0.3107   0.2621   2.1384  
#> 
#> Coefficients:
#>             Estimate Std. Error z value Pr(>|z|)    
#> (Intercept)   1.7843     0.0672   26.55   <2e-16 ***
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> (Dispersion parameter for Negative Binomial(0.8) family taken to be 1)
#> 
#>     Null deviance: 357.49  on 313  degrees of freedom
#> Residual deviance: 357.49  on 313  degrees of freedom
#> AIC: 1796.9
#> 
#> Number of Fisher Scoring iterations: 1
#> 
#> 
#>               Theta:  0.8000 
#>           Std. Err.:  0.0758 
#> 
#>  2 x log-likelihood:  -1792.9450
```

For the results of Negative Binomial Regression when not using any predictor in the model, it can be seen when using Poisson regression and negative binomial regression the estimated intercept is the same value.

## Negative Binomial Regression

**We want to predict daysabs based on math scores and student programs using Negative Binomial Regression**

```r
modelnb <- glm.nb(daysabs ~ math + prog, data = dat)
summary(modelnb)
```

```
#> 
#> Call:
#> glm.nb(formula = daysabs ~ math + prog, data = dat, init.theta = 1.032713156, 
#>     link = log)
#> 
#> Deviance Residuals: 
#>     Min       1Q   Median       3Q      Max  
#> -2.1547  -1.0192  -0.3694   0.2285   2.5273  
#> 
#> Coefficients:
#>                 Estimate Std. Error z value Pr(>|z|)    
#> (Intercept)     2.615265   0.197460  13.245  < 2e-16 ***
#> math           -0.005993   0.002505  -2.392   0.0167 *  
#> progAcademic   -0.440760   0.182610  -2.414   0.0158 *  
#> progVocational -1.278651   0.200720  -6.370 1.89e-10 ***
#> ---
#> Signif. codes:  0 '***' 0.001 '**' 0.01 '*' 0.05 '.' 0.1 ' ' 1
#> 
#> (Dispersion parameter for Negative Binomial(1.0327) family taken to be 1)
#> 
#>     Null deviance: 427.54  on 313  degrees of freedom
#> Residual deviance: 358.52  on 310  degrees of freedom
#> AIC: 1741.3
#> 
#> Number of Fisher Scoring iterations: 1
#> 
#> 
#>               Theta:  1.033 
#>           Std. Err.:  0.106 
#> 
#>  2 x log-likelihood:  -1731.258
```
From the summary(modelnb) above,
we get the Negative Binomial model: $$ \hat{daysabs} = exp(2.615265   -0.005993math-0.440760progAcademic-1.278651progVocational)$$ 

Intercept, math, progAcademic, and progVocational are significant to the model.

And the AIC (Akaike Information Criterion) value is 1741.3   



```r
#Negative Binomial
resnb <- simulateResiduals(modelnb, refit = T)
plot(resnb)
```

<img src="/blog/2019-10-22-poisson-regression-and-negative-binomial-regression_files/figure-html/unnamed-chunk-16-1.png" width="672" style="display: block; margin: auto;" />

In the Q-Q Plot Residuals above, it can be seen that the residual point has followed a diagonal line (almost perfect). Therefore the residuals are normally distributed.



```r
testDispersion(resnb, plot=F)
```

```
#> 
#> 	DHARMa nonparametric dispersion test via mean deviance residual
#> 	fitted vs. simulated-refitted
#> 
#> data:  resnb
#> dispersion = 1.0913, p-value = 0.216
#> alternative hypothesis: two.sided
```


From the results of the parameter dispersion above, it can be seen that the parameter dispersion value is 1.0913 already close to 1. It means that the Negative Binomial Regression model can reduce the overdispersion problem.

## The Best Model Comparison

- *Comparison of AIC values*

```r
modelpois$aic 
```

```
#> [1] 2665.285
```

```r
modelnb$aic 
```

```
#> [1] 1741.258
```
Compare the models, the Poisson Regression model and the Negative Binomial Regression model based on the AIC value. Then the Negative Binomial Regression model is the model with the smallest AIC value.

- *Comparison Dispersion Parameters*

Dispersion (Poisson Regression) value : 6.5899

Dispersion (Negative Binomial Regression) value  : 1.0913

Based on the value of the dispersion parameters of the two models above, it can be seen the Negative Binomial Regression
model that has a dispersion value close to 1.

- *Comparison The Residuals*

The comparison of residuals based on the Q-Q plot residuals, the residuals Binomial Negative regression model is obtained which has a normally distributed residual.

## Model Interpretation 

Based on the explanation above, the best model chosen is the *Binomial Negative Regression* model.

Negative Binomial model: $$ \hat{daysabs} = exp(2.615265   -0.005993math-0.440760progAcademic-1.278651progVocational)$$ 

*or* : $$ \hat{daysabs} = exp(2.615265)*exp(-0.005993*math)*exp(-0.440760*progAcademic)*exp(-1.278651*progVocational)$$ 

```r
(estnb <- cbind(Estimate = coef(modelnb)))
```

```
#>                    Estimate
#> (Intercept)     2.615265446
#> math           -0.005992988
#> progAcademic   -0.440760012
#> progVocational -1.278650721
```


```r
exp(estnb)
```

```
#>                  Estimate
#> (Intercept)    13.6708448
#> math            0.9940249
#> progAcademic    0.6435471
#> progVocational  0.2784127
```

  - **Every one-unit increase in the value of math, then daysabs(y) will decrease (-) by 0.9940249 times, provided that other predictor variables are constant**

  - **If a student is an academic program, then daysabs(y) will decrease (-) by 0.6435471 times compared to if the student is a general program, provided that other predictor variables are constant**

  - **If a student is an Vocational program, then daysabs(y) will decrease (-) by 0.2784127 times compared to if the student is a general program, provided that other predictor variables are constant**




## Predicted Value


*Proof :*

**"Every one-unit increase in the value of math, then daysabs(y) will decrease (-) by 0.9940249 times, provided that other predictor variables are constant"**

If Ani and Budi are general program students and have math grades 97 and 96, then how many days are they absent?

```r
daysabs.Ani <- exp(2.615265-0.005993*97)
daysabs.Budi <- exp(2.615265-0.005993*96)
daysabs.Ani 
```

```
#> [1] 7.644176
```

```r
daysabs.Budi
```

```
#> [1] 7.690125
```



```r
daysabs.Ani2 <- daysabs.Budi*0.9940249

daysabs.Ani 
```

```
#> [1] 7.644176
```

```r
daysabs.Ani2
```

```
#> [1] 7.644175
```
From the results above it is proven that : **"Every one-unit increase in the value of math, then daysabs(y) will decrease (-) by 0.9940249 times, provided that other predictor variables are constant"**



If I make a new data frame with 3 observations and have a different program.
Look for the predicted daysabs value based on the new data.


```r
newdata1 <- data.frame(math = mean(dat$math), prog = factor(1:3, levels = 1:3, labels = levels(dat$prog)))
```

type = "link" produces a predict value in the form of "log".

```r
newdata1$pred <- predict(modelnb, newdata1, type = "link")
newdata1
```

```
#>       math       prog     pred
#> 1 48.26752    General 2.325999
#> 2 48.26752   Academic 1.885239
#> 3 48.26752 Vocational 1.047348
```

type = "response"  generate predictive value that has been exponential "exp".

```r
newdata1$pred <- predict(modelnb, newdata1, type = "response")
newdata1
```

```
#>       math       prog      pred
#> 1 48.26752    General 10.236899
#> 2 48.26752   Academic  6.587927
#> 3 48.26752 Vocational  2.850083
```

```r
exp(estnb)
```

```
#>                  Estimate
#> (Intercept)    13.6708448
#> math            0.9940249
#> progAcademic    0.6435471
#> progVocational  0.2784127
```


```r
#academic dan general
10.236899*0.6435471 #academic
```

```
#> [1] 6.587927
```

```r
#vocational dan general
10.236899*0.2784127
```

```
#> [1] 2.850083
```
From the results above it is proven that :

- **If a student is an academic program, then daysabs(y) will decrease (-) by 0.6435471 times compared to if the student is a general program, provided that other predictor variables are constant**

- **If a student is an Vocational program, then daysabs(y) will decrease (-) by 0.2784127 times compared to if the student is a general program, provided that other predictor variables are constant**
