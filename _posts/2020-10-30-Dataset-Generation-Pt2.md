---
layout:     	post
title:     		Dataset generation pt. 2
author:     	Vegard Bergsvik Øvstegård
tags:           post machine-learning 
subtitle:    	A short update on the orthophoto map order.
toc:            true
category:       semantic_segmentation
---

As stated in the former [post]({% post_url 2020-10-28-Dataset-Generation%}), the plan was to build the
dataset based on an order of orthophotos with a Ground Sampling Distance(GSD) of 20cm.
However, a decision was made to train the network on either 512x512 or 1024x1024 images. This to 
hopefully maximize efficiency and memory usage on the GPUs while training. A new order is in place, with the following specs:

<style>
tr {
  font-size: 10px
}
td {
  font-size: 10px
}
</style>

### Order specifications

| No. sheets | Scale  | GSD     | Height | Width | Area height | Area length | Total size | Drone FOV | Drone height | No. sets |
|:-----------|-------:|--------:|------------------:|-----------------:|----------------------:|-----------------------:|-----------:|--------------:|-------------:|---------:|
| 11         | 1:1000 | 4.0 cm  | 20000 px          | 15000 px         | 800 m                 | 600 m                  | 9.9 Gb     | 20 m          | 43 m         | 12441    |
| 11         | 1:2000 | 8.0 cm  | 20000 px          | 15000 px         | 1600 m                | 1200 m                 | 9.9 Gb     | 41 m          | 87 m         | 12441    |
| 11         | 1:2000 | 10.0 cm | 16000 px          | 12000 px         | 1600 m                | 1200 m                 | 6.6 Gb     | 51 m          | 109 m        | 7843     |
| 11         | 1:5000 | 12.5 cm | 25600 px          | 19200 px         | 3200 m                | 2400 m                 | 11.0 Gb    | 64 m          | 136 m        | 20350    |
| 11         | 1:5000 | 20.0 cm | 16000 px          | 12000 px         | 3200 m                | 2400 m                 | 6.6 Gb     | 102 m         | 217 m        | 7843     |
NB! Drone field of view (FOV) and height are approximations.

This order will provide the dataset with a total of 60918 different sets with a height variance from
about 40m to 220m. The map sheets will be extracted from different locations across each county in Norway to
provide some locational variance as well. The most important aspect when choosing locations is to get as
many different roof-tops and buildings as possible. Ranging from industrial, residential, and other
buildings such as cabins. The latter might be the hardest for the network to segment as many cabins
in Norway have turfed roofing that looks very similar to the ground below.

![Example of a city center](/img/center.png)

![Example of turfed roofing](/img/cabins.png)

![Example industrial area](/img/industrial.png)

![Example of a residential area](/img/residental.png)


### These are the general chosen locations:

* Agder: Kristiansand
* Innlandet: Hamar
* Møre og Romsdal: Ålesund
* Nordland: Bodø
* Oslo: Oslo
* Rogarland: Stavanger
* Vestfold og Telemark: Rjukan
* Troms og Finnmark: Kirkenes
* Trøndelag: Trondheim
* Vestland: Bergen
* Viken: Sarpsborg
