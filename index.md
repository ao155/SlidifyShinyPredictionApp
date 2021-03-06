---
title       : Shiny Prediction App
subtitle    : Evaluate and compare different prediction models
author      : AO155
job         : 
framework   : io2012        # {io2012, html5slides, shower, dzslides, ...}
highlighter : highlight.js  # {highlight.js, prettify, highlight}
hitheme     : tomorrow      # 
widgets     : []            # {mathjax, quiz, bootstrap}
mode        : selfcontained # {standalone, draft}
knit        : slidify::knit2slides
---

## Background information
* <b>Data</b><br>
The app uses the 'Human Activity Recognition' data set to train different predictive models, with the variable 'classe" as target variable. <br>
More info on the dataset can be found at: http://groupware.les.inf.puc-rio.br/har <br>
<br>
* <b>Algorithms</b><br>
The app offers the user a selection of two decision tree algorithms:
    + <b>rPart:</b> recursive partitioning and regression trees
    + <b>Random Forest:</b> classification and regression based on a forest of trees using random inputs<br>
<br>
* <b>Evaluation</b><br>
The app evaluates the selected algorithm based on model accuracy.<br>
<br>

---

## UI Functionality
1. The user can <b>select between the rPart and the Random Forest algorithm</b> through a radio button.
The app confirms the selection by printing out the selected model in the results panel.<br>
<br>
2. The user can let the app <b>build the selected model</b>.
The app confirms the completion of the model building process with a notice in the results panel.<br>
<br>
3. The user can let the app <b>test the model</b>.
The app displays the accuracy of the selected model in % in the results panel.<br>
<br>
4. The user can then <b>select the other model and re-run these steps</b> to compare the accuracy of both models.<br>

---

## Server Functionality
* <b>Reactive Outputs to UI</b><br>
The server outputs are rendered depending on the action buttons in the UI. This way, the results panel shows interactive responses to the user activity.<br>
<br>
* <b>Parameterized function for building the predictive model</b><br>
The server calls another function which builds the predictive model. The server provides the name of the model selected by the user as a parameter to the other function. This other function then builds the model specified in the parameters, tests is using cross validation and returns the accuracy to the server.<br>
(See next slide for a shortened demo of this function.)<br>

---

## Demo of the 'buildmodel' function (shortened)

```r
buildmodel <- function(modelname) {load("data.Rda"); library(caret); set.seed(3875)
        inTrain <- createDataPartition(y=trainingclean$classe, p=0.6, list=FALSE)
        traindata <- trainingclean[inTrain,]
        testdata <- trainingclean[-inTrain,]
        if (modelname == "rPart") {library(rpart)
                rpartmodel <- rpart(classe~., data=traindata, method="class")
                rpartpred <- predict(rpartmodel, testdata, type="class")
                round(confusionMatrix(rpartpred, testdata$classe)$overall['Accuracy'] *100, digits = 2)}
        else if (modelname == "Random Forest") {library(randomForest)
                rfmodel <- randomForest(classe~., data=traindata, method="rf")
                rfpred <- predict(rfmodel, testdata, type="class")
                round(confusionMatrix(rfpred, testdata$classe)$overall['Accuracy'] *100, digits = 2)}
        }
buildmodel("rPart")     
```

```
## Accuracy 
##    76.87
```






