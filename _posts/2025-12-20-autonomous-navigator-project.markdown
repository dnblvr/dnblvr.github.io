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

- [1. AFAICT - Autonomous Navigation](#1-afaict---autonomous-navigation)
- [2. Hardware engineering](#2-hardware-engineering)
  - [2.1. Motor - Estimation and Control](#21-motor---estimation-and-control)

---

<br>

## 1. AFAICT - Autonomous Navigation

Over the summer, I've had the privilege to work with a beloved professor to build an autonomous navigation platform. This is a two-wheeled robot in an embedded systems environment.

This is how this task was performed.

## 2. Hardware engineering

The work I did centered around the MSP432 and various materials I had available to me.

Part of my job was the develop drivers to be able to use these sensors. One of the sensors wwas the budget-freindly RPLiDAR C1, which is a 360-degree 2D LiDAR sensor. As many people will tell you, this exact sensor has an uncooperative SDK that refuses to make itself clear. FOrtunately it was easy to understand it through the application notes.

Here's how it goes:

The sensor itself gives us a heavy stream of data sent over UART. Strong as this stream is that it is sent continuously at 460800 baud. This presents our first problem with the collection. While nice in theory as it is sent as fast as possible, the embedded system has to be able to process and use this data within the reasonable limits of the platform. This means the processing overhead has to be low enough, the temporary storage is not too large, and the processing time is not too long.

These decisions are downstream on what is needed from this data in the first place. How many points in the eventual point cloud is needed to express the landmark? How much data points are necessary while maintaining some measure of real-time performance?

I decided these decisions on these matters. from the data collection and processing perspective, the data is expected to be 80 points per scan, and at two-seconds between scans, this would be enough to perform a global scan-matching algorithm. In a sense, this part of the algorithm is meant to globalize the inherently dead-reckoning-based odometry data. If we record more than about 100 points per scan, we potentially collect more data and scan-match with a higher resolution but with little improvement in the quality, leading to diminishing returns. Here we recognize a balance in quality of the scan and the processing time.

Every 5 bytes read the distance and angle data. We have to scan-match the first few messages first to be able to read the concatenated data.

<!-- ```mermaid
---
title: "5-byte message Packet"
---
packet
+7: Sync 
8-15: Angle (MSB)
16-23: Angle (LSB)
24-31: Distance (MSB)
32-39: Distance (LSB)
``` -->

| Bits  | Field        |
|-------|--------------|
| 0-7   | Sync         |
| 8-15  | Angle (MSB)  |
| 16-23 | Angle (LSB)  |
| 24-31 | Distance (MSB) |
| 32-39 | Distance (LSB) |

From there we must be able to skip the bytes we don't need and read the data we do need. This is a more elaborate form of decimation that incorporates a state machine to read relevant indices of the data stream. The state machine exists in the first place with the notion that for each state, there is an associated limit that is reached before the state transitions.

```mermaid
---
title: State Diagram v1 to define where the limits are  (assumes all the limits have been reached)
---
stateDiagram-v2

    state HOLD_0
    state FIND_PATTERN_1
    state if_choice <<choice>>
    state ADD_OFFSET_2
    state SKIP_3
    state RECORD_4


    [*] --> HOLD_0: on init

    FIND_PATTERN_1 --> if_choice
    note left of FIND_PATTERN_1
      lim = 4*5
    end note

    if_choice --> FIND_PATTERN_1: if pattern not found
    if_choice --> ADD_OFFSET_2: if pattern found
    note left of ADD_OFFSET_2
      lim = 5 - offset
    end note

    ADD_OFFSET_2 --> SKIP_3

    SKIP_3 --> HOLD_0: print_counter >= BUFFER_SIZE
    SKIP_3 --> RECORD_4
    note left of SKIP_3
      lim = skip_factor*5
    end note

    RECORD_4 --> SKIP_3
    note right of RECORD_4
      lim = 5
    end note

    RECORD_4 --> HOLD_0: print_counter >= BUFFER_SIZE

    HOLD_0 --> FIND_PATTERN_1: after processing data
```
*credit: me*

The limits are the crucial part of this state machine. When the pattern-matching is achieved, the offset is added to the limit to be able to read the relevant data. Then, the skip factor is added to the limit to be able to skip the irrelevant data. Finally, the limit is set to the message length to be able to read the next relevant data. The cycle repeats until the buffer is full.

Further steps involve bit-packing the distance and angle data (both under 16 bits) into a single 32-bit integer for efficient storage and processing. Angle, in degrees, is concatenated in the upper 16 bits so that it is easier to sort the data by angle.

At this point, a binary insertion sort is used.

The decision to pare the amount of data further is compounded by the fact that the amount of data recorded has to cover all 360 degrees of circular travel. If we record more than 100 points per scan, we potentially collect more data and have to sort it for later decimation algorithms to return an even set of data. This again is a balancing act of the designer between the amount of scans performed and the coverage of the scan. The issue is if we record too few points, we might not have enough data to perform a good scan-matching pass.

### 2.1. Motor - Estimation and Control

In the world of state estimation and Kalman-filtering in specific, there are two maps of meaning to make comparisons between, as the wonderful explainer by bzarg has made central to [their teaching of the Kalman Filter](https://www.bzarg.com/p/how-a-kalman-filter-works-in-pictures).

There is the motion model between two points in **timespace**. In the case of a differential drive robot, let's say the state $x_n$ is a prediction of the motion model $f(\cdot)$ predicted from previous state $x_{n-1}$ and the actions performed by the two motors $u_n$ to arrive at $x_n$. It is inferred that there is there is a linear transformation that can map one state to the next with a different timeframe. In the SE(2) space which adequately describes the motion. Of course, as with any model, uncertainty has to be accounted for. actually, as the model progresses through timespace, the uncertainty increases. This uncertainty is accounted for as process noise which is added in to .

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

Both are represented with some level of noise and, in practical terms, a Gaussian noise. These noises are represented as ellipsoids whose shape can represent no correlation between any two states of the system, or it can be a covariance matrix.

<!-- The aforementioned task of correcting pose data can be accomplished with one type of data: odometry.

one characteristic of modeling a differential-drive robot like this is that the state of the system cannot be inferred using classical Netwonian mechanics. Each wheel rotation command is independent of the other and so a differential rotation speed between the two wheels implies a curved path of travel with radius $r$. Such movements are non-integrable (cannot be inferred from velocity and time measurements) and are **nonholomic**. -->

It was not simply important to control the movement of the robot i.e. what the control inputs are. the literature in GraphSLAM systems often state a hypothetical motion model. It helps to know that the motion model represents a rigid transformation model, or in Lie algebra parlance the SE(2) group, where the motion is described with a rotation matrix $R$ and a translation vector $t$. in general, x and y pose transformation is described:

$$
\begin{align}
\boldsymbol{T} (x_{n}) = \boldsymbol{R}(\theta) x_{n-1} + \boldsymbol{t}
\end{align}
$$

$$
\begin{align*}
\end{align*}
$$

This is where the Kalman Filter's logic comes in. The Kalman Filter makes use of the fact that both uncertainties are Gaussian and have their own mean and covariance. The intuition of the Kalman Filter posits that these two estimates, or distributions in a probabilistic sense, can be combined to give a better estimate of the state. If treated as the matrices that they are, the two uncertainties can be multiplied together to give a new distribution with a mean that is somewhere between the two and a covariance that is smaller than either of the two. This is the correction step of the Kalman Filter. Cribbing from the images of bzarg:

<center>
<!-- width="621" height="572" -->
<img alt="alt text"
     src="/assets/img/2025-12-20-autonomous-navigator/ANP_gauss_4.jpg"
     width="310" height="286">
<img alt="alt text"
     src="/assets/img/2025-12-20-autonomous-navigator/ANP_gauss_6a.png"
     width="310" height="286">
<br>*Credit: bzarg*
</center>
<br>
The predition mean and covariance, represented as a pink cloud, and the measurement mean and covariance, represented as a green cloud, meet at the middle to give a new estimate, represented as a white cloud.

On the updated state equation, this idea is transmitted through but in a slightly formulaic way. The added estimate $z_n - h(\hat{x}_n)$ is weighted by the Kalman Gain $K$, which is a ratio of the uncertainty in the measurement and the uncertainty in the state. The more certain the measurement is, the more it is weighted in the correction.

$$ \begin{align}
\hat{x}_n &= \hat{x}_n + K (z_n - h(\hat{x}_n)) \\
P_n &= (I - K H_n) P_n
\end{align} $$

<!-- ### 2.3. RPLiDAR C1 -->

<!-- ## 3. Software Algorithms -->

<!-- ### 3.1. Dynamic Window Approach -->

<!-- #### 3.1.1. Hybridized with MPC?? -->

<!-- ### 2.2. GraphSLAM -->

<!-- ## 4. The wider debate -->
