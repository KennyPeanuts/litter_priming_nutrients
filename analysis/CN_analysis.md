# Analysis of CN mass for LPNL experiment

## Created:

* 4 March 2019

## Modified

* 2019-April-19 - KF - added analysis of the CN results
* 2020-11-01 - KF- worked on analysis of perc_C for the manuscript
* 2020-11-05 - KF- worked on analysis of perc_N and CN for the manuscript


## Authors

* KF
* AO

## Description

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

## Summary Statistics
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
        
After 14 weeks, the percent C of the TOP samples minus the percent C of the SED samples was not equal to 0 and had a mean of 0.889 percent, which indicates that the TOP samples had a greater percent C after 14 weeks of incubation.
    
## Summary of Perc N
#### Summary of Perc N by date across all locations and treatments
    
    tapply(CN$perc_N, CN$Date, summary)
    tapply(CN$perc_N, CN$Date, sd)
    
##################################################    
# Percent C by date across all locations and treatments
    
    $`2018-11-12`
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD 
    0.650   0.900   1.080   1.113   1.250   2.050 0.3092492  
    
    $`2019-02-07`
    Min. 1st Qu.  Median    Mean 3rd Qu.    Max.  SD
    0.750   1.285   1.695   1.666   1.905   3.300 0.5300879 
    
##################################################    

### Summary of Perc N by location    
#### Week 2
    
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
#### Week 14
    
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
##### Percent N
    
    t.test(perc.N.diff[CN.diff$Date == "2018-11-12"], mu = 0)
    
    #===========================
    
    One Sample t-test
    
    data:  perc.N.diff[CN.diff$Date == "2018-11-12"]
    t = 0.03782, df = 15, p-value = 0.9703
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -0.2075923  0.2150923
    sample estimates:
      mean of x 
    0.00375 
    
    #===========================
    
##### CN 
    
    t.test(CN.mol.diff[CN.diff$Date == "2018-11-12"], mu = 0)
    
    #===========================
    
    One Sample t-test
    
    data:  CN.mol.diff[CN.diff$Date == "2018-11-12"]
    t = 1.334, df = 15, p-value = 0.2021
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -2.770642 12.039666
    sample estimates:
      mean of x 
    4.634512 
    
    #============================
    
    
#### 14 Weeks
##### Percent C
    
    
##### Percent N
    
    t.test(perc.N.diff[CN.diff$Date == "2019-02-07"], mu = 0)
    
    #===========================
    
    One Sample t-test
    
    data:  perc.N.diff[CN.diff$Date == "2019-02-07"]
    t = 0.2777, df = 15, p-value = 0.785
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -0.275355  0.357855
    sample estimates:
      mean of x 
    0.04125 
    
    #===========================
    
##### CN 
    
    t.test(CN.mol.diff[CN.diff$Date == "2019-02-07"], mu = 0)
    
    #===========================
    
    One Sample t-test
    
    data:  CN.mol.diff[CN.diff$Date == "2019-02-07"]
    t = 2.0818, df = 15, p-value = 0.0549
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -0.1094715  9.2843269
    sample estimates:
      mean of x 
    4.587428 
    
    #============================
    
