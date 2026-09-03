---
layout: page
title: ASMoV Robot
description: Autonomous robotic platform for vegetation clearing and maintenance in electrical substations
img: assets/img/projects/1_asmov_robot/robot_in_electric_station_v2.JPG
importance: 1
category: work
related_publications: false
---

## Overview
 
ASMOV is an autonomous ground robot developed at **Innovo** (Lima, Peru) to perform vegetation clearing and maintenance tasks inside electrical substations — a hazardous, safety-critical environment near live high-voltage equipment. The project was developed across two prototypes, progressing from mechanical/electrical validation to a fully autonomous coverage system.
 
<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/1_asmov_robot/asmov_principal_v2.JPG" title="ASMOV in electric substation" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    ASMoV in electric substation.
</div>

## Prototype I — Design Validation
 
The first prototype focused on validating the robot's core physical platform. **I contributed to the electronics, power electronics, and safety circuit design** during this phase, alongside:
 
- Mechanical, electrical, and electronic design of the base platform
- Power electronics and safety circuit design for reliable operation near energized equipment
- Integration and validation of the primary sensor suite
- Baseline testing of the robot's ability to operate in a substation-like environment

## Prototype II — Autonomyasmov_Renderasmov
 
The second prototype shifted focus toward full autonomy. **I led the development of this phase**, which included:

- **Navigation control** — Control of navigation using GNSS-RTK as principal sensor
- **Coverage path planning** — algorithms for systematically covering the vegetation-clearing area, defining how the robot approached and executed the task
- **Software architecture** — design of the overall autonomy stack integrating perception, planning, and control
- **Computer vision** — instance segmentation pipeline for identifying vegetation and operational boundaries
- **Low-level CAN networking** — communication architecture between onboard modules and actuation systems
- **Redundancy and safety systems** — critical given the robot's proximity to energized substation equipment

As part of the second prototype, we developed a mini version of our robot to validate the navigation stack. Here are some photos of the mini ASMoV.

<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/1_asmov_robot/p2_asmov_mini.jpg" title="Mini ASMoV view1" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/1_asmov_robot/p2_asmov_mini2.jpg" title="Mini ASMoV view2" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Mini ASMoV ready to be tested at electric substation.
</div>


The final robot was tested initially in an environment different from the final one, and later at the electric substation.

<div class="row">
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/1_asmov_robot/p2_working.jpg" title="working on asmov" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/1_asmov_robot/p2_robot_in_electric_station.jpeg" title="asmov" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm mt-3 mt-md-0">
        {% include figure.liquid loading="eager" path="assets/img/projects/1_asmov_robot/p2_asmov_render.JPG" title="asmov render" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    ASMoV Robot. Working at wiring with <a href='https://pe.linkedin.com/in/esau-vladimir-arqueros-plasencia-062802188'>Esau Arqueros</a>. Robot in electric substation. Final robot
</div>

## Technical Stack
 
**Robotics Middleware & Software**
- ROS2
- C++ / Python
- CAN bus (low-level communication with onboard modules and actuators)
- STM32 (Microcontrollers)
**Electronics & Power Electronics**
- Power distribution and actuation circuit design
- Dedicated safety circuits for hazardous-environment operation
**Perception & Computer Vision**
- Instance segmentation
- PyTorch / OpenCV
- NVIDIA Jetson (embedded AI deployment)
**Navigation & Localization**
- GNSS-RTK for precise outdoor positioning
- Stereo Camera / Point Cloud
- Coverage path planning algorithms / Graphs
**Safety & Reliability**
- Redundant sensing and fail-safe design for operation near energized equipment

## Role & Impact
 
As lead of the second prototype, I was responsible for taking the platform from a manually validated base into an autonomous system capable of planning and executing its own coverage task in a constrained, safety-critical environment. I also contributed to the electronics and safety-critical circuit design established in the first prototype.
 
---
 
*Due to a confidentiality agreement with Innovo, technical designs, schematics, and source code from this project cannot be shared publicly.*