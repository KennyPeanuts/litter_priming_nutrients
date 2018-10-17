# Calculation of the treatment level concentrations 
## Date
 * created: 17 Oct 2018
 * modified:

## Author
KF

## Purpose
These calculations are to determine the amount of DIN, DIP, and Glucose that needs to be added to the treatments in the litter priming nutrient limitation experiment.

The CPOM Flux experiment used a target concentration of `300 ug/L` DIN and `30 ug/L` DIP based on personal communication with Vlad Gulis, that these concentrations would be saturating for stream fungi.  In that experiment, we did see and increase in respiration (as SOD) with the amended nutrients.

Hotchkiss et al. 2014. JGR Biogeosciences 119:982-995 saw positive priming with a glucose concentration of 2 mg/L DOC.

## Calculations
### General Formula

    (Vt / Ct) = (Vs / Cs)

* Vt = the volume of the treatment bottle (L)
* Ct = the target concentration of the treatment bottle (g/L)
* Vs = the volume of the stock added to the treatment (L)
* Cs = the concentration of the stock solution (g/L)

To calculate the concentration of stock needed this gets rearranged to:

    Cs = (CtVt)/Vt

### Target Concentrations

The water column in the jars is 9 cm tall and has radius of 2.25 cm, so the volume is:

    Vt = 9 cm * (&pi; * 2.25^2 cm) = 143.1 cm^3 = 143.1 ml
    Vt = 143.1 ml / (1000 ml/L) =  0.1431 L

Weekly we will add 0.001 L of each stock solution to the treatment bottle to achieve target concentrations of:

* 0.0003 g/L DIN
* 0.00003 g/L DIP
* 0.002 g/L DLOC

#### DIN

To get a target concentration of 0.0003 g/L DIN by diluting 0.001 L of stock to 0.1431 L we will need:

    Cs = (0.0003 * 0.1431) / 0.001 = 0.043 g/L DIN

The molecular weight of NH4NO3 = 80.052 g/mol

The molecular weight of N = 14 g/mol

Since NH4NO3 has 2 N, the N weighs 28 g/mol

Thus the N is `35%` of the mass of the NH4NO3

    28 / 80.052 = 0.35 

So to get 0.043 g/L, we need to dissolve `0.123` NH4NO3 into 1 L of di water

    T * f = P

and 

    T = P / f

where:

    T = the total 
    f = the fraction
    P = the part that is that fraction of the total 

So in this case

    f = 0.35
    P = 0.043 g/L

    T = 0.043 / 0.35 = 0.123 g NH4NO3
    
#### DIP

To get a target concentration of 0.00003 g/L DIP by diluting 0.001 L of stock to 0.1431 L we will need:

    Cs = (0.00003 * 0.1431) / 0.001 = 0.0043 g/L DIP

The molecular weight of KH2PO4 = 136 g/mol

The P weighs 30.97 g/mol

Thus the P is `26%` of the mass of the PO4

    30.97 / 136 = 0.23

So to get 0.0043 g/L, we need to dissolve `0.0187` KH2PO4 into 1 L of di water

    T * f = P

and 

    T = P / f

where:

    T = the total 
    f = the fraction
    P = the part that is that fraction of the total 

So in this case

    f = 0.23
    P = 0.0043 g/L

    T = 0.0043 / 0.23 = 0.0187 g KH2PO4

#### DLOC

To get a target concentration of 0.002 g/L DIP by diluting 0.001 L of stock to 0.1431 L we will need:

    Cs = (0.002 * 0.1431) / 0.001 = 0.286 g/L DIP

The molecular weight of C6H12O6 = 180 g/mol

The C weighs 12 g/mol

Since there are 6 C atoms in a glucose, the total C = 6 * 12 = 72 gC/mol glucose

Thus the C is `` of the mass of the 

    72 / 180 = 0.4

So to get 0.286 g/L, we need to dissolve `0.715` C6H12O6 into 1 L of di water

    T * f = P

and 

    T = P / f

where:

    T = the total 
    f = the fraction
    P = the part that is that fraction of the total 

So in this case

    f = 0.40
    P = 0.286 g/L

    T = 0.286 / 0.4 = 0.715 g KH2PO4
