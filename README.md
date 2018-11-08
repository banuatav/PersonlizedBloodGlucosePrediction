## Personlized Bloodglucose Prediction

My journey in analyzing bloodglucose data of a type 1 diabetic patient, modelling this data and making predicition based on this model/these models.

## Data

The data mainly concerns bloodglucose-levels, grams of carbohydrates consumed and insuline injections of one type 1 diabetic patient over the course of 6 months.

## Data

The data mainly concerns bloodglucose-levels, grams of carbohydrates consumed and insuline injections of one type 1 diabetic patient over the course of 6 months. For each event, the date, timestamp an a description of the even is available. Due to this, it is necessary to clean the data first.

```{r, include=FALSE}
BGData = read.csv("Data BG.csv", header = TRUE)
```
```{r BGData}
summary(BGData)
```

```{r}
library(knitr) # the package that renders R markdown and has some good additional functionality
kable(BGData)
```


