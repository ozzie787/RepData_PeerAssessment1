---
title: "Reproducible Research: Peer Assessment 1"
output: 
  html_document:
    keep_md: true
    toc : true
---


## Loading and preprocessing the data

### The Data

The data was accessed from:  
**URL:** <https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip>  
**Access Time:** 2018-08-07 20:14:20 BST.  
**Data Format:** ZIP Archive

### Download of, and Extracting the Raw Data

If the data is not downloaded, the data is downloaded from the above URL. If the data has been previously downloaded and named as data.zip, the script prints a message and moves onto the next step.


```r
if (!file.exists("data.zip")) {
     message("Downloading data from 'https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip'... Please wait")
     download.file(url = "https://d396qusza40orc.cloudfront.net/repdata%2Fdata%2Factivity.zip", destfile = "data.zip", method = "curl")
} else {
     message("Data already downloaded as data.zip")
}
```

If the data is not extracted, the data is extracted from data.zip. If the data has been previously extracted to to "UCI HAR Dataset", the script prints a message and moves onto the next step.


```r
if (file.exists("data.zip") & !file.exists("activity.csv")) {
     message("Extracting data to './activity.csv'... Please wait")
     unzip("data.zip")
} else {
     message("Data already extracted to ./activity.csv")
}
```

### Preprocessing the data

The following libraries and versions, were used in the preprocessing and manipulation of the data

| Package    | Version |
|------------|:--------|
| data.table | 1.11.4  |
| dplyr      | 0.7.6   |
| tidyr      | 0.8.1   |
| lubridate  | 1.7.4   |

If installed, they can be loaded as follows:


```r
library(data.table)
library(dplyr)
library(tidyr)
library(lubridate)
```

The data was read into a data.frame, `data` using `read.csv()`.


```r
data <- read.csv("activity.csv")
```

The data.frame `data` contained three columns as shown by the `names()` function: "steps"; "date"; and "interval". 


```r
names(data)
```

```
## [1] "steps"    "date"     "interval"
```
The variables included in this dataset are:

* `steps`: *\<integer\>* Number of steps taking in a 5-minute interval (missing
    values are coded as `NA`).

* `date`: *\<factor\>* The date on which the measurement was taken in YYYY-MM-DD
    format.

* `interval`: *\<integer\>* Identifier for the 5-minute interval in which
    measurement was taken. Note, values for this variable of 345, 2125 and 0 would correspond to times (in HH:MM format) of 03:45, 21:25 and 00:00 (midnight) respectively.

As mapping trends relating to specific days and/or parts of the week would be of interest, two character vectors were defined to easily map the result of inputting date data into the lubridate function `wday()` to the desired result (i.e. the day of the week or if day of the week is weekday/weekend).


```r
week_day <- c("Sunday","Monday","Tuesday","Wednesday","Thursday","Friday","Saturday")
week_end <- c("weekend","weekday","weekday","weekday","weekday","weekday","weekend")
```

* `week_day`: *\<character\>* Character vector of the days of the week where `week_day[1]` maps to Sunday to be consistant with the output of the lubridate function `wday()`.

* `week_end`: *\<character\>* Character vector based upon the days of the week where `week_end[1]` maps to the result for a Sunday to be consistant with the output of the lubridate function `wday()`. In this variable, Saturday and Sunday are classified as weekend days (`weekend`), while Monday, Tuesday, Wednesday, Thursday and Friday are classified as week days (`weekday`).

In order to be able to calculate satistics based upon day of the week and part of the week the columns `week.day` and `week.end` were added to the original data set, `data` by using the `mutate()` function. A random sample of rows from the mutated data.frame, `data` is also given to demonstrate the effect of the transformation and the current state of the data set.


```r
data <- mutate(data,
     week.day   = week_day[wday(date)],
     week.end   = week_end[wday(date)]
     )
data[sample(nrow(data), 10),]
```

```
##       steps       date interval  week.day week.end
## 11500    NA 2012-11-09     2215    Friday  weekday
## 14334     0 2012-11-19     1825    Monday  weekday
## 10925     0 2012-11-07     2220 Wednesday  weekday
## 11206     0 2012-11-08     2145  Thursday  weekday
## 16804     0 2012-11-28      815 Wednesday  weekday
## 10688     0 2012-11-07      235 Wednesday  weekday
## 16082     0 2012-11-25     2005    Sunday  weekend
## 12918    NA 2012-11-14     2025 Wednesday  weekday
## 13330     0 2012-11-16      645    Friday  weekday
## 5275      0 2012-10-19      730    Friday  weekday
```

## What is mean total number of steps taken per day?



## What is the average daily activity pattern?



## Imputing missing values



## Are there differences in activity patterns between weekdays and weekends?
