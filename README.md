# G-CDProject
Getting and Cleaning Data Course Project 

##Load Activity Labels File
aLabels<-read.csv("UCI HAR Dataset/activity_labels.txt",sep="",header=FALSE)

##Load and clean up names of the features.txt file using standard camel case

fea<-read.csv("UCI HAR Dataset/features.txt",sep="",header=FALSE)
fea[,2]=gsub("mean","Mean", fea[,2])
fea[,2]=gsub("std","Std", fea[,2])
fea[,2]=gsub("[-()]","", fea[,2])

##Pull in X_train.txt file and store to variable
tr<-read.csv("UCI HAR Dataset/train/X_train.txt", sep="", header = FALSE)

##Load the other columns(Activity and Subject) and then add them using cbind

tryt<-read.csv("UCI HAR DataSet/train/Y_train.txt", sep="",header=FALSE)
trst<-read.csv("UCI HAR DataSet/train/subject_train.txt", sep="", header=FALSE)

tr<-cbind(tr,tryt)
tr<-cbind(tr,trst)

##Pull in X_test.txt and store it to a variable
te<-read.csv("UCI HAR Dataset/test/X_test.txt",sep="", header=FALSE)

##Load the other columns(Activity and Subject) and then add them using cbind
teyt<-read.csv("UCI HAR Dataset/test/Y_test.txt",sep="", header=FALSE)
test<-read.csv("UCI HAR Dataset/test/subject_test.txt",sep="",header=FALSE)

te<-cbind(te,teyt)
te<-cbind(te,test)

##Merge the data
m<-rbind(tr,te)

##Create a variable to hold the columns that we want to keep.  

cols<-grep( "Mean|Std", fea[,2])


##Apply cols to fea to trim it down to the objective columns

fea<-fea[cols,]

##Add in the subject and activity columns

cols<-c(cols,562,563)

##Now apply cols to m to remove columns we don't want

m<-m[,cols]

##Add Column names using the fea variable as the names

colnames(m)<-c(fea$V2, "Activity", "Subject")

## Apply aggregate function which splits the data into subsets, 
##computers summary stats for each and returns in a conveient form

tidydata<-aggregate(m,by=list(activity= m$Activity, subject=m$Subject), mean)

##remove Repeated Acitivity and Subject columns
tidydata[,90]<-NULL
tidydata[,89]<-NULL

##write the data applying the row.name = FALSE as directed

write.table(tidydata, "tidydata.txt", sep="\t", row.name=FALSE)
