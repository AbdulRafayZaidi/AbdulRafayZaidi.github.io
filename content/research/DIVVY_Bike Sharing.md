title: "Project: Cyclistic bike-share analysis"
date: 2021-03-21T20:34:46Z
draft: false
tags: [
    "Research","Data Analytics", "Bike Sharing", "Cyclist data"
]

#Project: Cyclistic bike-share analysis
##Year-2019

#Introduction

Cyclistic is a bike-share company based in Chicago, USA. Its users are both the subscribers with annual membership and customers without any membership. The bikes can be unlocked from one station and returned to any other station in the system anytime. 

The goal is to understand how both the types of users use the service differently to target appropriate marketing campaign and increase the number of subscribers. The most data  used is for year 2019 available here https://divvy-tripdata.s3.amazonaws.com/index.html and divided in four quarters. The data for 1st, 2nd, and 4th quarter is in the same format, however for the 3rd quarter, columns names are different. After right corrections, the dataset is merged.  

#Overview

Using R, we can take a quick peek of the dataset with following commands. 

> dim(Divvy_Trips_2019_full)
> [1] 3818004      12

> colnames(Divvy_Trips_2019_full)
>  [1] "trip_id"           "start_time"        "end_time"          "bikeid"           
>  [5] "tripduration"      "from_station_id"   "from_station_name" "to_station_id"    
>  [9] "to_station_name"   "usertype"          "gender"            "birthyear"  "

> Divvy_Trips_2019_full %>% 
> 	distinct(bikeid)
> #A tibble: 6,017 x 1

> Divvy_Trips_2019_full %>% 
> 	distinct(to_station_id)
> #A tibble: 617 x 1

The combined data has a record 3,818,004 rides, split into 12 columns having Trip ID, Start Time, End Time, Bike ID, Trip Duration, From Station (Starting Point), To Station (End Station), User Type, Gender, and Birthyear. The company has 6,017 bikes and 617 stations.



Short Summary(Gender, User)

Weekly/Daily Usage

Most Active Stations

Most Active Routes



Time Growth of Subscribers