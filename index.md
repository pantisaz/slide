---
title       : R crash course
framework   : revealjs
highlighter : html5slides   # {io2012, html5slides, shower, dzslides, ...}
hitheme     : default 
widgets     : []
mode        : selfcontained
knit        : slidify::knit2slides
---


## Developing Data Products Course Project

Santiago Paz, June 2015

---

## Introduction

This project takes a glance at hospitals from all US states
and gives the worst or best ranked hospital from each state according
to three different medical issues

1. Heart Attacks 
2. Heart Failure
3. Pneumonia

---

The app can be found 
[here](https://www.shinyapps.io/admin/#/application/47905)

Code for the app can be found 
[here](http://github.com/pantisaz/developing-data-products)

---
The data for the project was obtained from 
[here](https://data.medicare.gov/data/hospital-compare)

---
## Some Data


```r
bigdata<-read.csv("outcome-of-care-measures.csv", colClasses = "character", na.string = "Not Available")

head(bigdata)
```

```
##   Provider.Number                    Hospital.Name
## 1          010001 SOUTHEAST ALABAMA MEDICAL CENTER
## 2          010005    MARSHALL MEDICAL CENTER SOUTH
## 3          010006   ELIZA COFFEE MEMORIAL HOSPITAL
## 4          010007         MIZELL MEMORIAL HOSPITAL
## 5          010008      CRENSHAW COMMUNITY HOSPITAL
## 6          010010    MARSHALL MEDICAL CENTER NORTH
##                    Address.1 Address.2 Address.3         City State
## 1     1108 ROSS CLARK CIRCLE                           DOTHAN    AL
## 2 2505 U S HIGHWAY 431 NORTH                             BOAZ    AL
## 3         205 MARENGO STREET                         FLORENCE    AL
## 4              702 N MAIN ST                              OPP    AL
## 5        101 HOSPITAL CIRCLE                          LUVERNE    AL
## 6    8000 ALABAMA HIGHWAY 69                     GUNTERSVILLE    AL
##   ZIP.Code County.Name Phone.Number
## 1    36301     HOUSTON   3347938701
## 2    35957    MARSHALL   2565938310
## 3    35631  LAUDERDALE   2567688400
## 4    36467   COVINGTON   3344933541
## 5    36049    CRENSHAW   3343353374
## 6    35976    MARSHALL   2565718000
##   Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack
## 1                                                      14.3
## 2                                                      18.5
## 3                                                      18.1
## 4                                                      <NA>
## 5                                                      <NA>
## 6                                                      <NA>
##   Comparison.to.U.S..Rate...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack
## 1                                                No Different than U.S. National Rate
## 2                                                No Different than U.S. National Rate
## 3                                                No Different than U.S. National Rate
## 4                                                           Number of Cases Too Small
## 5                                                           Number of Cases Too Small
## 6                                                           Number of Cases Too Small
##   Lower.Mortality.Estimate...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack
## 1                                                                                 12.1
## 2                                                                                 14.7
## 3                                                                                 14.8
## 4                                                                                 <NA>
## 5                                                                                 <NA>
## 6                                                                                 <NA>
##   Upper.Mortality.Estimate...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack
## 1                                                                                 17.0
## 2                                                                                 23.0
## 3                                                                                 21.8
## 4                                                                                 <NA>
## 5                                                                                 <NA>
## 6                                                                                 <NA>
##   Number.of.Patients...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack
## 1                                                                            666
## 2                                                                             44
## 3                                                                            329
## 4                                                                             14
## 5                                                                              9
## 6                                                                             22
##                                Footnote...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Attack
## 1                                                                                                  
## 2                                                                                                  
## 3                                                                                                  
## 4 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
## 5 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
## 6 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
##   Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure
## 1                                                       11.4
## 2                                                       15.2
## 3                                                       11.3
## 4                                                       13.6
## 5                                                       13.8
## 6                                                       12.5
##   Comparison.to.U.S..Rate...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure
## 1                                                 No Different than U.S. National Rate
## 2                                                        Worse than U.S. National Rate
## 3                                                 No Different than U.S. National Rate
## 4                                                 No Different than U.S. National Rate
## 5                                                 No Different than U.S. National Rate
## 6                                                 No Different than U.S. National Rate
##   Lower.Mortality.Estimate...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure
## 1                                                                                   9.5
## 2                                                                                  12.2
## 3                                                                                   9.1
## 4                                                                                  10.0
## 5                                                                                   9.9
## 6                                                                                   9.9
##   Upper.Mortality.Estimate...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure
## 1                                                                                  13.7
## 2                                                                                  18.8
## 3                                                                                  13.9
## 4                                                                                  18.2
## 5                                                                                  18.7
## 6                                                                                  15.6
##   Number.of.Patients...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure
## 1                                                                             741
## 2                                                                             234
## 3                                                                             523
## 4                                                                             113
## 5                                                                              53
## 6                                                                             163
##   Footnote...Hospital.30.Day.Death..Mortality..Rates.from.Heart.Failure
## 1                                                                      
## 2                                                                      
## 3                                                                      
## 4                                                                      
## 5                                                                      
## 6                                                                      
##   Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia
## 1                                                   10.9
## 2                                                   13.9
## 3                                                   13.4
## 4                                                   14.9
## 5                                                   15.8
## 6                                                    8.7
##   Comparison.to.U.S..Rate...Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia
## 1                                             No Different than U.S. National Rate
## 2                                             No Different than U.S. National Rate
## 3                                             No Different than U.S. National Rate
## 4                                             No Different than U.S. National Rate
## 5                                             No Different than U.S. National Rate
## 6                                                   Better than U.S. National Rate
##   Lower.Mortality.Estimate...Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia
## 1                                                                               8.6
## 2                                                                              11.3
## 3                                                                              11.2
## 4                                                                              11.6
## 5                                                                              11.4
## 6                                                                               6.8
##   Upper.Mortality.Estimate...Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia
## 1                                                                              13.7
## 2                                                                              17.0
## 3                                                                              15.8
## 4                                                                              19.0
## 5                                                                              21.5
## 6                                                                              11.0
##   Number.of.Patients...Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia
## 1                                                                         371
## 2                                                                         372
## 3                                                                         836
## 4                                                                         239
## 5                                                                          61
## 6                                                                         315
##   Footnote...Hospital.30.Day.Death..Mortality..Rates.from.Pneumonia
## 1                                                                  
## 2                                                                  
## 3                                                                  
## 4                                                                  
## 5                                                                  
## 6                                                                  
##   Hospital.30.Day.Readmission.Rates.from.Heart.Attack
## 1                                                19.0
## 2                                                <NA>
## 3                                                17.8
## 4                                                <NA>
## 5                                                <NA>
## 6                                                <NA>
##   Comparison.to.U.S..Rate...Hospital.30.Day.Readmission.Rates.from.Heart.Attack
## 1                                          No Different than U.S. National Rate
## 2                                                     Number of Cases Too Small
## 3                                          No Different than U.S. National Rate
## 4                                                     Number of Cases Too Small
## 5                                                     Number of Cases Too Small
## 6                                                     Number of Cases Too Small
##   Lower.Readmission.Estimate...Hospital.30.Day.Readmission.Rates.from.Heart.Attack
## 1                                                                             16.6
## 2                                                                             <NA>
## 3                                                                             14.9
## 4                                                                             <NA>
## 5                                                                             <NA>
## 6                                                                             <NA>
##   Upper.Readmission.Estimate...Hospital.30.Day.Readmission.Rates.from.Heart.Attack
## 1                                                                             21.7
## 2                                                                             <NA>
## 3                                                                             21.5
## 4                                                                             <NA>
## 5                                                                             <NA>
## 6                                                                             <NA>
##   Number.of.Patients...Hospital.30.Day.Readmission.Rates.from.Heart.Attack
## 1                                                                      728
## 2                                                                       21
## 3                                                                      342
## 4                                                                        1
## 5                                                                        4
## 6                                                                       13
##                                      Footnote...Hospital.30.Day.Readmission.Rates.from.Heart.Attack
## 1                                                                                                  
## 2 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
## 3                                                                                                  
## 4 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
## 5 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
## 6 number of cases is too small (fewer than 25) to reliably tell how well the hospital is performing
##   Hospital.30.Day.Readmission.Rates.from.Heart.Failure
## 1                                                 23.7
## 2                                                 22.5
## 3                                                 19.8
## 4                                                 27.1
## 5                                                 24.7
## 6                                                 23.9
##   Comparison.to.U.S..Rate...Hospital.30.Day.Readmission.Rates.from.Heart.Failure
## 1                                           No Different than U.S. National Rate
## 2                                           No Different than U.S. National Rate
## 3                                                 Better than U.S. National Rate
## 4                                           No Different than U.S. National Rate
## 5                                           No Different than U.S. National Rate
## 6                                           No Different than U.S. National Rate
##   Lower.Readmission.Estimate...Hospital.30.Day.Readmission.Rates.from.Heart.Failure
## 1                                                                              21.3
## 2                                                                              19.2
## 3                                                                              17.2
## 4                                                                              22.4
## 5                                                                              19.9
## 6                                                                              20.1
##   Upper.Readmission.Estimate...Hospital.30.Day.Readmission.Rates.from.Heart.Failure
## 1                                                                              26.5
## 2                                                                              26.1
## 3                                                                              22.9
## 4                                                                              31.9
## 5                                                                              30.2
## 6                                                                              28.2
##   Number.of.Patients...Hospital.30.Day.Readmission.Rates.from.Heart.Failure
## 1                                                                       891
## 2                                                                       264
## 3                                                                       614
## 4                                                                       135
## 5                                                                        59
## 6                                                                       173
##   Footnote...Hospital.30.Day.Readmission.Rates.from.Heart.Failure
## 1                                                                
## 2                                                                
## 3                                                                
## 4                                                                
## 5                                                                
## 6                                                                
##   Hospital.30.Day.Readmission.Rates.from.Pneumonia
## 1                                             17.1
## 2                                             17.6
## 3                                             16.9
## 4                                             19.4
## 5                                             18.0
## 6                                             18.7
##   Comparison.to.U.S..Rate...Hospital.30.Day.Readmission.Rates.from.Pneumonia
## 1                                       No Different than U.S. National Rate
## 2                                       No Different than U.S. National Rate
## 3                                       No Different than U.S. National Rate
## 4                                       No Different than U.S. National Rate
## 5                                       No Different than U.S. National Rate
## 6                                       No Different than U.S. National Rate
##   Lower.Readmission.Estimate...Hospital.30.Day.Readmission.Rates.from.Pneumonia
## 1                                                                          14.4
## 2                                                                          15.0
## 3                                                                          14.7
## 4                                                                          15.9
## 5                                                                          14.0
## 6                                                                          15.7
##   Upper.Readmission.Estimate...Hospital.30.Day.Readmission.Rates.from.Pneumonia
## 1                                                                          20.4
## 2                                                                          20.6
## 3                                                                          19.5
## 4                                                                          23.2
## 5                                                                          22.8
## 6                                                                          22.2
##   Number.of.Patients...Hospital.30.Day.Readmission.Rates.from.Pneumonia
## 1                                                                   400
## 2                                                                   374
## 3                                                                   842
## 4                                                                   254
## 5                                                                    56
## 6                                                                   326
##   Footnote...Hospital.30.Day.Readmission.Rates.from.Pneumonia
## 1                                                            
## 2                                                            
## 3                                                            
## 4                                                            
## 5                                                            
## 6
```
