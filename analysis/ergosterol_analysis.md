# Analysis of Ergosterol 

## Created:

* 2020-02-05

## Modified


## Authors

* KF
* AO

## Description

## Analysis

### Import Data

    erg <- read.table("./data/ergosterol.csv", header = T, sep = ",")

### Normalize to leaf number
    
    ErgLeaf <- erg$Erg / erg$LeafNum
    
### Create Location Variable
    
    Location <- c(rep(c(rep("Sed", 4), rep("Top", 4)), 8))
    
### Add Created Variables to erg data.frame
    
    erg <- data.frame(erg, ErgLeaf, Location)

### Create Difference Variable 
    
    erg.diff.2 <- erg$ErgLeaf[erg$HarvestDate == "11/12/18" & erg$Location == "Top"] - erg$ErgLeaf[erg$HarvestDate == "11/12/18" & erg$Location == "Sed"] 
    
    erg.diff.14 <- erg$ErgLeaf[erg$HarvestDate == "2/7/19" & erg$Location == "Top"] - erg$ErgLeaf[erg$HarvestDate == "2/7/19" & erg$Location == "Sed"]
    
### Create Lables For Difference Variables
    
    treat.diff <- c(rep("NGNN", 4), rep("NGYN", 4), rep("YGNN", 4), rep("YGYN", 4))
    
### Make Difference Data Frame 
    
    erg.diff <- data.frame(treat.diff, erg.diff.2, erg.diff.14)
    
## Treatment Effect Analysis
    
    plot(erg.diff.2 ~ treat.diff, data= erg.diff)
    abline(h = 0)

    plot(erg.diff.14 ~ treat.diff, data= erg.diff)    
    abline(h = 0)
    
    
### Test for Location Effect
    
    t.test(erg.diff$erg.diff.2, mu=0)

################################       
    > t.test(erg.diff$erg.diff.2, mu=0)
    
    One Sample t-test
    
    data:  erg.diff$erg.diff.2
    t = 2.4851, df = 14, p-value = 0.02621
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      0.02531584 0.34443083
    sample estimates:
      mean of x 
    0.1848733 
    
    t.test(erg.diff$erg.diff.14, mu=0)
    
    > t.test(erg.diff$erg.diff.14, mu=0)
    
    One Sample t-test
    
    data:  erg.diff$erg.diff.14
    t = 3.7838, df = 15, p-value = 0.001802
    alternative hypothesis: true mean is not equal to 0
    95 percent confidence interval:
      0.3663114 1.3113574
    sample estimates:
      mean of x 
    0.8388344 
###############################    

    anova(lm(erg.diff.2~ treat.diff, data= erg.diff))

################################    

    anova(lm(erg.diff.2~ treat.diff, data= erg.diff))
    Analysis of Variance Table
    
    Response: erg.diff.2
    Df  Sum Sq  Mean Sq F value Pr(>F)
    treat.diff  3 0.09435 0.031450   0.324 0.8081
    Residuals  11 1.06786 0.097079
    
##########################
    
    anova(lm(erg.diff.14~ treat.diff, data= erg.diff))
    
    anova(lm(erg.diff.14~ treat.diff, data= erg.diff))
    Analysis of Variance Table
    
    Response: erg.diff.14
    Df  Sum Sq Mean Sq F value Pr(>F)
    treat.diff  3  1.3579 0.45262  0.5204 0.6763
    Residuals  12 10.4374 0.86978 
    
#######################################
  
## Summary Statistics

### Ergosterol by Location
    
    tapply(erg$ErgLeaf[erg$HarvestDate == "11/12/18"], erg$Location[erg$HarvestDate == "11/12/18"], summary)
    tapply(erg$ErgLeaf[erg$HarvestDate == "11/12/18"], erg$Location[erg$HarvestDate == "11/12/18"], sd, na.rm = T)
    

###################################
# 2 weeks
    
$Sed
Min.     1st Qu.  Median    Mean     3rd Qu.  Max.    SD
0.02205  0.07279  0.09095   0.11460  0.15676  0.25760 0.06641734
    
$Top
Min.    1st Qu.  Median   Mean    3rd Qu.   Max.    SD          NAs 
0.0853  0.1003   0.1648   0.2930  0.3795    1.3065  0.32386367     1 

###################################

tapply(erg$ErgLeaf[erg$HarvestDate == "2/7/19"], erg$Location[erg$HarvestDate == "2/7/19"], summary)
tapply(erg$ErgLeaf[erg$HarvestDate == "2/7/19"], erg$Location[erg$HarvestDate == "2/7/19"], sd, na.rm = T)

####################################

$Sed
Min.     1st Qu.  Median    Mean     3rd Qu.    Max.    SD
0.06115  0.14814  0.38545   0.63532  0.85955    2.23295 0.6810563

$Top
Min.    1st Qu.  Median    Mean     3rd Qu.    Max.     SD
0.1039  0.6278   1.5230    1.4742   2.0782     4.0646   1.0420158 

########################################

### Summary for Ergosterol Difference
#### Week 2

    summary(erg.diff$erg.diff.2)
    sd(erg.diff$erg.diff.2, na.rm = T)

#################    
# Week 2
    
Min.      1st Qu.   Median     Mean     3rd Qu.     Max.     SD        NAs 
-0.02435  0.02405   0.10515    0.18487  0.18190     1.10425  0.2881235     1 
    
####################
    
### Week `14
    
    summary(erg.diff$erg.diff.14)
    sd(erg.diff$erg.diff.14, na.rm = T) 
    
###############################
# Week 14
    
Min.     1st Qu.  Median    Mean    3rd Qu.    Max.    SD
-0.5226  0.1668   0.7249    0.8388  1.5106     2.3681  0.8867637
    
###################################
    