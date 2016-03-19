## Data variables

This is codebook is a modified version from the original file features_info.txt from Anguita et al. [1]. For the sake of consistency, variable names were kept as in the original dataset to avoid confusion by the processing steps. The run_analysis.R file creates two datasets:

* mergeddata.csv - This file is the merged version of the feature train and test datasets in the original files, with variable names and particpant IDs included. Only means and standard deviations were chosen to comprise this file.

* meanbyparticipant.csv - This file is the mean per participant and per activity for all the features as specified in the project guidelines, same variable names as mergeddata.csv to keep consistency. 

Activities are names as LAYING, SITTING, STANDING, WALKING, WALKING_DOWNSTAIRS, WALKING_UPSTAIRS

For more information about the variables, refer to feature_info.txt original file. Here I reproduce the bits that explain the name of each of the variables, as these were coded.

From the original feature_info.txt: The features selected for this database come from the accelerometer and gyroscope 3-axial raw signals tAcc-XYZ and tGyro-XYZ. These time domain signals (prefix 't' to denote time) were captured at a constant rate of 50 Hz. Then they were filtered using a median filter and a 3rd order low pass Butterworth filter with a corner frequency of 20 Hz to remove noise. Similarly, the acceleration signal was then separated into body and gravity acceleration signals (tBodyAcc-XYZ and tGravityAcc-XYZ) using another low pass Butterworth filter with a corner frequency of 0.3 Hz. 

Subsequently, the body linear acceleration and angular velocity were derived in time to obtain Jerk signals (tBodyAccJerk-XYZ and tBodyGyroJerk-XYZ). Also the magnitude of these three-dimensional signals were calculated using the Euclidean norm (tBodyAccMag, tGravityAccMag, tBodyAccJerkMag, tBodyGyroMag, tBodyGyroJerkMag). 

Finally a Fast Fourier Transform (FFT) was applied to some of these signals producing fBodyAcc-XYZ, fBodyAccJerk-XYZ, fBodyGyro-XYZ, fBodyAccJerkMag, fBodyGyroMag, fBodyGyroJerkMag. (Note the 'f' to indicate frequency domain signals). 

These signals were used to estimate variables of the feature vector for each pattern:  
'-XYZ' is used to denote 3-axial signals in the X, Y and Z directions.

- tBodyAcc-XYZ
- tGravityAcc-XYZ
- tBodyAccJerk-XYZ
- tBodyGyro-XYZ
- tBodyGyroJerk-XYZ
- tBodyAccMag
- tGravityAccMag
- tBodyAccJerkMag
- tBodyGyroMag
- tBodyGyroJerkMag
- fBodyAcc-XYZ
- fBodyAccJerk-XYZ
- fBodyGyro-XYZ
- fBodyAccMag
- fBodyAccJerkMag
- fBodyGyroMag
- fBodyGyroJerkMag

The previous variables make 33 activities whose means and standard deviations were estimated giving a total of 66 columns in my tiddy datasets; In the mergeddata.csv and meanbyparticipant.csv datasets, the suffices -mean and -std were left to indicate mean and standard deviation, and simlarly the suffices -XYZ indicate the accelerometer directions. For instance

"tbodyacc-mean-x" : from tBodyAcc variable in the X axis mean value       
      
[1] Davide Anguita, Alessandro Ghio, Luca Oneto, Xavier Parra and Jorge L. Reyes-Ortiz. Human Activity Recognition on Smartphones using a Multiclass Hardware-Friendly Support Vector Machine. International Workshop of Ambient Assisted Living (IWAAL 2012). Vitoria-Gasteiz, Spain. Dec 2012



