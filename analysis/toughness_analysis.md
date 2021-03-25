# Analysis of leaf toughness for LPNL experiment 14 Weeks

## Created:

* 14 Feb 2019

## Modified

* 25 April 2019 - KF - completed the analysis based on the diff between the top and sediment
* 25 April 2019 - KF - began coding figure of toughness difference
* 26 Feb 2020 - KF - summary stats for treatment levels 
* 11 March 2021 - KF - calculated the % difference in the mass required to puncture the leaves.

## Authors

* KF
* AO
* GH

## Description

## Analysis

### Import Data

    tough2 <- read.table("./data/toughness_week2.csv", header = T, sep = ",") # for the 2 week incubation
    tough14 <- read.table("./data/toughness_week14.csv", header = T, sep = ",") # for the 14 week incubation
    
### Calculate total mass to puncture leaf
    
    tot.mass2 <- tough2$water_mass + tough2$shelf_mass + tough2$res_mass
    tot.mass14 <- tough14$Water_Mass + tough14$shelf_mass + tough14$res_mass

### Calculate the mean mass required to puncture the leaf 
    
Three replicate leaves were randomly selected from the top and sediments from each jar for use the the penetrometer.  Thus we need to calculate the average for each bottle.

    mean.tot.mass2.top <- as.numeric(tapply(tot.mass2[tough2$location == "top"], tough2$bottle[tough2$location == "top"], mean))
    mean.tot.mass2.sed <- as.numeric(tapply(tot.mass2[tough2$location == "sed"], tough2$bottle[tough2$location == "sed"], mean))
    mean.tot.mass14.top <- as.numeric(tapply(tot.mass14[tough14$location == "top"], tough14$bottle[tough14$location == "top"], mean))
    mean.tot.mass14.sed <- as.numeric(tapply(tot.mass14[tough14$location == "sed"], tough14$bottle[tough14$location == "sed"], mean))
    
#### Create Data.frame of mean values
    
    mean.tot.mass2 <- c(mean.tot.mass2.top, mean.tot.mass2.sed)
    mean.tot.mass14 <- c(mean.tot.mass14.top, mean.tot.mass14.sed)
    location <- c(rep("top", 16), rep("sed", 16))
    treat <- rep(c(rep("NGNN", 4), rep("NGYN", 4), rep("YGNN", 4), rep("YGYN", 4)), 2)
    nutrients <- rep(c("N", "N", "N", "N", "Y", "Y", "Y", "Y", "N", "N", "N", "N", "Y", "Y", "Y", "Y"), 2)
    glucose <- rep(c(rep("N", 8), rep("Y", 8)), 2)

    mean.tough <- data.frame(location, treat, nutrients, glucose, mean.tot.mass2, mean.tot.mass14)    

### Calculate Difference Between Sed and Top
    
    mean.tot.mass2.diff <- mean.tough$mean.tot.mass2[mean.tough$location == "top"] - mean.tough$mean.tot.mass2[mean.tough$location == "sed"]
    mean.tot.mass14.diff <- mean.tough$mean.tot.mass14[mean.tough$location == "top"] - mean.tough$mean.tot.mass14[mean.tough$location == "sed"]
    
    mean.tot.mass2.percDiff <- ((mean.tough$mean.tot.mass2[mean.tough$location == "top"] - mean.tough$mean.tot.mass2[mean.tough$location == "sed"]) / mean.tough$mean.tot.mass2[mean.tough$location == "top"]) * 100
    
    mean.tot.mass14.percDiff <- ((mean.tough$mean.tot.mass14[mean.tough$location == "top"] - mean.tough$mean.tot.mass14[mean.tough$location == "sed"]) / mean.tough$mean.tot.mass14[mean.tough$location == "top"]) * 100
    
#### Create treatment name variable
    
    treat <- c(rep("NGNN", 4), rep("NGYN", 4), rep("YGNN", 4), rep("YGYN", 4))
    gluc <- c(rep("N", 8), rep("Y", 8))
    nut <- c(rep("N", 4), rep("Y", 4), rep("N", 4), rep("Y", 4))
    
#### Create data.frame
    
    diff.mean.tough <- data.frame(treat, gluc, nut, mean.tot.mass2.diff, mean.tot.mass14.diff, mean.tot.mass2.percDiff, mean.tot.mass14.percDiff)

