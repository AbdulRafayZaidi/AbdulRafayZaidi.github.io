---

title: "Geo-mapping Cyclistic Data"
date: 2021-08-27T22:34:46Z
draft: false
---

## Introduction

Cyclistic is a bike-share company based in Chicago, USA. Its users are both the subscribers with annual membership and customers without any membership. The bikes can be unlocked from one station and returned to any other station in the system anytime. 

The goal is to understand how both the types of users use the service differently to target appropriate marketing campaign and increase the number of subscribers. 

Previously, data for year 2019 was analyzed. The older datasets don't have values for longitude and latitude. To map the statistics, data from year 2020 is used that contains the geo-locations for the bike stations.

All the data is available [here](https://divvy-tripdata.s3.amazonaws.com/index.html).



## Prep Work

### Overview

The data for year 2020 is divided into 1 quarter and 9 months. The data is downloaded, skimmed, prepared, cleaned, and manipulated. This part deals with all the dirty work prior to the actual analysis and visualization. This prep-work lays the foundation of our data problem solving, and can take twice as much time and effort as the rest of it. 

If you are more interested with the analysis, and results, you can jump ahead to [Data Summary](projects/bike_sharing_2020/bike_sharing_2020/#summary)

### R Libraries

A number of R libraries were called to perform different functions. Following were used to clean, prepare, and analyze the data:

```rst
library("readr") 		## To import dataset
library("janitor") 		## To clean the data
library("dplyr") 		## To manipulate the dataframes
library("lubridate") 	## To work with datetime values
```

For the visualization part, these libraries came into play:

```rst
# Broadly, the following were used for mapping:

library("leaflet") 		## To prepare JS based maps
library("igraph")		## To find networks/edges
library("sp")			## To work with long/lat coords
library("htmlwidgets")	## To export interactive web maps

# Lastly, one monstrous library to create plots is loaded
 library("ggplot2")
```

###  Preparing and Cleaning

  First part of tidying is to look for problems in the data. There are a couple of them: 

* **In the data for first 11months, station Ids are unique numbers. However, in the last month's file, the id is sometimes in accordance with the previous data, at times missing, and at others a combination of alphanumeric characters.**

* **Some Station Names and/or Station Ids are missing.**

* **In some rows complete information(both Id and name) is missing about a start or end station or both.** 

* **Station Names have duplicates with an end phrase or a word.**

Using 11month's data, we create a data-frame having only Station Ids and their Names. This is cleaned up by getting rid of duplicate names, so we get a set of unique station Ids and their respective names. We may call it our *station_id_names* data frame.

Since the data from Jan to Nov conforms, we combine it into a single data frame. Using the *station_id_names*, the missing station Ids are populated by referencing against their station names.

The data for December has problematic station Ids so it's cleaned up separately. Firstly, missing station names are filled up using *station_id_names* by referencing against the Id columns. The Id columns are then removed, and are then cross referenced and merged by using station_id_names.

The whole dataset is then merged. Rows where both Id and Name for a start or end station are missing are removed because they can't be referenced in any way. After cleaning up, we get 3.38 million rows out of 3.52 million.

### Data Structure

Using R, we can take a quick peek of the merged data with following commands. 

```rst
 dim(divvy_trips_2020)
[1] 3385515      13
```

```rst
 colnames(divvy_trips_2020)
 [1] "ride_id"            "rideable_type"      "started_at"         "ended_at"           "start_station_name" "start_station_id"  
 [7] "end_station_name"   "end_station_id"     "start_lat"          "start_lng"          "end_lat"            "end_lng"           
[13] "member_casual"
```

```rst
 head(divvy_trips_2020)
# A tibble: 6 x 13
  ride_id         rideable_type started_at          ended_at            start_station_name                 start_station_id end_station_name           end_station_id start_lat start_lng end_lat end_lng member_casual
  <chr>           <chr>         <dttm>              <dttm>              <chr>                                         <dbl> <chr>                               <dbl>     <dbl>     <dbl>   <dbl>   <dbl> <chr>        
1 000001004784CD~ docked_bike   2020-07-22 15:38:23 2020-07-22 15:56:47 Wolcott (Ravenswood) Ave & Montro~              238 Southport Ave & Clybourn ~            307      42.0     -87.7    41.9   -87.7 casual       
2 00000550C66510~ docked_bike   2020-06-06 15:20:01 2020-06-06 16:28:09 Sheffield Ave & Waveland Ave                    114 Kedzie Ave & Milwaukee Ave            260      41.9     -87.7    41.9   -87.7 casual       
3 0000127970C84F~ docked_bike   2020-05-30 06:36:36 2020-05-30 06:55:28 Green St & Madison St                           198 Wells St & Concord Ln                 289      41.9     -87.6    41.9   -87.6 member       
4 00001E17DEF409~ docked_bike   2020-07-08 21:45:01 2020-07-08 21:57:57 Wabash Ave & Roosevelt Rd                        59 Indiana Ave & 26th St                 147      41.9     -87.6    41.8   -87.6 member       
5 00002279D7D315~ docked_bike   2020-09-26 15:19:31 2020-09-26 15:44:07 Aberdeen St & Monroe St                          80 Rush St & Superior St                 161      41.9     -87.7    41.9   -87.6 casual       
6 000027AD78DF9C~ docked_bike   2020-06-27 22:35:16 2020-06-27 22:46:41 Halsted St & Wrightwood Ave                     349 Pine Grove Ave & Waveland~            232      41.9     -87.6    41.9   -87.6 member   
```

### Manipulation

After the cleaning operation, our data is ready for analysis. But before that, we do some manipulation to help in our analysis. 

* Cyclistic data for year 2020 had different naming convention for its variables. To reuse some of our earlier code, and to correlate the two sets, we change some of the column names. Moreover, user types are renamed from Casual and Member to Customer and Subscriber.
* Since we have already created *station_id_names*, we no longer need station name columns in our combined dataset since they could always be referenced by their station Ids, and so they are removed.
* Longitude / latitude values for the stations are not given separately and  are included in the same row as each ride's information, alongside the station id and name. Since each bike has its own GPS device, there is slight variance in long/lat values of every station. However, each station can only have one unique geo-location, so we take mean value for all respective lat/long values of a station. These values are then merged in *station_id_names* data frame, and are removed from the *divvy_trips_2020*. 
* By grouping, and summarizing *divvy_trips_2020*, total number of subscribers and casual riders visiting any station are calculated. First, we figure out total rides from starting stations, then for ending stations, and then we sum them up. The result is converted from long to wide format, number of rides by subscribers and customers falling into two separate columns, and is added to *station_id_names* data frame.

```rst
head(station_id_names)
  id      lat       lng                       label Customer Subscriber
1  2 41.87650 -87.62053         Buckingham Fountain    18802       4671
2  3 41.86722 -87.61536              Shedd Aquarium    17329       7885
3  4 41.85625 -87.61333              Burnham Harbor     6670      18945
4  5 41.87406 -87.62771      State St & Harrison St     6975       6166
5  6 41.88697 -87.61281              Dusable Harbor    14411       8207
6  7 41.88630 -87.61749 Field Blvd & South Water St     7519      10860
```

### Data Summary {#summary}

The data can be summarized as below:

* Total number of stations is **669**.
* Out of 3.4 million rides, **61.6%** were taken by subscribers and the rest by casual customers.
* Average ride by a casual rider is **46 minutes** long, while by a subscriber is **12 minutes** long.

## Mapping the Data

Bike stations in this dataset can be mapped to see if there is anything out of ordinary with any station, and how the user activity might be related to the locations. 


### Cluster Map

We begin with visualizing the clusters--hotspots where most of the activity takes place.

<iframe seamless src="projects/Bike_Sharing_2020/cluster_map.html" width="100%" height="700"></iframe>

*Most of the stations are located to the center-right, while on the periphery, the density decreases. Hovering over the stations, you can read their labels.*

### Connectivity Map

From the above visualization, it is clear that some stations are more connected than the rest. People are traveling from all over the city to the stations to the center-right. While fewer people are moving between the stations on the periphery. 
This is visualized as under. To reduce the clutter, only top three routes (destination points accessed from a station) are presented. 

<iframe seamless src="projects/Bike_Sharing_2020/connectivity_map.html" width="100%" height="700"></iframe>

*You can zoom in and tap on the stations to read their details, hover over a connecting line to see the annual rides between the two stations.*



In the above map, some very useful information is revealed:

***Some stations on the periphery, are not connected to any other station***

* If we tap on those stations, we would see that their only destination is themselves, meaning that users in those neighborhood take the bike for short-travel naerby and park it back to the origin once their chores are done.

***One station inside the center-cluster is connected to only two other stations, popping out as a small pink dot***

* The two stations titled **HQ QR** and **HUBBARD ST BIKE CHECKING (LBS-WH-TEST)** are probably the company's headquarter, as the further analysis reveals only one way flow of bikes.

***Stations to the right, near the coast, have the most connectivity***

* Station called **Streeter Dr & Grand Ave** is the most connected station, and lies at the coast near *Milton Lee Olive Park*. People travel to more than 450 destinations across the city from this point. Three other such stations lie just near the coast, while the others in a close proximity. 

***The origin station is also the top destination for many stations***

* If you tap on stations, opening their popup window, you would see the station name also listed as the top destination for many stations. Analyzing the data reveals that in fact most frequent routes are those that end on themselves, suggesting that users in those neighborhoods frequently use the service for short sistance traveling. To further establish this hypothesis, we return to our original data and analyze it further.
  
  ![Top Routes by Customers](projects/Bike_Sharing_2020/sorted_by_customers.png)
  *The above table, sorted in descending order of rides taken by customers, show that the top routes are those that end at the starting station.*
  
  
  
  ![Top Routes by Subscribers](projects/Bike_Sharing_2020/sorted_by_Subscribers.png)
  
  *The above table, sorted in descending order of rides taken by subscribers, also offers the same result.* 
  
  * Upon further investigation, we see that out of *3.4 million* rides,  *356 thousand* started and ended at the same station, making them **10%** of all the rides taken.
  
  * Average time for these rides is **49 minutes**.
  
    

### User Distribution Map

Now that we understand overall activity at bike stations, we need to estimate the numbers of **Subscribers** and **Customers** at all stations, to taget appropriate marketing campaigns. 
Each station can serve as both the starting and ending point for a ride. To get a holistic picture of complete activity at every station, all the rides at a station, either starting or ending, need to be accounted for.  In the original data, rides taken by subscribers and customers are listed with a start and end point. The rides at each station are aggregated separately and then summed up,  giving total rides (both starting and ending) at every station.

The size of a circle represents the magnitude of activity at that station, and its color the type of its majority users.

<iframe seamless src="projects/Bike_Sharing_2020/leafMap.html" width="100%" height="700"></iframe>

*You can zoom in and tap on the stations to read their details.*



The map shows cluster of large circles, highly active stations dominated with subscribers and smaller circles, moderately active stations where majority users are casual customers, on its periphery.

Sniffing the above map, we find some insightful points:



**A big green blob in the center**

* This is the part dominated with subscribers, with a number of stations having more than **70%** Subscribers, represented in deep green circles.

**Two small circles in red, pop out near the central cluster**

* Two stations titled **HQ QR** and **HUBBARD ST BIKE CHECKING (LBS-WH-TEST)**, although surrounded with a lot of Subscribers type stations,  have majority user type listed as Customers. These stations, as explained in our *Connectivity Map* above, are only connected amongst themselves and are likely the company's own facilities, causing this apparent deviation. 

**A Subscriber Cluster is forming to the bottom-right**

* A subscriber cluster with 12 stations is taking birth, having three deep green stations in the center, and 9 light-green stations around it. If properly targeted, this cluster could expand and unite with the big cluster in the center.

**To the right of big cluster of subscribers, there are some outliers**

![Focus Cluster on map](projects/Bike_Sharing_2020/Sub_cus_map_snap.png)

* The center-right of the map is dominated with subscribers, however, some stations to the far right, close to the coast, have a majority of casual riders, with some having more than **70%** of them.  Given their proximity to Subscriber hotspots, this seems like an anomaly. These stations are also very active, as apparent by their relative size. To get more information about them, we need to dig deeper into our data. For our case, we investigate **Streeter Dr & Grand Ave**,  **Millennium Park**, and **Buckingham Fountain**, the three leading stations in this group, having 75%, 76%, and 80% customers respectively.
* First thing to notice is that all three of these are located on Public Parks, thereby attracting people from all over the city. So it could be the case that people come here during their weekends, and because they don't do so very often, they haven't taken any membership. To confirm this hypothesis, we analyze them by summarizing and aggregating.
  * Results show that average trip duration to and fro these stations is about **45 minutes** for both subscribers and customers.
  
  * Our earlier hypothesis gains merit as we plot this data, calculating daily rides.
  
    ![Daily rides at three stations](projects/Bike_Sharing_2020/tday_three_stations.png)
  
    The number of subscribers visiting these stations(in green) remains the same throughout the week, and the number is much smaller than those of casual riders(in orange), whose activity nearly triples on weekends. 
    
    This can also be compared with average daily trips on all other stations.
    
    ![Daily rides at all other stations](projects/Bike_Sharing_2020/tday_all_stations.png)
    
    As can be seen here, the result is significantly different for all other stations, where daily subscribers are far greater in number than the irregular customers. 
    
  * For both the groups, we can also compare the time of day when the ride was taken.
  
    ![Daily rides at all other stations](projects/Bike_Sharing_2020/time_three_stations.png)
  
    As apparent in this chart, unlike subscribers, casual riders access these stations more during the day and evening, suggesting that they come here mostly for leisure and not for work.  
    The hourly rides chart for these stations further establishes this point, pointing at 12pm to 5pm as the peak hours. 
  
    ![Daily rides at all other stations](projects/Bike_Sharing_2020/hourly_three_stations.png)
  
  * The same might no be true for other stations in the cluster and they would need to be studied separately. But since these three were the most active points in the cluster, the marketing team would perhaps have a better chance at getting required conversions from them.
  





## Conclusion

The above analysis has revealed very useful statistics that can help marketing team launch a successful media campaign. 

* Instead of focusing on particular stations, the company could target regions, the clusters where most casual riders are.
* Further, the activity is different during different times of day, and different days of week. And the behavior is not the same for both our user types. So campaigning at particular days, and times can provide better results.
* Some routes are more frequented than the rest, and need special attention. 

This initial analysis provides a surface level picture of  the company's userbase. Some of the initial result for particular target cluster can be readily used for initial marketing, however, to make it successful, the analytics team must work in coordination with the marketing time, providing them base level information about specific stations or cluster whenever required.







***Author: Syed Abdul Rafay***

