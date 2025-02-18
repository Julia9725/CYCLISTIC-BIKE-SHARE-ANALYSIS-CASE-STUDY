# CYCLISTIC-BIKE-SHARE-ANALYSIS-CASE-STUDY
# INTRODUCTION
Cyclistic is a bike-share program based in Chicago, USA. Launched in 2016, the program aims to provide an eco-friendly and convenient transportation alternative for Chicago residents and visitors.
As the company has grown, it has expanded its services and customer base. Cyclistic’s finance analysts have concluded that annual members are much more profitable than casual riders.
Although the pricing flexibility helps Cyclistic attract more customers, Lily, the company's marketing director, suggests focusing on converting casual riders into annual members rather than creating a marketing campaign that targets all-new customers.
She notes that casual riders are already aware of the service but need more incentives to become annual members.
This case study aims to analyze the usage patterns of Cyclistic's bike-share program and identify strategies to increase annual memberships. 
The data for this analysis comes from Cyclistic's historical trip data, customer surveys, and financial records.
# DATA ANALYSIS PROCESS
# ASK: DEFINING THE PROBLEM
Design marketing strategies aimed at converting casual riders into annual members. The marketing analyst team needs to better understand how annual members and casual riders differ, why casual riders would buy a membership and how digital media could affect their marketing tactics. Moreno and her team are interested in analysing the Cyclistic historical bike trip data to identify trends.

These three questions will guide the future marketing program:

How do annual members and casual riders use Cyclistic bikes differently?
Why would casual riders buy Cyclistic annual memberships?
How can Cyclistic use digital media to influence casual riders to become members?
Our first business task is to identify how annual members, and casual riders use Cyclistic bikes differently. Understanding how these two different customer profiles interact with the company’s offerings is key to designing effective marketing strategies that convert casual riders to annual memberships while using data to drive these decisions and fuel the company’s future growth. 
# PREPARE
We will be using Cyclistic’s historical trip data to analyze and identify trends. The data has been made available by Motivate International Inc. under this license.

It is important to note that due to data-privacy issues, we are unable to use riders’ personally identifiable information, meaning we cannot connect pass purchases to credit card numbers to determine if casual riders live in the Cyclistic service area or if they have purchased multiple single passes.
Additionally, we can ensure the integrity and reliability of the data using the ROCCC test.
*	The data is reliable. It is reflective of the population.
*	The data is original. The primary source can be located.
*	The data is comprehensive. It contains the necessary critical information to solve the task at hand.
*	The data is current. We are using relevant data from the past 12 months.
*  The data is cited. It is from a vetted and credible source
  
