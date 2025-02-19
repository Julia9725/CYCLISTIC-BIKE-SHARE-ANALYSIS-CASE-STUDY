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
# To ensure that the trip_id values stack correnctly, it's necessary to convert them to characters.

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

# ANALYSIS

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


# Clean Up and Add Data to Prepare for Analysis
Remove NA and Duplicate Data


# Remove rows with NAs
combined_data <- na.omit(combined_data)

# Remove duplicate rows
combined_data <- distinct(combined_data)

# Check the updated structure of the data frame
str(combined_data)


Checking variables and preparing data for analysis.
# We will modify the values so that the data becomes consistent. 
Checking variables and preparing data for analysis.
# We will modify the values so that the data becomes consistent. 
combined_data <- combined_data %>%
  mutate(usertype = recode(usertype
                           , "member" = "Subscriber"
                           , "casual" = "Customer"))

Confirming normal distribution of observations.
table(combined_data$usertype)

  **Customer**  **Subscriber**
      71643     720313


Adding "ride_length" to combined_data in seconds.


# Convert start_time and end_time to POSIXct objects
combined_data$start_time <- as.POSIXct(combined_data$start_time)
combined_data$end_time <- as.POSIXct(combined_data$end_time)

# Calculate ride_length in seconds
combined_data$ride_length <- as.numeric(difftime(combined_data$end_time, combined_data$start_time, units = "secs"))

# Check the updated structure of the data frame
str(combined_data)
tibble [791,956 × 9] (S3: tbl_df/tbl/data.frame)
 $ trip_id           : chr [1:791956] "21742443" "21742444" "21742445" "21742446" ...
 $ start_time        : POSIXct[1:791956], format: "2019-01-01 00:04:37" ...
 $ end_time          : POSIXct[1:791956], format: "2019-01-01 00:11:07" ...
 $ start_station_id  : num [1:791956] 199 44 15 123 173 98 98 211 150 268 ...
 $ start_station_name: chr [1:791956] "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
 $ end_station_id    : num [1:791956] 84 624 644 176 35 49 49 142 148 141 ...
 $ end_station_name  : chr [1:791956] "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
 $ usertype          : chr [1:791956] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
 $ ride_length       : num [1:791956] 390 441 829 1783 364 ...


# Add date columns such as month, day and year to the combined_data frame.
This will enable us to aggregate ride data by month, day, or year.
```
combined_data$date <- as.Date(combined_data$start_time) 
combined_data$month <- format(as.Date(combined_data$date), "%m")
combined_data$day <- format(as.Date(combined_data$date), "%d")
combined_data$year <- format(as.Date(combined_data$date), "%Y")
combined_data$day_of_week <- format(as.Date(combined_data$date), "%A")
````
# Inspecting column structure.
```
str(combined_data)
tibble [791,956 × 14] (S3: tbl_df/tbl/data.frame)
 $ trip_id           : chr [1:791956] "21742443" "21742444" "21742445" "21742446" ...
 $ start_time        : POSIXct[1:791956], format: "2019-01-01 00:04:37" ...
 $ end_time          : POSIXct[1:791956], format: "2019-01-01 00:11:07" ...
 $ start_station_id  : num [1:791956] 199 44 15 123 173 98 98 211 150 268 ...
 $ start_station_name: chr [1:791956] "Wabash Ave & Grand Ave" "State St & Randolph St" "Racine Ave & 18th St" "California Ave & Milwaukee Ave" ...
 $ end_station_id    : num [1:791956] 84 624 644 176 35 49 49 142 148 141 ...
 $ end_station_name  : chr [1:791956] "Milwaukee Ave & Grand Ave" "Dearborn St & Van Buren St (*)" "Western Ave & Fillmore St (*)" "Clark St & Elm St" ...
 $ usertype          : chr [1:791956] "Subscriber" "Subscriber" "Subscriber" "Subscriber" ...
 $ ride_length       : num [1:791956] 390 441 829 1783 364 ...
 $ date              : Date[1:791956], format: "2019-01-01" ...
 $ month             : chr [1:791956] "01" "01" "01" "01" ...
 $ day               : chr [1:791956] "01" "01" "01" "01" ...
 $ year              : chr [1:791956] "2019" "2019" "2019" "2019" ...
 $ day_of_week       : chr [1:791956] "Tuesday" "Tuesday" "Tuesday" "Tuesday" ...
