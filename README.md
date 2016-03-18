## Human Activity Recognition Using Smartphones Dataset - Week 4 project
### Getting and Cleaning data in R

The dataset used in this project was first published by Anguita et al. (2012) [1] and can be downloaded in original format from [UCI Machine Learning Repository](http://archive.ics.uci.edu/ml/datasets/Human+Activity+Recognition+Using+Smartphones). 

This repository contains 1) the CodeBook.md file describing the R code for the analysis of these datasets, 2) the tiddy datasets for the merged files (test and train datasets) and the averaged results per participant, and 3) the run_analysis.R file that perform all needed transformations for this project.

The R file and code runs as follows:

```
#run_analysis.R week 4 project
library(dplyr) 
rm(list = ls()) #remove any previous element in the workspace

#Load the feature/activity labels
Fnames <- readLines("features.txt")

#Get the means and stds only - I am ignoring all mean frequencies
meanstds <- grep("mean\\(|std\\(",Fnames)
indvec <- -rep(1,561) # a total of 561 variables (From Fnames)
indvec[meanstds] = 1 # Just read the variables we need

#Making tiddy names
Mynames <- tolower(Fnames[meanstds])
secondElement <- function(x){x[2]}
Mynames <- sapply(strsplit(Mynames, " "), secondElement)
#Get rid of the parenthesis
Mynames <- sub("\\(\\)", "", Mynames)
Mynames <- gsub("`","", Mynames)

#Read test and train data
testfile = "./test/X_test.txt"
Numlines = length(readLines(testfile))
testdata <- read.fwf(testfile, widths = rep(16,561)*indvec, sep = "", n = Numlines)
trainfile = "./train/X_train.txt"
Numlines = length(readLines(trainfile))
traindata <- read.fwf(trainfile, widths = rep(16,561)*indvec, sep = "", n = Numlines) 

#Merge test and train data in one matrix
MergedData <- rbind(testdata,traindata)
#rm(list = c("testdata","traindata")) #Remove the separated datasets
#Ad the column names for the dataset
colnames(MergedData) <- Mynames

#The Participant indices per test and train data
testPart <- readLines("./test/subject_test.txt")
trainPart <- readLines("./train/subject_train.txt")
SubID <- c(testPart, trainPart)

MergedData <- mutate(MergedData, participant_id = SubID)
MergedData <- MergedData[, c(67, 1:66)] #Move the Participant column to the first column
Partdata <- group_by(MergedData, participant_id)
#Estimate mean for each study participant
MeanbyPart <- summarise_each(Partdata, funs(mean))

#Write both datasets in two csv files
write.csv(MergedData, "mergeddata.csv",row.names=FALSE)
write.csv(MeanbyPart, "meanbyparticipant.csv",row.names=FALSE)
```




[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012

