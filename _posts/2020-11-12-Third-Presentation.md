---
layout:     	slide
title:     		Third progress presentation
author:     	Vegard Bergsvik Øvstegård
tags:           presentation 
subtitle:    	Brief presentation of current progress.
mcl_video_id: "Mlp27dQZ7ws?start=9"

theme:		night # default/beige/blood/moon/night/serif/simple/sky/solarized
trans:		cube # default/cube/page/concave/zoom/linear/fade/none

horizontal:	</section></section><section markdown="1" data-background=""><section markdown="1">
vertical:		</section><section markdown="1">
---
<section markdown="1" data-background=""><section markdown="1">
## {{ page.title }}

<hr>

#### {{ page.author }}
#### {{ "November 13, 2020" | date: "%a - %d %b %Y"}}

<br/>
<br/>

Days until delivery: 186 days

{{ page.horizontal }}
<!-- Start Writing Below in Markdown -->

## GIL-UAV
**G**PS-**I**ndependent **L**ocalization for **UAV**s

![System diagram](/img/framework.png)

{{ page.vertical }}

## Framework simulation

<iframe src="https://drive.google.com/file/d/1mx2gudpn-xdRaBKmrMXjMMrKQjHGAlqx/preview" width="640" height="480" allowfullscreen="true"></iframe>

{{ page.horizontal }}

### Status from previous progress presentation
#### <progress value="45" max="100" ></progress>

| Task | Progress |
|-------------------------------------------------|---------------------------------------------------------------:|
| *In progress*:                                  |                                                                |
| 1. *Tune the U-net*\*\* | <progress value="50" max="100" ></progress>                    |
| 2. *Acquire &amp; improve Dataset*\*                   | <progress value="20" max="100" ></progress>                    |
| 3. *Train the U-net*\* | <progress value="20" max="100" ></progress>                    |
| *To do*:                                        |                                                                |
| 4. Code dataset-producing software                 | <progress value="0" max="100" ></progress>                     |
| 5. Get drone footage                               | <progress value="0" max="100" ></progress>                     |
| 6. Implement framework(C++, SIMD, CUDA)   | <progress value="0" max="100" ></progress>                     |
| *Completed*                                     |                                                                |
| **Create U-net (PyTorch, multi-GPU)**\*                  | <progress value="100" max="100" ></progress>                    |

*Updated tasks*\*  *New tasks\*\**

{{ page.vertical }}

### Updated tasks

| Task | Progress |
|-------------------------------------------------|---------------------------------------------------------------:|
| 2. Acquire &amp; improve Dataset                   | <progress value="50" max="100" ></progress>                    |
| 3. Train the U-net                                 | <progress value="40" max="100" ></progress>                    |
| 4. Code dataset-producing software                 | <progress value="100" max="100" ></progress>                     |

{{ page.vertical }}

### [Acquire &amp; improve
Dataset](https://gil-uav.github.io/semantic_segmentation/2020/11/09/Dataset-V1/)
#### <progress value="50" max="100" ></progress>

* Orthophotos:
    * Location: Norway
    * Year taken: 2018-2020
    * Ca 64 map-tiles.
    * Spanning over 200$ km^2$
* Classes: 
    * Buildings
    * Water bodies
    * Background

{{ page.vertical }}

### [DS Version 0.0.1:](https://gil-uav.github.io/semantic_segmentation/2020/11/09/Dataset-V1/)

* 25,1Gb of data
* 67219 samples
* Issues:
    * Poor building annotations due to rectification
    * Missing annotations on some buildings
    * Very inaccurate water annotations

{{ page.vertical }}

### [DS Bump to Version 0.1.0:](https://gil-uav.github.io/semantic_segmentation/2020/11/11/Dataset-V2/)

* Improvements:
    * Removed sets with images out of bounds from aerial photos.
    * Removed sets containing only background class.
    * Removed sets containing only water bodies class.
* 17,4Gb of data
* 43802 samples
* Class balance:
    * Building percentage: 24 %
    * Water pixels: 6 %
    * Background pixels: 69 %

{{ page.vertical }}

### Code dataset-producing software
#### <progress value="100" max="100" ></progress>

Features:

* Automated extraction of ground truth labels from vector maps.
* Generates datasets from orthophotos and GT images.
* Discards (some)noisy sets.
* Produces dataset statistics
    * Class balance
    * Mean
    * Standard deviation

Backlog:

* Remove sets with non-annotated buildings.

{{ page.vertical }}

### Train the U-net
#### <progress value="40" max="100" ></progress>
Test set example from newest dataset:

![Images and predictions](/img/example_v2.png)

{{ page.vertical }}

### Train the U-net
#### <progress value="40" max="100" ></progress>

![Ground truth and predictions](/img/example_v2_2.png)

{{ page.vertical }}

#### Current status and progress

| Task                                    | Progress                                     |
|-----------------------------------------|---------------------------------------------:|
| *In progress*:                          |                                              |
| 1. *Tune the U-net*                     | <progress value="50" max="100" ></progress>  |
| 2. *Acquire &amp; improve Dataset*\*    | <progress value="50" max="100" ></progress>  |
| 3. *Train the U-net*\*                  | <progress value="40" max="100" ></progress>  |
| *To do*:                                |                                              |
| 4. Get drone footage                    | <progress value="0" max="100" ></progress>   |
| 5. Implement framework(C++, SIMD, CUDA) | <progress value="0" max="100" ></progress>   |
| *Completed*                             |                                              |
| **Code dataset-producing software**      | <progress value="100" max="100" ></progress>   |

*Updated tasks*\*  *New tasks\*\**

{{ page.vertical }}

#### Completed tasks
* **Code dataset-producing software**
* Create U-net (PyTorch, multi-GPU)
* Implement naive MCL algorithm (Python)
* Get hardware (nVIDIA Jetson TX1)

{{ page.vertical }}

## Plan for the next fortnight:

<br/>
<br/>

| Week 47                             | Week 48                                          |
|-------------------------------------|--------------------------------------------------|
| Tune and train the U-net          | Port MCL code from Python to C++|
| Get drone footage | Get drone footage |
| Acquire &amp; improve Dataset       | Create orthophotos from drone footage |

<!-- End Here -->
{{ page.horizontal }}

# [Print]({{ site.url }}{{ site.baseurl }}{{ page.url }}/?print-pdf#)

# [Back]({{ site.url }}{{ site.baseurl }})

</section></section>
