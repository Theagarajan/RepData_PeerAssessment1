# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data
...{r}  
Activity <- read.csv('activity.csv', header= TRUE)  
...


## What is mean total number of steps taken per day?

...{r}  
totalstepsbydays <- aggregate(Activity$steps ~ Activity$date,data = Activity, FUN= sum) 
totalstepsbydays[,2] <- as.numeric(totalstepsbydays[,2])
hist(totalstepsbydays[,2])
meanstepsbydays <- aggregate(Activity$steps ~ Activity$date,data = Activity, FUN= mean) 
medianstepsbydays <- aggregate(Activity$steps ~ Activity$date,data = Activity, FUN= median)
...

## What is the average daily activity pattern?
...{r}
plot(meanstepsbyinterval[,2]~meanstepsbyinterval[,1], type ="l")
meanstepsbyinterval[which.max(meanstepsbyinterval[,2]),1]
...
## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
...{r}
Day<- weekdays(as.POSIXct(Activity[,2]))
Activity <-cbind(Activity,Day)
Dayfactor <- rep('Weekday',NROW(Activity))
Activity <-cbind(Activity,Dayfactor)
Activity$Day <- as.character(Activity$Day)
Activity$Dayfactor <- as.character(Activity$Dayfactor)
for(i in 1:NROW(Activity))
{
  
 if(Activity[i,4]=="Sunday")
 {
 
 Activity[i,5] ="Weekend"
 }
}
Activity$Day <- as.factor(Activity$Day)
Activity$Dayfactor <- as.factor(Activity$Dayfactor)
meanbydaybyinterval<- aggregate(Activity$steps ~ Activity$interval * Activity$Dayfactor , data =Activity, FUN=mean)
xyplot(meanbydaybyinterval[,3] ~ meanbydaybyinterval[,1]|meanbydaybyinterval[,2], type ='l')
...
