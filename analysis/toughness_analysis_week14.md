# Analysis of leaf toughness for LPNL experiment 14 Weeks

## Created:

* 14 Feb 2019

## Authors

* KF
* AO
* GH

## Description

## Analysis

### Import Data

    tough14 <- read.table("./data/toughness_week14.csv", header = T, sep = ",") # for the 14 week incubation
    
## For 14 Week Incubation
    
### Calculate total mass to puncture leaf
    
    tot.mass14 <- tough14$Water_Mass + tough14$shelf_mass + tough14$res_mass

### 3 - way ANOVA
    
    anova(lm(tot.mass14 ~ location * glucose * nutrients, data = tough14))

############
    Analysis of Variance Table

    Response: tot.mass14
    Df Sum Sq Mean Sq F value  Pr(>F)    
    location                    1  28798 28797.6 28.1168 8.4e-07 ***
    glucose                     1   1774  1774.2  1.7322 0.19154    
    nutrients                   1   6316  6316.4  6.1671 0.01491 *  
    location:glucose            1   2677  2676.5  2.6133 0.10955    
    location:nutrients          1   2082  2082.3  2.0330 0.15745    
    glucose:nutrients           1    881   881.5  0.8606 0.35610    
    location:glucose:nutrients  1   2244  2243.6  2.1906 0.14243    
    Residuals                  88  90131  1024.2                
############
    
### Plot of Location Effect

    plot(tot.mass14 ~ location, data = tough14, ylim = c(0, 200))

### Plot of Nutrient Effect

    plot(tot.mass14 ~ nutrients, data = tough14, subset = location == "top", ylim = c(0, 200), main = "Top 14-week")
    plot(tot.mass14 ~ nutrients, data = tough14, subset = location == "sed", ylim = c(0, 200), main = "Sediment 14-week")
    
    
### Plot of Glucose Effect

    plot(tot.mass14 ~ glucose, data = tough14, subset = location == "top", main = "Top 14-week")
    plot(tot.mass14 ~ glucose, data = tough14, subset = location == "sed", main = "Sediment 14-week")


### 2 - way ANOVA
    
    anova(lm(tot.mass14 ~ location * nutrients, data = tough14))

#################

    Analysis of Variance Table

    Response: tot.mass14
                   Df Sum Sq Mean Sq F value    Pr(>F)    
    location            1  28798 28797.6 27.1156 1.163e-06 ***
    nutrients           1   6316  6316.4  5.9475   0.01666 *  
    location:nutrients  1   2082  2082.3  1.9607   0.16481    
    Residuals          92  97707  1062.0                    

###################
  
### 1-way ANOVAs
  
    anova(lm(tot.mass14 ~ nutrients, data = tough14, subset = location == "top"))

##################
    1-way ANOVA for top leaves by nutrients

    Analysis of Variance Table

    Response: tot.mass14
    Df Sum Sq Mean Sq F value  Pr(>F)  
    nutrients  1   7826    7826  5.1084 0.02859 
    Residuals 46  70471    1532     

###################

    anova(lm(tot.mass14 ~ nutrients, data = tough14, subset = location == "sed"))
  
###################
    1-way ANOVA for sed leaves by nutrients
    Analysis of Variance Table
    
    Response: tot.mass14
    Df  Sum Sq Mean Sq F value Pr(>F)
    nutrients  1   572.7  572.70  0.9673 0.3305
    Residuals 46 27235.9  592.09   

###################
    