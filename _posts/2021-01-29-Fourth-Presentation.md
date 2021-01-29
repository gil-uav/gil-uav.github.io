---
layout:     	slide
title:     		Fifth progress presentation
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

Days until delivery: 109 days

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

### Updated tasks

| Task                                    | Progress                                     |
|-----------------------------------------|---------------------------------------------:|
| 4. Get drone footage                    | <progress value="25" max="100" ></progress>   |
| 5. Implement framework(C++, SIMD, CUDA) | <progress value="10" max="100" ></progress>   |
| 5. Report writing | <progress value="20" max="100" ></progress>   |

{{ page.vertical }}

### Get drone footage
#### <progress value="25" max="100" ></progress>

* A1/A2/A3 drone licence
    * A2 is not free of charge..
* Footage of different environments:
    * Snow
    * No leafs
    * Rain

{{ page.vertical }}

### Implement framework
#### <progress value="10" max="100" ></progress>
* Acquired Nvidia Jetson Nano and Nvidia Jets Tegra TX1.
* Started coding project in C++.

{{ page.vertical }}

#### Current status and progress

| Task                                    | Progress                                     |
|-----------------------------------------|---------------------------------------------:|
| *In progress*:                          |                                              |
| 1. *Tune the U-net*                     | <progress value="50" max="100" ></progress>  |
| 2. Acquire &amp; improve Dataset    | <progress value="50" max="100" ></progress>  |
| 3. Train the U-net                  | <progress value="40" max="100" ></progress>  |
| 4. Get drone footage\*                    | <progress value="25" max="100" ></progress>   |
| 5. Implement framework(C++, SIMD, CUDA)\* | <progress value="10" max="100" ></progress>   |
| 5. *Report writing*\* | <progress value="20" max="100" ></progress>   |
| *To do*:                                |                                              |
| 6. *Create test-set from drone footage*\** | <progress value="0" max="100" ></progress>   |
| 7. *Test segmentation on Nvidia boards*\** | <progress value="0" max="100" ></progress>   |

*Updated tasks*\*  *New tasks\*\**

{{ page.vertical }}

#### Completed tasks
* Code dataset-producing software
* Create U-net (PyTorch, multi-GPU)
* Implement naive MCL algorithm (Python)
* Get hardware (nVIDIA Jetson TX1 & Jetson Nano)

{{ page.vertical }}

## Plan for the next fortnight:

<br/>
<br/>

| Week 5                             | Week 6                                          |
|-------------------------------------|--------------------------------------------------|
| Test segmentation on Nvidia Boards | Get more drone footage|
| Develop further on MCL C++ implementation | Write more on report.
| Acquire &amp; improve Dataset       | Write loads more on the report. |

<!-- End Here -->
{{ page.horizontal }}

# [Print]({{ site.url }}{{ site.baseurl }}{{ page.url }}/?print-pdf#)

# [Back]({{ site.url }}{{ site.baseurl }})

</section></section>