````


# Inspecting column structure.
str(combined_data)
#to ensure accuracy of data, it is necessary to remove the insufficient data from the combined_data frame which contains a some entries with negative ride_length because some bikes checked for quality by Cyclistic. Then We will create a new version of the dataframe after removing the insufficient data.
```
combined_data <- combined_data[combined_data$ride_length >= 0, ]
```
bike_share <- combined_data[!(combined_data$start_station_name == "HQ QR" | combined_data$ride_length<0),]
````


# STATISTICAL SUMMARY
```
summary(bike_share)
 ##    trip_id            start_time                    
 ##  Length:788189      Min.   :2019-01-01 00:04:37.00  
 ##  Class :character   1st Qu.:2019-02-28 13:39:58.00  
 ##  Mode  :character   Median :2020-01-07 07:59:53.00  
 ##                     Mean   :2019-08-31 14:14:43.81  
 ##                     3rd Qu.:2020-02-19 12:38:46.00  
 ##                     Max.   :2020-03-31 23:51:34.00  
 ##     end_time                      start_station_id start_station_name
 ##  Min.   :2019-01-01 00:11:07.00   Min.   :  2.0    Length:788189     
 ##  1st Qu.:2019-02-28 13:51:45.00   1st Qu.: 77.0    Class :character  
 ##  Median :2020-01-07 08:10:57.00   Median :174.0    Mode  :character  
 ##  Mean   :2019-08-31 14:34:33.26   Mean   :202.2                      
 ##  3rd Qu.:2020-02-19 12:57:45.00   3rd Qu.:289.0                      
 ##  Max.   :2020-05-19 20:10:34.00   Max.   :673.0                      
 ##  end_station_id  end_station_name     usertype          ride_length      
 ##  Min.   :  2.0   Length:788189      Length:788189      Min.   :       1  
 ##  1st Qu.: 77.0   Class :character   Class :character   1st Qu.:     331  
 ##  Median :173.0   Mode  :character   Mode  :character   Median :     539  
 ##  Mean   :202.1                                         Mean   :    1189  
 ##  3rd Qu.:289.0                                         3rd Qu.:     912  
 ##  Max.   :675.0                                         Max.   :10632022  
 ##       date               month               day                year          
 ##  Min.   :2019-01-01   Length:788189      Length:788189      Length:788189     
 ##  1st Qu.:2019-02-28   Class :character   Class :character   Class :character  
 ##  Median :2020-01-07   Mode  :character   Mode  :character   Mode  :character  
 ##  Mean   :2019-08-31                                                           
 ##  3rd Qu.:2020-02-19                                                           
 ##  Max.   :2020-04-01                                                           
 ##  day_of_week       
 ##  Length:788189     
 ##  Class :character  
 ##  Mode  :character  
````
# Comparison Between Subscriber and Customer Users
```
# Comparison of subscribers and customers.
aggregate(bike_share$ride_length ~ bike_share$usertype, FUN = mean)
##   bike_share$usertype bike_share$ride_length
## 1            Customer              5372.7839
## 2          Subscriber               795.2523

aggregate(bike_share$ride_length ~ bike_share$usertype, FUN = median)
##   bike_share$usertype bike_share$ride_length
## 1            Customer                   1393
## 2          Subscriber                    508

aggregate(bike_share$ride_length ~ bike_share$usertype, FUN = max)
##   bike_share$usertype bike_share$ride_length
## 1            Customer               10632022
## 2          Subscriber                6096428

