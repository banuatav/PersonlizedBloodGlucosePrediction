Initial Data Analysis
================

Personlized Bloodglucose Prediction
-----------------------------------

My journey in analyzing bloodglucose data of a type 1 diabetic patient, investigating the appropriate model design for this data and making predicitions based on this model/these models.

Data
----

The data mainly concerns bloodglucose-levels, grams of carbohydrates consumed and insuline injections of one type 1 diabetic patient over the course of 6 months. For each event, the date, timestamp an a description of the even is available. Due to this, it is necessary to clean the data first.

``` r
summary(BGData)
```

    ##   Dag           Datum           Tijd     
    ##  Di.:489   24.09.18:  29   23:59  : 190  
    ##  Do.:490   09.08.18:  28   7:39   :  20  
    ##  Ma.:498   10.07.18:  26   7:43   :  19  
    ##  Vr.:426   21.05.18:  26   7:27   :  18  
    ##  Wo.:488   22.09.18:  26   18:50  :  14  
    ##  Za.:477   09.09.18:  25   18:58  :  14  
    ##  Zo.:475   (Other) :3183   (Other):3068  
    ##                               Gebeurtenis       Waarde       Eenheid    
    ##  Bloedglucose                       :1090          : 138         : 138  
    ##  Koolhydraten                       : 941   20     :  69   g KH  : 941  
    ##  Bolus                              : 746   30     :  58   mmol/l:1090  
    ##  Combinatiebolus                    : 231   25     :  55   U     :1174  
    ##  Totale hoeveelheid insuline per dag: 183   40     :  35                
    ##  Pod geactiveerd                    :  63   1,50   :  32                
    ##  (Other)                            :  89   (Other):2956                
    ##              Informatie1         H1                 Informatie2  
    ##                    :2175          :2178                   :2929  
    ##  Voorgestelde bolus: 736    1,50 U:  27    Basaalinsuline : 183  
    ##  Directe dosis     : 231    1,90 U:  25    Verlengde dosis: 231  
    ##  Bolusinsuline     : 183    1,00 U:  20                          
    ##  Verlengingsperiode:  14    2,00 U:  19                          
    ##  Voor maaltijd     :   2    2,30 U:  19                          
    ##  (Other)           :   2   (Other):1055                          
    ##         H2                    Informatie3  
    ##          :2929                      :3111  
    ##   6,40 U :  99    Verlengingsperiode: 231  
    ##   7,60 U :  30   x                  :   1  
    ##   6,35 U :  26                             
    ##   7,55 U :  10                             
    ##   2,20 U :   7                             
    ##  (Other) : 242                             
    ##                           Informatie4         H3      
    ##                                 :3112          :3112  
    ##   60 min.Voorgestelde bolus was : 114    3,80 U:   7  
    ##   120 min.Voorgestelde bolus was:  72    5,50 U:   6  
    ##   90 min.Voorgestelde bolus was :  34    2,65 U:   4  
    ##   150 min.Voorgestelde bolus was:   3    2,80 U:   4  
    ##   30 min.Voorgestelde bolus was :   2    3,00 U:   4  
    ##  (Other)                        :   6   (Other): 206
