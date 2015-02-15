# Reproducible Research: Peer Assessment 1

## Loading and preprocessing the data


```r
unzip("activity.zip")
data <- read.csv("activity.csv")
data[,2] <- as.Date(data[,2], format = "%Y-%m-%d")
```


## What is mean total number of steps taken per day?


```r
totalstep <- aggregate(steps ~ date, data=data, sum)
library(ggplot2)
qplot(steps, data=totalstep, geom = "histogram", binwidth = 300)
```

![](PA1_template_files/figure-html/unnamed-chunk-2-1.png) 

```r
mean(totalstep$steps)
```

```
## [1] 10766.19
```

```r
median(totalstep$steps)
```

```
## [1] 10765
```


## What is the average daily activity pattern?


```r
meanstep <- aggregate(steps ~ interval, data=data, mean)
plot(meanstep, type = "l")
```

![](PA1_template_files/figure-html/unnamed-chunk-3-1.png) 

## Imputing missing values


```r
sum(is.na(data))
```

```
## [1] 2304
```

```r
temp <- merge(data,meanstep, by = "interval")
data2 <- data
data2$steps[is.na(data2$steps)] <- temp$steps.y[is.na(data2$steps)]
rm(temp)

totalstep2 <- aggregate(steps ~ date, data=data2, sum)
qplot(steps, data=totalstep2, geom = "histogram", binwidth = 300)
```

![](PA1_template_files/figure-html/unnamed-chunk-4-1.png) 

```r
mean(totalstep2$steps)
```

```
## [1] 10889.8
```

```r
median(totalstep2$steps)
```

```
## [1] 11015
```

We observe that both values differ from the original computation.

Imputing missing data increase both mean and median.

## Are there differences in activity patterns between weekdays and weekends?


