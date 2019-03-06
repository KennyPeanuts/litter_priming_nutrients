# Analysis of Leaf LOI LPNL experiment 14 Weeks

## Created:

* 6 March 2019

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

    
    