### Summary Stats for difference
    
    summary(mean.tot.mass2.diff)
    sd(mean.tot.mass2.diff, na.rm = T)

    summary(mean.tot.mass14.diff)
    sd(mean.tot.mass14.diff, na.rm = T)
    
    summary(mean.tot.mass2.percDiff)
    sd(mean.tot.mass2.percDiff, na.rm = T)

    summary(mean.tot.mass14.percDiff)
    sd(mean.tot.mass14.percDiff, na.rm = T)
    
    ###########################
    # Mass difference between top - sed
    
    #> summary(mean.tot.mass2.diff)
    Min.      1st Qu.  Median    Mean    3rd Qu.    Max.     NAs  SD 
    -31.267   4.833    20.667    34.342  56.733     130.133  1    42.82169
    
    #> summary(mean.tot.mass14.diff)
    Min.      1st Qu.  Median    Mean    3rd Qu.    Max.   SD
    -19.833   5.008    37.467    34.640  64.858     94.700 35.55487 
    
    ##############################
    
    #############################
    # Percent difference in mass for top - sed / top
    
    ## 2 weeks
    Min.     1st Qu.  Median    Mean    3rd Qu.    Max.    NAs   SD
   -23.807   2.844    12.957    18.766  32.443     63.274   1     23.82531
    
    ## 14 weeks
    Min.    1st Qu.  Median    Mean    3rd Qu.    Max.   SD
   -53.99   15.17    58.71     35.86   62.40     73.54   38.555
    
    ##############################
   
### test for location effect
    
    t.test(diff.mean.tough$mean.tot.mass2, mu = 0)
    t.test(diff.mean.tough$mean.tot.mass14, mu = 0)
    
    #=============================================
    
    Week 2 One Sample t-test
    
    data:  diff.mean.tough$mean.tot.mass2
    t = 3.1061, df = 14, p-value = 0.007739
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      10.62836 58.05608
    sample estimates:
      mean of x 
    34.34222 

#===============================================
    
#================================================
    
    14 Week One Sample t-test
    
    data:  diff.mean.tough$mean.tot.mass14
    t = 3.897, df = 15, p-value = 0.00143
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      15.69373 53.58544
    sample estimates:
      mean of x 
    34.63958 
    
#===============================================
    
## Plot of toughness difference by week
    
    boxplot(mean.tot.mass2.diff, mean.tot.mass14.diff, ylab = "Toughness Difference", axes = F, col = 8)
    axis(2)
    axis(1, c("2-weeks", "14-weeks"), at = c(1, 2))
    abline(h = 0)
    box()
    
### ANOVA by treatment
    
    summary(aov(mean.tot.mass2.diff ~ treat, data = diff.mean.tough))
    
    #===============================================
    
    Week 2
    
    Df Sum Sq Mean Sq F value Pr(>F)
    treat        3   3414    1138   0.562  0.651
    Residuals   11  22257    2023               
    1 observation deleted due to missingness
    
    #==============================================
    
    summary(aov(mean.tot.mass14.diff ~ treat, data = diff.mean.tough))
    
    #===============================================
    
    Week 14
    
    Df Sum Sq Mean Sq F value Pr(>F)
    treat        3   4668    1556   1.306  0.318
    Residuals   12  14294    1191     
    
    #==============================================
    
### Look for Homogenity of Variance
    
    tapply(diff.mean.tough$mean.tot.mass14.diff, diff.mean.tough$treat, sd)
    
    #===============================================
    
    # Standard Deviation of the Difference of the treatments
    
    NGNN     NGYN     YGNN     YGYN 
    47.99450 43.21679 16.23251 18.16559 
    
    #===============================================

##########################
    
## Plot of Treatment Effect on Difference between TOP and BOTTOM
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(mean.tot.mass2.diff ~ treat, data = diff.mean.tough, xlab = " ", ylab = "Toughness Difference (g)", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-50, 150), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, lwd = 3)
    text(2, 150, "No Sediment Contact is Greater")
    text(2, -50, "Sediment Contact is Greater")
    box()
    dev.copy(jpeg, "./output/plots/toughness_diff_treat_week2.jpg")
    dev.off()
    
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(mean.tot.mass14.diff ~ treat, data = diff.mean.tough, xlab = " ", ylab = "Toughness Difference (g)", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-50, 150), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, lwd = 3)
    text(2, 150, "No Sediment Contact is Greater")
    text(2, -50, "Sediment Contact is Greater")
    box()
    dev.copy(jpeg, "./output/plots/toughness_diff_treat_week14.jpg")
    dev.off()
    
## Test of Glucose and Nutrient Additions
#### 2 weeks    
    tough.diff.mod2 <- lm(mean.tot.mass2.diff ~ gluc * nut, data = diff.mean.tough)
    anova(tough.diff.mod2)
    
##########################################
#2 - way ANOVA results for 2 weeks
    
    Analysis of Variance Table
    
    Response: mean.tot.mass2.diff
    Df  Sum Sq Mean Sq F value Pr(>F)
    gluc       1    29.6   29.56  0.0146 0.9060
    nut        1  2765.2 2765.23  1.3666 0.2671
    gluc:nut   1   619.5  619.47  0.3062 0.5911
    Residuals 11 22257.5 2023.41   
    
###########################################

#### 14 weeks    
    tough.diff.mod14 <- lm(mean.tot.mass14.diff ~ gluc * nut, data = diff.mean.tough)
    anova(tough.diff.mod14)
    
##########################################
#2 - way ANOVA results for 14 weeks

    Analysis of Variance Table
    
    Response: mean.tot.mass14.diff
    Df  Sum Sq Mean Sq F value Pr(>F)
    gluc       1  1784.4  1784.4  1.4980 0.2445
    nut        1  1388.2  1388.2  1.1654 0.3016
    gluc:nut   1  1495.8  1495.8  1.2557 0.2844
    Residuals 12 14293.9  1191.2       

