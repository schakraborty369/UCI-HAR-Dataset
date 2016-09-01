# Explanation of the analysis file

##1. Merging the training and the test sets to create one data set

###Read the data from individual sources

Training set, dataframe of dim 7352 * 561

Training_set <- read.table("./train/X_train.txt")      

Test set, dataframe of dim 2947 * 561
           
Test_set <- read.table("./test/X_test.txt")   

Vector of trained subjects length 7352

Subject_trained <- read.table("./train/subject_train.txt")[,1]    

Vector of tested subjects length 2947

Subject_tested <- read.table("./test/subject_test.txt")[,1]       

Training labels vector length 7352

Training_labels <- read.table("./train/y_train.txt")[,1]          

Test labels vector length 2947

Test_labels <- read.table("./test/y_test.txt")[,1]                
