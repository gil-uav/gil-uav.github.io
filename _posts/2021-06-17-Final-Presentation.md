---
layout:     	slide
title:     		GIL - GPS Independent Localization Framework
author:     	Vegard Bergsvik Øvstegård
tags:           presentation 
subtitle:    	Brief presentation of current progress.
mcl_video_id: "Mlp27dQZ7ws?start=9"

theme:		sky # default/beige/blood/moon/night/serif/simple/sky/solarized
trans:		cube # default/cube/page/concave/zoom/linear/fade/none

horizontal:	</section></section><section markdown="1" data-background=""><section markdown="1">
vertical:		</section><section markdown="1">
---
<section markdown="1" data-background=""><section markdown="1">
## {{ page.title }}

<hr>

#### {{ page.author }}
#### {{ "June 17, 2021" | date: "%a - %d %b %Y"}}

<br/>
<br/>

A framework for UAV localization utilizing
object segmentation

{{ page.horizontal }}
<!-- Start Writing Below in Markdown -->

## Motivation

![VG - 29/03/2019](/img/jamming.png)

{{ page.vertical }}

## Motivation
![GRIFF 135 UAV](/img/master/drone.png)

* GPS-Issues
    * Jamming
    * Occlusion
    * Corruption
* INS and visual odometry drift
    * Hardware imperfections
    * Noise

{{ page.horizontal }}

## A possible solution?
* Mantelli et al. - Vision based approach using MCL.
    * Promising results
    * Lacking robustness

![Inducing robustness using object segmentation](/img/presentation_1.png)

{{ page.vertical }}

## Tasks and challenges:
* Applicable measurement model
    * Global localization?
* ML Model performance
    * Robustness
* Dataset
* Computational cost

{{ page.horizontal }}

### The framework in question:

![Simplified framework overview](/img/master/framework.png)

{{ page.horizontal }}

### Segmentation network
* Choosing a model:
    * Hu et al. -> U-net vs Deeplab

![U-net](/img/master/unet.png)

{{ page.vertical }}

### Improving U-net

![Normalization methods](/img/master/group_norm.png)

* High resolution images
* GPU memory
* Wu et al.
    * Batch Norm increases model error on BS < 32
    * Group Norm independent of BS

{{ page.vertical }}

### Group normalization

![ImageNet classification error versus BSs. This is a ResNet-50
model trained in the ImageNet training set using 8 work- ers (GPUs), evaluated
in the validation set.](/img/master/gn.png)

{{ page.vertical }}

### Group normalization

![Altered 3x3 convolution step](/img/master/unetgn.png)

{{ page.horizontal }}

### Dataset

* Static objects
    * Buildings
    * Roads
    * Water
    * Other
* Decent GSD/Resolution
    * Mimic a drone sensor
* Enough Samples
* Appropriate for testing in Norway

{{ page.vertical }}

### Producing a new dataset
* Kartverket
    * Aerial Images
    * Orthophotos
    * Ground Truth Map
        * Buildings (X)
        * Water (X)
        * Roads ( )

![Missing Roads](/img/master/missing_roads.png)

{{ page.vertical }}

### Variance
* Locational
* Seasons
* Time of day
* GSD
* Height

{{ page.vertical }}

### Variance
![Examples](/img/examp.png)

{{ page.vertical }}

### Issues
![Issues](/img/issues.png)

{{ page.vertical }}

### Samples from Kartverket data

![Sample example](/img/samex.png)

* 40000 samples @ 22Gb

{{ page.vertical }}

### Adding more variance
* Regulations 2021
* DJI Drone
    * Firmware issues
* OpenDroneMap

{{ page.vertical }}

### Adding more variance

![Sample map](/img/master/drone_map_1.png)

{{ page.vertical }}

### Adding more variance

![Sample map](/img/master/drone_map_2.png)

{{ page.horizontal }}

### Training
* Binary Cross-entropy
* Adam optimizer
* Validation every 10%
* Early stopping mechanism
* Checkpoints
* ReduceLROnPlateau
* DDP
    * 8 x 2080 TI (Ai-hub)

{{ page.vertical }}

### Data augmentation

Randomly applied with varied strength

* Rotations
* Flipping
* Color jitter
    * Brightness
    * Contrast
    * Saturation
* Noise
    * Gaussian
    * S&P
    * Poisson
    * Speckle

{{ page.vertical }}

### Data augmentation

![Augmentation examples](/img/dataaug1.png)

{{ page.vertical }}

### Data augmentation
![Augmentation examples](/img/dataaug2.png)

{{ page.vertical }}

### Data augmentation
![Augmentation examples](/img/dataaug3.png)

{{ page.horizontal }}

### Framework

![Framework overview](/img/master/framework.png)

{{ page.vertical }}

### Segmentation block

![Detailed overview](/img/master/block1.png)

