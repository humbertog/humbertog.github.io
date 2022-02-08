---
layout: distill
title: Station'Air
description: Postdoctoral project in land vehicle tracking from drone videos
img: assets/img/station_air_1.png
importance: 1
#redirect: https://unsplash.com
category: data science
date: 2021-08-31
bibliography: stationair.bib

authors:
  - name: Humberto González R.
    affiliations:
      name: LICIT, Univ. Gustave Eiffel
  - name: Pierre Lemaire
    affiliations:
      name: LICIT, Univ. Gustave Eiffel
  - name: Nour-Eddin El Faouzi
    affiliations:
      name: LICIT, Univ. Gustave Eiffel

# Optionally, you can add a table of contents to your post.
# NOTES:
#   - make sure that TOC names match the actual section names
#     for hyperlinks within the post to work correctly.
#   - we may want to automate TOC generation in the future using
#     jekyll-toc plugin (https://github.com/toshimaru/jekyll-toc).
toc:
  - name: Introduction
    # if a section has subsections, you can add them as follows:
    # subsections:
    #   - name: Example Child Subsection 1
    #   - name: Example Child Subsection 2
  - name: Resuls
    subsections:
        - name: Performance
  - name: Conclusionsd
  
---
**Keywords:** multiple object tracking, Kalman filter, convolutional network detector, unmanned aerial vehicle (UAV), traffic monitoring 


## Introduction

#### What
A methodology for online (pseudo real-time) vehicle tracking and estimation of road traffic summaries from videos acquired by tethered (wired) unmanned aerial vehicles (UAVs). The advantage of UAVs in traffic monitoring is that they can be deployed quickly, in a variety of places and at a reasonable cost, when compared to classic fixed surveillance cameras or loop detectors. 

#### Problem 
Firstly, since the camera is not fixed, videos are prone to `small instabilities` and variations of the viewpoint, caused by windy conditions and the drone navigation system. Secondly, due to the tether and for safety reasons, our drone cannot be placed directly above the road infrastructure and, thus, the `aerial view is oblique`, producing images with a large depth of field (far away objects) and occlusions. Furthermore, distances in the visual plane (pixels) need to be transformed to metric in order to be able to compute the speeds of the vehicles in the scene. Thirdly, the solution must be `site-agnostic`, meaning that we cannot embed the scene topology into most steps: the solution must be able to track vehicles and compute traffic metrics at any place, requiring the minimum human intervention. 

#### Proposed solution 
The four steps of our methodology:
1. `Vehicle detection`. Vehicles in a single frame are located and classified using a single-stage deep convolutional network detector [YOLOv5](https://github.com/ultralytics/yolov5).
2. `Stabilisation and registration`. Hybrid solution for jitter-free video registration in <d-cite key="lemaire2021registering"></d-cite>. 
3. `Association`. The detections between different frames, that are likely to come from the same vehicle, are identified to produce trajectories. Our association algorithm matches trajectories to detections in a frame-wise manner using Kalman filters and solving the linear assignment problem. This algorithm is based on the [SORT](https://github.com/abewley/sort) algorithm <d-cite key="SORT"></d-cite><d-cite key="deepSORT"></d-cite>.
4. `Orthorectification`. The coordinates are transformed from the visual to the metric domain in order to be able to compute the speeds of the vehicles. 

## Results

At each frame, all the information required to compute the traffic summaries is contained in the estimated trajectories: the class (car, truck or utility), position and the velocity of all the tracked vehicles.

Videos contain the complete spatial information of the scene, which facilitates to estimate the occupancy and the mean spatial speed of the vehicles within certain region. We implement `zone detectors` for this purpose. Also, we implement `virtual loop detectors`, which allow us to estimate the number of vehicles passing through an edge, and to obtain the flow. 

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/station_air_1.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Tracker interface. The zone detector (Z01) is highlighted in blue, the entries (E01, E02 and E03) and the exits (S01 and S02) are shown in green and yellow. The summaries (instantaneous count and speed in zone Z01 and cumulative counts in the entries and exits) are shown in the top right corner of the video.
</div>

`Even though the tracker often losses the trajectories of the vehicles (due to occlusions and noisy images), traffic metrics can be estimated without loosing much information`. For example, in the case of the time-space diagram, it suffices to know the start position and the travelled distance of each trajectory within the zone.

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/station_air_2.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Time-space diagram of the trajectories in the zone. The trajectories, in visual coordinates, for the period of time between minutes 22 and 28 are shown in the bottom panel. Note that even though the trajectories have discontinuities, it is still possible to observe the evolution of traffic and to detect traffic situations. For example, in the highlighted area between minutes 22 and 28, we can observe the formation of congestion and the shockwave propagating upstream caused by queuing vehicles trying to turn in the roundabout.
</div>

`Time series` of occupancy, mean spatial speed and flow could be useful in real-time traffic monitoring, since the trends can be observed easily and disaggregated by vehicle class. Moreover, the relation between these variables can be studied in the `traffic flow fundamental diagram`. 

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/station_air_3.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Occupancy, mean spatial speed and flow. The time series (left panel) and fundamental diagram (right panel). The green rectangle highlights the period between minute 22 and minute 28, the same situation described in the time-space diagram above. 
</div>


### Performance

`How does the quality of the tracker impacts the quality of the traffic indicators?`

We execute the tracker over a test video sequence using combinations of parameters with high, medium and bad performance (HOTA metric). This will allow us to measure the sensitivity of the flow, occupancy and speed estimators with respect to the quality of the tracker. Additionally, we compute the traffic indicators for _idealised_ trajectories, obtained by executing the association step directly over the annotated data. In other words, the idealised trajectories can be defined as the result of tracking with perfect detections. 

<div class="row l-body-outset">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.html path="assets/img/station_air_4.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Sensitivity of traffic indicators to the choice of parameters of the Kalman filter. The traffic indicators computed with the three sets of parameters follow the same trend of the indicators produced with the idealised trajectories, but have a larger variation. The better the quality of the tracker, the better the traffic indicators computed from the trajectories.
</div>


## Conclusions

We have shown how traffic indicators can be obtained from long videos captured by tethered UAVs. From the moment the raw video is captured to the calculation of traffic metrics, there are several aspects that need to be considered, such as video stabilisation and perspective. Our methodology addresses these aspects. Additionally, we show that `even when the tracker is not optimal, the produced traffic indicators are still reliable, and they can provide useful information`, such as the flow, occupancy and speeds of the vehicles within a zone. 