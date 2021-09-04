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

In this section, the basic methodology is explained. You may jump ahead to [analysis](projects/uhi_rwp_isb_19aug2021/urbanheatislandsrwpisb19aug2021/#analysis) and results.

### Overview

Band 10 and Band 11 in Landsat 8 are surface temperature bands. For this case, Mono-Window Algorithm was used to extract lands surface temperature, which although not very accurate, suited well to this basic heat distribution study.

All the processing was done in ESRI ArcGIS.

### Model

To make the processing easier, a model was created inside the ArcGIS. You can download the toolbox [here](projects\UHI_RWP_ISB_19Aug2021\resources\LST.tbx).

![LST Model](projects/UHI_RWP_ISB_19Aug2021/images/model_lst.png)

<center>
        Model to create Land Surface Temprature Raster
</center>



![UHI Model](projects/UHI_RWP_ISB_19Aug2021/images/model_uhi.png)

<center>
        Model to create Urban Heat Island Raster
</center>

### Classification

Supervised classification is run to identify various land uses. 

![Land-Use Classification](projects/UHI_RWP_ISB_19Aug2021/images/supervised_classification.jpg)

The resulting raster is then processed by filtering smaller pixels, smoothing class boundaries, and removing isolated regions, which is then converted to polygon.

![Land-Use Classification](projects/UHI_RWP_ISB_19Aug2021/images/class_result.jpg)





## Analysis {#analysis}





***Author: Syed Abdul Rafay***

