---

title: "Cyclistic bike-share analysis"
date: 2021-03-21T22:34:46Z
draft: false
---

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

The data has Age Gender information for almost all the subscribers, but for very few walk in riders, so filtering is done to avoid skewing. 

<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\Gender_Summary.png" alt="Gender_Summary" style="zoom:67%;" />

73% of subscribers identified themselves as Male, 26% as Female, and rest as neither.





<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\age_rides_summary.png" alt="age_rides_summary" style="zoom:67%;" />

Most riders fell between 20-40years age.

#Distribution by User Type

Grouping the data by User Type reveals interesting statistics. The bar graph displays average daily rides, while the pie chart shows percentage of the total rides.

<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\user_type_daily_distribution.png" alt="user_type_daily_distribution" style="zoom:67%;" />

Subscribers are riding more from Tuesday to Saturday and less on Mondays and Sundays. The result is opposite for customers who use it more frequently on Mondays and Sundays.  

<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\time_day_summary.png" alt="time_day_summary" style="zoom:67%;" />

Looking at the stacked-area chart, usage pattern during Evening and Night time is same for both the groups. However,  the trends is opposite for the Afternoon and Morning rides. 

#Leading Statistics

Specific stats about most active bikes, stations, and routes can be very helpful for targeted marketing. Every station serves as both the starting and ending point for a ride, so to find the most active stations, rides starting and ending at each station are aggregated, which are then summed together, giving total rides (Start and End) for all of them. 

<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\top_stations_circplot.png" alt="top_stations_circplot" style="zoom:67%;" />

The above circular chart shows 50 most active stations.

<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\top stations comb bar.png" alt="25 Most Active Stations by Subscribers and Customers" style="zoom:67%;" />

25 Most active Stations for Subscribers and Customers are given above.



##Routes Analysis

The data can provide valuable insight into the busiest routes and their connectivity. Because there are 617 stations in total, displaying them all would cause unnecessary clutter. To avoid that, only the most active stations, with total annual rides greater than 200 are filtered. Some rides end at the same starting point, which are also filtered out for this analysis. The data is then manipulated for network analysis using VisNetwork on R.

> edges_200 <- routes_complete%>% 
>   filter(rides>200 & to!=from)

<html
src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\routes_network_plot.html"
/>





<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\Most_Frequent_Routes_by_Subscribers.png" alt="Most Frequent Routes by Subscribers" style="zoom:67%;" />

The above plot shows most frequent routes taken by subscribers over the year.

<img src="P:\arzaidi_blog\AbdulRafayZaidi.github.io\content\research\DIVVY_Bike_Sharing_2019\Most_Frequent_Routes_by_Customers.png" alt="Most Frequent Routes by Customers" style="zoom:67%;" />

The above plot shows most frequent routes taken by customers over the year.

