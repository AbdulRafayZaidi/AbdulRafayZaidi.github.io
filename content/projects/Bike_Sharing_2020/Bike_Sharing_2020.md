---

title: "Geo-mapping Cyclistic Data"
date: 2021-03-21T22:34:46Z
draft: false
---

## Introduction

Cyclistic is a bike-share company based in Chicago, USA. Its users are both the subscribers with annual membership and customers without any membership. The bikes can be unlocked from one station and returned to any other station in the system anytime. 

The goal is to understand how both the types of users use the service differently to target appropriate marketing campaign and increase the number of subscribers. 

Previously, data for year 2019 was analyzed. The older datasets don't have values for longitude and latitude. To map the statistics, data from year 2020 is used that contains the geo-locations for the bike stations.

All the data is available here https://divvy-tripdata.s3.amazonaws.com/index.html. 

Naming convention is different for year 2020 and 2019, so for consistency, the variables for present data are renamed.  

## Overview

Using R, we can take a quick peek of the dataset with following commands. 

```rst
dim(Divvy_Trips_2019_full)
[1] 3818004      12
```

```rst
colnames(Divvy_Trips_2019_full)
[1] "trip_id"           "start_time"        "end_time"          "bikeid"           
[5] "tripduration"      "from_station_id"   "from_station_name" "to_station_id"    
[9] "to_station_name"   "usertype"          "gender"            "birthyear"
```

```rst
Divvy_Trips_2019_full %>% 
	distinct(bikeid)
#A tibble: 6,017 x 1
```

```rst
Divvy_Trips_2019_full %>% 
	distinct(to_station_id)
#A tibble: 617 x 1
```

**The combined data has a record 3,818,004 rides, split into 12 columns having Trip ID, Start Time, End Time, Bike ID, Trip Duration, From Station (Starting Point), To Station (End Station), User Type, Gender, and Birthyear. The company has 6,017 bikes and 617 stations.**



## Mapping the Data

Bike stations in this dataset can be mapped to see if there is anything out of ordinary with any station, and how the user activity might be related to the locations. 
Long/Lat values for the stations are not given separately and  are included in the same row as each ride's information along with the station name. Since each bike has its own gps device, there is slight variance in long/lat values of every station. Since each station must have a unique geo-location, mean value is used for mapping. 

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
  
  * Upon further investigation, we see that out of *3.3 million* rides,  *356 thousand* started and ended at the same station, making them **10%** of all the rides taken.
  
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
* Further, the activity is different during different times of day, and different days of week. The behavior is not the same for our user types. So campaigning at particular days, and times can provide better results.
* Some routes are more frequented than the rest, and need special attention. 

This initial analysis provides a surface level picture of  the company's userbase. Some of the initial result for particular target cluster can be readily used for initial marketing, however, to make it successful, the analytics team must work in coordination with the marketing time, providing them base level information about specific stations or cluster whenever required.



***Author: Syed Abdul Rafay***

