---
layout:         post
title:          Monte Carlo Localization Background
author:         Vegard Bergsvik Øvstegård
tags:           Localization
subtitle:       This post contains an overview and description of former work and terminology related to Monte Carlo Localization.
toc:            true
category:       mcl
---

# Robot localization
Robot localization refers to a robot's ability to establish its position and orientation within its frame of reference. Vision-based localization or optical localization uses computer vision techniques and algorithms, with optical sensors to extract features of the surrounding environment required to localize an agent.

# Particle filter
A Particle filter is a type of algorithm used to solve states in dynamical systems[@mclalg]. They are often called Monte Carlo algorithms as they rely on repeated random sampling to obtain results. In other words, these algorithms use randomness to solve problems that might be deterministic in principle. Our problem is locating the UAV on a map, and a Monte Carlo approach is to compare the environment visible to the UAV with random locations on the map. The probability of the UAV being in an area is related to how similar the environment is around the UAV and a given random sample in the map.

# Monte Carlo localization
Monte Carlo Localization, a.k.a particle filter localization[@mclalg], is an algorithm for robots to obtain pose and orientation. The algorithm uses a particle filter to represent probable states' distribution, where each particle represents a possible state, i.e., a hypothesis of where the robot is[@mclalg]. As the robot has no information about where it is and assumes any pose and orientation is equally likely, the algorithm typically starts with a uniform random distribution of the configuration space.
The particles shift whenever the robot moves to predict its next state. Every time the robot senses its environment, the particles are resampled based on how well the sensed data correlate with the predicted state. This resampling method is called recursive Bayesian estimation. Over time, the particles should converge towards the actual position of the robot[@mclalg].

## Description
Consider a robot with an internal map of its surroundings. As the robot moves around, it needs to know its position within said map. Predicting its pose and orientation by using its sensor observations is known as previously mentioned, robot localization.
Due to imperfections within the robot's sensors, its mechanics, and perhaps the environment's randomness, the movement might not behave entirely predictably.
For this reason, it generates many random guesses on where heading. These guesses are known as particles. Each particle contains a full description of a future state, such as orientation, coordinates, etc. When the robot does an observation, it discards the guesses inconsistent with what the robot can see and generates more particles of those that appear consistent. Over time, the idea is that most particles, or guesses, converge to where the robot is.

## Example of 1D robot:
Consider a robot in a 1D circular corridor with three identical doors, using a sensor that returns either true or false depending on whether there is a door.

### $t=0$
![The algorithm initializes with a uniform distribution of particles. The robot considers itself equally likely to be at any point in space along the corridor, even though it is physically at the first door.
](/img/Mcl_t_0_1.svg)

![Sensor update: the robot detects a door. It assigns a weight to each of the particles. The particles which are likely to give this sensor reading receive a higher weight.
](/img/Mcl_t_0_2.svg)

![Resampling: the robot generates a set of new particles, with most of them generated around the previous particles with more weight. It now believes it is at one of the three doors.
](/img/Mcl_t_0_3.svg)

### $t=1$

![Motion update: the robot moves some distance to the right. All particles also move right, and some noise is applied. The robot is physically between the second and third doors.
](/img/Mcl_t_1_1.svg)

![Sensor update: the robot detects no door. It assigns a weight to each of the particles. The particles likely to give this sensor reading receive a higher weight.
](/img/Mcl_t_1_2.svg)

![Resampling: the robot generates a set of new particles, with most of them generated around the previous particles with more weight. It now believes it is at one of two locations.
](/img/Mcl_t_1_3.svg)


### $t=2$

![Motion update: the robot moves some distance to the left. All particles also move left, and some noise is applied. The robot is physically at the second door.
](/img/Mcl_t_2_1.svg)

![Sensor update: the robot detects a door. It assigns a weight to each of the particles. The particles likely to give this sensor reading receive a higher weight.
](/img/Mcl_t_2_2.svg)

![Resampling: the robot generates a set of new particles, with most of them generated around the previous particles with more weight. The robot has successfully localized itself.
](/img/Mcl_t_2_3.svg)


At the end of the three iterations, most of the particles are converged on the actual position of the robot as desired.

## Algorithm:
Given an *a priori* known map of the environment, the goal of the algorithm is for the robot to determine its pose within the environment.

At every time $t$ the algorithm takes as input the previous belief ${\displaystyle X_{t-1}=\lbrace x_{t-1}^{[1]},x_{t-1}^{[2]},\ldots ,x_{t-1}^{[M]}\rbrace }$, an actuation command $u_{t}$, and data received from sensors $z_{t}$; and the algorithm outputs the new belief $X_{t}$.[@mclalg]

![Monte Carlo Localization algorithm [@mclalg]](/img/mclalg.png)


## GIL Simulation:

Below is a short simulation of the GIL-framework demonstrating MCL. Red dot is UAV and black dots are particles.

<iframe src="https://drive.google.com/file/d/1mx2gudpn-xdRaBKmrMXjMMrKQjHGAlqx/preview" width="640" height="480" allowfullscreen="true"></iframe>

