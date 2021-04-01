# Analysis of CN mass for LPNL experiment

## Created:

* 4 March 2019

## Modified

* 2019-04-19 - KF - added analysis of the CN results
* 2020-11-01 - KF- worked on analysis of perc_C for the manuscript
* 2020-11-05 - KF- worked on analysis of perc_N and CN for the manuscript
* 2021-03-24 - KF- calculated the change in percent C and percent Nof the leaves
* 2021-04-01 - KF- continued calculation of the change in percent C and percent N during the incubation


## Authors

* Kenneth Fortino
* Alyssa Oppedisano

## Description

This file contains the code for the statistical analysis of the percent C and percent N data from the litter priming nutrient experiment. The details of the experimental design can be found in the lab notes: 
[https://drive.google.com/drive/folders/15sfGYy5rDuMRrgP2-NH2359eWHkWOVbm?usp=sharing](https://drive.google.com/drive/folders/15sfGYy5rDuMRrgP2-NH2359eWHkWOVbm?usp=sharing)

## Analysis

### Import Data

    CN <- read.table("./data/CN_all.csv", header = T, sep = ",")
    
## Analysis
    
### Calculate the difference between the location for response variables
    
    perc.N.diff <- CN$perc_N[CN$Location == "Top"] - CN$perc_N[CN$Location == "Sed"]
    perc.C.diff <- CN$perc_C[CN$Location == "Top"] - CN$perc_C[CN$Location == "Sed"]
    CN.mol.diff <- CN$CN_mol[CN$Location == "Top"] - CN$CN_mol[CN$Location == "Sed"]
    
### Create Treatment Values for Diff data.frame
    
    Date.diff <- CN$Date[CN$Location == "Top"]
    Treat.diff <- CN$Treat[CN$Location == "Top"]
    Nutrients.diff <- CN$Nutrients[CN$Location == "Top"]
    Glucose.diff <- CN$Glucose[CN$Location == "Top"]
    
#### Create CN.diff data.frame
    
    CN.diff <- data.frame(Date.diff, Treat.diff, Nutrients.diff, Glucose.diff, perc.C.diff, perc.N.diff, CN.mol.diff)

## Summary of Perc C
### Summary of Perc C by date across both locations and treatments
    
    tapply(CN$perc_C, CN$Date, summary)
    tapply(CN$perc_C, CN$Date, sd)
    
    #############################################
    # Percent C by date harvested across all samples
    
    $`2018-11-12`
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    41.25   45.99   46.52   46.38   47.06   48.63 1.265312   
    
    $`2019-02-07`
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    31.83   41.03   43.20   42.60   45.03   50.00 3.862320 
    
    ##################################################

### Effect of Treatment Additions on Perc C
    
Although there was an effect of location on the perc C (i.e., the difference between the TOP and the SED perc C did not equal 0), there was no effect of the treatment additions on the difference (see below). Given this, I am analyzing the effect of the treatments on percent C for each date but I am pooling the TOP and SED samples.
    
#### Effect of treatment additons on Perc C after 2 Weeks
    
    anova(lm(perc_C ~ Glucose * Nutrients, data = CN, subset = Date == "2018-11-12"))
    
    ##################################################
    # Two-way ANOVA of the effect of the treatment additions on the percent C after 2 weeks
    
    Analysis of Variance Table
    
    Response: perc_C
                       Df Sum Sq  Mean Sq F value  Pr(>F)  
    Glucose            1  4.938   4.9377  3.5503   0.06996 .
    Nutrients          1  4.083   4.0827  2.9356   0.09770 .
    Glucose:Nutrients  1  1.670   1.6699  1.2007   0.28252  
    Residuals         28  38.941  1.3908       
    
    ################################################## 

#### Effect of treatment additons on Perc C after 2 Weeks
    
    anova(lm(perc_C ~ Glucose * Nutrients, data = CN, subset = Date == "2019-02-07"))

    ##################################################
    # Two-way ANOVA of the effect of the treatment additions on the percent C after 14 weeks
    Analysis of Variance Table
    
    Response: perc_C
                       Df Sum Sq  Mean Sq F value Pr(>F)
    Glucose            1   4.97   4.9691  0.3071  0.5838
    Nutrients          1   4.46   4.4626  0.2758  0.6036
    Glucose:Nutrients  1   0.00   0.0000  0.0000  0.9997
    Residuals         28   453.01 16.1790  
    
    ################################################## 
    
After 2 weeks and 14 weeks, both the glucose and nutrient additions showed no significant effect on percent C.
    
### Percent C by location for each week    
#### Week 2    
   
    tapply(CN$perc_C[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], summary)
    tapply(CN$perc_C[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], sd)
    
    ##################################################    
    # Percent C by location after Week 2
    
    $Sed
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    41.25   45.55   46.19   45.94   46.61   47.41 1.4140472
    
    $Top
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD 
    44.90   46.46   46.95   46.83   47.46   48.63 0.9426346  
    
    ##################################################    

#### Week 14
    
    tapply(CN$perc_C[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2019-02-07"], summary, na.rm = T)
    tapply(CN$perc_C[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2019-02-07"], sd, na.rm = T)
    
    ##################################################
    # Percent C by location after Week 14
    
    $Sed
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    31.83   38.31   40.83   40.96   44.22   50.00 4.844574
    
    $Top
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    42.46   43.11   44.20   44.24   45.03   47.09 1.262288
    
    ##################################################    

    
### Summary Stats for Differences 
#### Perc_C in the TOP location minus the Perc C in the SED location by date 

    tapply(CN.diff$perc.C.diff, CN.diff$Date.diff, summary)
    tapply(CN.diff$perc.C.diff, CN.diff$Date.diff, sd)
    
    ##########################################
    # Summary of Perc C top - Perc C sed by date 
    
    $`2018-11-12`
       Min.    1st Qu.  Median  Mean    3rd Qu.    Max.     SD 
      -0.4900  -0.1150  0.7100  0.8881  1.3600     3.6500   1.228361

    $`2019-02-07`
       Min.    1st Qu.  Median    Mean    3rd Qu.    Max.       SD
      -5.0200  0.8925   4.0400    3.2881  6.0025     12.4600    4.567996 
       
    ##########################################
       
### Test of location Effect
#### 2 Weeks
##### Percent C Difference (TOP - SED)
    
    t.test(perc.C.diff[CN.diff$Date == "2018-11-12"], mu = 0)
    
    ##################################################    
    # t-test results for the hypothesis that the mean difference between the TOP and SED location is equal to 0.
       
    One Sample t-test
    
    data:  perc.C.diff[CN.diff$Date == "2018-11-12"]
    t = 2.8921, df = 15, p-value = 0.01117
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      0.2335779 1.5426721
    sample estimates:
      mean of x 
    0.888125  

    ##################################################        

After 2 weeks, the percent C of the TOP samples minus the percent C of the SED samples was not equal to 0 and had a mean of 0.889 percent, which indicates that the TOP samples had a greater percent C after 2 weeks of incubation.
    
#### 14 Weeks
##### Percent C Difference (TOP - SED)
    
    t.test(perc.C.diff[CN.diff$Date == "2019-02-07"], mu = 0)
    
    ##################################################    
    # t-test results for the hypothesis that the mean difference between the TOP and SED location is equal to 0.
    
    One Sample t-test
    
    data:  perc.C.diff[CN.diff$Date == "2019-02-07"]
    t = 2.8793, df = 15, p-value = 0.01147
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      0.8540116 5.7222384
    sample estimates:
      mean of x 
    3.288125 
    
    ##################################################    
        
After 14 weeks, the percent C of the TOP samples minus the percent C of the SED samples was not equal to 0 and had a mean of 3.288 percent, which indicates that the TOP samples had a greater percent C after 14 weeks of incubation and that the difference was greater than in was after 2 weeks.

### Test of treatment effects on percent C difference between TOP and SED locations
#### Two-way ANOVA of the effect of glucose and nutrient additions on the difference between the perc C of the TOP and SED samples after 2 weeks
    
    anova(lm(perc.C.diff ~ Glucose.diff * Nutrients.diff, data = CN.diff, subset = Date.diff == "2018-11-12"))
   
    ################################################## 
    # ANOVA of the effect of glucose and nutrients on perc C TOP - perc C SED after 2 weeks
    
    Analysis of Variance Table
    
    Response: perc.C.diff
                                 Df  Sum Sq   Mean Sq F value Pr(>F)
    Glucose.diff                 1   0.0053   0.0053  0.0035  0.9540
    Nutrients.diff               1   1.0973   1.0973  0.7246  0.4113
    Glucose.diff:Nutrients.diff  1   3.3581   3.3581  2.2175  0.1623
    Residuals                   12   18.1725  1.5144     
    
    ##################################################

After 2 weeks, there were no significant effects of the treatment additions on the difference in the perc C between the TOP and SED locations, indicating that the greater percent C in the TOP location was not affected by the increased labile C or N and P.
    
        
#### Two-way ANOVA of the effect of glucose and nutrient additions on the difference between the perc C of the TOP and SED samples after 14 weeks.
    
    anova(lm(perc.C.diff ~ Glucose.diff * Nutrients.diff, data = CN.diff, subset = Date.diff == "2019-02-07"))
    
    ################################################## 
    # ANOVA of the effect of glucose and nutrients on perc C TOP - perc C SED after 14 weeks
    
    Analysis of Variance Table
    
    Response: perc.C.diff
                                 Df  Sum Sq  Mean Sq F value Pr(>F)
    Glucose.diff                 1   0.183   0.1828  0.0070  0.9346
    Nutrients.diff               1   0.574   0.5738  0.0221  0.8844
    Glucose.diff:Nutrients.diff  1   0.041   0.0410  0.0016  0.9690
    Residuals                   12   312.201 26.0168
    
    ################################################## 
    
After 14 weeks, there were no significant effects of the treatment additions on the difference in the perc C between the TOP and SED locations, indicating that the greater percent C in the TOP location was not affected by the increased labile C or N and P.
    
## Summary of Perc N
### Summary of Perc N by date across all locations and treatments
    
    tapply(CN$perc_N, CN$Date, summary)
    tapply(CN$perc_N, CN$Date, sd)
    
    ##################################################    
    # Percent N by date across all locations and treatments
    
    $`2018-11-12`
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD 
    0.650   0.900   1.080   1.113   1.250   2.050 0.3092492  
    
    $`2019-02-07`
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    0.750   1.285   1.695   1.666   1.905   3.300 0.5300879 
    
    ##################################################    

### Effect of Treatment Additions on Perc N
    
There was no effect of location on percent N (see below). The following tests to see if there was an effect of the additions of glucose and nutrients on the percent nitrogen after 2 weeks and 14 weeks of incubation.
    
#### Effect of treatment additons on Perc N after 2 Weeks
    
    anova(lm(perc_N ~ Glucose * Nutrients, data = CN, subset = Date == "2018-11-12"))
    
    ##################################################
    # Two-way ANOVA of the effect of the treatment additions on the percent N after 2 weeks
    
    Analysis of Variance Table
    
    Response: perc_N
                       Df  Sum Sq  Mean Sq  F value  Pr(>F)  
    Glucose            1   0.37411 0.37411  4.6615   0.03956 *
    Nutrients          1   0.33620 0.33620  4.1891   0.05018 .
    Glucose:Nutrients  1   0.00720 0.00720  0.0897   0.76675  
    Residuals         28   2.24717 0.08026          
    
    ################################################## 
    
##### Summary of percent N by glucose and nutrient additions after 2 weeks
###### Glucose
    
    tapply(CN$perc_N[CN$Date == "2018-11-12"], CN$Glucose[CN$Date == "2018-11-12"], summary)
    tapply(CN$perc_N[CN$Date == "2018-11-12"], CN$Glucose[CN$Date == "2018-11-12"], sd)

    ################################################## 
    # Summary of the perc N in samples with and without glucose additions after 2 weeks
    
    $N
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.   SD
    0.840   0.995   1.090   1.221   1.327   2.050  0.3369842 
    
    $Y
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.   SD
    0.650   0.775   1.060   1.005   1.165   1.390  0.2432009 
    
    ################################################## 
    
The perc N after 2 weeks was significantly different in the samples that received glucose additions. Evaluation of the magnitiude of the effect shows that the mean percent N of the samples with glucose additions was lower than those without by 0.216 percent.
    
###### Nutrients
    
    tapply(CN$perc_N[CN$Date == "2018-11-12"], CN$Nutrients[CN$Date == "2018-11-12"], summary)
    tapply(CN$perc_N[CN$Date == "2018-11-12"], CN$Nutrients[CN$Date == "2018-11-12"], sd)

    ################################################## 
    # Summary of the perc N in samples with and without nutrients additions after 2 weeks
    
    $N
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    SD
    0.6500  0.8375  1.0300  1.0106  1.0850  1.8100  0.2607801 
    
    $Y
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    SD
    0.760   1.018   1.240   1.216   1.353   2.050   0.3274542 
    
    ################################################## 
    
The perc N after 2 weeks was significantly different in the samples that received nutrients additions. Evaluation of the magnitiude of the effect shows that the mean percent N of the samples with nutrient additions was greater than those without by 0.186 percent.
    
There was no significant interaction between the nutrient and glucose additions.
   
#### Effect of treatment additons on Perc N after 14 Weeks
    
    anova(lm(perc_N ~ Glucose * Nutrients, data = CN, subset = Date == "2019-02-07"))

    ##################################################
    # Two-way ANOVA of the effect of the treatment additions on the percent C after 14 weeks
    
    Analysis of Variance Table
    
    Response: perc_N
                       Df Sum Sq Mean Sq  F value Pr(>F)  
    Glucose            1  0.0021 0.00211  0.0083  0.9280  
    Nutrients          1  1.4535 1.45351  5.7140  0.0238 *
    Glucose:Nutrients  1  0.1326 0.13261  0.5213  0.4763  
    Residuals         28  7.1225 0.25438  
    
    ################################################## 

##### Summary of percent N of the samples with and without nutrient additions after 14 weeks

    tapply(CN$perc_N[CN$Date == "2019-02-07"], CN$Nutrients[CN$Date == "2019-02-07"], summary)
    tapply(CN$perc_N[CN$Date == "2019-02-07"], CN$Nutrients[CN$Date == "2019-02-07"], sd)

    ################################################## 
    # Summary of the perc N in samples with and without nutrients additions after 14 weeks
    
    $N
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    SD 
    0.750   1.107   1.455   1.452   1.810   2.130   0.4030467 
    
    $Y
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.    SD
    1.070   1.510   1.760   1.879   2.225   3.300   0.5668965 
    
    ################################################## 
    
    
After 14 weeks the effect of the addition of glucose seen in after 2 weeks is no longer significant but there remains a significant effect of nutrients. The magnitiude of the effect was approximately double with the samples with nutrient additions having a mean percent N that was 0.427 percent greater than those without nutrient additions.
    
    
### Summary of Perc N by location    
#### Percent N in the TOP and SED locations across all treatments after Week 2
    
    tapply(CN$perc_N[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], summary)
    tapply(CN$perc_N[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], sd)
    
    ##################################################    
    # Summary of Perc N by location after Week 2
    
    $Sed
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    0.760   1.038   1.080   1.111   1.260   1.390 0.1915507 
    
    $Top
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.   SD
    0.650   0.825   1.020   1.115   1.250   2.050 0.4011816
    
    ##################################################    
    
#### Percent N in the TOP and SED locations across all treatments after Week 14
    
    tapply(CN$perc_N[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2019-02-07"], summary, na.rm = T)
    tapply(CN$perc_N[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2019-02-07"], sd, na.rm = T)
    
    ##################################################    
    # Percent N by location after Week 14
    
    $Sed
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.   SD 
    0.900   1.465   1.735   1.645   1.840   2.330 0.3953732  
    
    $Top
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD 
    0.750   1.228   1.650   1.686   2.145   3.300 0.6507624
    
    ##################################################    
    
### Test of location Effect on Percent N
#### 2 Weeks
##### Percent N Difference (TOP - SED)
    
    t.test(perc.N.diff[CN.diff$Date == "2018-11-12"], mu = 0)
    
    ##################################################
    # t-test that the percent N in the TOP location minus the percent N in the SED location is equal to 0 after 2 weeks.
    
    One Sample t-test
    
    data:  perc.N.diff[CN.diff$Date == "2018-11-12"]
    t = 0.03782, df = 15, p-value = 0.9703
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -0.2075923  0.2150923
    sample estimates:
      mean of x 
    0.00375 
    
    ##################################################
    
The mean of the difference in percent N between the TOP and SED samples was not different from 0 after 2 weeks, which indicates that there was not effect of location on the percent N.
    
    
#### 14 Weeks
##### Percent N Difference (TOP - SED)
    
    t.test(perc.N.diff[CN.diff$Date == "2019-02-07"], mu = 0)
    
    ##################################################
    
    One Sample t-test
    
    data:  perc.N.diff[CN.diff$Date == "2019-02-07"]
    t = 0.2777, df = 15, p-value = 0.785
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -0.275355  0.357855
    sample estimates:
      mean of x 
    0.04125 
    
    ##################################################
    
The mean of the difference in percent N between the TOP and SED samples was not different from 0 after 14 weeks, which indicates that there was not effect of location on the percent N.

# Analysis of the change in the C and N content of the leaves
    
These analyses are to determine how the C and N content of the leaves change during the course of the incubation. 

## Import the initial leaf C and N content

The initial percent C and N comes from the analyses from the leached litter experiment.  The details can be found in the mass loss analysis script [https://github.com/KennyPeanuts/sediment_priming/blob/master/lab_notebook/analysis/mass_loss_analysis.md#determine-the-average-initial-percent-c-and-percent-n](https://github.com/KennyPeanuts/sediment_priming/blob/master/lab_notebook/analysis/mass_loss_analysis.md#determine-the-average-initial-percent-c-and-percent-n)
 
    mean_initial_percC <- 45.13
    mean_initial_percN <- 0.9850
    
### Variable descriptions
    
* mean_initial_percC = the estimated percent C in the leaves prior to incubation in the experiment. This value was collected during the leached litter exp. (percent).
    
* mean_initial_percN = the estimated percent N in the leaves prior to incubation in the experiment. This value was collected during the leached litter exp. (percent).

* delta_C = the loss of percent C of the leaf discs from the estimated initial percent C (percent) - note: positive numbers indicate a loss of percent C

* delta_N = the loss of percent N of the leaf discs from the estimated initial percent N (percent) - note: positive numbers indicate a loss of percent N
    
## Determine the loss of percent C and N
### Loss of percent C    

    delta_C <- mean_initial_percC - CN$perc_C
    
#### Summary of all data by date

    tapply(delta_C, CN$Date, summary)
    tapply(delta_C, CN$Date, sd)
    
    ##################################################
    # Data summary of the loss of  percent C in the leaves for 2 and 14 weeks of incubation
    
    $`2018-11-12`
    Min.      1st Qu.    Median    Mean      3rd Qu.    Max.     SD 
    -3.5000   -1.9275    -1.3850   -1.2509   -0.8575    3.8800   1.265312  

    $`2019-02-07`
    Min.      1st Qu.   Median     Mean      3rd Qu.    Max.     SD 
    -4.870    0.100     1.935      2.530     4.095      13.300   3.862320 
    
    ##################################################

#### Summary of loss of percent C by location for both incubation times
    
    tapply(delta_C[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], summary)
    tapply(delta_C[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], sd)
    tapply(delta_C[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2018-11-12"], summary)
    tapply(delta_C[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2018-11-12"], sd)
    
    ################################################## 
    # Summary of the loss in the percent C by location for the 2 and 14 week incubations
    
    ## 2 weeks 
    $Sed
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.     SD 
    -2.2800    -1.4825    -1.0550   -0.8069   -0.4200    3.8800   1.4140472 

    $Top
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.     SD
    -3.500     -2.333     -1.825    -1.695    -1.330     0.230    0.9426346
    
    ## 14 Weeks
    $Sed
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.     SD
    -4.8700     0.9075     4.3000    4.1744    6.8175     13.3000  4.844574 

    $Top
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.     SD 
    -1.9600    0.1000     0.9250    0.8862    2.0225     2.6700   1.262288 
    
    ##################################################  
  
The percent C of the leaf discs increased during the 2 week incubation with a greater increase seen in the leaf discs in the top location. However, this pattern reversed in the 14 week incubation where the percent C of the leaf discs decresed over the 14 week incbation and the greater decrease was in the leaves in the Sed location.
    

### Loss of percent N    
    
    delta_N <- mean_initial_percN - CN$perc_N
    
#### Summary of all data by date

    tapply(delta_N, CN$Date, summary)
    tapply(delta_N, CN$Date, sd)
    
    ##################################################
    # Data summary of the loss in percent N in the leaves for 2 and 14 weeks of incubation
    
    $`2018-11-12`
    Min.      1st Qu.   Median    Mean     3rd Qu.    Max.     SD 
    -1.0650   -0.2650  -0.0950    -0.1281  0.0850     0.3350   0.3092492  

    $`2019-02-07`
    Min.      1st Qu.   Median    Mean     3rd Qu.    Max.     SD 
    -2.3150   -0.9200  -0.7100    -0.6806  -0.3000    0.2350   0.5300879

    ##################################################
    
The percent N of the leaf discs increased (negative loss) over the course of the experiment after both 2 and 14 weeks of incubation.

#### Summary of change in percent N by location for both incubation times
    
    tapply(delta_N[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], summary)
    tapply(delta_N[CN$Date == "2018-11-12"], CN$Location[CN$Date == "2018-11-12"], sd)
    tapply(delta_N[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2019-02-07"], summary)
    tapply(delta_N[CN$Date == "2019-02-07"], CN$Location[CN$Date == "2019-02-07"], sd)
    
    ################################################## 
    # Summary of the loss in the percent C by location for the 2 and 14 week incubations
    
    ## 2 weeks 
    
    $Sed
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.      SD 
    -0.4050    -0.2750    -0.0950   -0.1263   -0.0525    0.2250    0.1915507 

    $Top
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.      SD
    -1.065     -0.265     -0.035    -0.130    0.160      0.335     0.4011816 
    
    ## 14 Weeks
    
    $Sed
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.      SD 
    -1.345     -0.855     -0.750    -0.660    -0.480     0.085     0.3953732 

    $Top
    Min.       1st Qu.    Median    Mean      3rd Qu.    Max.      SD 
    -2.3150    -1.1600    -0.6650   -0.7013   -0.2425    0.2350    0.6507624 
    
    ##################################################  
  
The increase in the percent N of the leaves was not related to location and was greater after 14 weeks than 2 weeks.