<<<<<<< HEAD
### Analysis of Treatment and Date Effects
#### Perc C Difference by Treatment ANOVA
    
    perc.C.diff.aov.2weeks <- aov(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2018-11-12")
    summary(perc.C.diff.aov.2weeks)
    
##################################################
    
    perc.C.diff.aov.14 <- aov(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2019-02-07")
    summary(perc.C.diff.aov.14)
    
    plot(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2018-11-12", ylim = c(-5, 15))
    abline(h = 0)
    
    plot(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2019-02-07", ylim = c(-5, 15))
=======
### Plot of Difference by Date
    
    boxplot(CN.diff$perc.C.diff[CN.diff$Date.diff == "2018-11-12"], CN.diff$perc.C.diff[CN.diff$Date.diff == "2019-02-07"], axes = F, ylab = "Difference in Percent C")
    axis(2)
    axis(1, c("2-weeks", "14-weeks"), at = c(1, 2))
>>>>>>> fe5a92e4e836f7209f4c948322b16e8113a41441
    abline(h = 0)
    box()
    
#### Analysis of Treatment Effect
    
    perc.C.diff.aov <- aov(perc.C.diff ~ Treat.diff * Date.diff, data = CN.diff)
    summary(perc.C.diff.aov)
##### Percent C
##### 2-week 
    anova(lm(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2018-11-12"))
    
########################################
# Difference in percent C between the top and sediment after 2 weeks
    
    Analysis of Variance Table

    Response: perc.C.diff
                Df  Sum Sq Mean Sq F value Pr(>F)
    Treat.diff  3  4.4606  1.4869  0.9818 0.4338
    Residuals  12 18.1725  1.5144  
    
<<<<<<< HEAD
    anova(lm(perc_C ~ Nutrients * Glucose * Date, data = CN))
    
    anova(lm(perc_N ~ Nutrients * Glucose * Date, data = CN))
    anova(lm(perc_N ~ Nutrients * Date, data = CN))
    tapply(perc_N, Nutrients, 
=======
########################################
##### 14-week 
>>>>>>> fe5a92e4e836f7209f4c948322b16e8113a41441
    
    anova(lm(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2019-02-07"))

########################################
# Difference in percent C between the top and sediment after 14 weeks
    
    Analysis of Variance Table

    Response: perc.C.diff
               Df  Sum Sq Mean Sq F value Pr(>F)
    Treat.diff  3   0.798  0.2659  0.0102 0.9985
    Residuals  12 312.201 26.0168 
    
########################################    
# Plots for SFS
## Differences
   # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc.N.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2018-11-12", xlab = " ", ylab = "Percent N Difference", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-1.5, 1.50), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, lwd = 3)
    text(2, 1.50, "No Sediment Contact is Greater")
    text(2, -1.50, "Sediment Contact is Greater")
    box()
    dev.copy(jpeg, "./output/plots/percN_diff_treat_wk2.jpg")
    dev.off()
    
   # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc.N.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2019-02-07", xlab = " ", ylab = "Percent N Difference", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-1.5, 1.50), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, lwd = 3)
    text(2, 1.50, "No Sediment Contact is Greater")
    text(2, -1.50, "Sediment Contact is Greater")
    box()
    dev.copy(jpeg, "./output/plots/percN_diff_treat_wk14.jpg")
    dev.off()
    

   # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2018-11-12", xlab = " ", ylab = "Percent C Difference", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-10, 15), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, lwd = 3)
    text(2, 15, "No Sediment Contact is Greater")
    text(2, -10, "Sediment Contact is Greater")
    box()
    dev.copy(jpeg, "./output/plots/percC_diff_treat_wk2.jpg")
    dev.off()
    
   # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2019-02-07", xlab = " ", ylab = "Percent C Difference", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-10, 15), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, lwd = 3)
    text(2, 15, "No Sediment Contact is Greater")
    text(2, -10, "Sediment Contact is Greater")
    box()
    dev.copy(jpeg, "./output/plots/percC_diff_treat_wk14.jpg")
    dev.off()

## Mass
### Perc C

   # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_C ~ Treat, data = CN, subset = Date == "2018-11-12" & Location =="Top", xlab = " ", ylab = "Percent C", cex.lab = 1.5, cex.axis = 1.2, ylim = c(30, 50), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percC_treat_top_wk2.jpg")
    dev.off()
    
   # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_C ~ Treat, data = CN, subset = Date == "2018-11-12" & Location =="Sed", xlab = " ", ylab = "Percent C", cex.lab = 1.5, cex.axis = 1.2, ylim = c(30, 50), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percC_treat_sed_wk2.jpg")
    dev.off()
    
   # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_C ~ Treat, data = CN, subset = Date == "2019-02-07" & Location =="Top", xlab = " ", ylab = "Percent C", cex.lab = 1.5, cex.axis = 1.2, ylim = c(30, 50), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percC_treat_top_wk14.jpg")
    dev.off()
    
   # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_C ~ Treat, data = CN, subset = Date == "2019-02-07" & Location =="Sed", xlab = " ", ylab = "Percent C", cex.lab = 1.5, cex.axis = 1.2, ylim = c(30, 50), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percC_treat_sed_wk14.jpg")
    dev.off()
    
### Perc N

   # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_N ~ Treat, data = CN, subset = Date == "2018-11-12" & Location =="Top", xlab = " ", ylab = "Percent N", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 4), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percN_treat_top_wk2.jpg")
    dev.off()
    
   # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_N ~ Treat, data = CN, subset = Date == "2018-11-12" & Location =="Sed", xlab = " ", ylab = "Percent N", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 4), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percN_treat_sed_wk2.jpg")
    dev.off()
    
   # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_N ~ Treat, data = CN, subset = Date == "2019-02-07" & Location =="Top", xlab = " ", ylab = "Percent N", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 4), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percN_treat_top_wk14.jpg")
    dev.off()
    
   # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(perc_N ~ Treat, data = CN, subset = Date == "2019-02-07" & Location =="Sed", xlab = " ", ylab = "Percent N", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 4), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/percN_treat_sed_wk14.jpg")
    dev.off()
    