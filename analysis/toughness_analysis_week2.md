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
    
    tot.mass2 <- tough2$water_mass + tough2$shelf_mass + tough2$res_mass

### 3 - way ANOVA
    
    anova(lm(tot.mass2 ~ location * glucose * nutrients, data = tough2))

#####################
     ANOVA of the effect of Location * Glucose * Nutrients on the toughness of the leaves - measured as the mass required to puncture them
    
    Analysis of Variance Table
    
    Response: tot.mass2
    Df Sum Sq Mean Sq F value   Pr(>F)   
    location                    1  22297 22297.1  7.8658 0.006213 **
    glucose                     1     48    48.1  0.0170 0.896668   
    nutrients                   1  11549 11549.3  4.0743 0.046621 * 
    location:glucose            1    114   113.7  0.0401 0.841700   
    location:nutrients          1   6419  6418.9  2.2644 0.135998   
    glucose:nutrients           1    515   514.7  0.1816 0.671068   
    location:glucose:nutrients  1    253   252.9  0.0892 0.765886   
    Residuals                  87 246619  2834.7     
    
##################

### 2 - way anova of location and nutrient effects
    
    anova(lm(tot.mass2 ~ location * nutrients, data = tough2))
  
##################
    Analysis of Variance Table
    
    Response: tot.mass2
    Df Sum Sq Mean Sq F value  Pr(>F)   
    location            1  22297 22297.1  8.1960 0.00521 **
    nutrients           1  11564 11564.0  4.2507 0.04209 * 
    location:nutrients  1   6389  6388.6  2.3483 0.12889   
    Residuals          91 247564  2720.5   
    
##################
### 1 - way anovas of nutrient effects
    
    anova(lm(tot.mass2 ~ nutrients, data = tough2, subset = location == "top"))
    
##################
    Analysis of Variance Table
    
    Response: tot.mass2
    Df Sum Sq Mean Sq F value Pr(>F)
    nutrients  1    354  353.76  0.2017 0.6555
    Residuals 45  78929 1753.99        
    
##################
    
    anova(lm(tot.mass2 ~ nutrients, data = tough2, subset = location == "sed"))

##################
    Analysis of Variance Table
    
    Response: tot.mass2
    Df Sum Sq Mean Sq F value  Pr(>F)  
    nutrients  1  17599   17599  4.8006 0.03355 *
    Residuals 46 168635    3666 
    
##################
    
    plot(tot.mass2 ~ nutrients, data = tough2, subset = location == "top", ylim = c(0,300), main = "TOP 2-week")          
    plot(tot.mass2 ~ nutrients, data = tough2, subset = location == "sed", ylim = c(0, 300), main = "SED 2-week")          
    