##########################################
#### Plot of location effect
  
    par(las = 1, mfcol = c(2, 1))
    par(mar = c(1, 10, 4, 10))
    plot(mean.tot.mass2 ~ location, data = mean.tough, ylim = c(0, 250), axes = F, ylab = "Grams of Water", xlab = "", col = 8 )
    axis(2)
    box()
    text(1, mean(mean.tough$mean.tot.mass2[mean.tough$location == "sed"]), "*", cex = 2)
    text(2, mean(mean.tough$mean.tot.mass2[mean.tough$location == "top"], na.rm = T), "*", cex = 2)
    par(mar = c(4, 10, 1, 10))
    plot(mean.tot.mass14 ~ location, data = mean.tough, ylim = c(0, 250), axes = F, ylab = "Grams of Water", xlab = "", col = 8)
    axis(2)
    axis(1, c("Sediment", "No Sediment"), at = c(1, 2))
    box()
    text(1, mean(mean.tough$mean.tot.mass14[mean.tough$location == "sed"]), "*", cex = 2)
    text(2, mean(mean.tough$mean.tot.mass14[mean.tough$location == "top"]), "*", cex = 2)
    dev.copy(jpeg, "./output/plots/toughness.jpg")
    dev.off()
    
![Boxplot of toughness](../output/plots/toughness.jpg)

## Plot of Toughness Difference by Treatment (Figure 1)
    
    par(las = 1, cex = 1, lwd = 2)
    par(mfcol = c(2, 2))
    # two weeks by treatment
    par(mar = c(1, 10, 3, 10))
    plot(mean.tot.mass2.percDiff ~ treat, data = diff.mean.tough, xlab = " ", ylab = " ", cex.lab = 0.5, cex.axis = 1.2, ylim = c(-50, 100), col = 8, axes = F, cex.lab = 0.5)
    axis(2)
    #axis(1)# c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, cex = 2)
    text(1, 100, "Two-weeks")
    box()
    #dev.copy(jpeg, "./output/plots/mean_tough_top_treat_wk2.jpg")
    #dev.off()
    # fourteen weeks by treatment
    par(mar = c(3, 10, 1, 10))
    plot(mean.tot.mass14.percDiff ~ treat, data = diff.mean.tough, xlab = " ", ylab = " ", cex.lab = 0.5, cex.axis = 1.2, ylim = c(-50, 100), col = 8, axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    abline(h = 0, cex = 2)
    text(1, 100, "Fourteen-weeks")
    box()
    # two weeks mean 
    par(mar = c(3, 10, 1, 10))
    boxplot(mean.tot.mass2.percDiff, data = diff.mean.tough, xlab = " ", ylab = " ", cex.lab = 0.5, cex.axis = 1.2, ylim = c(-50, 100), col = 8, axes = F, cex.lab = 0.5)
    axis(2)
    #axis(1)
    abline(h = 0, cex = 2)
    text(1, 100, "two-weeks")
    box()
    # fourteen weeks mean
    par(mar = c(3, 10, 1, 10))
    boxplot(mean.tot.mass14.percDiff, data = diff.mean.tough, xlab = " ", ylab = " ", cex.lab = 0.5, cex.axis = 1.2, ylim = c(-50, 100), col = 8, axes = F, cex.lab = 0.5)
    axis(2)
    #axis(1)
    abline(h = 0, cex = 2)
    text(1, 100, "two-weeks")
    box()
    
###########################################################################    
    # Week 2 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(mean.tot.mass2 ~ treat, data = mean.tough, subset = location == "sed", xlab = " ", ylab = "Toughness (g of water)", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 250), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/mean_tough_sed_wk2.jpg")
    dev.off()
    
    # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(mean.tot.mass14 ~ treat, data = mean.tough, subset = location == "top", xlab = " ", ylab = "Toughness (g of water)", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 250), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F, cex.lab = 0.5)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F)
    box()
    dev.copy(jpeg, "./output/plots/mean_tough_top_treat_wk14.jpg")
    dev.off()
    
    # Week 14 
    par(las = 1, cex = 1, lwd = 2)
    #par(mfcol = c(2, 1))
    par(mar = c(5, 5, 5, 5))
    plot(mean.tot.mass14 ~ treat, data = mean.tough, subset = location == "sed", xlab = " ", ylab = "Toughness (g of water)", cex.lab = 1.5, cex.axis = 1.2, ylim = c(0, 250), col = c(0, "gold1", "lightskyblue2", "olivedrab3"), axes = F)
    axis(2)
    axis(1, c("No Addition", "+N +P", "+Glucose", "+Glucose\n +N + P"), at = c(1, 2, 3, 4), tick = F, cex.axis = 1)
    box()
    dev.copy(jpeg, "./output/plots/mean_tough_sed_wk14.jpg")
    dev.off()
    