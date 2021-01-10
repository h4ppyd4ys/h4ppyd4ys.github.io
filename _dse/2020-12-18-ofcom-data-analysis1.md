---
title: "Data Science Foundations Using R Coursera Course"
date: '2020-12-18T15:34:30-04:00'
categories: dse
layout: single
classes: wide
author_profile: yes
read_time: yes
show_date: yes
---


## **Introduction**

Just completed the Data Science Foundations Using R Coursera Course with John Hopkins University. It was a steep learning curve as programming doesn't come easy for me at all, I'll admit, I nearly gave up on more than several occasions. Motivation to persevere came mostly from a lot of online help from countless kind souls, so if you have ever posted on a GitHub or Coursera forum, please accept my sincerest gratitude. After many hours of trial and error, I finally got to the part of the course, for which I had enrolled, asking questions and creating pictorial representations that would illuminate the wisdom lying dormant in the data, which I enjoyed very much. Excited to use these new skills I decided to practice with data obtained from the industry I know best and a topic that has been most prevalent in all our lives for 2020. I look forward to sharing with you my data science exploration as I continue to develop my capabilities in this space.   


##  **Title: OfCom Data Analysis: Impact Of Covid-19 On UK Mobile Call Volumes**

A graphical representation of OfCOM Telecommunications Market Data Update Q2 2020 with specific focus on the impact of covid-19 on UK Mobile Call Volumes. The primary objective of the exercise was to demonsrate newly acquired skills from both the DataScience and the AWS Cloud Fundamentals Coursera courses to re-represent text format data into graphical representations and host the output on a AWS S3 server. Source Data obtained from: https://www.ofcom.org.uk/research-and-data/telecoms-research/data-updates/telecommunications-market-data-update-q2-2020

##  **Loading and pre-processing the data**

###  Extract tables from pdf file


```r
setwd("~/RStudioScripts/h4ppyd4ys.github.io/_dse")
library(dplyr)
```

```
## 
## Attaching package: 'dplyr'
```

```
## The following objects are masked from 'package:stats':
## 
##     filter, lag
```

```
## The following objects are masked from 'package:base':
## 
##     intersect, setdiff, setequal, union
```

```r
library(tabulizer)
library(tidyverse)
```

```
## -- Attaching packages ---------------------------------------------------------- tidyverse 1.3.0 --
```

```
## v ggplot2 3.3.2     v purrr   0.3.4
## v tibble  3.0.4     v stringr 1.4.0
## v tidyr   1.1.2     v forcats 0.5.0
## v readr   1.4.0
```

```
## -- Conflicts ------------------------------------------------------------- tidyverse_conflicts() --
## x dplyr::filter() masks stats::filter()
## x dplyr::lag()    masks stats::lag()
```

```r
library(knitr)
library(reshape2)
```

```
## 
## Attaching package: 'reshape2'
```

```
## The following object is masked from 'package:tidyr':
## 
##     smiths
```

```r
out <- extract_tables("telecoms-data-update-q2-2020.pdf")
```

###  Call and message volumes by call type (billions of minutes/messages/PB)
####  Extract required table

```r
temp <- data.frame(out[[12]])
temp1 <- temp[27:31,]
```
####  Tidy Data

```r
temp2 <- temp1 %>%
  separate(X2, c("X2", "X2.1","X2.2"), sep =" ") %>%
  separate(X4, c("X4", "X4.1"), sep =" ")

temp3 <- temp2 %>%
        select(X1,X2,X2.1,X5) %>%
        rename("Period" = X1,
               "AllCalls" = X2,
               "UKFixedCalls" = X2.1,
               "Roaming" = X5)

temp3$AllCalls <- as.numeric(temp3$AllCalls)
temp3$UKFixedCalls <- as.numeric(temp3$UKFixedCalls)
temp3$Roaming <- as.character(temp3$Roaming)

temp3
```

```
##     Period AllCalls UKFixedCalls Roaming
## 27 2019 Q2    40.06         8.25    0.77
## 28 2019 Q3    39.64         8.32    1.00
## 29 2019 Q4    41.45         8.76    0.50
## 30 2020 Q1    44.88         9.73    0.51
## 31 2020 Q2    50.05        11.38    0.33
```

##  Results

###  1. The number of mobile-originated voice calls minutes increased by 10.0 billion (24.9%) to 50 billion minutes in Q2 2020

![plot of chunk unnamed-chunk-4](figure/unnamed-chunk-4-1.png)


###  2. Calls to landlines increasing by 37.9% to 11.4 billion minutes


```r
ggplot(data = temp3)+
  aes(x=Period,y=UKFixedCalls, group=1)+
  geom_line(col="green")+
  labs(title= "The Number Of Call To Landlines")+
  ylab("Call Volumes (billion of minutes)")+
  theme_bw()
```

![plot of chunk unnamed-chunk-5](figure/unnamed-chunk-5-1.png)


###  3. Roaming call volumes decreased by 57.1% to 0.3 billion minutes compared to a year previously, again likely due to Covid-related travel restrictions.


```r
ggplot(data = temp3)+
  aes(x=Period,y=Roaming, group=1)+
  geom_line(col="red")+
  labs(title= "Roaming Call Volumes")+
  ylab("Call Volumes (billion of minutes)")+
  theme_bw()
```

![plot of chunk unnamed-chunk-6](figure/unnamed-chunk-6-1.png)

###  Summary: The number of mobile-originated voice calls minutes increased by 10.0 billion (24.9%) to 50 billion minutes in Q2 2020, with calls to landlines increasing by 37.9% to 11.4 billion minutes. These increases are likely due to the Covid-19 restrictions, which began in late March 2020.Roaming call volumes decreased by 57.1% to 0.3 billion minutes compared to a year previously, again likely due to Covid-related travel restrictions.


