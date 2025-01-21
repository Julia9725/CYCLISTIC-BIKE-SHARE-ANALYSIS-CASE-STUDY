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
We will be using Cyclistic’s historical trip data to analyze and identify trends. The data has been made available by Motivate International Inc. under this license. The dataset that we are using contains CSV files that details every ride logged from Cyclistic customers ranging from April 2020 to October 2023. For this case study, we will be focusing on data from the previous 12 months, November 2022 to October 2023.

The data is organized into rows and columns with ride_id as the unique identifier for each trip. The data includes the following 13 fields:

|**No.** | **variable**        | **description**                                         |
|------- |---------------------|---------------------------------------------------------|           
|1       | ride_id             | Unique ID assigned to each ride                         |
| 2      | rideable_type       | classic, docked, or electric                            |
| 3      | started_at          | Date and time at the start of trip                      |
| 4      | ended_at            | Date and time at the end of trip                        |
| 5      | start_station_name  | Name of the station where the ride journey started from |
| 6      | start_station_id    | ID of the station where the ride journey started from   |
| 7      | end_station_name    | Name of the station where the ride trip ended at        |
| 8      | end_station_id      | ID of the station where the ride trip ended at          |
| 9      | start_lat           | Latitude of starting station                            |
| 10     | start_lng           | Longitude of starting station                           |
| 11     | end_lat             | Latitude of ending station                              |
| 12     | end_lng             | Longitude of ending station                             |                            
| 13     | member_casual       | Type of membership of each rider                        |

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
Our first course of action is to download the zip files for the past 12 months, organize them into folders and subfolders using appropriate naming conventions, and convert the formatting of each .CSV file to .XLSX so that we have an original copy of our data.	

Since we have such a large volume of data, using SQL/BigQuery might be more efficient as we continue to process and clean our data. With that being said, I used all 12 files to create tables within our dataset. Then, I aggregated all the tables into a singular table using the following function so that I can process the data as a whole:	
	
	
	
	
	
	
	
	
