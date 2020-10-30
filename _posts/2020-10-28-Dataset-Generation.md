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
this results in a horizontal viewing field of approximately 57m by 32m on a 16:9 aspect ratio, and
a Ground Sampling Distance (GSD) 2.95cm:

$$ GSD_w = \frac{S_w \times H}{F_r \times im_W} = 2.95cm $$

$$
\begin{align*}
D_w = GSD \times im_W = 57m
\end{align*}
$$
$$
\begin{align*}
D_h = GSD \times im_H = 32m
\end{align*}
$$

* $D_w$ - Horizontal plane footprint width
* $D_h$ - Horizontal plane footprint height
* $S_w = 13.2$ - Sensor width(mm)
* $H = 120$ - Height(m)
* $F_r = 28$ - Focal length(mm)
* $im_W = 1920$ - Image width(px)
* $im_H = 1080$ - Image height(px)

These specifications are somewhat important as they describe what the network will work on and most
likely should train on. Ground sampling distance (GSD) is the "real-life" spatial distance between each pixel center.
Some orthophotos from Kartverket can be downloaded with a GSD down to 4cm, and with a 4:3 aspect ration
on full HD (1440px by 1080px), gives us a GSD of about 3.93. However, with this spatial resolution and an area of 3.2km by 2.4km, 
it would result in an uncompressed TIFF file of about 14Gb according to Kartverket.
As the objective is to segment buildings, it seems that this high-resolution is not necessary.
Then again, this work is research and such inquiries must be sought out.
Below are two images comparing the GSD of 25cm and 44cm.

![GSD = 25cm](/img/bo25.jpg)

![GSD = 44cm](/img/bo44.jpg)

Previous work such as Sahu et al. [@sahu,] used GSD as high as 100cm with decent results, on a very similar task as this. 
On the other hand Horwath et al [@horwath]. mentions improvements in accuracy for high resolution images in electron microscopy images, but not without challenges.
The aforementioned task is although not directly transferable to segmenting buildings, as they were looking for very small particles with a radius of circa 4-6 pixels in a 1024x1024 image.
E.g a small shed with a size of $1m^2$ would take up about 20x20 pixels, and most static buildings are bigger than $1m^2$.

Another point is that the network will run on a drone with limited computational resources. Floating-point operations are not free and unlimited.
For this reason images with a GSD of 20 will suffice as a start, and if need be, access to higher resolution images is possible.
As previously mentioned, images captured with a drone might also be added due to the
lacking environmental variance that the images from Kartverket have. These will have almost the
same GSD calculated above as the drone that will be used is a DJI Mavic Pro.

50 Gb of raster maps have been ordered, and the dataset will be adapted and updated continuously
until adequate results on real drone photos have been produced by a potential network. With this in mind, the test-set will mostly
contain drone footage as this is what the network will process when the framework is in production.
The first version of the dataset is hopefully finished by the end of next week. The complete version, however,  will not be ready until enough environmental variance is captured,
and to no one's surprise, time nor weather can be controlled.
