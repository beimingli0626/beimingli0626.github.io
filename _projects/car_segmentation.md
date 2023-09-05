---
layout: page
title: PointRCNN-based Vehicle Segmentation
description: Segmentation and semantic mapping of vehicles from LiDAR data using PointRCNN
img: assets/img/car_segmentation/segmentation.png
importance: 2
category: work
github_reponame: david-yan/semantic_car_mapping
---

We aim to improve the prediction of cuboidal bounding boxes for cars given LiDAR data, by showing that applying a Deep Learning-based approach (PointRCNN) can outperform existing geometric approaches by requiring less accumulation pointclouds to generate accurate bounding box prediction. This project proposes a pipeline for labelling, generating data, and pre-processing data to convert the data into proper inputs for the PointRCNN model. Our training and evaluation results demonstrate the learning-based approach is able to predict bounding boxes for segmented car pointclouds with a single LiDAR scan with reasonable results. Further work to improve the model accuracy can be taken by re-training with a larger dataset to reduce overfitting and improved training resources, training with few-shot instead of single-shot LiDAR scans, with the goal of deploying the model on a live quadrotor for real-time bounding box prediction. 

## Background
Lots of flight data has been previously collected on Vijay Kumar Lab's Falcon 4 platform, which features an on-board NUC 10 with i7-10710U processor and Ouster OS1-64 LiDAR. This LiDAR data has been labelled and trained on a RangeNet++ based segmentation model to provide semantic labels for trees, cars, roads, and various other classes. This project will leverage this previously trained segmentation model to provide segmented point clouds for our bounding box predictions. 

## Data Collection
We acquired the ROS bags from Kumar's lab. Robot trajectory, accumulated LiDAR maps are reconstructed, and ground truth car cuboids are being labeled as indicated in the following figure.
<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/car_segmentation/dataset.png" width="400px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

## PointRCNN
PointRCNN is a state-of-the-art two-stage object detection model proposed in [PointRCNN: 3D Object Proposal Generation and Detection from Point Cloud](https://arxiv.org/abs/1812.04244). We transfer our data to the form of the expected input for PointRCNN by rotation, ground plane skew correction, and axis alignment.

## Results
The first stage of the model is trained for 200 epochs with batch size 16, which takes 4 hrs. The second stage is trained for 70 epochs with batch size 4, taking another 4 hrs. We trained locally on System76 Desktop with Nvidia A5000 GPU. A sample segmentation result is shown as follow, where the ground-truth cars are in blue (17 G.T. cuboids), and predictions are in red
(8 true positive, 2 false positive).

<div class="row">
    <div class="col-sm mt-3 mt-md-0" style="text-align: center;">
        {% include figure.html path="assets/img/car_segmentation/segmentation.png" width="400px" title="example image" class="img-fluid rounded z-depth-1" %}
    </div>
</div>

 