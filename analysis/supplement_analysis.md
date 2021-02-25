# Analysis of the amount of glucose and nutrients added to the treatments.

## Created:

* 2021-02-02

## Modified

* 2021-02-24 - KF - added the dates that the experiment began and when the samples were harvested. Also calculated the incubation times.

## Authors

* KF
* AO
* GH

## Description

## Experiment Dates
### Create Dataset

    sediment_collected <- as.POSIXct("2018-10-02")
    jars_setup <- as.POSIXct("2018-10-03")
    jar_water_change <- as.POSIXct(c("2018-10-10", "2018-10-17"))
    leaves_added_exp_begin <- as.POSIXct("2018-10-31")
    sampling_no1 <- as.POSIXct("2018-11-14")
    sampling_no2 <- as.POSIXct("2019-02-06")
    addition_dates <- as.POSIXct(c("2018-10-31", "2018-11-19", "2018-11-28", "2018-12-06", "2018-12-14", "2018-12-20", "2019-01-04", "2019-01-17", "2019-01-24", "2019-01-31"))
    
### Calculate Times
    
    sampling_no1_incubation <- difftime(sampling_no1, leaves_added_exp_begin, units = "weeks")
    sampling_no2_incubation <- difftime(sampling_no2, leaves_added_exp_begin, units = "weeks")
    supplement_times <- difftime(addition_dates, leaves_added_exp_begin, units = "days")

## Nutrient and Glucose Additions
### Create Dataset

    glucose_stock <- 0.286
    nitrogen_stock <- 0.043
    phosphorus_stock <- 0.0187
    glucose_vol <- 0.001
    nitrogen_vol <- 0.001
    phosphorus_vol <- 0.001
    glucose_mass <- (glucose_vol * glucose_stock) * 1000
    nitrogen_mass <- (nitrogen_vol * nitrogen_stock) * 1000
    phosphorus_mass <- (phosphorus_vol * phosphorus_stock) * 1000
    jar_volume <- (pi * (2.5^2)) * 10
    jar_glucose_conc <- (glucose_mass / jar_volume) * 1000
    jar_nitrogen_conc <- (nitrogen_mass / jar_volume) * 1000
    jar_phosphorus_conc <- (phosphorus_mass / jar_volume) * 1000
    

### Variable Descriptions

* glucose_stock = the concentration of the glucose stock solution that was added to the treatments (g/L). The description of how the stock was made can be found in [litter_priming_nutrient_nut_stock_prep](https://docs.google.com/document/d/1qf_M_EQpM8oQec1bGvvScHu9cLStaY0TueYi5lqEyg0/edit?usp=sharing) lab notes.

* nitrogen_stock = the concentration of the nitrogen stock solution that was added to the treatments (gN/L). The description of how the stock was made can be found in [litter_priming_nutrient_nut_stock_prep](https://docs.google.com/document/d/1qf_M_EQpM8oQec1bGvvScHu9cLStaY0TueYi5lqEyg0/edit?usp=sharing) lab notes.

* phosphorus_stock = the concentration of the phosphorus stock solution that was added to the treatments (gN/L). The description of how the stock was made can be found in [litter_priming_nutrient_nut_stock_prep](https://docs.google.com/document/d/1qf_M_EQpM8oQec1bGvvScHu9cLStaY0TueYi5lqEyg0/edit?usp=sharing) lab notes.

* addition_dates = the dates that the supplements were added to each treatment jar (YYYY-MM-DDG).

* glucose_volume = the volume of glucose stock solution that was added to each jar on the addition date (L).

* nitrogen_volume = the volume of nitrogen stock solution that was added to each jar on the addition date (L).

* phosphorus_volume = the volume of phosphorus stock solution that was added to each jar on the addition date (L).

* glucose_mass= the mass of glucose that was added to each jar on the addition date (mg).

* nitrogen_mass = the mass of nitrogen that was added to each jar on the addition date (mg).

* phosphorus_mass = the mass of phosphorus that was added to each jar on the addition date (mg).

* jar_volume = the volume of water in the jars during the experiment. The jars were 250 ml glass jars that are 13 cm tall with a 5 cm diameter. The sediments in the jar occupied the bottom 2 cm of the height (page 14 in the lab notebook) and jars were only filled to the lower portion of the lip at the top, which removed another 1 cm of height. Thus the dimensions of the cylinder that was filled with water was 5 cm diameter and 10 cm height. The volume was calculated as pi * r^2 (ml).

* jar_glucose_conc = the increased concentration of glucose in the jar immediately after the supplement was added (mg/L).

* jar_nitrogen_conc = the increased concentration of nitrogen in the jar immediately after the supplement was added (mgN/L).

* jar_phosphorus_conc = the increased concentration of phosphorus in the jar immediately after the supplement was added (mgP/L).
    
### Variable Values
    
    addition_dates
    [1] "2018-10-31 EDT" "2018-11-19 EST" "2018-11-28 EST" "2018-12-06 EST" "2018-12-14 EST"
    [6] "2018-12-20 EST" "2019-01-04 EST" "2019-01-17 EST" "2019-01-24 EST" "2019-01-31 EST"

    > glucose_mass
    [1] 0.286
    > glucose_stock
    [1] 0.286
    > glucose_vol
    [1] 0.001
    
    > nitrogen_mass
    [1] 0.043
    > nitrogen_stock
    [1] 0.043
    > nitrogen_vol
    [1] 0.001
    
    > phosphorus_mass
    [1] 0.0187
    > phosphorus_stock
    [1] 0.0187
    > phosphorus_vol
    [1] 0.001
    
    > jar_glucose_conc
    [1] 1.456586
    > jar_nitrogen_conc
    [1] 0.2189972
    > jar_phosphorus_conc
    [1] 0.09523832
    > jar_volume
    [1] 196.3495