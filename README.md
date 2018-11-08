Initial Data Analysis
================

Personlized Bloodglucose Prediction
-----------------------------------

My journey in analyzing bloodglucose data of a type 1 diabetic patient, investigating the appropriate model design for this data and making predicitions based on this model/these models.

Data
----

The data mainly concerns bloodglucose-levels, grams of carbohydrates consumed and insuline injections of one type 1 diabetic patient over the course of 6 months. For each event, the date, timestamp an a description of the even is available. Due to this, it is necessary to clean the data first. Below you can find the first few observations and a summary of the data before cleaning.

``` r
head(BGData)
```

    ##   DayOfWeek     Date  Time             Event Value   Unit  Descripition1
    ## 1       Wed 07.11.18 18:56             Carbs  96.0      g               
    ## 2       Wed 07.11.18 18:55      Bloodglucose   4.7 mmol/l               
    ## 3       Wed 07.11.18 16:31             Bolus   1.5      U Proposed bolus
    ## 4       Wed 07.11.18 16:31             Carbs  20.0      g               
    ## 5       Wed 07.11.18 16:09 Combination bolus   5.5      U   Direct dosis
    ## 6       Wed 07.11.18 16:08      Bloodglucose   4.3 mmol/l               
    ##   ValueD1   Descripition2 ValueD2              Descripition3 ValueD3
    ## 1    <NA>            <NA>      NA                       <NA>      NA
    ## 2                    <NA>      NA                       <NA>      NA
    ## 3     1.5            <NA>      NA                       <NA>      NA
    ## 4                    <NA>      NA                       <NA>      NA
    ## 5     3.3  Extended dosis     2.2 Duration of extended dosis      60
    ## 6                    <NA>      NA                       <NA>      NA
    ##    Descripition4 ValueD4
    ## 1           <NA>    <NA>
    ## 2           <NA>    <NA>
    ## 3           <NA>    <NA>
    ## 4           <NA>    <NA>
    ## 5 Proposed bolus     5.5
    ## 6           <NA>    <NA>

``` r
summary(BGData)
```

    ##  DayOfWeek       Date           Time     
    ##  Fri:426   24.09.18:  29   23:59  : 190  
    ##  Mon:498   09.08.18:  28   7:39   :  20  
    ##  Sat:477   10.07.18:  26   7:43   :  19  
    ##  Sun:475   21.05.18:  26   7:27   :  18  
    ##  Thu:490   22.09.18:  26   18:50  :  14  
    ##  Tue:489   09.09.18:  25   18:58  :  14  
    ##  Wed:488   (Other) :3183   (Other):3068  
    ##                           Event          Value            Unit     
    ##  Bloodglucose                :1090   Min.   :  0.05         : 138  
    ##  Carbs                       : 941   1st Qu.:  4.00   g     : 941  
    ##  Bolus                       : 746   Median :  8.05   mmol/l:1090  
    ##  Combination bolus           : 231   Mean   : 16.99   U     :1174  
    ##  Total amount insulin per day: 183   3rd Qu.: 21.95                
    ##  Pod activated               :  63   Max.   :189.00                
    ##  (Other)                     :  89   NA's   :138                   
    ##                     Descripition1     ValueD1             Descripition2 
    ##                            :2175          :2178                  :   2  
    ##  Proposed bolus            : 736   1.5    :  30    Basal insulin : 183  
    ##  Direct dosis              : 231   1.9    :  27    Extended dosis: 231  
    ##  Bolus insulin             : 183   2.3    :  24   NA's           :2927  
    ##  Duration of extended dosis:  14   1      :  23                         
    ##  (Other)                   :   3   (Other):1060                         
    ##  NA's                      :   1   NA's   :   1                         
    ##     ValueD2                          Descripition3     ValueD3    
    ##  Min.   : 0.200                             :   2   Min.   : 30   
    ##  1st Qu.: 2.263   Duration of extended dosis: 231   1st Qu.: 60   
    ##  Median : 4.250   NA's                      :3110   Median : 60   
    ##  Mean   : 4.398                                     Mean   : 85   
    ##  3rd Qu.: 6.400                                     3rd Qu.:120   
    ##  Max.   :13.050                                     Max.   :150   
    ##  NA's   :2929                                       NA's   :3112  
    ##         Descripition4     ValueD4    
    ##                :   2   3.8    :   7  
    ##  Proposed bolus: 231   5.5    :   6  
    ##  NA's          :3110   2.65   :   4  
    ##                        2.8    :   4  
    ##                        3      :   4  
    ##                        (Other): 208  
    ##                        NA's   :3110

