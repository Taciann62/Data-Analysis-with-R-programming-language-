# Cyclistic-Bike-Share-Data-Analysis
---
## Table Of Content
- [Introduction](#introduction)

- [Project Overview](#project-overview)
  
- [Business Question](business-question)

- [Data Source](data-source)

- [Tools](tools)

- [Prepare](prepare)

   - [Data preparation and Observation](data-preparation-and-observation)
  
- [Process](process)
  
   - [Data Cleaning and Manipulation](data-cleaning-and-manipulation)

- [Analyze](analyze)

   - [Exploratory Data Analysis](exploratory-data-analysis)

- [Further Analysis With Charts](further-analysis-with-charts)

- [Findings](findings)

- [Recommendations](recommendations)


# Introduction
This is a data analytics capstone project is carried out in fulfillment of the requirement of the Google data analytics professional certificate course, Course 8, module 2. 

# Project overview
Cyclistic is a bike-share company that enables people to rent bikes for their mobility needs and return them at various locations.
Moreno, the finance analyst of the Cyclistic aims to maximize the number of annual members which she believes would impact future growth, by converting casual bike users to Members. 

Rather than creating a marketing campaign that targets all-new customers, Moreno believes there is a solid opportunity to convert casual riders into members. She also notes that casual riders are already aware of the Cyclistic program and have chosen Cyclistic for their mobility needs.

Note: Members are riders with annual subscription, while casual riders use a single-ride or full day passes.

### My role:
In this project, I assumed the role of a junior data analyst in the marketing team to uncover insights from their data, necessary to influence the company’s objective and offer recommendations and optimize work process. 
Business objective:
Design an effective marketing strategy targeted at converting causal riders to annual members.

## Business Question:
The Cyclistic bike share marketing needs to understand the following in order to carryout an effective marketing program in future:
1.	How do annual members and casual riders use Cyclistic bikes differently?
2.	Why would casual riders buy Cyclistic annual memberships?
3.	How can Cyclistic use digital media to influence casual riders to become members?


## Data Source
The dataset of the previous 12 months (January 2023- December 2023) was accessible and named contained in an index file call divvy trip data index file, from which I downloaded and extracted the datasets as csv file.

## Tools 
- Rstudio : Data Cleaning, manipulation and visualization
  
- Tableau: [Cyclistic Bikes Share Visualization Interactive Dashboard](https://public.tableau.com/app/profile/ogochukwu.ezeogu/viz/CyclisticBikeTripVisualization2023/Dashboard1)

## Prepare
### Data Preparation and Observation
I worked with most recent annual dataset which was available for use. The dataset was first uploaded to google sheet but after figuring out that some months contained very large amount of datasets, I decided to use the RStudio desktop application. I uploaded the dataset into the global environment in R.

In order to have access to the full functionalities of R, I installed the necessary packages for my analysis and loaded them.
```{r Load Packages}

library(tidyverse)
library(janitor)
library(skimr)
library(here)
library(lubridate)
library(stringr)
library(DescTools)
```
Using the str() function, I observed each datset and found that all of them contains 13 variables, labeled:
-	ride_id
-	member_casual
-	rideable_type
-	started_at
-	ended_at 
-	start_station_id
-	end_station_id
-	start_station_name
-	end_station_name
-	start_lat
-	end_lat
-	start_lng
-	end_lng
```{r To preview individual dataset}
str(January_2023_divvy_dataset)
str(February_2023_divy_dataset)
str(March_2023_divvy_dataset)
str(April_2023_divvydataset)
str(May_2023_divvydataset)
str(June_2023_divvy_dataset)
str(July_2023_Divvy_Trip_datase)
str(August_2023_Divvy_dataset)
str(September_2023_divvydataset)
str(October_Divvy_Dataset)
str(November_Divvy_tripdata)
str(December_2023_divvy_dataset)
```
Note that columns with the label: start_station_name and end_station_name were hidden for most of the months and so was not used in my analysis and my outcome not influenced by them.



## Process
### Data cleaning and manipulation

####	Merge dataset: 
After loading the dataset for 2023 into my R studio global environment, I observed the consistency in the column names and arrangement before proceeding to write a code chunk:

```{r}
divvytrips_2023Merged <- bind_rows(January_2023_divvy_dataset,February_2023_divy_dataset,March_2023_divvy_dataset,April_2023_divvydataset,May_2023_divvydataset,June_2023_divvy_dataset,July_2023_Divvy_Trip_datase,August_2023_Divvy_dataset,September_2023_divvydataset,October_Divvy_Dataset,November_Divvy_tripdata,December_2023_divvy_dataset)
```
to merge them into one dataframe. This way I could save time, stay organized and focus better. After merging, I had a total of 57,198,77 obs. and 13 variables.


### Analyze

#### Exploratory Data Analysis
The following exploratory analysis was carried out on the dataset to gradually gain insight into Cyclistic bike data for the year 2023:

#### Total number of rides: The total number of rides = 57,186,08 rides
```{r}
total_ride <- divvytrips_2023 %>% 
  summarise(total_ride = n(),.groups = 'drop') %>% 
  as.data.frame()
```


#### Average trip duration: The average trip duration = 18.189 mins
```{r}
mean_ride_length <- divvytrips_2023 %>%
  summarise(mean_ride_length = mean(tripduration, na.rm = FALSE))
```


#### Maximum trip duration/longest ride length = 98489.07mins 
```{r}
 max_ridelength <- divvytrips_2023 %>% 
  summarise(max_ride_length = max(tripduration))
```


#### Mode trips by day = Saturday
```{r}
day_with_most_rides <-divvytrips_2023 %>%
  summarise(day_with_most_rides = Mode(day_of_the_week))
```

Next, I performed more specific analysis tailored to the mode of bike usage between annual members of Cyclistic bike share and casual riders. I also developed charts in R to better communicate my derived insights.

#### Average trip duration by user type (member and causal):
```{r}
average_ridelength_user <- divvytrips_2023 %>% 
  group_by(member_casual) %>% 
  summarise(avg_ride_by_user = mean(tripduration))
```
   | Member_Casual | Avg_ride_by_user|
   |-------------- | --------------- |      
   |  Member       |  28.25396 mins  |
   |  Casual       |  12.52779 mins  |
   
   ![Average Trip Duration by Usertype](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/2ba82bd4-5f90-4490-8152-ab0768f2ad87)

   
#### Average trip duration by user type by day of the week: 
```{r}
average_ridelength_DOW <- divvytrips_2023 %>% 
  group_by(day_of_the_week, member_casual) %>% 
  summarise(avg_ridelength= mean(tripduration))
```
![Average Trip Duration By DOW - Copy (2)](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/9d412946-f902-4337-a9bd-ed10abc61284)


 
#### Total rides by day of the week: 
```{r}
 total_rideby_user <- divvytrips_2023 %>% 
  group_by(day_of_week) %>% 
  summarise(total_ride = n(),.groups = 'drop') %>% 
  as.data.frame()
```


#### Total of rides by user : 
```{r}
total_rideby_user <- divvytrips_2023 %>% 
  group_by(member_casual) %>% 
  summarise(total_ride = n(),.groups = 'drop') %>% 
  as.data.frame()
```
![Total Rides By User](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/abd9f7c4-3b15-4812-b1ed-e08a3a9358f4)


#### Percentage of bike usage: Cyclistic bike share company provides her users with three rideable bike options, namely: Docked bike, Electric bike, Classic bike.
```{r}
table(divvytrips_2023$rideable_type)/length(divvytrips_2023$rideable_type)*100
```
![Most Preferred Bike (1)](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/4bb4f498-e352-48b0-8d95-498c004e60dc)




### Further Analysis With Charts

#### Busiest days of the week:
In the year 2023, Cyclistic bike had more riders on Saturdays and less on Mondays and Sundays.
```{r}
 total_ride_length_day_of_week<- divvytrips_2023 %>% 
  group_by(day_of_the_week) %>% 
  summarise(total_ride = n(),.groups = 'drop') %>% 
  as.data.frame()
```
![Busiest Days By Ride Count](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/285a1da7-9495-4419-8bd8-6f4955d8fd18)


#### Busiest Months of the year by ride count: 
Cyclistic bike usage increases in the Summer season, around the months of June, July and August. And decreases towards Winter season up till Fall when it peaks again in the month of May.
```{r}
total_ride_length_month<- divvytrips_2023 %>% 
  group_by(month) %>% 
  summarise(total_ride = n(),.groups = 'drop') %>% 
  as.data.frame()
```
 ![Total Rides Per Month](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/b982f089-c02b-41ac-9a32-7de344741b37)



## Findings

The insights derived from the Cyclistic bike data are summarized thus:

1.	Casual riders trust Cyclistic bike for long trips and so makes up to 69.28% of the total average trip duration with Annual members taking up 30.72%.
2.	Annual members covered 64% of the total bike rides in the year 2023, compared to Casual riders.
![Percentage of Users (1)](https://github.com/Taciann62/Cyclistic-Bike-Share-Data-Analysis/assets/132772773/e2490813-f0ec-4e7e-b412-e65e453afe9a)


4.	Saturday and the summer season have the highest number of bike rides, which is possibly influenced by the weekend spree and the summer holidays respectively.
5.	Annual member has a relatively similar ride length and so do not use Cyclistic bikes for longer durations as would Casual riders.

## Recommendations

1.	Annual members are typically stable users of Cyclistic bikes, as they use the bike for their basic mobility needs, and make up a higher percentage of the total bike usage compared to Casual riders.
Casual riders are seasonal riders who make use of Cyclistic bike for recreational activities, tours among others.

However, based on the average trip duration of both user types, Casual riders make up the longest average trip durations for the year with around 70% of the total average ride length.

2.  In order to effectively convert casual riders to annual members, Cyclistic should strategically target their digital adverts on weekends, especially on Saturdays.
And also, during the Summer season where there is a noticeable increase in bike usage by causal riders. The rise in bike usage by casual riders during this season can be attributed to the summer holidays. 

3.  To make the digital advert yield successful conversion of casual riders and the general public, I recommend that Cyclistic offers new subscribers discounts on their subscription plans as a summer holiday bonus.
This bonus should last for a period of time within the holiday season to attract a large number of new subscribers and convert casual riders.
After which, they can curate adverts for Saturdays to make their normal subscription plans known to both casual riders and the public at large.


## Thank You!

