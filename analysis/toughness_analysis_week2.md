# Analysis of leaf toughness for LPNL experiment

## Created:

* 16 Nov 2018

## Authors

* KF
* AO

## Description

## Analysis

### Import Data

    tough2 <- read.table("./data/toughness_week2.csv", header = T, sep = ",") # for the 2 week incubation
    
## For 2 Week Incubation
    
### Calculate total mass to puncture leaf
    
    tot.mass <- tough2$water_mass + tough2$shelf_mass + tough2$res_mass

### 3 - way ANOVA
    
    anova(lm(tot.mass ~ location * glucose * nutrients, data = tough2))

~~~~
ANOVA of the effect of Location * Glucose * Nutrients on the toughness of the leaves - measured as the mass required to puncture them
    
    Analysis of Variance Table
    
    Response: tot.mass
    Df Sum Sq Mean Sq F value   Pr(>F)   
    location                    1  22297 22297.1  7.8658 0.006213 **
    glucose                     1     48    48.1  0.0170 0.896668   
    nutrients                   1  11549 11549.3  4.0743 0.046621 * 
    location:glucose            1    114   113.7  0.0401 0.841700   
    location:nutrients          1   6419  6418.9  2.2644 0.135998   
    glucose:nutrients           1    515   514.7  0.1816 0.671068   
    location:glucose:nutrients  1    253   252.9  0.0892 0.765886   
    Residuals                  87 246619  2834.7     
    
~~~~
      