## Human Activity Recognition Using Smartphones Dataset - Week 4 project
### Getting and Cleaning data in R

The dataset used in this project was first published by Anguita et al. (2012) [1] and can be downloaded in original format from the [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones). 

This repository contains 1) the CodeBook.md file describing the variables for the datasets, 2) the tiddy datasets for the merged files (test and train datasets) and the averaged results per participant, and 3) the run_analysis.R file that does all needed transformations for this project.

The R file and code runs as follows:

I first load the {dplyr} library and remove any variable from the workspace
```
#run_analysis.R week 4 project
library(dplyr) 
rm(list = ls()) #remove any previous element in the workspace
```
The feature.txt file contains the variable names; These are loaded to the workspace
```
#Load the feature/activity labels
Fnames <- readLines("features.txt")
```
The data files are fixed width files, and because of their size, these are very heavy to load. Because the project asked to work with the mean values from each of the variables, I loaded only the mean variables in the datasets filtering these with Fnames and metacharacters as follows:
```
#Get the means and stds only - I am ignoring all mean frequencies
meanstds <- grep("mean\\(|std\\(",Fnames)
indvec <- -rep(1,561) # a total of 561 variables (From Fnames)
indvec[meanstds] = 1 # Just read the variables we need
```
Then the program makes the names tiddy too; no parenthesis, no numbers and all low cased
```
#Making tiddy names
Mynames <- tolower(Fnames[meanstds])
secondElement <- function(x){x[2]}
Mynames <- sapply(strsplit(Mynames, " "), secondElement)
#Get rid of the parenthesis
Mynames <- sub("\\(\\)", "", Mynames)
Mynames <- gsub("`","", Mynames)
```
The next step is to load the desired variables. The read.fwf works best when the size is given in advance (according to tutorials I read on internet). Also the columns for the 561 variables are comprised by 16 character widths, rep(16,561) gives the location of the column starts and indvec tells to the read.fwf function which columns should be loaded and which ones ingnored.
```
#Read test and train data
testfile = "./test/X_test.txt"
Numlines = length(readLines(testfile))
testdata <- read.fwf(testfile, widths = rep(16,561)*indvec, sep = "", n = Numlines)
trainfile = "./train/X_train.txt"
Numlines = length(readLines(trainfile))
traindata <- read.fwf(trainfile, widths = rep(16,561)*indvec, sep = "", n = Numlines) 
```
Nest step is merging both the test and train datasets and add the name variables to the resultant data frame
```
#Merge test and train data in one matrix
MergedData <- rbind(testdata,traindata)
#rm(list = c("testdata","traindata")) #Remove the separated datasets
#Ad the column names for the dataset
colnames(MergedData) <- Mynames
```
For the second part of the project, I loaded the indices of the 30 participants in this study and created a variable called subID.
```
#The Participant indices per test and train data
testPart <- readLines("./test/subject_test.txt")
trainPart <- readLines("./train/subject_train.txt")
SubID <- c(testPart, trainPart)
```
Then the SubID variable is added to the merged dataframe MergedData and grouped by participant_id. In this way, I can call summmarise_each R function to obtain the mean of all columns by participant, see below
```
MergedData <- mutate(MergedData, participant_id = SubID)
MergedData <- MergedData[, c(67, 1:66)] #Move the Participant column to the first column
Partdata <- group_by(MergedData, participant_id)
#Estimate mean for each study participant
MeanbyPart <- summarise_each(Partdata, funs(mean))
```
The final step is to create the data files to be uploaded in this repo. Because I am not interested in saving the row names, I wrote row.names=FALSE.
```
#Write both datasets in two csv files
write.csv(MergedData, "mergeddata.csv",row.names=FALSE)
write.csv(MeanbyPart, "meanbyparticipant.csv",row.names=FALSE)
```

Cheers!

[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

