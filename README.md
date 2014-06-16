Practice-Machine-Learning
=========================
## Script 

##### download and load training and test files
```
download.file(url="https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv", destfile="training.csv")
download.file(url="https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv", destfile="testing.csv")
library(caret)
training<-read.csv("training.csv", header=TRUE)
testing<-read.csv("testing.csv", header=TRUE)
NAs <- apply(training,2,function(x) {sum(is.na(x)| x=="")}) 
```

##### remove all the columns with empty or null values 

```
training<- training[,which(NAs == 0)]
```

##### remove first 6 columns because dont affect the classification 

```
training<-training[,-c(1:6)]
```

##### same for testing data 

```
names<-names(training)
names<-names[-length(names)]
testing<- testing[,names]
```

##### invoke RF algorithm using cross validation with 4 k-folds implying 75% data for training, 25% data for cross validation 

#####K-fold data slicing:

```
foldsTrain<-createFolds(y=training$classe,k=4,list=TRUE,returnTrain=T)
sapply(folds,length)
Fold1 Fold2 Fold3 Fold4 
14715 14717 14718 14716 
foldsCrossVal<-createFolds(y=training$classe,k=4,list=TRUE,returnTrain=F)
sapply(foldsCrossVal,length)
Fold1 Fold2 Fold3 Fold4 
4904  4907  4906  4905
sapply(folds,length)/(sapply(folds,length)+sapply(foldsCrossVal,length))
Fold1     Fold2     Fold3     Fold4 
0.7500382 0.7499490 0.7500000 0.7500127
if we use k=4 the training data is the 75% of whole input data. CrossValData will be 25% of the whole input data
```

```
modFit <- train(classe ~.,data = training,method="rf", prox=TRUE,allowParallel=T,trControl = trainControl(method = "cv", number = 4))
```
```
prediction<-predict(modFit,testing)
```
