---
layout:     	post
title:     		Dataset generation
author:     	Vegard Bergsvik Øvstegård
tags:           post machine-learning 
subtitle:    	Some thoughts and status around the dataset.
toc:            true
category:       semantic_segmentation
---

# Background
In machine learning, a data set is a collection of data. In this work, it is a collection of sets of
2 images, **x**, and **y**. Where **x**, the input image, is an [orthophoto](https://www.sciencedirect.com/topics/earth-and-planetary-sciences/orthophoto) while the other, **y**, is the
ground truth of buildings and other classes.

![Example of a set with **x**(left) and **y**(right)](/img/set_example.png)

While there are many open-source and publicly available datasets, that contain such sets of images.
It is difficult finding datasets that have the specifications needed to train such a network that this
work requires. E.g lack of environmental variance, poor resolution, and different labels/ground truth.

Therefore, a completely new dataset with the following requirements has to be created:

* Ground truth containing at least buildings.
    * Water, roads, and other static objects are welcome.
* Environmental variance such as:
    * Weather
    * Season
    * Time
    * Location
* At least 1000 images per, class, weather state, season, time, and location.
* Good resolution and a low [ground sample
    distance](https://en.wikipedia.org/wiki/Ground_sample_distance).

# Getting the data
As a student at the University of Oslo, one can be provided access to some of the data not
available to the public at the Norwegian Mapping Authority,
[Kartverket](https://www.kartverket.no/). This not only provides vectorized maps working as very good ground truths for
buildings and water but also some very decent orthophotos. However, they are all taken at a time
when there is little to no occlusion from leaves, snow, etc, and the timing of day to provide the
best possible lighting conditions for photography. I.e lacking the environmental variance needed.
The aforementioned vectorized maps did also include segmented roads, but only public roads and not drive-ways to private properties as so on. 
Public roads and private driveways are usually both made of asphalt, and look the same.
There is a suspicion that having, say only half the valid data classified as true will cause too much noise
during training. None the less a very good starting point, and with tools like [QGIS](https://qgis.org/en/site/), it is possible to apply
orthophotos collected elsewhere and maps created using [photographic mosaic](https://en.wikipedia.org/wiki/Photographic_mosaic) and a drone.

## Ground sampling distance and field of view
As the Norwegian Civil Aviation Authority states, the max flight altitude in Norway for uncertified
and private drones is a maximum of 120 meters relative to the ground. Given the specifications on a typical drone such as the [DJI Mavic 2 Pro](https://www.dji.com/no/mavic-2/info),
this results in a horizontal viewing field of approximately 55m by 31m on a 16:9 aspect ration, and
a Ground sampling distance (GSD) 2.86cm:

$$ GSD = \frac{S_w \times H}{F_r \times im_W} = 2.86cm $$
$$
\begin{align*}
D_w = GSD \times im_W = 55m
\end{align*}
$$
$$
\begin{align*}
D_h = GSD \times im_H = 31m
\end{align*}
$$

* $D_w$ - Horizontal plane footprint width
* $D_h$ - Horizontal plane footprint height
* $S_w = 12.8$ - Sensor width(mm)
* $H = 120$ - Height(m)
* $F_r = 28$ - Focal length(mm)
* $im_W = 1920$ - Image width(px)
* $im_H = 1080$ - Image height(px)

These specifications are somewhat important as they describe what the network should train on.
Ground sampling distance (GSD) is the "real-life" spatial distance between each pixel center.
Orthophotos from Kartverket can be downloaded with a GSD down to 4cm, but with this spatial resolution
and area 3.2km by 2.4km would then result in an uncompressed TIFF file of about 14Gb. As the
objective is to segment buildings, it seems that this high resolution is not necessary. Below are two images
comparing the GSD of 25cm and 44cm.

![GSD = 25cm](/img/bo25.jpg)

![GSD = 44cm](/img/bo44.jpg)

Previous work such as Sahu et al. [@sahu,] used GSD as high as 100cm with decent results. For now,
images with a GSD of 20 will suffice, and if need be access to higher resolution images is possible.
As previously mentioned, images captured with a drone will also possibly be added due to the
lacking environmental variance that the images from Kartverket have. These will have almost the
same GSD calculated above as the drone that will be used is a DJI Mavic Pro. 

For now, almost 50 Gb of raster maps have been ordered, and the dataset will be adapted and updated continuously 
until adequate results on real drone photos have been produced by a potential network. With this in mind, the test-set will mostly
contain drone footage as this is what the network will process when the framework is in production.
The first version of the dataset is hopefully finished by the end of next week. The complete version, however,  will not be ready until enough environmental variance is captured, 
and to no one's surprise, time nor weather cannot be controlled.
