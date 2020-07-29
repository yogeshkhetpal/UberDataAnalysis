---
title: "Portfolio 2"
author: "Yogesh Kumar"
date: "20 May 2018"
output: 
  html_document:
    keep_md: true
---

## install the required packages and load the required libraries

```r
library(readr)
library(ggmap)
```

```
## Loading required package: ggplot2
```

```
## Google's Terms of Service: https://cloud.google.com/maps-platform/terms/.
```

```
## Please cite ggmap if you use it! See citation("ggmap") for details.
```

```r
library(mapproj)
```

```
## Loading required package: maps
```
## Loading data using read_csv command

```r
apr <- read_csv("uber-raw-data-apr14.csv")
```

```
## Parsed with column specification:
## cols(
##   `Date/Time` = col_character(),
##   Lat = col_double(),
##   Lon = col_double(),
##   Base = col_character()
## )
```

```r
may <- read_csv("uber-raw-data-may14.csv")
```

```
## Parsed with column specification:
## cols(
##   `Date/Time` = col_character(),
##   Lat = col_double(),
##   Lon = col_double(),
##   Base = col_character()
## )
```

```r
jun <- read_csv("uber-raw-data-jun14.csv")
```

```
## Parsed with column specification:
## cols(
##   `Date/Time` = col_character(),
##   Lat = col_double(),
##   Lon = col_double(),
##   Base = col_character()
## )
```

```r
jul <- read_csv("uber-raw-data-jul14.csv")
```

```
## Parsed with column specification:
## cols(
##   `Date/Time` = col_character(),
##   Lat = col_double(),
##   Lon = col_double(),
##   Base = col_character()
## )
```

```r
aug <- read_csv("uber-raw-data-aug14.csv")
```

```
## Parsed with column specification:
## cols(
##   `Date/Time` = col_character(),
##   Lat = col_double(),
##   Lon = col_double(),
##   Base = col_character()
## )
```

```r
sep <- read_csv("uber-raw-data-sep14.csv")
```

```
## Parsed with column specification:
## cols(
##   `Date/Time` = col_character(),
##   Lat = col_double(),
##   Lon = col_double(),
##   Base = col_character()
## )
```
## Merge the data using rbind 

```r
uberData <- rbind(apr, may, jun,jul,aug,sep)
```

## Data cleaning

```r
uberData <- uberData[!is.na(uberData$Lat),]
uberData <- uberData[!is.na(uberData$Lon),]
uberData <- uberData[uberData$Lat != 0,]
uberData <- uberData[uberData$Lon != 0,]
uberData <- uberData[!is.na(uberData$'Date/Time'),]
```


## fetching subsets of whole data(jun and july) and data data cleaning to proceed with data visualisation

```r
uberMinorData <- rbind( jun,jul)
uberMinorData <- uberMinorData[uberMinorData$Lat != 0,]
uberMinorData <- uberMinorData[uberMinorData$Lon != 0,]
uberMinorData <- uberMinorData[!is.na(uberMinorData$Lat),]
uberMinorData <- uberMinorData[!is.na(uberMinorData$Lat),]
uberMinorData <- uberMinorData[!is.na(uberMinorData$'Date/Time'),]
```
##Task 1.1
## calculating statistics from the dataset and visualising the data to show daily trends in pick ups.
## Task 1.1 
Day is extracted from Date/Time colum . The code involves explicit consideration of column value as Date/Time. The original csv is update with a new column known as week day. Generate Frequency of Days and Sort Data accordng to Days. Afterwars, plot the data with Day as x axis and Frequency on Y



```r
uberData$day <- weekdays(as.Date(uberData$'Date/Time',"%m/%d/%Y"))
```


```r
Day_count<-as.data.frame(table(uberData$day))
names(Day_count)<-c("Days","Freq")
ggplot(data=Day_count,aes(x=Days,y=Freq,fill=rownames(Day_count)))+geom_bar(stat="identity",colour = "black")+guides(fill=FALSE)+labs(title = "Frequency by Days ",x="Day",y="Frequency")+ geom_text(aes(label=Freq),vjust=1.6,color="white",size=4)+theme_minimal()
```

![](uberAnalysis_files/figure-html/Plot on frequency of pick up to the week day-1.png)<!-- -->
##Task 1.1 Statistics 
 step 1: Find data statistics and get minimum and maximum

```r
uberDataDay<-factor(uberData$day)
table(uberDataDay)
```

```
## uberDataDay
##    Friday    Monday  Saturday    Sunday  Thursday   Tuesday Wednesday 
##    741139    541472    646114    490180    755145    663789    696488
```

```r
min(table(uberDataDay))
```

```
## [1] 490180
```

```r
max(table(uberDataDay))
```

