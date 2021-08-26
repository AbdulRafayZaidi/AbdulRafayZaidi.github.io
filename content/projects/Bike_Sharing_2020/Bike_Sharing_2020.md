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

* If we tap on those stations, we would see that their only destination is themselves, meaning that users in those neighborhood take the bike for short-travel in the area and park it back to the origin once their chores are done.

***One station inside the center-cluster is connected to only two other stations, popping out as a small pink dot***

* The two stations titled **HQ QR** and **HUBBARD ST BIKE CHECKING (LBS-WH-TEST)** are probably the company's headquarter, as the further analysis reveals only one way flow of bikes.

***Stations to the right, near the coast, have the most connectivity***

* Station called **Streeter Dr & Grand Ave** is the most connected station, and lies at the coast near *Milton Lee Olive Park*. People travel to more than 450 destinations across the city from this point. Three other such stations lie just near the coast, while the others in close proximity. 

***The origin station is also the top destination for many stations***

* Inside the popup window for many of the stations, its name is also listed as the top destination. Analyzing the data reveals that in fact most frequent routes are those that end on themselves, suggesting that users in those neighborhoods frequently use the service for short nearby traveling. To further establish this hypothesis, we return to our original data and analyze it further. 
  * Out of *3.3 million* rides  *356 thousand* started and ended at the same station, making them **10%** of all the rides taken.
  * Average time for these rides is **49 minutes**.

### User Distribution Map

Now that we understand overall activity at bike stations, we need to estimate the numbers of **Subscribers** and **Customers** at all stations, to taget appropriate marketing campaigns. 
Each station can serve as both the starting and ending point for a ride. To get a holistic picture of complete activity at every station, all the rides at a station, either starting or ending, need to be accounted for.  In the original data, rides taken by subscribers and customers are listed with a start and end point. The rides at each station are aggregated separately and then summed up,  giving total rides (both starting and ending) at every station.

The size of a circle represents the magnitude of activity at that station, and its color the type of its majority users.

<iframe seamless src="projects/Bike_Sharing_2020/leafMap.html" width="100%" height="700"></iframe>

*You can zoom in and tap on the stations to read their details.*



The map shows cluster of large circles, highly active stations dominated with subscribers and smaller circles, moderately active stations where majority users are casual customers, on its periphery.

Sniffing the above map, we find some insightful points:

**Two small circles in red, pop out near the center left**

* Two stations titled **HQ QR** and **HUBBARD ST BIKE CHECKING (LBS-WH-TEST)**, although surrounded with a lot of Subscribers type stations,  have majority user type listed as Customers. These stations, as explained in our *Connectivity Map* above, are only connected amongst themselves and are likely the company's own facilities, causing this apparent deviation. 

**To the right of big cluster of subscribers, there are some outliers**

* The right center of the map is dominated with subscribers, however, some stations to the far right, close to the coast, are dominated with casual riders.  These stations are also very active, as apparent by their relative size. To get more information about them, we need to dig deeper in our data.





## Conclusion

The above analysis has revealed very useful statistics that can help marketing team launch a successful media campaign. By focusing on the specific stations, which are broadly connected and have a high number of casual customers, or by offering special discounts on specific routes to the subscribers, or by offering special referrals in particular areas, the company can gain high conversion.



***Author: Syed Abdul Rafay***

