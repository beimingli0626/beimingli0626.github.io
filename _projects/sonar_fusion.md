---
layout: page
title: Sonar and Depth Camera Fusion
description: Sensor Fusion of Depth Camera and Ultrasonic Sensor for Glass Detection
img: assets/img/sonar_fusion/sonar_fusion.png
importance: 1
category: work
github_reponame: beimingli0626/sonar_fusion
---

We investigate the challenge posed by glass windows, which are common components in indoor environments, as they are not detectable by depth cameras. In contrast, ultrasonic sensors emit high-pitched sound pulses that are reflected regardless of lighting conditions or object's optical properties. Therefore, we investigate the feasibility of integrating ultrasonic sensors as a secondary sensing modality to our current mapping stack. The outcomes of this independent study include the development of simulation environments in Gazebo to replicate indoor building with windows, the implementation of a sensor fusion mechanism for combining depth image and sonar data, and the design of a RANSAC-based approach for window segmentation in indoor exploration scenarios. These outcomes enable the robot to recognize glass as obstacles and identify potential window regions based on geometric characteristics. This study provides insights into potential benefits and limitations of sensor fusion of depth camera and ultrasonic sensor.

## Simulation Environment
We developed a collection of simulation environments in Gazebo, specifically designed to replicate the layouts of typical indoor building floor plans. Notice that the glass region has the collision volume but does not has a visual property.
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/sonar_fusion/sim_env.png" width="500px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## Ultrasonic Sensor Model
Sonar sensor can usually modeled as shown in the following figure, which presents a clear representation of a 2D sector of a 3D sonar beam, where the X-axis denotes the direction towards which the sonar sensor is pointing (referred as sonar beam center). The figure corresponds to a sonar beam having a FOV of $$\omega_i$$ and a range measurement of $$z_i$$.
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/sonar_fusion/sonar_model_2d.png" width="300px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

We presented a visualization of the occupancy probability along four rays that vary in angles with the sonar beam center in the following figure.
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/sonar_fusion/sonar_model_1.png" width="700px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/sonar_fusion/sonar_model_2.png" width="700px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

Unlike depth images, sonar readings are only scalar values. In order to update the probability of each relevant grid cell in the occupancy map, we need an effective way to discretize the sonar beam. To address this, we chose to discretize the bottom surface of the spherical cone into dense equidistributed points, which is detailed in [Algorithm.1](https://github.com/beimingli0626/sonar_fusion/blob/main/report/report.pdf).

## System Architecture
To maintain an accurate and comprehensive representation of the surrounding environment, we maintained two separate occupancy maps. The first occupancy map is created solely using depth camera data, which provides reliable and precise information about the environment. The second occupancy map is updated with sonar readings, which can detect objects that are not visible to the camera. We also propose a RANSAC-based approach for window segmentation. The method initially identifies the region of interest based on the discrepancy between the sonar reading and the depth-image-based occupancy map. Then, the potential location of the window is further determined by its geometric characteristics. Please refer to the [report](https://github.com/beimingli0626/sonar_fusion/blob/main/report/report.pdf) for more details.

<div class="row justify-content-sm-center" style="text-align: center;">
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/sonar_fusion/occ_flow.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-6 mt-3 mt-md-0">
        {% include figure.html path="assets/img/sonar_fusion/ransac_flow.png" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## Results
To demonstrate the effectiveness of our approach, we compare the occupancy map built solely using depth images with the occupancy map maintained by fusing depth images and sonar readings as shown in the picture below.

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/sonar_fusion/occ_comp.png" width="400px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


Following figure shows an example of the segmented window area, which is marked in grey and framed by the colorful depth-camera-based occupancy grids. The left image displays all the inliers on the fitted largest plane. The right image depicts the occupancy map, with the window segmentation marked in grey.

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/sonar_fusion/ransac.png" width="400px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>


## Limitations
The limitations of the proposed approach include:
- The sonar readings are only reliable when the drone is very close to the window and facing it directly, limiting its use in scenarios where the drone cannot get close or has obstructed views.
- The RANSAC-based plane segmentation technique may struggle in real-life indoor settings with many obstacles, potentially leading to incorrect segmentation.
- The approach assumes that windows are surrounded by solid, camera-visible walls, which may not always be the case, especially with thin frames or large tilted windows.
