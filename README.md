# Explanation of the analysis file

##1. Merging the training and the test sets to create one data set

###Read the data from individual sources

Training set, dataframe of dim 7352 * 561
```{r} 
Training_set <- read.table("./train/X_train.txt")      
```
Test set, dataframe of dim 2947 * 561
```{r}           
Test_set <- read.table("./test/X_test.txt")   
``` 
Vector of trained subjects length 7352
```{r} 
Subject_trained <- read.table("./train/subject_train.txt")[,1]    
``` 
Vector of tested subjects length 2947
```{r} 
Subject_tested <- read.table("./test/subject_test.txt")[,1]       
``` 
Training labels vector length 7352
```{r} 
Training_labels <- read.table("./train/y_train.txt")[,1]          
``` 
Test labels vector length 2947
```{r} 
Test_labels <- read.table("./test/y_test.txt")[,1]                
``` 
Feature vector length 561
```{r} 
features <- read.table("features.txt")[,2] 
```                       

###Combination using cbind and rbind function

```{r} 
tempx <- rbind(Training_set, Test_set)
names(tempx) <- make.unique(as.character(features))               #headings of tempx replaced with features#
Activity <- append(Training_labels, Test_labels)
Subjects <- append(Subject_trained, Subject_tested)
Combined_data <- cbind(Subjects, tempx, Activity)
```

##2. Extracting only the measurements on the mean and standard deviation for each measurement

```{r} 
filtered_data <- select(Combined_data, 
contains("Subjects"),                                     contains("mean"), 
contains("std"), 
contains("Activity"),  
-contains("meanFreq"), 
-contains("angle"))
```
