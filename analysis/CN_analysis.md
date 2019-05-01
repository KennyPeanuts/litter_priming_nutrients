# Analysis of CN mass for LPNL experiment

## Created:

* 4 March 2019

## Modified

* 2019-April-19 - KF - added analysis of the CN results

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
    
### Test of location Effect
#### 2 Weeks
##### Percent C
    
    t.test(perc.C.diff[CN.diff$Date == "2018-11-12"], mu = 0)
    
    #===========================
    
    One Sample t-test
    
    data:  perc.C.diff[CN.diff$Date == "2018-11-12"]
    t = 2.8921, df = 15, p-value = 0.01117
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      0.2335779 1.5426721
    sample estimates:
      mean of x 
    0.888125  
    
    #===========================
    
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
    
    t.test(perc.C.diff[CN.diff$Date == "2019-02-07"], mu = 0)
    
    #===========================
    
    One Sample t-test
    
    data:  perc.C.diff[CN.diff$Date == "2019-02-07"]
    t = 2.8793, df = 15, p-value = 0.01147
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      0.8540116 5.7222384
    sample estimates:
      mean of x 
    3.288125 
    
    #===========================
    
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
    
#### Analysis of Treatment Effect
    
    perc.C.diff.aov <- aov(perc.C.diff ~ Treat.diff * Date.diff, data = CN.diff)
    summary(perc.C.diff.aov)
    
    plot(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2018-11-12", ylim = c(-5, 15))
    abline(h = 0)
    
    plot(perc.C.diff ~ Treat.diff, data = CN.diff, subset = Date.diff == "2019-02-07", ylim = c(-5, 15))
    abline(h = 0)
    
    plot(perc_C ~ Treat, data = CN, subset = Location == "Sed" & Date == "2018-11-12", ylim = c(40, 50)) 
    plot(perc_C ~ Treat, data = CN, subset = Location == "Top" & Date == "2018-11-12", add = T, col = 8) 
    
    plot(perc_C ~ Treat, data = CN, subset = Location == "Sed" & Date == "2019-02-07", ylim = c(20, 50)) 
    plot(perc_C ~ Treat, data = CN, subset = Location == "Top" & Date == "2019-02-07", add = T, col = 8) 
    
    
    plot(perc_N ~ Treat, data = CN, subset = Location == "Sed" & Date == "2018-11-12", ylim = c(0, 5)) 
    plot(perc_N ~ Treat, data = CN, subset = Location == "Top" & Date == "2018-11-12", add = T, col = 8) 
   
    par(las = 1, mfcol = c(1, 2)) 
    plot(perc_C ~ Treat, data = CN, subset = Date == "2018-11-12", ylim = c(0, 50)) 
    plot(perc_C ~ Treat, data = CN, Date == "2019-02-07", ylim = c(0, 50), col = 8) 
    
    par(las = 1, mfcol = c(1, 2)) 
    plot(perc_N ~ Treat, data = CN, subset = Date == "2018-11-12", ylim = c(0, 5)) 
    plot(perc_N ~ Treat, data = CN, Date == "2019-02-07", ylim = c(0, 5), col = 8) 
    
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Sed" & Date == "2018-11-12", main = "Sed", ylim = c(20, 100))
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Top" & Date == "2018-11-12", main = "Top", ylim = c(20, 100))
    
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Sed" & Date == "2019-02-07", main = "Sed", ylim =c(20, 100))
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Top" & Date == "2019-02-07", main = "Top", ylim = c(20, 100))
    
    par(las = 1, mfcol = c(1, 2)) 
    plot(CN_mol ~ Nutrients, data = CN, subset = Location == "Top", ylim = c(0, 100), main = "Top")
    plot(CN_mol ~ Nutrients, data = CN, subset = Location == "Sed", ylim = c(0, 100), main = "Sed")
   
    par(las = 1, mfcol = c(2, 2)) 
    plot(perc_N ~ Nutrients, data = CN, subset = Location == "Top" & Date == "2018-11-12", ylim = c(0, 5), main = "Top")
    plot(perc_N ~ Nutrients, data = CN, subset = Location == "Sed" & Date == "2018-11-12", ylim = c(0, 5), main = "Sed")
    plot(perc_N ~ Nutrients, data = CN, subset = Location == "Top" & Date == "2019-02-07", ylim = c(0, 5), main = "Top")
    plot(perc_N ~ Nutrients, data = CN, subset = Location == "Sed" & Date == "2019-02-07", ylim = c(0, 5), main = "Sed")
    
    
    
    anova(lm(perc_C ~ Nutrients * Glucose * Date, data = CN))
    anova(lm(perc_N ~ Nutrients * Glucose * Date, data = CN))
    
    
    
# Plots for SFS
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
    