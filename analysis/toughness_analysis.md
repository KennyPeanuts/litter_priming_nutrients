# Analysis of leaf toughness for LPNL experiment 14 Weeks

## Created:

* 14 Feb 2019

## Modified

* 25 April 2019 - KF - completed the analysis based on the diff between the top and sediment
* 25 April 2019 - KF - began coding figure of toughness difference

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
    nutrients <- rep(c("N", "N", "N", "N", "Y", "Y", "Y", "Y", "N", "N", "N", "N", "Y", "Y", "Y", "Y"), 2)
    glucose <- rep(c(rep("N", 8), rep("Y", 8)), 2)

    mean.tough <- data.frame(location, nutrients, glucose, mean.tot.mass2, mean.tot.mass14)    

### Calculate Difference Between Sed and Top
    
    mean.tot.mass2.diff <- mean.tough$mean.tot.mass2[mean.tough$location == "top"] - mean.tough$mean.tot.mass2[mean.tough$location == "sed"]
    mean.tot.mass14.diff <- mean.tough$mean.tot.mass14[mean.tough$location == "top"] - mean.tough$mean.tot.mass14[mean.tough$location == "sed"]
    
#### Create treatment name variable
    
    treat <- c(rep("NGNN", 4), rep("NNYN", 4), rep("YGNN", 4), rep("YGYN", 4))
    
#### Create data.frame
    
    diff.mean.tough <- data.frame(treat, mean.tot.mass2.diff, mean.tot.mass14.diff)
    
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
    
    NGNN     NNYN     YGNN     YGYN 
    47.99450 43.21679 16.23251 18.16559 
    
    #===============================================

##########################
    
## Plot of Treatment Effect on Difference between TOP and BOTTOM
    par(las = 1, mfcol = c(2, 1), cex = 1, lwd = 2)
    par(mar = c(2, 10, 4, 10))
    plot(mean.tot.mass2.diff ~ treat, data = diff.mean.tough, xlab = " ", ylab = "Toughness Difference", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-50, 150))
    abline(h = 0)
    par(mar = c(4, 10, 2, 10))
    plot(mean.tot.mass14.diff ~ treat, data = diff.mean.tough, xlab = " ", ylab = "Toughness Difference", cex.lab = 1.5, cex.axis = 1.2, ylim = c(-50, 150))
    abline(h = 0)
    
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
    
    