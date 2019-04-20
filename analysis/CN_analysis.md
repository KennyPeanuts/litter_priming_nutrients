# Analysis of CN mass for LPNL experiment

## Created:

* 4 March 2019

## Modified

* 2019-April-19 - KF - added analysis of the CN results

## Authors

* KF
* AO

## Description

## Analysis

### Import Data

    CN <- read.table("./data/CN_all.csv", header = T, sep = ",")
    
## Analysis
    
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Sed" & Date == "2018-11-12", main = "Sed", ylim = c(20, 100))
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Top" & Date == "2018-11-12", main = "Top", ylim = c(20, 100))
    
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Sed" & Date == "2019-02-07", main = "Sed", ylim =c(20, 100))
    plot(CN_mol ~ Treat, data = CN, subset = Location == "Top" & Date == "2019-02-07", main = "Top", ylim = c(20, 100))
    
    plot(CN_mol ~ Nutrients, data = CN, subset = Location == "Top")
    