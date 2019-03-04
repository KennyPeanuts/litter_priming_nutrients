# Analysis of CN mass for LPNL experiment

## Created:

* 4 March 2019

## Authors

* KF
* AO

## Description

## Analysis

### Import Data

    CN2 <- read.table("./data/CN_week2.csv", header = T, sep = ",") # for the 2 week incubation
    CN14 <- read.table("./data/CN_week14.csv", header = T, sep = ",") # for the 2 week incubation
    
### Calculate Mass of Sample
    
    CN2_samp_mass <- CN2$Vial_leaf_mass - CN2$Vial_mass
    CN14_samp_mass <- CN14$Vial_Leaf_Mass - CN14$Vial_mass

    stem(CN2_samp_mass)    
    stem(CN14_samp_mass)    
    