# Analysis of Leaf LOI LPNL experiment 14 Weeks

## Created:

* 6 March 2019

## Modified

* 25 April 2019 - KF - Completed analysis of LOI based on difference
* 18 March 2021 - KF - Added summary stats
* 20 May 2021 - KF - Added 2-way ANOVA to make consistent with other resp var

## Authors

* Kenneth Fortino
* Alyssa Oppedisano
* Gabrielle Huerta

## Description

## Analysis

### Install Packages

    library("tidyverse")

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
    
### Create Data frame with calculated variable
    
    mass_loss_14weeks <- data.frame(LOI14, AFDM_disc, AFDM_samp, ash_mass, leaf_mass, mass_loss_disc)
    
#### Variable Descriptions in mass_loss_14weeks 
    
* Treat = the description of the treatment level combinations, where NG is no glucose addition, YG is glucose addition, NN is no nutrient addition, and YN is N and P addition.

* Location = the description of the location of the leaf discs during incubation, where "Sed" indicates that the leaf discs were incubated in contact with the sediments and "Top" indicates that the leaf discs were incubated on a wire shelf 4 cm above the sediment surface.
    
* Glucose = the identifier if glucose was added to the jar, where Y is yes, and N is no.
    
* Nutrients = the identifier if N and P were added to the jar, where Y is yes, and N is no.
    
* Cruc_number = the number label on the crucuble used in LOI determination.
    
* Cruc_mass = the mass of the crucible used in LOI determination in grams.
    
* Leaf_number = the number of leaf discs that were in the crucuble used for LOI determination.
    
* Jar = the replicate identifier on the jar.
    
* Cruc_leaf_mass = the mass of the crucible and leaf discs before ashing at 550 dC (g).
    
* Cruc_ash_mass = the mass of the crucible and ash after ashing at 550 dC (g).
    
* leaf_mass = the mass of all the leaf discs in the crucible before ashing at 550 dC (g).
    
* ash_mass = the mass of the total amount of ash in the crucibe after ashing at 550 dC (g).
    
* AFDM_samp = the AFDM of the total sample (all the leaves) in the crucible (g).
  
* AFDM_disc = the estimated AFDM of a single leaf determined by dividing AFDM_samp by leaf_number (g).
    
* mass_loss_disc = the estimated change in mass of the leaves over the 14 week incubation based on the initial mass of the leaves sampled from the same leaves used in the incubation (g).

## Summary Statistics
### ADFM of a single leaf disc
    
    tapply(mass_loss_14weeks$AFDM_disc, mass_loss_14weeks$Location, summary, na.rm = T)
    tapply(mass_loss_14weeks$AFDM_disc, mass_loss_14weeks$Location, sd, na.rm = T)
    
    ##################################################
    # AFDM of a single leaf disc at the end of the 14 week incubation in the different locations.
    
    $Sed
    Min.      1st Qu.   Median     Mean       3rd Qu.     Max.        NAs  SD
    0.001700  0.002237  0.002525   0.002572   0.002875    0.003600     1    0.0005288014
    
    $Top
    Min.      1st Qu.   Median     Mean      3rd Qu.     Max.       NAs  SD 
    0.001450  0.002112  0.002525   0.002599  0.002917    0.003867    1   0.0006722819
    
    ##################################################
    
### Estimated AFDM mass loss of a single leaf disc
    
    tapply(mass_loss_14weeks$mass_loss_disc, mass_loss_14weeks$Location, summary, na.rm = T)
    tapply(mass_loss_14weeks$mass_loss_disc, mass_loss_14weeks$Location, sd, na.rm = T)
    
    ##################################################
    # Estimated AFDM mass loss of a single leaf disc at the end of the 14 week incubation in the different locations.
    
    $Sed
    Min.        1st Qu.    Median     Mean       3rd Qu.     Max.        NAs  SD
    -0.0000600  0.0006650  0.0010150  0.0009683  0.0013025  0.0018400    1    0.0005288014
    
    $Top
    Min.        1st Qu.    Median     Mean      3rd Qu.     Max.       NAs  SD 
    -0.0003267  0.0006233  0.0010150  0.0009411  0.0014275  0.0020900  1    0.0006722819 
    
    
    ##################################################
    
## Analysis
    
### Calc the differenece between the TOP and SED samples
    
    mass.loss.disc.diff <- mass_loss_disc[LOI14$Location == "Top"] - mass_loss_disc[LOI14$Location == "Sed"]
    treat.diff <- LOI14$Treat[LOI14$Location == "Top"]
    
### Test the effect of the location effect on mass lost
    
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
  
    summary(aov(mass_loss_disc ~ Glucose * Nutrients, data = mass_loss_14weeks))
    
    ################################################## 
    # 2-way ANOVA of mass loss after 14 weeks
    
    > summary(aov(mass_loss_disc ~ Glucose * Nutrients, data = mass_loss_14weeks))
                       Df Sum Sq   Mean Sq     F value Pr(>F)  
    Glucose            1 2.880e-07 2.884e-07   0.918   0.3469  
    Nutrients          1 4.700e-08 4.650e-08   0.148   0.7035  
    Glucose:Nutrients  1 1.742e-06 1.742e-06   5.542   0.0264 *
    Residuals         26 8.171e-06 3.143e-07                 
    
    ################################################## 
    
## Plots
    
    ggplot(mass_loss_14weeks, mapping = aes(y = mass_loss_disc * 1000, x = factor(Glucose))) +
      geom_jitter(
        col = 8,
        width = 0.1) +
      facet_wrap(
        ~ Nutrients
      ) +
      stat_summary(
        fun = mean,
        fun.min = function(x) mean(x) - sd(x),
        fun.max = function(x) mean(x) + sd(x)
      ) +
      theme_classic()
    
    ggplot(mass_loss_14weeks, mapping = aes(y = mass_loss_disc * 1000, x = factor(Nutrients))) +
      geom_jitter(
        col = 8,
        width = 0.1) +
      facet_wrap(
        ~ Glucose
      ) +
      stat_summary(
        fun = mean,
        fun.min = function(x) mean(x) - sd(x),
        fun.max = function(x) mean(x) + sd(x)
      ) +
      theme_classic()
    
    ggplot(mass_loss_14weeks, mapping = aes(y = mass_loss_disc * 1000, x = factor(Nutrients), color = factor(Glucose))) +
      geom_jitter(
        width = 0.1) +
      stat_summary(
        fun = mean,
        #fun.min = function(x) mean(x) - sd(x),
        #fun.max = function(x) mean(x) + sd(x)
      ) +
      theme_classic()
    