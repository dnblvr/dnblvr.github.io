---
layout: post
title: "Autonomous Navigator Project"
date: 2025-12-20 16:26:00 -0800
categories: jekyll update
tags: afaict
toc: true
---

<!-- omit in toc -->
# Contents

<!-- markdownlint-disable MD033 -->
<!-- markdownlint-disable MD025 -->

- [1. AFAICT - 2D LiDAR Mapping](#1-afaict---2d-lidar-mapping)
- [2. Hardware engineering](#2-hardware-engineering)
  - [2.1. UART Friend](#21-uart-friend)
  - [2.2. Motor - Estimation and Control](#22-motor---estimation-and-control)
  - [2.3. RPLiDAR C1](#23-rplidar-c1)
- [3. Software Algorithms](#3-software-algorithms)
  - [3.1. Dynamic Window Approach](#31-dynamic-window-approach)
    - [3.1.1. Hybridized with MPC??](#311-hybridized-with-mpc)
  - [3.2. GraphSLAM](#32-graphslam)
- [4. The wider debate](#4-the-wider-debate)

---

<br>

## 1. AFAICT - 2D LiDAR Mapping

Over the summer, I built an autonomous navigator. This is a two-wheeled robot in an embedded systems environment.

This is how this task was performed.

## 2. Hardware engineering

The work I did centered around the MSP432 and various materials I had available to me.

part of my job was the develop drivers to be able to use these sensors.


### 2.1. UART Friend


### 2.2. Motor - Estimation and Control

In the world of state estimation, there are two maps of meaning to make comparisons between, as the wonderful explainer by bzarg has made central to their teaching of the Kalman Filter.

There is the motion model between two points in **timespace**. In the case of a differential drive robot, let's say the state $x_n$ is a prediction of the motion model $f(\cdot)$ predicted from previous state $x_{n-1}$ and the actions performed by the two motors $u_n$ to arrive at $x_n$. It is inferred that there is there is a linear transformation that can map one state to the next with a different timeframe. of course, as with any model, uncertainty has to be accounted for. actually, as the model progresses through timespace, the uncertainty increases

the new model is below.

$$
\begin{align}
x_n &= \begin{pmatrix}
          x      \\
          y      \\
          \theta
      \end{pmatrix} \\
    &= \notag
\end{align}
$$

THe other is the expected measurements of the sensor space to the actual model space. The uncertainty in noise is also apparent.

Both are represented with some level of noise and, in practical terms, a gaussian noise. These noises are represented as ellipsoids whose shape can represent no correlation between any two states of the system, or it can be a covariance matrix.

The aforementioned task of correcting pose data can be accomplished with one type of data: odometry.

one characteristic of modeling a differential-drive robot like this is that the state of the system cannot be inferred using classical Netwonian mechanics. Each wheel rotation command is independent of the other and so a differential rotation speed between the two wheels implies a curved path of travel with radius $r$. Such movements are non-integrable (cannot be inferred from velocity and time measurements) and are **nonholomic**.

It was not simply important to control the movement of the robot i.e. what the control inputs are. the literature in GraphSLAM systems often state a hypoethetical motion model. It helps to know that the motion model represents a rigid transformation model where the motion is described with a transformation matrix $T$. in general, x and y pose transformation is described:

$$
\begin{align}
\boldsymbol{T} (x_{n}) = \boldsymbol{R}(\theta) x_{n-1} + \boldsymbol{t}
\end{align}
$$

$$
\begin{align*}
\end{align*}
$$

### 2.3. RPLiDAR C1

## 3. Software Algorithms

### 3.1. Dynamic Window Approach

#### 3.1.1. Hybridized with MPC??

### 3.2. GraphSLAM

## 4. The wider debate
