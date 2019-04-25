# Analysis of Leaf LOI LPNL experiment 14 Weeks

## Created:

* 6 March 2019

## Modified

* 25 April 2019 - KF - Completed analysis of LOI based on difference

## Authors

* KF
* AO
* GH

## Description

## Analysis

### Import Data

    LOI14 <- read.table("./data/leaf_LOI_week14.csv", header = T, sep = ",") # for the 14 week incubation
    
## For 14 Week Incubation
    
### Calculated Variables
    
    leaf_mass <- LOI14$Cruc_leaf_mass - LOI14$Cruc_mass
    ash_mass <- LOI14$Cruc_ash_mass - LOI14$Cruc_mass    

#### Convert negative numbers in ash mass to 0

    ash_mass[ash_mass <= 0] <- 0
    
#### Convert negative leaf_mass to NA
    
    leaf_mass[leaf_mass <= 0] <- NA

    AFDM_samp <- leaf_mass - ash_mass
    
#### Estimate the AFDM of a single leaf disc
    
    AFDM_disc <- AFDM_samp / LOI14$Leaf_number

### Initial Leaf Mass of a single leaf disc from sediment_priming exp
    
    Initial_leaf_mass <- 0.00354
    
### Calculate Mass Lost from a Single Leaf Disc
    
    mass_loss_disc <- Initial_leaf_mass - AFDM_disc

## Analysis
    
### Calc the differenece between the TOP and SED samples
    
    mass.loss.disc.diff <- mass_loss_disc[LOI14$Location == "Top"] - mass_loss_disc[LOI14$Location == "Sed"]
    
    t.test(mass.loss.disc.diff, mu = 0)
    
    #===========================================
    
    One Sample t-test
    
    data:  mass.loss.disc.diff
    t = -0.40672, df = 13, p-value = 0.6908
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      -0.0003982413  0.0002720508
    sample estimates:
      mean of x 
    -6.309524e-05 
    
    #==========================================

## Analyize Treatment effect on Mass Loss
    
Since there was no effect of location, I analyzed the effect of treatment on mass loss with the locations pooled
  
    anova(lm(AFDM_disc ~ Treat, data = LOI14))
    
    #========================================
    
    Analysis of Variance Table
    
    Response: AFDM_disc
    Df     Sum Sq    Mean Sq F value Pr(>F)
    Treat      3 2.0766e-06 6.9220e-07  2.2025 0.1118
    Residuals 26 8.1713e-06 3.1428e-07  

    #========================================    
    
    par(las = 1, cex = 1, lwd = 2, mar = c(6, 6, 6, 6))
    plot(mass_loss_disc * 1000 ~ Location, data = LOI14, ylab = "Change in Mass (mg)", cex.lab = 1.5, cex.axis = 1.2, col = c("light green", "light blue"))
    dev.copy(jpeg, "./output/plots/mass_loss_location.jpg")
    dev.off()
    
![Mass Loss by Location](../output/plots/mass_loss_location.jpg)
    