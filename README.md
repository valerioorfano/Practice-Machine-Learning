Practice-Machine-Learning
=========================
** Script**

download.file(url="https://d396qusza40orc.cloudfront.net/predmachlearn/pml-training.csv", destfile="training.csv")
download.file(url="https://d396qusza40orc.cloudfront.net/predmachlearn/pml-testing.csv", destfile="testing.csv")
library(caret)
training<-read.csv("training.csv", header=TRUE)
testing<-read.csv("testing.csv", header=TRUE)
NAs <- apply(training,2,function(x) {sum(is.na(x)| x=="")}) 


** remove all the columns with empty or null values**


training<- training[,which(NAs == 0)]


** remove first 6 columns because don't affect the classification**


training<-training[,-c(1:6)]


** same for testing data**


names<-names(training)
names<-names[-length(names)]
testing<- testing[,names]


**invoke RF algorithm using cross validation with 4 k-folds implying 70% data for training, 30% data for cross validation**


modFit <- train(classe ~.,data = training,method="rf", prox=TRUE,allowParallel=T,trControl = trainControl(method = "cv", number = 4))
prediction<-predict(modFit,testing)

