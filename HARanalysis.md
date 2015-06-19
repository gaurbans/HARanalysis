# Practical Machine Learning Course Project
## Analyzing HAR Data 

In this project we will predict the manner in which people exercise. 

```r
setwd("C:/Users/gaurav.bansal/Favorites/Downloads/HARanalysis") ##set working directory
library(caret)
library(ggplot2)
```

We read in the data. We then split the Training data into 'training' and 'validation' datasets.   

```r
Training <- read.csv("pml-training.csv")
Testing <- read.csv("pml-testing.csv")

inTrain <- createDataPartition(y=Training$classe, p=0.80, list=F)
training <- Training[inTrain, ]
valid <- Training[-inTrain, ]
```

As it turns out, using 'raw_timestamp_part_1' and 'raw_timestamp_part_2' as predictors leads to very good prediction of 'classe'. Let's build a random forests model with these two predictors.  

```r
fit <- train(classe ~ raw_timestamp_part_1 + raw_timestamp_part_2, data=training, method="rf")
```

```
## Loading required package: randomForest
## randomForest 4.6-10
## Type rfNews() to see new features/changes/bug fixes.
```

```
## note: only 1 unique complexity parameters in default grid. Truncating the grid to 1 .
```

```r
predvalid <- predict(fit, valid)
tablevalid <- table(predvalid, valid$classe)
```

Let's now look at the accuracy of the model we've built on the validation dataset. 

```r
accuracy <- sum(diag(tablevalid))/sum(tablevalid)
print(accuracy)
```

```
## [1] 0.9992353
```




