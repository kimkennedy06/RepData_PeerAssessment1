Reproducible Research: Peer Assessment 1
========================================


## Loading and Preprocessing the Data
After downloading the **Activity monitoring data** from the course website, we will have a zip called **repdata-data-activity.zip**. After the download is complete, move and extract the .zip file to your working directory for R. Once you have done that we can start loading and preprocessing the data.  

In order to load the data **activity.csv**, I am going to read in the data using read.csv(). Once I have read the data into a variable called `data`, I am going to convert the `date` factor of `data` into a Date-time class.

```r
data <- read.csv("activity.csv")
data$date <- strptime(data$date, "%Y-%m-%d")
```




## What is mean total number of steps taken per day?
I am going to summarize `data` by `date` and keep track of the total number of steps per day.

```r
library(plyr)
data.summary <- ddply(data, ~date, summarize, total = sum(steps, na.rm = TRUE))
```

Below is the code and the histogram to display the total number of steps taken per day.

```r
with(data.summary, hist(total, breaks = 10, xlab = "Total Number of Steps", 
    main = "Histogram of Total Number of Steps"))
```

![plot of chunk steps.histogram](figure/steps_histogram.png) 

Now that we have a rough idea  of the number of steps taken per day visually, I am going to calculate the mean and median total number of steps taken per day.  

**Mean total number of steps**  
Here is the mean total number of steps taken per day:

```r
mean.total.steps <- mean(data.summary$total)
print(mean.total.steps)
```

```
## [1] 9354
```


**Median total number of steps**  
Here is the median total number of steps taken per day:

```r
median.total.steps <- median(data.summary$total)
print(median.total.steps)
```

```
## [1] 10395
```


## What is the average daily activity pattern?
I am going to summarize `data` by `interval` and keep track of the average number of steps per interval.

```r
library(plyr)
data.summary2 <- ddply(data, ~interval, summarize, avg = mean(steps, na.rm = TRUE))
```

Next, we will plot 5-minute interval versus the average number of steps taken, averaged across all days.

```r
with(data.summary2, plot(interval, avg, xlab = "Interval", ylab = "Average Number of Steps", 
    type = "l", main = "Average Number of Steps across all Days by Interval"))
```

![plot of chunk plot.interval.avgsteps](figure/plot_interval_avgsteps.png) 

The 5-minute interval, on average across all the days in the dataset, that contains the maximum number of steps can be found with the following code:

```r
max.avgSteps.interval <- data.summary2[which.max(data.summary2$avg), 1]
```

Therefore the 5-minute interval with the maximum number of steps is 835.

## Inputing missing values



## Are there differences in activity patterns between weekdays and weekends?