#### 🔗 Quick Links
**Data Source:** [divvy-tripdata](https://divvy-tripdata.s3.amazonaws.com/index.html) <br>
[Note that the data has been made available by Motivate International Inc. under this [<ins>license</ins>](https://www.divvybikes.com/data-license-agreement).]  
	
# PROCESS		
The data used for this case study cover rider information from the first three months of 2019 and 2020. This data has been provided by Motivate International Inc. under license and is originally stored in separate CVS files.

#load Packages
```
library(tidyverse)
library(lubridate)
library(ggplot2)
library(scales)
library(dplyr)
```	
#load Data	
```	
Divvy_Trips_2019_Q1 <- read_csv("Divvy_Trips_2019_Q1.csv")	
Divvy_Trips_2020_Q1 <- read_csv("Divvy_Trips_2020_Q1.csv")	
```	

# Step 2: Wrangle Data and Combine into a Single File

Compare Column Names Each of the Files
````r
 colnames(Divvy_Trips_2019_Q1)
  ##  [1] "trip_id"           "start_time"        "end_time"         
  ##  [4] "bikeid"            "tripduration"      "from_station_id"  
  ##  [7] "from_station_name" "to_station_id"     "to_station_name"  
  ## [10] "usertype"          "gender"            "birthyear"
        
  colnames(Divvy_Trips_2020_Q1 )
  ##  [1] "ride_id"            "rideable_type"      "started_at"        
  ##  [4] "ended_at"           "start_station_name" "start_station_id"  
  ##  [7] "end_station_name"   "end_station_id"     "start_lat"         
  ## [10] "start_lng"          "end_lat"            "end_lng"           
  ## [13] "member_casual"      


Rename Columns for Consistency
```
divvy_2019 <- divvy_2019 %>%
  rename(
    start_station_id = from_station_id,
    end_station_id = to_station_id,
    start_station_name = from_station_name,
    end_station_name = to_station_name
  )

divvy_2020 <- divvy_2020 %>%
  rename(
    trip_id = ride_id,
    usertype = member_casual,
    start_time = started_at,
    end_time = ended_at
  )
```
 #Inspect the dataframes 
```r
str(Divvy_Trips_2019_Q1)
spc_tbl_ [365,069 × 12] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ trip_id           : num [1:365069] 21742443 21742444 21742445 21742446 21742447 ...
 $ start_time        : POSIXct[1:365069], format: "2019-01-01 00:04:37" ...
 $ end_time          : POSIXct[1:365069], format: "2019-01-01 00:11:07" ...
 $ bikeid            : num [1:365069] 2167 4386 1524 252 1170 ...
 $ tripduration      : num [1:365069] 390 441 829 1783 364 ...
 $ start_station_id  : num [1:365069] 199 44 15 123 173 98 98 211 150 268 ...
 $ start_station_name: chr [1:365069] "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
 $ end_station_id    : num [1:365069] 84 624 644 176 35 49 49 142 148 141 ...
 $ end_station_name  : chr [1:365069] "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
 $ usertype          : chr [1:365069] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
 $ gender            : chr [1:365069] "Male" "Female" "Female" "Male" ...
 $ birthyear         : num [1:365069] 1989 1990 1994 1993 1994 ...
 - attr(*, "spec")=
  .. cols(
  ..   trip_id = col_double(),
  ..   start_time = col_datetime(format = ""),
  ..   end_time = col_datetime(format = ""),
  ..   bikeid = col_double(),
  ..   tripduration = col_number(),
  ..   from_station_id = col_double(),
  ..   from_station_name = col_character(),
  ..   to_station_id = col_double(),
  ..   to_station_name = col_character(),
  ..   usertype = col_character(),
  ..   gender = col_character(),
  ..   birthyear = col_double()
  .. )
 - attr(*, "problems")=<externalptr>


 str(Divvy_Trips_2020_Q1)
 spc_tbl_ [426,887 × 14] (S3: spec_tbl_df/tbl_df/tbl/data.frame)
 $ trip_id           : chr [1:426887] "EACB19130B0CDA4A" "8FED874C809DC021" "789F3C21E472CA96" "C9A388DAC6ABF313" ...
 $ rideable_type     : chr [1:426887] "docked_bike" "docked_bike" "docked_bike" "docked_bike" ...
 $ start_time        : POSIXct[1:426887], format: "2020-01-21 20:06:59" ...
 $ end_time          : POSIXct[1:426887], format: "2020-01-21 20:14:30" ...
 $ start_station_name: chr [1:426887] "Western Ave & Leland Ave" "Clark St & Montrose Ave" "Broadway & Belmont Ave" "Clark 
 St & Randolph St" ...
 $ start_station_id  : num [1:426887] 239 234 296 51 66 212 96 96 212 38 ...
 $ end_station_name  : chr [1:426887] "Clark St & Leland Ave" "Southport Ave & Irving Park Rd" "Wilton Ave & Belmont Ave" 
 "Fairbanks Ct & Grand Ave" ...
 $ end_station_id    : num [1:426887] 326 318 117 24 212 96 212 212 96 100 ...
 $ start_lat         : num [1:426887] 42 42 41.9 41.9 41.9 ...
 $ start_lng         : num [1:426887] -87.7 -87.7 -87.6 -87.6 -87.6 ...
 $ end_lat           : num [1:426887] 42 42 41.9 41.9 41.9 ...
 $ end_lng           : num [1:426887] -87.7 -87.7 -87.7 -87.6 -87.6 ...
 $ usertype          : chr [1:426887] "member" "member" "member" "member" ...
 $ ride_length       : num [1:426887] 7.52 3.72 2.85 8.82 5.53 4.82 4.82 4.95 4.92 3.38 ...
 - attr(*, "spec")=
  .. cols(
  ..   ride_id = col_character(),
  ..   rideable_type = col_character(),
  ..   started_at = col_datetime(format = ""),
  ..   ended_at = col_datetime(format = ""),
  ..   start_station_name = col_character(),
  ..   start_station_id = col_double(),
  ..   end_station_name = col_character(),
  ..   end_station_id = col_double(),
  ..   start_lat = col_double(),
  ..   start_lng = col_double(),
  ..   end_lat = col_double(),
  ..   end_lng = col_double(),
  ..   member_casual = col_character()
  .. )
```

# Remove Unnecessary Variables
```
Divvy_Trips_2019_Q1 <- Divvy_Trips_2019_Q1 %>%  
  select(-c(gender, birthyear,bikeid,tripduration))

Divvy_Trips_2020_Q1 <- Divvy_Trips_2020_Q1 %>%  
  select(-c(start_lat, end_lat,start_lng,end_lng,rideable_type,ride_length))
```
To ensure that the trip_id values stack correnctly, it's necessary to convert them to characters.
```
Divvy_Trips_2019_Q1 <- mutate(Divvy_Trips_2019_Q1, trip_id= as.character(trip_id))
```

# Creating Combined Data Frame
```
combined_data <- bind_rows(Divvy_Trips_2019_Q1,Divvy_Trips_2020_Q1)
colnames(combined_data)
## [1] "trip_id"            "start_time"         "end_time"          
## [4] "start_station_id"   "start_station_name" "end_station_id"    
## [7] "end_station_name"   "usertype"

str(combined_data)
## 'data.frame':    791956 obs. of  8 variables:
##  $ trip_id           : chr  "21742443" "21742444" "21742445" "21742446" ...
##  $ start_time        : chr  "2019-01-01 0:04:37" "2019-01-01 0:08:13" "2019-01-01 0:13:23" "2019-01-01 0:13:45" ...
##  $ end_time          : chr  "2019-01-01 0:11:07" "2019-01-01 0:15:34" "2019-01-01 0:27:12" "2019-01-01 0:43:28" ...
##  $ start_station_id  : int  199 44 15 123 173 98 98 211 150 268 ...
##  $ start_station_name: chr  "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
##  $ end_station_id    : int  84 624 644 176 35 49 49 142 148 141 ...
##  $ end_station_name  : chr  "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
##  $ usertype          : chr  "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
```

#ANALYSIS

```R
nrow(combined_data)  # Numbers of the Rows
## [1] 791956
```
dim(combined_data)  #Dimensions
## [1] 791956      8
```
head(combined_data)  # The first 6 rows of the combined data frame
##    trip_id         start_time           end_time start_station_id
## 1 21742443 2019-01-01 0:04:37 2019-01-01 0:11:07              199
## 2 21742444 2019-01-01 0:08:13 2019-01-01 0:15:34               44
## 3 21742445 2019-01-01 0:13:23 2019-01-01 0:27:12               15
## 4 21742446 2019-01-01 0:13:45 2019-01-01 0:43:28              123
## 5 21742447 2019-01-01 0:14:52 2019-01-01 0:20:56              173
## 6 21742448 2019-01-01 0:15:33 2019-01-01 0:19:09               98
##                    start_station_name end_station_id
## 1              Wabash Ave & Grand Ave             84
## 2              State St & Randolph St            624
## 3                Racine Ave & 18th St            644
## 4      California Ave & Milwaukee Ave            176
## 5 Mies van der Rohe Way & Chicago Ave             35
## 6          LaSalle St & Washington St             49
##                 end_station_name   usertype
## 1      Milwaukee Ave & Grand Ave Subscriber
## 2 Dearborn St & Van Buren St (*) Subscriber
## 3  Western Ave & Fillmore St (*) Subscriber
## 4              Clark St & Elm St Subscriber
## 5        Streeter Dr & Grand Ave Subscriber
## 6        Dearborn St & Monroe St Subscriber
```

str(combined_data)  # List of Columns and Data Types
## 'data.frame':    791956 obs. of  8 variables:
##  $ trip_id           : chr  "21742443" "21742444" "21742445" "21742446" ...
##  $ start_time        : chr  "2019-01-01 0:04:37" "2019-01-01 0:08:13" "2019-01-01 0:13:23" "2019-01-01 0:13:45" ...
##  $ end_time          : chr  "2019-01-01 0:11:07" "2019-01-01 0:15:34" "2019-01-01 0:27:12" "2019-01-01 0:43:28" ...
##  $ start_station_id  : int  199 44 15 123 173 98 98 211 150 268 ...
##  $ start_station_name: chr  "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
##  $ end_station_id    : int  84 624 644 176 35 49 49 142 148 141 ...
##  $ end_station_name  : chr  "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
##  $ usertype          : chr  "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
```

summary(combined_data) # Summary of the Combined Data Frame
##    trip_id           start_time          end_time         start_station_id
##  Length:791956      Length:791956      Length:791956      Min.   :  2.0   
##  Class :character   Class :character   Class :character   1st Qu.: 77.0   
##  Mode  :character   Mode  :character   Mode  :character   Median :174.0   
##                                                           Mean   :204.4   
##                                                           3rd Qu.:291.0   
##                                                           Max.   :675.0   
##                                                                           
##  start_station_name end_station_id  end_station_name     usertype        
##  Length:791956      Min.   :  2.0   Length:791956      Length:791956     
##  Class :character   1st Qu.: 77.0   Class :character   Class :character  
##  Mode  :character   Median :174.0   Mode  :character   Mode  :character  
##                     Mean   :204.4                                        
##                     3rd Qu.:291.0                                        
##                     Max.   :675.0                                        
##                     NA's   :1
```


 Clean Up and Add Data to Prepare for Analysis
Remove NA and Duplicate Data


# Remove rows with NAs
combined_data <- na.omit(combined_data)

# Remove duplicate rows
combined_data <- distinct(combined_data)

# Check the updated structure of the data frame
str(combined_data)