```
## [1] 755145
```
##Task 1.1 Summary
The visualization and statistics above demonstrate the following result. Sunday sees the Lowest with a count of 490180. While, Thursday sees the Highest number of Uber Pickups with 755145 and 

##Task 1.2
## Task 1.2 Plot
### Task 1.2 Steps
Month is extracted from Date/Time colum . The code invlves explicit consideration of column value as Date/Time. The original csv is update with a new column knows as month.Generate Frequency of Month and Sort Data accordng to Month. Afterwards, Plot bargraph with Month on x-axis and Frequency on Y-Axis for Visualization


```r
uberData$month <- months(as.Date(uberData$'Date/Time',"%m/%d/%Y"))
```


```r
Month_count<-as.data.frame(table(uberData$month))
names(Month_count)<-c("Month","Freq")
ggplot(data=Month_count,aes(x=Month,y=Freq,fill=rownames(Month_count)))+geom_bar(stat="identity",colour = "black")+guides(fill=FALSE)+labs(title = "Frequency by Month ",x="Month of year ",y="Frequency")+ geom_text(aes(label=Freq),vjust=1.6,color="white",size=4)+theme_minimal()
```

![](uberAnalysis_files/figure-html/Plot on frequency vs Month data-1.png)<!-- -->
##Task 1.2 Statistics 
 step 1: Find data statistics and get minimum and maximum

```r
uberDataMonth<-factor(uberData$month)
table(uberDataMonth)
```

```
## uberDataMonth
##     April    August      July      June       May September 
##    564516    829275    796121    663844    652435   1028136
```

```r
min(table(uberDataMonth))
```

```
## [1] 564516
```

```r
max(table(uberDataMonth))
```

```
## [1] 1028136
```
##Task 1.2 Summary
The visualization and statistics above demonstrate the following result. April sees the lowest with a count of 564516
September sees the highest number of Uber Pickups with 1028136.

##Task 1.3
## Task 1.3 Plot
### Task 1.3 Steps
Hour is extracted from Date/Time colum . The code invlves explicit consideration of column value as Date/Time. The original csv is update with a new column knows as Hour. Generate Frequency of Hour and then Sort Data accordng to Hour. Afterwards, we p
lot bargraph with Hour on x-axis and Frequency on Y-Axis for Visualization


```r
uberData$hour <- format(strptime(uberData$'Date/Time',format = '%m/%d/%Y %H:%M:%S'), "%H")
```



```r
Hour_count<-as.data.frame(table(uberData$hour))
names(Hour_count)<-c("Hour","Freq")
ggplot(data=Hour_count,aes(x=Hour,y=Freq,fill=rownames(Hour_count)))+geom_bar(stat="identity",colour = "black")+guides(fill=FALSE)+labs(title = "Frequency by Hour ",x="Hour of day ",y="Frequency")+theme_minimal()
```

![](uberAnalysis_files/figure-html/Plot on frequency vs Hour-1.png)<!-- -->
##Task 1.3 Statistics 
 step 1: Find data statistics and get minimum and maximum
 

```r
uberDataHour<-factor(uberData$hour)
table(uberDataHour)
```

```
## uberDataHour
##     00     01     02     03     04     05     06     07     08     09 
## 103836  67227  45865  48287  55230  83939 143213 193094 190504 159967 
##     10     11     12     13     14     15     16     17     18     19 
## 159148 165703 170452 195877 230625 275466 313400 336190 324679 294513 
##     20     21     22     23 
## 284604 281460 241858 169190
```

```r
min(table(uberDataHour))
```

```
## [1] 45865
```

```r
max(table(uberDataHour))
```

```
## [1] 336190
```
##Task 1.3 Summary
The visualization and statistics above demonstrate the following result
**17 hours** sees the highest number of Uber Pickups with 336190 and **02 Hours** sees the lowest with a count of 45865

## Task 2.0
### Steps
First, Obtain the New York map using get_map API and plot density map of data accroding to Latitude vs Longitude of uber pick ups. Afterwards, For better visulaization the points need to be set to size 2. Bin the points and drop the bins which dont have any samples in them


```r
register_google(key = "AIzaSyCD2-pP0yfM8aC8sjCB4g--CaCcExP6NhU")
densityPlot <- get_map("New York", zoom = 12,color="bw")
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=New%20York&zoom=12&size=640x640&scale=2&maptype=terrain&language=en-EN&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=New+York&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```r
ggmap(densityPlot)+stat_density2d(aes(x = uberMinorData$Lon, y = uberMinorData$Lat,fill = ..level..),alpha=0.5,size=2,bins=30,geom="polygon",data=uberMinorData)+geom_density_2d(data=uberMinorData,aes(x = uberMinorData$Lon,y = uberMinorData$Lat),alpha=0.5,bins=30)+
  labs(title = "Uber Pickup Density By Location\n", x = "\nLongitude", y = "Latitude\n")
