---
layout:     	slide
title:     		Second progress presentation
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
#### {{ "October 16, 2020" | date: "%a - %d %b %Y"}}

<br/>
<br/>

Days until delivery: 214 days

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

### Updated tasks

| Task | Progress |
|-------------------------------------------------|---------------------------------------------------------------:|
| 1. Create U-net (PyTorch, multi-GPU)                  | <progress value="100" max="100" ></progress>                    |
| 2. Acquire &amp; improve Dataset                   | <progress value="20" max="100" ></progress>                    |
| 3. Train the U-net | <progress value="20" max="100" ></progress>                    |

{{ page.vertical }}

### Create U-net (PyTorch, multi-GPU) <progress value="100" max="100" ></progress>

* Base model and framework implemented with *PyTorch* and *PyTorch Ignite*
* Features:
    * Gradient clipping
    * Gradient accumulation
    * Early stopping
    * Metrics
    * Distributed data parallel(DDP)
    * Multi GPU training
    * Training notification(Discord)
    * Learning rate scheduler (ReduceLROnPlateau)
    * Rescaling of images.

{{ page.vertical }}

### Create U-net (PyTorch, multi-GPU) <progress value="100" max="100" ></progress>

* Features:
    * Data augmentation for training(All are random to some degree):
        * Contrast
        * Brightness
        * Saturation
        * Noise; Gaussian, Salt & Pepper, Poission, Speckle.
        * Rotations($$n*\frac{pi}{2}$$)
        * Vertical and Horisontal flips.

{{ page.vertical }}

### Acquire &amp; improve Dataset <progress value="10" max="100" ></progress>

* Extracted larger aerial photographs from existing data, of which will aid in expanding the
  dataset by several images.
* Fine-tuned the ground truth segmentation images as they where not overlapping buildings enough.
* Have finally gotten access to Kartverkets database: <img src="/img/kartverket.jpg" alt="Kartverket meme" width="200"/>

{{ page.vertical }}

### Acquire &amp; improve Dataset <progress value="10" max="100" ></progress>

#### Results\Important notes: 
* Ground truth is far from perfect even after fine-tuning. This does yield noise in the dataset,
  witch may lead to the following cases: 
    1. NEGATIVE: Learning gets corrupted and a suitable global optimum may never occur.
    2. POSITIVE: Noise may induce better generalization for the network.
        * Real life scenarios contain occlusions in the images, and network may learn to draw
          buildings as squares despite say occluding trees.

{{ page.vertical }}

#### Train the U-net <progress value="30" max="100" ></progress>
* Attained several good preliminary results despite the network not being tuned and with a lacking
  dataset.
* Somewhat sceptical to the results. Test and validation data is completely separate, but they are
  very similar. I assume images from example drone-footage will not be as good.
* Results look like overfitting, but no indications of it from metrics.

{{ page.vertical }}

#### Train the U-net <progress value="30" max="100" ></progress>

<img src="/img/unet_prelim1.png" alt="U-net preliminary results" />
Segmented buildings masked in white to the right.

{{ page.vertical }}

#### Train the U-net <progress value="30" max="100" ></progress>

<img src="/img/unet_prelim2.png" alt="U-net preliminary results" />
Ground truth left and prediction right.

{{ page.vertical }}

#### Train the U-net <progress value="30" max="100" ></progress>

<img src="/img/unet_prelim3.png" alt="U-net preliminary results" />

Training rounds left, validation rounds right. 
<br> No indications of overfitting.
Light blue had learning-rate of 0.001.
Metric is Binary Cross Entropy with Logits(BCEWithLogitsLoss).

{{ page.vertical }}

#### Train the U-net <progress value="30" max="100" ></progress>
#### Results\Important notes: 
* Results appear to be overfitting, however without indications in metrics.
* Image resolution plays a big part, 512x512px images gave far better results then 256x256px.
    * I am considering increasing image size further, how ever this will affect batch-size and
      memory usage on GPUs.
* To much variation and randomness in data augmentation is disruptive for training. I.e to much noise 
results in no training(Duh).
* Considering adding elastic transformations to data-augmentation as not all buildings are square.
* Also considering adding grayscale transformations as the network appears to find red-bricked
  roof-tops much faster than any other. This might aid in a better global optimum.

{{ page.vertical }}

#### Current status and progress
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

#### Completed tasks
* **Create U-net (PyTorch, multi-GPU)**
* Implement naive MCL algorithm (Python)
* Get hardware (nVIDIA Jetson TX1)

{{ page.vertical }}

## Plan for the next fortnight:

<br/>
<br/>

| Week 43                             | Week 44                                          |
|-------------------------------------|--------------------------------------------------|
| Tune and train the U-net          | Tune and train the U-net                       |
| Acquire &amp; improve Dataset       | Get drone footage |
| Update master thesis with current results and findings | Create orthophoto maps from drone-footage |

<!-- End Here -->
{{ page.horizontal }}

# [Print]({{ site.url }}{{ site.baseurl }}{{ page.url }}/?print-pdf#)

# [Back]({{ site.url }}{{ site.baseurl }})

</section></section>