### Cleaning Data

We can see that the variable Event has many categories all measured in different ways. The category Carb (which indicates that the patient has eaten a meal that contained carbohydrates) is measured in grams (g) and can be easily seperated into its own variable. The latter is also true voor category Bloodglucose (bloodglucose value measured at a particular point of time in mmol/l).

The Carbs variable can be created by executing the following piece of code.

``` r
Carbs = rep(NA, 3343)
Carbs[BGData$Event == "Carbs"] = BGData$Value[BGData$Event == "Carbs"]
head(data.frame(BGData$Event, BGData$Value, Carbs))
```

    ##        BGData.Event BGData.Value Carbs
    ## 1             Carbs         96.0    96
    ## 2      Bloodglucose          4.7    NA
    ## 3             Bolus          1.5    NA
    ## 4             Carbs         20.0    20
    ## 5 Combination bolus          5.5    NA
    ## 6      Bloodglucose          4.3    NA

The last line allows us to check whether the transformation is performed correctly. Similarly, for Bloodglucose we execute

``` r
Bloodglucose = rep(NA, 3343)
Bloodglucose[BGData$Event == "Bloodglucose"] = BGData$Value[BGData$Event == "Bloodglucose"]
head(data.frame(BGData$Event, BGData$Value, Bloodglucose))
```

    ##        BGData.Event BGData.Value Bloodglucose
    ## 1             Carbs         96.0           NA
    ## 2      Bloodglucose          4.7          4.7
    ## 3             Bolus          1.5           NA
    ## 4             Carbs         20.0           NA
    ## 5 Combination bolus          5.5           NA
    ## 6      Bloodglucose          4.3          4.3

Now we can transform the original variable Event and corresponding variable Value to discard the information on Carbs and Bloodglucose.

``` r
BGData$Value[BGData$Event == "Carbs"] = NA
BGData$Value[BGData$Event == "Bloodglucose"] = NA
BGData$Event[BGData$Event == "Carbs"] = NA
BGData$Event[BGData$Event == "Bloodglucose"] = NA

head(BGData)
```

    ##   DayOfWeek     Date  Time             Event Value   Unit  Descripition1
    ## 1       Wed 07.11.18 18:56              <NA>    NA      g               
    ## 2       Wed 07.11.18 18:55              <NA>    NA mmol/l               
    ## 3       Wed 07.11.18 16:31             Bolus   1.5      U Proposed bolus
    ## 4       Wed 07.11.18 16:31              <NA>    NA      g               
    ## 5       Wed 07.11.18 16:09 Combination bolus   5.5      U   Direct dosis
    ## 6       Wed 07.11.18 16:08              <NA>    NA mmol/l               
    ##   ValueD1   Descripition2 ValueD2              Descripition3 ValueD3
    ## 1    <NA>            <NA>      NA                       <NA>      NA
    ## 2                    <NA>      NA                       <NA>      NA
    ## 3     1.5            <NA>      NA                       <NA>      NA
    ## 4                    <NA>      NA                       <NA>      NA
    ## 5     3.3  Extended dosis     2.2 Duration of extended dosis      60
    ## 6                    <NA>      NA                       <NA>      NA
    ##    Descripition4 ValueD4
    ## 1           <NA>    <NA>
    ## 2           <NA>    <NA>
    ## 3           <NA>    <NA>
    ## 4           <NA>    <NA>
    ## 5 Proposed bolus     5.5
    ## 6           <NA>    <NA>