{{ page.vertical }}

### Segmentation block

* Pre-processing
    * Resizing
    * History equalization
    * Gaussian filter

![Example](/img/prepro.png)

{{ page.vertical }}

### Segmentation block

* Post-processing
    * Dilation and erosion

![Example](/img/postpro.png)

{{ page.vertical }}

### Segmentation block

MCL-Preparation

![Sampling](/img/master/samples.png)

{{ page.vertical }}

### Segmentation block

MCL-Preparation

![Sampling](/img/master/samples_result.png)

Lastly: Flattened to a vector

{{ page.horizontal }}

### Localization block

* Monte Carlo Localization

![MCL Example](/img/master/mcl_robot.png)

{{ page.vertical }}

### Localization block

* KLD-Augmented-MCL
    * Relocalization on failure
    * Dynamic

![MCL Example](/img/master/kld_augmented_mcl.png)

{{ page.vertical }}

### Localization block

![Iteration 1](/img/master/1_iteration.png)

{{ page.vertical }}

### Localization block

![Iteration 2](/img/master/2_iteration.png)

{{ page.vertical }}

### Localization block

![Iteration 3](/img/master/3_iteration.png)

{{ page.vertical }}

### Localization block

![Iteration 4](/img/master/4_iteration.png)

{{ page.vertical }}

### Localization block

![Iteration 5](/img/master/5_iteration.png)

{{ page.vertical }}

### Localization block
Motion model

* Heading
* Speed
* Height

All have individual noise parameters:

1. Heading = UAV_Heading + heading noise
2. Heading = Particle_heading + heading noise

{{ page.vertical }}

### Localization block
Measurement model

Intersection over Union error -> zero-gauss = particle weight

{{ page.horizontal }}

### Experiments
1. Evaluation of the segmentation models
2. Evaluation of the framework via simulations
3. De-facto test using video footage and data from two UAV flights

{{ page.vertical }}

### Segmentation network evaluation
* Test set 20% of dataset

![Results](/img/segmod.png)

{{ page.vertical }}

### Segmentation network evaluation

![Results](/img/master/best1.png)
![Results](/img/master/best2.png)

{{ page.vertical }}

###  Evaluating the localization block
* Localization block only
* With navigation data

![Results](/img/master/sim_1_1_graph.png)

{{ page.vertical }}

### Evaluating the localization block
* With navigation data

![Results](/img/master/sim_1_1_plot.png)

{{ page.vertical }}

### Evaluating the localization block
* Without navigation data

![Results](/img/master/sim_1_2_graph.png)

{{ page.vertical }}

### Evaluating the localization block
* Without navigation data

![Results](/img/master/sim_1_2_plot.png)

{{ page.vertical }}

### Evaluating the framework concept
* With navigation data

![Results](/img/master/sim_2_1_graph.png)

{{ page.vertical }}

### Evaluating the framework concept
* With navigation data

![Results](/img/master/sim_2_1_plot.png)

{{ page.vertical }}

### Evaluating the framework concept
* Without navigation data

![Results](/img/master/sim_2_2_graph.png)

{{ page.vertical }}

### Evaluating the framework concept
* Without navigation data

![Results](/img/master/sim_2_2_plot.png)

{{ page.vertical }}

### Flight 1 with navigation data

![Results](/img/master/def_1_1_graph.png)

{{ page.vertical }}

### Flight 1 with navigation data

![Results](/img/master/def_1_1_plot.png)

{{ page.vertical }}

### Flight 1 without navigation data

![Results](/img/master/def_1_2_graph.png)

{{ page.vertical }}

### Flight 1 without navigation data

![Results](/img/master/def_1_2_plot.png)

{{ page.vertical }}

### Flight 2 with navigation data

![Results](/img/master/def_2_1_graph.png)

{{ page.vertical }}

### Flight 2 with navigation data

![Results](/img/master/def_2_1_plot.png)

{{ page.vertical }}

### Flight 2 without navigation data

![Results](/img/master/def_2_2_graph.png)

{{ page.vertical }}

### Flight 2 without navigation data

![Results](/img/master/def_2_2_plot.png)

{{ page.horizontal }}

### Summary

Topics: 

* Obtain a dataset capable of training a segmentation model with the
framework’s need.
* Develop and train a segmentation model able to learn the objective
from the dataset.
* Develop and program the framework.
* Evaluate the performance of the framework.

{{ page.vertical }}

### Summary
Research:

* Will a measurement model work after the proposed segmentation?
* Is it possible to achieve proper segmentation of static objects such as
buildings?
* Is the framework able to achieve localization in various environmental
conditions?

<!-- End Here -->
{{ page.horizontal }}


# [Print]({{ site.url }}{{ site.baseurl }}{{ page.url }}/?print-pdf#)

# [Back]({{ site.url }}{{ site.baseurl }})

</section></section>
