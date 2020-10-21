---
layout:     	slide
title:     		First progress presentation
author:     	Vegard Bergsvik Øvstegård
tags:           presentation 
subtitle:    	Short presentation of problem and possible solution.
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
#### {{ "September 18, 2020" | date: "%a - %d %b %Y"}}

<br/>
<br/>

Days until delivery: 245 days

{{ page.horizontal }}
<!-- Start Writing Below in Markdown -->

## GIL-UAV
**G**PS-**I**ndependent **L**ocalization for **UAV**s

![System diagram](/img/framework.png)

{{ page.vertical }}

## Framework simulation

<iframe src="https://drive.google.com/file/d/1mx2gudpn-xdRaBKmrMXjMMrKQjHGAlqx/preview" width="640" height="480" allowfullscreen="true"></iframe>

{{ page.horizontal }}

### Current status
#### <progress value="39" max="100" ></progress>

| Task | Progress |
|-------------------------------------------------|---------------------------------------------------------------:|
| *In progress*:                                  |                                                                |
| 1. Create U-net (PyTorch, multi-GPU)                  | <progress value="65" max="100" ></progress>                    |
| 2. Acquire &amp; improve Dataset                   | <progress value="10" max="100" ></progress>                    |
| *To do*:                                        |                                                                |
| 3. Train the U-net | <progress value="0" max="100" ></progress>                    |
| 4. Code dataset-producing software                 | <progress value="0" max="100" ></progress>                     |
| 5. Get drone footage                               | <progress value="0" max="100" ></progress>                     |
| 6. Implement framework(C++, SIMD, CUDA)   | <progress value="0" max="100" ></progress>                     |
| *Completed*                                     |                                                                |
| Implement naive MCL algorithm (Python)                | <progress value="100" max="100" ></progress>                   |
| Get hardware (nVIDIA Jetson TX1)                | <progress value="100" max="100" ></progress>                   |

{{ page.vertical }}

## Current results

* MCL simulation with current measurement model works like a charm.
    * This is given a perfect segmentation, which will not occur IRL.

{{ page.horizontal }}

## Plan for the next fortnight:

<br/>
<br/>

| Week 39                             | Week 40                                          |
|-------------------------------------|--------------------------------------------------|
| Finish U-net code | Code dataset-producing software                  |
| Acquire &amp; improve Dataset       | Start framework implementation  |
| Train the U-net                     | Get drone footage                                |
| Get drone footage                   |                                                  |

<!-- End Here -->
{{ page.horizontal }}

# [Print]({{ site.url }}{{ site.baseurl }}{{ page.url }}/?print-pdf#)

# [Back]({{ site.url }}{{ site.baseurl }})

</section></section>
