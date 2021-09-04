---

title: "Urban Heat Islands in Rawalpindi and Islamabad"
date: 2021-08-29T22:34:46Z
draft: false
---

<iframe frameborder="0" class="juxtapose" width="100%" height="600" src="https://cdn.knightlab.com/libs/juxtapose/latest/embed/index.html?uid=aa0a639e-0d9e-11ec-abb7-b9a7ff2ee17c"></iframe>



<center>
    <p>
        Side by side comparison of natural and thermal view of the twin cities: images taken on 19 August 2021
    </p>
</center>



## Introduction

The goal of this study is to analyze the heat distribution under various land-use conditions in Rawalpindi and Islamabad. Broadly speaking, the land-use could be classified into five types: Developed, Agriculture & Horticulture, Forest, Water, Barren. Because of limited local weather stations that monitor the air temperature, we have to rely on satellite imagery. 

Landsat 8 images dated 19th Aug, 2021 were used in this study.

## Methodology

You can jump ahead  [Data Summary](projects/bike_sharing_2020/bike_sharing_2020/#summary) to the classification and results.

### Overview

Band 10 and Band 11 in Landsat 8 are surface temperature bands. For this case, Mono-Window Algorithm was used to extract lands surface temperature, which although not very accurate, suited well to this basic heat distribution study.

All the processing was done in ESRI ArcGIS.

### Model

To make the processing easier, a model was created inside the ArcGIS. You can download the toolbox here.

![Top Routes by Customers](projects/Urban Heat Islands in Rawalpindi & Islamabad/images/model_lst.png)

### Data Summary {#summary}



<iframe seamless src="projects/Bike_Sharing_2020/cluster_map.html" width="100%" height="700"></iframe>

*Most of the stations are located to the center-right, while on the periphery, the density decreases. Hovering over the stations, you can read their labels.*

* 
  
  ![Top Routes by Customers](projects/Bike_Sharing_2020/sorted_by_customers.png)
  
  

***Author: Syed Abdul Rafay***

