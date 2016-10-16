---
title       : Predicting Home Runs
subtitle    : 
author      : David Smith
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Introduction

The Shiny application "Predicting Home Runs!" allows a user to enter some information on a hitters season and obtain a prediction of the number of home runs they hit. The application
relies on a simple linear model and a selection of data from the Lahman database (http://www.seanlahman.com/baseball-archive/statistics/). 

--- .class #id

## Filtered Data set

The Batting data from the Lahman set is filtered to remove years before the Year 2000 and players with under 300 at bats in a season. 

This leaves us 3,616 records. Of this we use at bats (AB), runs scored (R), hits (H), walks (BB), runs batted in (RBI), doubles (2B) and triples (3B) to create our linear model. 

--- .class #id

## Training the model


```r
require(Lahman)
```

```
## Loading required package: Lahman
```

```r
begYear <- 2000
minAtBats <- 300
data(Batting)
dataToUse <- filter(Batting, yearID>=begYear, AB>=minAtBats)
inTrain <- sample(seq_len(nrow(dataToUse)),size=floor(0.5*nrow(dataToUse)))
training <- dataToUse[inTrain,]
testing <- dataToUse[-inTrain,]

modelHR <- lm(HR~AB+R+H+BB+RBI+X2B+X3B,data=training)
```

--- .class #id

## Testing the model

```r
predictHR <- predict(modelHR, testing)

mean(predictHR-testing$HR)
```

```
## [1] 0.05566678
```
While our model isn't perfect (it predicts negative home runs in some cases!), it is fairly close.

