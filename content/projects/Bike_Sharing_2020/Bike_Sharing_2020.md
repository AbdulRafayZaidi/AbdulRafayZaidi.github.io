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

Each station can serve as both the starting and ending point for a ride. To get a holistic picture of complete activity at every station, all the rides at a station, either starting or ending, need to be accounted for.  In the original data, rides taken by subscribers and customers are listed with a start and end point. Since each ride registers activity at two stations, the data is split into two subsets, one with the starting stations and other with ending stations.

All rides at each station are aggregated separately and then summed up,  giving total rides (both starting and ending) at all stations.

Long/Lat values for the stations are not given separately and  are included in the same row as each ride's information along with the station name. Since each bike has its own gps device, there is slight variance in long/lat values of every station. Since each station must have a unique geo-location, mean value is used for mapping. 



<iframe seamless src="projects/Bike_Sharing_2020/map.html" width="100%" height="700"></iframe>

Most active stations, and their routes can be seen in the above network plot. You can Hover over a station to see its name, click on it to highlight the connecting stations.



## Conclusion

The above analysis has revealed very useful statistics that can help marketing team launch a successful media campaign. By focusing on the specific stations, which are broadly connected and have a high number of casual customers, or by offering special discounts on specific routes to the subscribers, or by offering special referrals in particular areas, the company can gain high conversion.



***Author: Syed Abdul Rafay***