aggregate(combined_data$ride_length ~ combined_data$usertype, FUN = min)
##   combined_data$usertype combined_data$ride_length
## 1               Customer                         0
## 2             Subscriber                         1
````
# The average ride time for subscribers and customers per day.
``` 
aggregate (bike_share$ride_length ~ bike_share$usertype + bike_share$day_of_week, FUN = mean)
##    bike_share$usertype bike_share$day_of_week bike_share$ride_length
## 1             Customer                 Friday              6729.3254
## 2           Subscriber                 Friday               754.0477
## 3             Customer                 Monday              4511.3061
## 4           Subscriber                 Monday               816.3495
## 5             Customer               Saturday              5388.6502
## 6           Subscriber               Saturday               936.7971
## 7             Customer                 Sunday              5159.2264
## 8           Subscriber                 Sunday              1012.5387
## 9             Customer               Thursday              6997.1665
## 10          Subscriber               Thursday               715.1399
## 11            Customer                Tuesday              4414.2919
## 12          Subscriber                Tuesday               814.3137
## 13            Customer              Wednesday              4525.9530
## 14          Subscriber              Wednesday               699.3865
```
 fixed the order of the days of the week.
```
bike_share$day_of_week <- ordered(bike_share$day_of_week, levels=c("Sunday", "Monday", "Tuesday", "Wednesday", "Thursday", "Friday", "Saturday"))
```
# Categorizing the bike share data by the type of riders and the day of the week. 
```r
bike_share %>%
  mutate(weekday = wday(start_time, label = TRUE)) %>%
  group_by(usertype, weekday) %>%
  summarise(
    number_of_rides = n(),
    average_duration = mean(ride_length)
  ) %>%
  arrange(usertype, weekday)
## `summarise()` has grouped output by 'usertype'. You can override using the
## `.groups` argument.
## # A tibble: 14 × 4
## # Groups:   usertype [2]
##    usertype   weekday number_of_rides average_duration
##    <chr>      <ord>             <int>            <dbl>
##  1 Customer   Sun               18652            5061.
##  2 Customer   Mon                5591            4752.
##  3 Customer   Tue                7311            4562.
##  4 Customer   Wed                7690            4480.
##  5 Customer   Thu                7147            8452.
##  6 Customer   Fri                8013            6091.
##  7 Customer   Sat               13473            4951.
##  8 Subscriber Sun               60197             973.
##  9 Subscriber Mon              110430             822.
## 10 Subscriber Tue              127974             769.
## 11 Subscriber Wed              121902             712.
## 12 Subscriber Thu              125228             707.
## 13 Subscriber Fri              115168             797.
## 14 Subscriber Sat               59413             974.
````
# DATA VISUALIZATION

This code calculates the total number of rides and average ride duration for each user type and day of the week. Then it creates a grouped bar plot with ggplot to display the total number of rides by user type on different days of the week. 
```R
bike_share %>%
  mutate(weekday = wday(start_time, label = TRUE)) %>%
  group_by(usertype, weekday) %>%
  summarise(
    number_of_rides = n(),
    average_duration = mean(ride_length)
  ) %>%
  arrange(usertype, weekday) %>%
  ggplot(aes(x = weekday, y = number_of_rides, fill = usertype)) +
  geom_col(position = "dodge") +
  labs(title = "Total Number of Rides by UserType in A Week", x = "Week Day", 
       y = "Number of Rides") +
  theme(axis.text.x = element_text(angle = 60, hjust = 1))
```
![image](https://github.com/user-attachments/assets/2876a1cb-e506-407a-963b-ceaa1075bada)

The number of rides taken by customers is significantly greater in comparison to that of subscribers. This suggests that customers tend to utilize the ride-sharing service more frequenty than subscribers.


This code calculates the average ride duration by user type and day of the week and creates a grouped bar plot using ggplot to visualize the results.
```
bike_share %>%
  mutate(weekday = format(start_time, "%A")) %>%
  group_by(usertype, weekday) %>%
  summarise(number_of_rides = n(), average_duration = mean(ride_length)) %>%
  arrange(usertype, weekday) %>%
  ggplot(aes(x = weekday, y = average_duration, fill = usertype)) +
  geom_col(position = "dodge") +
  labs(title = "Average Duration by UserType",
       x = "Week Day",
       y = "Average Duration") +
  theme(axis.text.x = element_text(angle = 60, hjust = 1))
