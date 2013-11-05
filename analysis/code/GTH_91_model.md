# Analysis of model predicting TD by Area, Kd, and Julian day

These analyses are for the revisions to transparency model manuscript. This MS is based on the model that was removed from the boondoggle manuscript for Island Waters

> They state that thermocline depth in GTH 91 was predicted from the relationship between thermocline depth, Kd, surface area, and Julian day established in the surveys.  In the first section (Lake surveys) the authors note that Kd was significantly related to lake surface area and Julian day. Is there not an issue with collinearly if both Kd, surface area and Julian day are then used as independent variables in a regression? Indeed, there is no mention here that any of the assumptions of linear regression were assessed.

In "model_colinearity_analysis.R" I assess the collinearity of the predictors and determine that Kd and Area and Kd and Julian Day are slightly collinear (r^2^ = 0.1 and 0.2 resp.)

In this analysis I am assessing the effect of removing the correlated predictors from the model of GTH 91

## Predicting TD via Kd and Julian

### Load Data

    load("./data/boondoggle")

### Model of TD based on Kd and Julian Day

    summary(lm(TD ~ Kd + Julian, data = boon.tot))

#### Output

~~~~

> summary(lm(TD ~ Kd + Julian, data = boon.tot))

Call:
lm(formula = TD ~ Kd + Julian, data = boon.tot)

Residuals:
    Min      1Q  Median      3Q     Max 
-1.9244 -0.6356 -0.1058  0.5026  4.0368 

Coefficients:
            Estimate Std. Error t value Pr(>|t|)    
(Intercept) -2.18072    3.37406  -0.646  0.52159    
Kd          -4.58008    0.89308  -5.128 7.01e-06 ***
Julian       0.04947    0.01492   3.317  0.00189 ** 
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

Residual standard error: 1.062 on 42 degrees of freedom
  (2 observations deleted due to missingness)
Multiple R-squared: 0.6253,	Adjusted R-squared: 0.6075 
F-statistic: 35.05 on 2 and 42 DF,  p-value: 1.115e-09

~~~~

## Using this model to predict TD in GTH 91 based on measurements of Kd and Julian day


### Load Workspace GTH91.case.ms

    load("./data/GTH91.case.ms")

### Define the model:

Thermocline Depth = -2.18072 + Kd * -4.58008 + Julian Day * 0.04947


    est.TD <- -2.18072 + GTH91.case.ms$Kd * -4.58008 + GTH91.case.ms$Julian * 0.04947
est.TD


#### Output Estimated TD based on the model

    > est.TD
    [1] 1.461317 2.382422 3.147795 3.745557 3.732310 5.813988 3.196771 3.450095
    [9] 4.382167 4.010301 5.001018


### Regression of the estimated TD based on Kd + Julian by the Julian Day


    summary(lm(est.TD ~ GTH91.case.ms$Julian))

#### Output

~~~~

> summary(lm(est.TD ~ GTH91.case.ms$Julian))

Call:
lm(formula = est.TD ~ GTH91.case.ms$Julian)

Residuals:
    Min      1Q  Median      3Q     Max 
-0.8935 -0.2864 -0.0159  0.3012  0.7188 

Coefficients:
                      Estimate Std. Error t value Pr(>|t|)    
(Intercept)          -11.56633    2.23180  -5.183 0.000577 ***
GTH91.case.ms$Julian   0.07822    0.01143   6.841 7.55e-05 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

Residual standard error: 0.5024 on 9 degrees of freedom
Multiple R-squared: 0.8387,	Adjusted R-squared: 0.8208 
F-statistic:  46.8 on 1 and 9 DF,  p-value: 7.551e-05 

~~~~

### Regression of the actual TD by Julian Day in GTH91

    summary(lm(GTH91.case.ms$TD ~ GTH91.case.ms$Julian))

# Output

~~~~

> summary(lm(GTH91.case.ms$TD ~ GTH91.case.ms$Julian))

Call:
lm(formula = GTH91.case.ms$TD ~ GTH91.case.ms$Julian)

Residuals:
     Min       1Q   Median       3Q      Max 
-0.61144 -0.19899 -0.01234  0.22725  0.53834 

Coefficients:
                       Estimate Std. Error t value Pr(>|t|)    
(Intercept)          -13.049642   1.624174  -8.035 2.14e-05 ***
GTH91.case.ms$Julian   0.087557   0.008322  10.522 2.34e-06 ***
---
Signif. codes:  0 ‘***’ 0.001 ‘**’ 0.01 ‘*’ 0.05 ‘.’ 0.1 ‘ ’ 1 

Residual standard error: 0.3656 on 9 degrees of freedom
Multiple R-squared: 0.9248,	Adjusted R-squared: 0.9165 
F-statistic: 110.7 on 1 and 9 DF,  p-value: 2.34e-06

~~~~

### Regression of estimated TD by Julian using median Kd

#### load GTH91 data

    load("./data/GTH91.case.ms")

#### Define the model:

Thermocline Depth = -2.18072 + Kd * -4.58008 + Julian Day * 0.04947


    est.TD.medianKd <- -2.18072 + median(GTH91.case.ms$Kd) * -4.58008 + GTH91.case.ms$Julian * 0.04947

    est.TD.medianKd

#### Output Estimated TD based on the median Kd 

~~~~

est.TD.medianKd
 [1] 2.377791 2.872491 3.268251 3.861891 4.208181 4.505001 2.971431 3.416661
 [9] 3.664011 4.010301 4.455531

~~~~

#### Regression of estimated TD with median by Julian Day

    summary(lm(est.TD.medianKd ~ Julian, data = GTH91.case.ms))

##### Output

~~~~

summary(lm(est.TD.medianKd ~ Julian, data = GTH91.case.ms))

Call:
lm(formula = est.TD.medianKd ~ Julian, data = GTH91.case.ms)

Residuals:
       Min         1Q     Median         3Q        Max 
-5.661e-16 -3.260e-16  1.286e-17  3.188e-16  6.060e-16 

Coefficients:
              Estimate Std. Error    t value Pr(>|t|)    
(Intercept) -6.032e+00  1.881e-15 -3.208e+15   <2e-16 ***
Julian       4.947e-02  9.635e-18  5.134e+15   <2e-16 ***
---


Residual standard error: 4.233e-16 on 9 degrees of freedom
Multiple R-squared:     1,	Adjusted R-squared:     1 
F-statistic: 2.636e+31 on 1 and 9 DF,  p-value: < 2.2e-16

~~~~

#### Estimation of TD using the DOC value from SCW

The 1.04 d-1 value is the estimated Kd based on the 5.5 mg L-1 concentration from SCW (see `climate_change_range_analysis.md` )

    est.TD.DOCKd <- -2.18072 + 1.04 * -4.58008 + GTH91.case.ms$Julian * 0.04947

    est.TD.DOCKd

##### Output

~~~~

est.TD.DOCKd
 [1] 1.465897 1.960597 2.356357 2.949997 3.296287 3.593107 2.059537 2.504767
 [9] 2.752117 3.098407 3.543637

~~~~

##### Regression of TD estimated from SCW-DOC value by Julian

    summary(lm(set.TD.DOCKd        
