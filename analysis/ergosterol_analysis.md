# Analysis of Ergosterol 

## Created:

* 2020-02-05

## Modified

* 2020-10-21 - KF - cleaned up formatting and added metadata

## Authors

* KF
* AO

## Description

## Analysis

### Import Data

    erg <- read.table("./data/ergosterol.csv", header = T, sep = ",")
    leaf_mass14 <- read.table("./data/leaf_LOI_week14.csv", header = T, sep = ",")

### Normalize to leaf number
The data from the Khuen lab are reported as the total ergosterol (ug) per sample. There were 2 leaf discs in most samples but some only had 1 due to un-recovered leaf discs. The ErgLeaf variable is created to normalize the ergosterol mass to a single leaf disc.
    
    ErgLeaf <- erg$Erg / erg$LeafNum
    
### Create Location Variable
    
    Location <- c(rep(c(rep("Sed", 4), rep("Top", 4)), 8))
    
### Add Created Variables to erg data.frame
    
    erg <- data.frame(erg, ErgLeaf, Location)
    
## Metadata for the erg data frame
This data frame contains the cleaned ergosterol data.

### Variable Descriptions

* Samp = the identifier for the sample 
* Treat =  the designation of the treatment level for the nutrient and/or glucose addition, where "NG" refers to no glucose addition, "YG" refers to the addition of glucose, "NN" refers to no N and P addition, and "YP" refers to the addition of N and P.
* HarvestDate = the date that the leaf discs were sampled from the experiment.
* Erg = the total mass of ergosterol in the sample in ug
* LeafNum = the number of leaves in the sample
* ErgLeaf = the estmated mass of ergosterol per leaf (ug)
* Location = the position of the leaf discs in the microcosm, where "Sed" means the leaves were resting on the sediment surface and "Top" means the leaves were elevated 4 cm above the sediment surface.

## Create Difference Variable 
    
    erg.diff.2 <- erg$ErgLeaf[erg$HarvestDate == "11/12/18" & erg$Location == "Top"] - erg$ErgLeaf[erg$HarvestDate == "11/12/18" & erg$Location == "Sed"] 
    
    erg.diff.14 <- erg$ErgLeaf[erg$HarvestDate == "2/7/19" & erg$Location == "Top"] - erg$ErgLeaf[erg$HarvestDate == "2/7/19" & erg$Location == "Sed"]
    
### Create Labels For Difference Variables
    
    treat.diff <- c(rep("NGNN", 4), rep("NGYN", 4), rep("YGNN", 4), rep("YGYN", 4))
    
### Make Difference Data Frame 
    
    erg.diff <- data.frame(treat.diff, erg.diff.2, erg.diff.14)

## Metadata for erg.diff
This data frame contains the difference between the ergosterol in the treatment levels of the experiment. It is used to run t-tests that the difference is equal to or not equal to 0 to determine the treatment effect.

### Variable Descriptions

* treat.diff is the treatment labels where NGNN = no glucose and no nutrient addition, NGYN = no glucose but nutrient addition, YGNN = yes glucose addition but no nutrient addition, and YGYN = addition of both glucose and nutrient.

* erg.diff.2 is the ergosterol concentration of the leaf discs not in contact with the sediment (TOP) minus the ergosterol concentration of the leaf discs in contact with the sediment (SED) at the 2-week harvesting time. Ergosterol concentration is in ug ergosterol / leaf disc. 

* erg.diff.14 is the ergosterol concentration of the leaf discs not in contact with the sediment (TOP) minus the ergosterol concentration of the leaf discs in contact with the sediment (SED) at the 14-week harvesting time. Ergosterol concentration is in ug ergosterol / leaf disc.

    
## Treatment Effect Analysis
    
    plot(erg.diff.2 ~ treat.diff, data= erg.diff)
    abline(h = 0)

    plot(erg.diff.14 ~ treat.diff, data= erg.diff)    
    abline(h = 0)
    
    
### Test for Location Effect
    
    boxplot(erg.diff$erg.diff.2, erg.diff$erg.diff.14, ylab = c(axes = F)
    abline(h = 0)
    axis(2)
    axis(1, c("2-weeks", "14-weeks"), at = c(1, 2))
    box()
    
    
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

## Test of the effect of the treatments on the difference in the ergosterol 
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
calculate the summary statistics for the ergosterol after 2 weeks of incubation for the sediment or water treatments
    
    tapply(erg$ErgLeaf[erg$HarvestDate == "11/12/18"], erg$Location[erg$HarvestDate == "11/12/18"], summary)
    tapply(erg$ErgLeaf[erg$HarvestDate == "11/12/18"], erg$Location[erg$HarvestDate == "11/12/18"], sd, na.rm = T)
    

###################################
# 2 weeks Ergosterol Mass (ug/leaf disc)
    
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
# 14-weeks Ergosterol Mass (ug/leaf disc)
    
    $Sed
    Min.     1st Qu.  Median    Mean     3rd Qu.    Max.    SD
    0.06115  0.14814  0.38545   0.63532  0.85955    2.23295 0.6810563

    $Top
    Min.    1st Qu.  Median    Mean     3rd Qu.    Max.     SD
    0.1039  0.6278   1.5230    1.4742   2.0782     4.0646   1.0420158 

########################################
    
### Plot of ErgLeaf
    
    plot(ErgLeaf ~ Location, subset = HarvestDate == "11/12/18", data = erg)
    plot(ErgLeaf ~ Location, subset = HarvestDate == "2/7/19", data = erg)

### Summary for Ergosterol Difference
#### Week 2

    summary(erg.diff$erg.diff.2)
    sd(erg.diff$erg.diff.2, na.rm = T)

#################    
# Week 2 ergosterol mass on top - ergosterol mass on sed (ug/leaf disc)
    
    Min.      1st Qu.   Median     Mean     3rd Qu.     Max.     SD        NAs 
    -0.02435  0.02405   0.10515    0.18487  0.18190     1.10425  0.2881235     1 
    
####################
    
### Week 14
    
    summary(erg.diff$erg.diff.14)
    sd(erg.diff$erg.diff.14, na.rm = T) 
    
###############################
# Week 14 ergosterol mass on top - ergosterol mass on sed (ug/leaf disc)
    
    Min.     1st Qu.  Median    Mean    3rd Qu.    Max.    SD
    -0.5226  0.1668   0.7249    0.8388  1.5106     2.3681  0.8867637
    
###################################
    