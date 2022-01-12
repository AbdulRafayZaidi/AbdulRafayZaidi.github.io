---

title: "Karachi Urban Growth and Flood Risk"
date: 2021-08-21T22:34:46Z
draft: false
---

## Introduction

Karachi is the largest city of Pakistan and is a significant contributor to the national economy, with some reports suggesting it be nearly 40 percent of the national GDP. The city has a very diverse population hailing from all parts of the country. According to the national census, its population has ballooned over the past two decades, increasing from 9.8 million in 2000 to 16.5 million in 2021. That’s an increase of 68%.

The increase in population has led to an unprecedented urban sprawl, increasing horizontally rather than vertically. A huge part of the agricultural land has been converted to urban land. The same has also happened to its coastal belt, which previously hosted a very thick Mangroves forest.

This land-use change has raised a number of problems in the urban areas, some of which might even be correlated.

This study will analyze the change over the past two decades to determine whether this growth was sustainable and will classify how much growth/ decline has occurred in various land uses. It will also try to determine if this change is correlated to increasing summer floods.

The study would benefit from the freely available Landsat 7, 8 data as well as data from AESTR and SRTM. 

## Methodology

The initial goal was to produce the natural water streams from the DEM data for 2000 and 2021 and analyze them to see how urban development has affected natural water pathways, thus causing Urban Floods.

However, free DEM data is only available up to the year 2010. Furthermore, the spatial resolution of this data is very small. For this project, DEM from ASTER V3.0 data was used to generate natural water stream using a flow accumulation algorithm. We get a stream network with an order of 5. It has a hierarchical pattern, where streams of order 1 are at the top, gradually flowing into lower-order streams, where stream order 5 is at the bottom. The stream of orders 3, 4, and 5 are the major drain channels running throughout the city.

After the initial process, Urban Growth was analyzed using Landsat Data. For the year 2000, Landsat7 data was used, and for the year 2021, Landsat8 images were used.

Analyzing the Urban Built Area, it was found that the built area increased from 328 square kilometers in the year 2000 to 554 square kilometers in the year 2021.

A buffer is generated around streams 3,4 and 5, having a width of 2-3km to analyze how the city has grown around the drain channels. Around these streams, the built area has increased from 122sqkm to 217 sq km. 

## **Inaccuracies**

Actual drain channels mapped by user “consultses” available at ArcGIS online were included for further investigation.

Significant differences exist between the natural water streams generated using DEM and the actual water streams. Some of these differences could be attributed to the fact that DEM has a pixel size of 30X30m which could result in inaccuracies.

A buffer zone of 500m width (250m on both sides of the river) is generated around the actual river map to find the flood risk areas. In the year 2000, 34.6 sq km of the built area was located within this zone; however, in 2021, it was 56.7 sq km 

## **Results**

From the analysis, we get the following results:

|      | Urban Built Area Square Km |   Population | Urban Built Area within 2-3 km of  key natural streams | Urban Built Area near 500m River  Buffer Zone |
| ---- | -------------------------- | -----------: | :----------------------------------------------------- | --------------------------------------------- |
| 2000 | 328                        |  9.8 million | 122 sq km                                              | 35 sq km                                      |
| 2021 | 554                        | 1.65 million | 217 sq km                                              | 57 sq km                                      |

![Karachi Map 2021](projects/Khi_flood_risk/khi_2021_flood.jpg)

## **Discussion**

The results show increased pressure on the existing water streams due to the increase in population and built area, thus resulting in more frequent Urban floods in the city. 

Moreover, about 10% of the built area is located in close proximity to the river. Not only do they have an increased chance of Urban flood, but they also contribute to flooding in the areas located further because they create a kind of blockage towards the inflow into the stream.



***Author: Syed Abdul Rafay***

