Description: a code book that describes the variables, the data, and any transformations or work that you performed to clean up the data called CodeBook.md.


#Assumptions#

It is assumed that the Samsung data is unzipped into the working directory. The data therefore resides in the same folder as run_analysis.R

In other words, it should be like this:

 ---
  |
  |---run_analysis.R
  |
  |---UCI HAR Dataset/
  
  
  
  The script run_analysis.R in this repository is divided into sub parts executing specific steps required in the assignment. The script has detailed comments for the same. Following is a brief description of the steps performed.
  
  -----------------------------------------------------------------------------------------------------------------------------------
  
 **Part 1 - Merge the training and the test sets to create one data set**
  
  Following variables are used:
  
  subjectTrain: holds training data from the file UCI HAR Dataset/train/subject_train.txt
  
  activityTrain: holds training data from the file UCI HAR Dataset/train/y_train.txt
  
  featuresTrain: holds training data from the file UCI HAR Dataset/train/X_train.txt
  
  subjectTest : holds testing data from the file UCI HAR Dataset/test/subject_test.txt
  
  activityTest : holds testing data from the file UCI HAR Dataset/test/y_test.txt
  
  featuresTest : holds testing data from the file UCI HAR Dataset/test/X_test.txt
  
  subject : holds data merged from data sets subjectTrain and subjectTest
  
  activity : holds data merged from data sets activityTrain and activityTest
  
  features : holds data merged from data sets featuresTrain and featuresTest
  
  completeData : holds data bound from data sets subject,activity,features
  
  
  The training and test data sets are loaded in R variables using function read.table.
  
  Ex : subjectTrain <- read.table("UCI HAR Dataset/train/subject_train.txt", header = FALSE)
  
  The training and test data sets are merged using function rbind.
  
  Ex : subject <- rbind(subjectTrain, subjectTest)
  
  The columns in the features data are named from the metadata in featureNames using the transpose function 't'
  
  colnames(features) <- t(featureNames[2])
  
  Data is merged into a single data set "completeData" using the function cbind
  
  ---------------------------------------------------------------------------------------------------------------------------
  
  **Part 2 - Extracts only the measurements on the mean and standard deviation for each measurement using the function grep**
  
  Following variables are used :
  
  columnsWithMeanSTD : holds  only the measurements on the mean and standard deviation for each measurement
  
  requiredColumns : activity and subject columns to the list of columns from variable columnsWithMeanSTD
  
  extractedData : contains data for columns defined in requiredColumns
  
  
  columnsWithMeanSTD <- grep(".*Mean.*|.*Std.*", names(completeData), ignore.case=TRUE)
  
  The data set "extractedData" is created with required columns from the parent data set completeData
  
  extractedData <- completeData[,requiredColumns]
  ---------------------------------------------------------------------------------------------------------------------------------------------------------
  
  **Part 3 - Uses descriptive activity names to name the activities in the data set**
  
 The activity field in extractedData is originally of numeric type. We need to change its type to character so that it can accept activity names. The activity names are taken from metadata activityLabels.

  extractedData$Activity <- as.character(extractedData$Activity)
  for (i in 1:6){
  extractedData$Activity[extractedData$Activity == i] <- as.character(activityLabels[i,2])
  }

-----------------------------------------------------------------------------------------------------------------------------

 **Part 4 - Appropriately labels the data set with descriptive variable names**

 Acc is replaced with Accelerometer

 Gyro is replaced with Gyroscope

 BodyBody is replaced with Body

 Mag is replaced with Magnitude

 Character f is replaced with Frequency

 Character t is replaced with Time

 Ex: names(extractedData)<-gsub("Acc", "Accelerometer", names(extractedData))

------------------------------------------------------------------------------------------------------------------------------------------------------------------------------------

**Part 5 - From the data set in step 4, creates a second, independent tidy data set with the average of each variable for each activity and each subject**

Create tidyData as a data set with average for each activity and subject. Then,order the enties in tidyData and write it into data file Tidy.txt that contains the processed data.

tidyData <- aggregate(. ~Subject + Activity, extractedData, mean)
tidyData <- tidyData[order(tidyData$Subject,tidyData$Activity),]
write.table(tidyData, file = "Tidy.txt", row.names = FALSE)
  
  
  
  
  
  
  
  
  
  
  
  
  
  
  
