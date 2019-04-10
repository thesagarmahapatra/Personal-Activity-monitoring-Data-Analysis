**Loading and preprocessing the data**
======================================

    data<-read.csv("activity.csv")
    head(data)

    ##   steps       date interval
    ## 1    NA 2012-10-01        0
    ## 2    NA 2012-10-01        5
    ## 3    NA 2012-10-01       10
    ## 4    NA 2012-10-01       15
    ## 5    NA 2012-10-01       20
    ## 6    NA 2012-10-01       25

**Total number of steps taken per day**
=======================================

    subset_data=aggregate(data$steps~data$date,FUN=sum)
    barplot(subset_data$`data$steps`,names.arg = subset_data$`data$date`,col=subset_data$`data$date`,main="Total number of steps taken per day",xlab="date",ylab="steps")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-2-1.png)

### Mean of the total number of steps taken per day

    mean(subset_data$`data$steps`,na.rm = T)

    ## [1] 10766.19

### Median of the total number of steps taken per day

    median(subset_data$`data$steps`,na.rm = T)

    ## [1] 10765

**Average daily activity pattern**
==================================

    subset_data=aggregate(data$steps~data$interval,FUN=mean)
    plot(subset_data$`data$interval`,subset_data$`data$steps`,type="l",xlab="Time  interval",ylab="Average steps",col="red",main="Average daily activity pattern")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-5-1.png)

### **5-minute interval containing the maximum number of steps**

    subset_data$`data$interval`[subset_data$`data$steps`==max(subset_data$`data$steps`)]

    ## [1] 835

**Inputing missing values**
===========================

    sum(is.na(data$steps))

    ## [1] 2304

### Strategy for filling in all of the missing values

    *Fill in the NA values with the mean of the steps in that particular day.*

### New dataset with the missing data filled.

    data1<-data  # new dataset
    x<-is.na(data1$steps)
    subset_data=aggregate(data1$steps~data1$interval,FUN=mean)
    names(subset_data)=c("interval","steps")
    mynewdata=merge(data1,subset_data,by="interval")
    data1$steps[x]=mynewdata$steps.y[x]
    sum(is.na(data1$steps))

    ## [1] 0

### Total number of steps taken per day

    subset_data_new=aggregate(data1$steps~data1$date,FUN=sum)
    barplot(subset_data_new$`data1$steps`,names.arg=subset_data_new$`data1$date`,col=subset_data_new$`data1$date`,xlab="Date",ylab="Steps",main="Steps taken per day")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-9-1.png)

### NEW MEAN

    subset_data_new=aggregate(data1$steps~data1$date,FUN=sum)
    mean(subset_data_new$`data1$steps`,na.rm=T)

    ## [1] 10889.8

### NEW MEDIAN

    subset_data_new=aggregate(data1$steps~data1$date,FUN=sum)
    median(subset_data_new$`data1$steps`,na.rm=T)

    ## [1] 11015

The impact of the missing data seems rather low, at least when
estimating the total number of steps per day.

**Differences in activity patterns between weekdays and weekends**
==================================================================

    type_of_day <- function(date) {
        if (weekdays(as.Date(date)) %in% c("Saturday", "Sunday")) {
            0
        } else {
            1
        }
    }
    data1$typeday <- as.factor(sapply(data1$date, type_of_day))
    head(data1)

    ##      steps       date interval typeday
    ## 1 1.716981 2012-10-01        0       1
    ## 2 1.716981 2012-10-01        5       1
    ## 3 1.716981 2012-10-01       10       1
    ## 4 1.716981 2012-10-01       15       1
    ## 5 1.716981 2012-10-01       20       1
    ## 6 1.716981 2012-10-01       25       1

HERE 1 refers to weekday and 0 refers to weekend

### Average number of steps taken

    par(mfrow=c(1,2))
    newdata<-aggregate(steps~interval,data=data1,subset=(data1$typeday==1),FUN=mean)
    plot(newdata$interval,newdata$steps,type="l",xlab="interval",ylab="steps",main="Weekdays")
    newdata<-aggregate(steps~interval,data=data1,subset=(data1$typeday==0),FUN=mean)
    plot(newdata$interval,newdata$steps,type="l",xlab="interval",ylab="steps",main="Weekends")

![](PA1_template_files/figure-markdown_strict/unnamed-chunk-13-1.png)
