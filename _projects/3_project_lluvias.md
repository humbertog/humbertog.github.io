---
layout: distill
title: Lluvias Mexico City
description: Rainfall interpolation and visualization for the Mexico City Water Management System
img: assets/img/lluvias_main.png
importance: 1
category: work
date: 2021-10-01
bibliography: lluvias.bib

authors:
  - name: Humberto González R.
    affiliations:
      name: _

toc:
  - name: Introduction
  - name: Resuls
  - name: Conclusions


---
**Keywords:** kriging, R, GIS, rainfall, CDMX


## Introduction

#### What
The Mexico City Water Management System (SACMEX) has a real-time precipitation monitoring system, which is responsible for measuring and recording the quantity of rainfall in Mexico City. The SACMEX seeks to automatize the reporting of rainfall for the entire city as well as predefined areas, such as municipalities and high flood risk areas.

#### Problem
Currently, SACMEX produces a report using two methodologies: (i) ordinary `kriging` to obtain isohyets and produce rainfall visualizations; (ii) the `Thiessen polygon method` to obtain the average precipitation (mm) calculated for the entire city and for the municipalities. The use of two methodologies can lead to inconsistencies in the reporting. Moreover, the Thiessen polygon method makes it difficult to automatize the reporting.

#### Proposed solution 
An `automatic report generation tool, based on kriging` <d-cite key="PardoIguzquiza1998"></d-cite><d-cite key="Mair2011"></d-cite><d-cite key="Walter2000"></d-cite>, which allows to easily compute the rainfall statistics in predefined areas, such as the municipalities and zones with high risk of flooding. 
<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/thiessen_polig1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/kriging_polig1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Thiessen (left) vs kriging (right) in computing the rainfall in a zone.
</div>

## Results
The solution is a highly customizable script, programmed in [R](https://www.r-project.org), that is called from the console and takes as inputs (i) a file with the rainfall measures in location and (ii) the predefined GIS polygons of the entire city, the municipalities and the areas of interest. The outputs are a report (.pdf), a file (.csv) with the statistics computed for each area, a Google Earth (.kml) file to use in the web Supervisory Control and Data Acquisition (SCADA) system.

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/lluvias_reporte_dia.pdf" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of a daily report. Cumulative rainfall from 00:00 to the time of the reporting.
</div>

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/lluvias_reporte_mes.pdf" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Example of a monthly report. Cumulative rainfall for the entire month.
</div>


## Conclusions

An important difference between the two methods is that kriging assumes a continuous behavior of the phenomenon, while Thiessen assumes a discrete behavior, derived from the partition of space into large-area polygons. Thiessen's method assumes a single value within the polygon and equal to the value observed at the monitoring station. On the other hand, the kriging method takes into account all the stations within a circumference to estimate the value of any point on the map. This implies that the kriging method has a higher resolution and greater flexibility to capture rainfall patterns. Other advantages of the kriging method over the Thiessen method are:
- the isohyets cannot be obtained from the estimates obtained by the Thiessen method;
- two adjacent Thiessen polygons can show very large jumps in their values;
- zones with few stations imply larger polygons (greater weights) and, therefore, their influence on the calculation of total precipitation is greater;
- Thiessen polygons need to be recalculated each time a station is added or removed from the analysis. Likewise, the weights of the polygons, both within the city and in the delegations, have to be recalculated, for which this method is impractical. 




