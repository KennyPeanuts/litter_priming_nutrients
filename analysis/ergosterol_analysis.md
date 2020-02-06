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

### Plot Ergosterol Per leaf by Treatment
#### 2 Week
    
    plot(ErgLeaf ~ Location, data = erg, subset = HarvestDate == "11/12/19")
    
#### 14 Week
    
    plot(ErgLeaf ~ Location, data = erg, subset = HarvestDate == "2/7/19")
    
### Plot Ergosterol Per leaf by Treatment
#### 2 Week
    
    plot(ErgLeaf ~ Treat, data = erg, subset = HarvestDate == "11/12/19")
    
#### 14 Week
    
    plot(ErgLeaf ~ Treat, data = erg, subset = HarvestDate == "2/7/19")
    