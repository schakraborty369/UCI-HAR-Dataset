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
contains("Subjects"),
contains("mean"),
contains("std"),
contains("Activity"),
-contains("meanFreq"),
-contains("angle"))
```

##3. Using descriptive activity names to name the activities in the data set

```{r} 
Activity <- read.table("activity_labels.txt", col.names = c("Activity_Labels", "Activity"))
for (i in Activity[,1])  
{filtered_data$Activity[filtered_data$Activity == i] <- as.character(Activity[i,2])}
```

##4. Appropriate labelling of the data set with descriptive variable names

```{r} 
names(filtered_data) <- gsub("std","stDev",names(filtered_data))
names(filtered_data) <- gsub("BodyBody","Body_",names(filtered_data))
names(filtered_data) <- gsub("^f","Frequency_",names(filtered_data))
names(filtered_data) <- gsub("^t","Time_",names(filtered_data))
names(filtered_data) <- gsub("Acc","Acceleration_",names(filtered_data))
names(filtered_data) <- gsub("Gyro","Gyroscope_",names(filtered_data))
names(filtered_data) <- gsub("-","",names(filtered_data))
names(filtered_data) <- gsub("Jerk","Jerk_",names(filtered_data))
names(filtered_data) <- gsub("Mag","Magnitude_",names(filtered_data))
names(filtered_data) <- gsub("gravityMean","Gravity_Mean",names(filtered_data))
names(filtered_data) <- gsub("Body","Body_",names(filtered_data))
names(filtered_data) <- gsub("\\()", "-", as.character(names(filtered_data)))
names(filtered_data) <- gsub("-$", "", as.character(names(filtered_data)))
names(filtered_data) <- gsub("Gravity", "Gravity_", as.character(names(filtered_data)))
names(filtered_data) <- gsub("__", "_", as.character(names(filtered_data)))
```

##5. Creating a secondary independent tidy data set with the average of each variable for each activity and each subject

```{r} 
new_data <- group_by(filtered_data, Subjects, Activity)
secondary_data <- summarise_each(new_data,funs(mean))
```

##6. Saved both filtered data and secondary tidy data

write.table(filtered_data, file="filtered_data.txt", row.name=FALSE)
write.table(secondary_data, file="secondary_data.txt", row.name=FALSE)