```
![image](https://github.com/user-attachments/assets/b5a04b1f-e522-42ed-84c5-e6603931bed0)


# Top 20 Station Name booked for the Customers

```
top_stations_customers <- bike_share %>%
  filter(!is.na(start_station_name)) %>%
  filter(usertype == "Customer") %>%
  group_by(start_station_name) %>%
  summarise(number_of_rides = n(),
            avg_ride_length = mean(ride_length),
            avg_ride_length_min = mean(ride_length) / 60) %>%
  arrange(desc(number_of_rides)) %>%
  head(20)

#Horizontal bar plot for customers
ggplot(top_stations_customers, aes(x = number_of_rides, y = reorder(start_station_name, -number_of_rides))) +
  geom_bar(stat = "identity", fill = "lightcoral") +
  labs(title = "Top 20 Stations by Number of Rides for Customers",
       x = "Number of Rides",
       y = "Station Name") +
  theme(axis.text.y = element_text(hjust = 0, vjust = 0.5))
```
 ![image](https://github.com/user-attachments/assets/6d5adaa0-38ed-4f32-84af-4c2feb6a5c7c)


# Top 20 Stations by number of rides booked for subscribers

 
 filtered and summarized data to identify the top 20 bike share stations with the highest number of subscriber rides. Then, I created a horizontal bar plot to visualize this information.
```
top_stations_subscribers <- bike_share %>%
  filter(!is.na(start_station_name)) %>%
  filter(usertype == "Subscriber") %>%
  group_by(start_station_name) %>%
  summarise(number_of_rides = n(),
            avg_ride_length = mean(ride_length),
            avg_ride_length_min = mean(ride_length) / 60) %>%
  arrange(desc(number_of_rides)) %>%
  head(20)

#Horizontal bar plot for subscribers
ggplot(top_stations_subscribers, aes(x = number_of_rides, y = reorder(start_station_name, -number_of_rides))) +
  geom_bar(stat = "identity", fill = "skyblue") +
  labs(title = "Top 20 Stations by Number of Rides for Subscribers",
       x = "Number of Rides",
       y = "Station Name") +
  theme(axis.text.y = element_text(hjust = 0, vjust = 0.5))
 ```
![image](https://github.com/user-attachments/assets/b09df381-612f-495f-b50f-756b9eac362b)


# Total rides of Subscribers vs Customer

 created a bar plot that shows the total number of rides for subscribers and customers. 
```
library(forcats)

ggplot(bike_share, aes(x = fct_infreq(usertype), fill = usertype)) +
  geom_bar(width = 0.5) +
  labs(x = NULL, y = "Number of rides", title = "Total rides of Subscribers vs Customers") +
  scale_y_continuous(labels = scales::unit_format(unit = "M", scale = 1e-6)) +
  scale_fill_manual(values = c("skyblue", "orange"))
```

![image](https://github.com/user-attachments/assets/a909b33d-e086-4cfe-8c23-298712da212b)



# Recommendations
1. Customer Engagement Strategies
After analyzing the data, it was found that customers tend to use Cyclistic bikes for longer rides compared to subscribers. To increase customer engagement and loyalty, Cyclistic should consider implementing targeted marketing strategies like promotional offers or rewards programs. These strategies can encourage more frequent usage and longer rides.

2. Operational Optimization on High Traffic Days
The analysis has shown that ride lengths differ on different days of the week. Cyclistic could enhance its operational processes, such as bike distribution and station maintenance, on days when there is more user activity. This could help to enhance to overall efficiency of the service and improve customer satisfaction.

3. Technology Enhancements for Subscriber Experience
A considerable number of users are subscribers, and they usually undertake shorter rides. To improve the overall experience of subscribers and encourage more frequent usage, Cyclistic could prioritize technologies upgrades such as a more advanced mobile app or additional features.

# Conclusion
After analyzing the data of Cyclistic Bike Share, we have gained important insights into the behavior of users and their ride patterns. I recommended implementing the above-mentioned suggestions to enhance customer engagement, operational efficiency, and the overall user experience. These recommendations can significantly contribute to the growth and success of Cyclistic Bike Share.