```

```
## Warning: Removed 158023 rows containing non-finite values (stat_density2d).

## Warning: Removed 158023 rows containing non-finite values (stat_density2d).
```

![](uberAnalysis_files/figure-html/Task2.0 Density Map on minor data-1.png)<!-- -->


### Steps
Obtain the New York map using get_map API and then, plot point data wrt Latitude and Longitude



```r
pointMap <- get_map("New York", zoom = 10)
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=New%20York&zoom=10&size=640x640&scale=2&maptype=terrain&language=en-EN&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=New+York&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```r
ggmap(pointMap)+geom_point(data=uberMinorData,aes(x = uberMinorData$Lon, y = uberMinorData$Lat,colour='Red'))+ scale_color_manual(values = c("Red"="red")) +  labs(title = "Uber Pickups By Location\n", x = "\nLongitude", y = "Latitude\n" ,colour="Uber Pickup")
```

```
## Warning: Removed 3277 rows containing missing values (geom_point).
```

![](uberAnalysis_files/figure-html/Task2.0 Point Map on minor data-1.png)<!-- -->

###Summary

The above graphs(density and point plot) shows where Uber Pickups occur in NYC according to the geospatial data.

#Task 2.1

##Steps
Extract Day from DateTime column and store it as another column 

```r
uberMinorData$day <- weekdays(as.Date(uberMinorData$'Date/Time',"%m/%d/%Y"))
```
#Task 2.1 Points plot
Download map for New York using get_map and set it to ny using getmap adn add points to the map using Lat and Lon data categorizing the color according to Day

```r
nymap <- get_map("New York", zoom = 10)
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=New%20York&zoom=10&size=640x640&scale=2&maptype=terrain&language=en-EN&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=New+York&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```r
ny <- ggmap(nymap)
ny + geom_point(aes(x = Lon, y = Lat, colour = uberMinorData$day), data = uberMinorData)+labs(title = "Uber Pickups By Day on Location\n", x = "\nLongitude", y = "Latitude\n",colour="Day")
```

```
## Warning: Removed 3277 rows containing missing values (geom_point).
```

![](uberAnalysis_files/figure-html/Task2.1 point plot minor data on Day vs frequency-1.png)<!-- -->


#Task 2.2
Extract Month from DateTime column and store it as another column 

```r
uberMinorData$month<- months(as.Date(uberMinorData$'Date/Time',"%m/%d/%Y"))
```
#Task 2.2 Points plot
Download map for New York using get_map and set it to ny using getmap and add points to the map using Lat and Lon data categorizing the color according to Month

```r
nymap <- get_map("New York", zoom = 10)
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=New%20York&zoom=10&size=640x640&scale=2&maptype=terrain&language=en-EN&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=New+York&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```r
ny <- ggmap(nymap)
ny + geom_point(aes(x = Lon, y = Lat, colour = uberMinorData$month), data = uberMinorData)+labs(title = "Uber Pickups By Month on Location\n", x = "\nLongitude", y = "Latitude\n",colour="Red")
```

```
## Warning: Removed 3277 rows containing missing values (geom_point).
```

![](uberAnalysis_files/figure-html/Task 2.2 point plot minor data on Month vs frequency-1.png)<!-- -->

#Task 2.3

Extract Hour from DateTime column and store it as another column 

```r
uberMinorData$hour <- format(strptime(uberMinorData$'Date/Time',format = '%m/%d/%Y %H:%M:%S'), "%H")
```
##Steps
Download map for New York using get_map and set it to ny using getmap and add points to the map using Lat and Lon data categorizing the color according to Hour
#Task 2.3 Points plot

```r
nymap <- get_map("New York", zoom = 10)
```

```
## Source : https://maps.googleapis.com/maps/api/staticmap?center=New%20York&zoom=10&size=640x640&scale=2&maptype=terrain&language=en-EN&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```
## Source : https://maps.googleapis.com/maps/api/geocode/json?address=New+York&key=xxx-pP0yfM8aC8sjCB4g--CaCcExP6NhU
```

```r
ny <- ggmap(nymap)
ny + geom_point(aes(x = Lon, y = Lat, colour = uberMinorData$hour), data = uberMinorData)+labs(title = "Uber Pickups By Hour on Location\n", x = "\nLongitude", y = "Latitude\n",colour="Hour")
```

```
## Warning: Removed 3277 rows containing missing values (geom_point).
```

![](uberAnalysis_files/figure-html/Task 2.3 point plot minor data on Hour vs frequency-1.png)<!-- -->
<img src="uberAnalysis_files/figure-html/Task 2.3 point plot minor data on Hour vs frequency-1.png">
