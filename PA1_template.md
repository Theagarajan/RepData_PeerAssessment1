# Reproducible Research: Peer Assessment 1


## Loading and preprocessing the data

```r
Activity <- read.csv('activity.csv', header= TRUE)  
```


## What is mean total number of steps taken per day?


```r
totalstepsbydays <- aggregate(Activity$steps ~ Activity$date,data = Activity, FUN= sum) 
totalstepsbydays[,2] <- as.numeric(totalstepsbydays[,2])
hist(totalstepsbydays[,2],main="Histogram of Steps", xlab='Steps')
```

![plot of chunk unnamed-chunk-2](figure/unnamed-chunk-2.png) 

### Mean Steps per day

```r
meanstepsbydays <- aggregate(Activity$steps ~ Activity$date,data = Activity, FUN= mean, is.na=TRUE)
mean(meanstepsbydays[,2])
```

```
## [1] 37.38
```

### Median Steps per day

```r
medianstepsbydays <- aggregate(Activity$steps ~ Activity$date,data = Activity, FUN= median)
median(medianstepsbydays[,2])
```

```
## [1] 0
```

## What is the average daily activity pattern?

```r
meanstepsbyinterval <- aggregate(Activity$steps ~ Activity$interval,data = Activity, FUN= mean, is.na=TRUE) 
plot(meanstepsbyinterval[,2]~meanstepsbyinterval[,1], type ="l", xlab='Interval',ylab='Steps')
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5.png) 
### Interval that has the highest average steps.

```r
meanstepsbyinterval[which.max(meanstepsbyinterval[,2]),1]
```

```
## [1] 835
```
## Imputing missing values

### Total number of missing values


```r
countna <- is.na(Activity$steps)
sum(countna)
```

```
## [1] 2304
```
### Imputing NA with Mean of the interval.

```r
Activitycomplete <- Activity
for(i in 1:NROW(Activity))
{
  if(is.na(Activity[i,1]))
  {
  Activitycomplete[i,1] <- meanstepsbyinterval[meanstepsbyinterval[,1]==Activity[i,3],2]
  }
}
```

### Histogram of Steps after Imputing NA Values.


```r
Ctotalstepsbydays <- aggregate(Activitycomplete$steps ~ Activitycomplete$date,data = Activitycomplete, FUN= sum) 
Ctotalstepsbydays[,2] <- as.numeric(Ctotalstepsbydays[,2])
hist(Ctotalstepsbydays[,2],xlab='Steps',main='Histogram of Steps')
```

![plot of chunk unnamed-chunk-9](figure/unnamed-chunk-9.png) 
### Mean Steps per day

```r
Cmeanstepsbydays <- aggregate(Activitycomplete$steps ~ Activitycomplete$date,data = Activitycomplete, FUN= mean) 
mean(Cmeanstepsbydays[,2])
```

```
## [1] 37.38
```
### Median Steps per day

```r
Cmedianstepsbydays <- aggregate(Activitycomplete$steps ~ Activitycomplete$date,data = Activitycomplete, FUN= median)
median(Cmedianstepsbydays[,2])
```

```
## [1] 0
```
### Difference between the mean of Imputed Dataset and Normal Dataset

```r
mean(meanstepsbydays[,2],na.rm=TRUE) - mean(Cmeanstepsbydays[,2],na.rm=TRUE)
```

```
## [1] 0
```

### Difference between the total steps of Imputed Dataset and Normal Dataset

```r
sum(Activitycomplete$steps,na.rm=TRUE)-sum(Activity$steps,na.rm=TRUE)
```

```
## [1] 86130
```
## Are there differences in activity patterns between weekdays and weekends?

```r
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
library(lattice)
xyplot(meanbydaybyinterval[,3] ~ meanbydaybyinterval[,1]|meanbydaybyinterval[,2], type ='l',xlab='Interval',ylab='Steps')
```

![plot of chunk unnamed-chunk-14](figure/unnamed-chunk-14.png) 
