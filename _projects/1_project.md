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
 
ASMoV is an autonomous ground robot developed at **Innovo** (Lima, Peru) to perform vegetation clearing and maintenance tasks inside electrical substations — a hazardous, safety-critical environment near live high-voltage equipment. The project was developed across two prototypes, progressing from mechanical/electrical validation to a fully autonomous coverage system.
 
<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
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
- Manual Teleoperation by Radio Frecuency


## Prototype II — Autonomy
 
The second prototype shifted focus toward full autonomy. **I led the development of this phase**, which included:

- **Navigation control** — Control of navigation using GNSS-RTK as principal sensor
- **Coverage path planning** — algorithms for systematically covering the vegetation-clearing area, defining how the robot approached and executed the task
- **Software architecture** — design of the overall autonomy stack integrating perception, planning, and control
- **Computer vision** — instance segmentation pipeline for identifying vegetation and operational boundaries
- **Low-level CAN networking** — communication architecture between onboard modules and actuation systems
- **Redundancy and safety systems** — critical given the robot's proximity to energized substation equipment

As part of the second prototype, we developed a mini version of our robot to validate the navigation stack. Here are some photos of the mini ASMoV.

<div class="row justify-content-sm-center">
    <div class="col-sm-4 mt-3 mt-md-0">
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
    ASMoV Robot. Left, working at wiring with <a href='https://pe.linkedin.com/in/esau-vladimir-arqueros-plasencia-062802188'>Esau Arqueros</a>. Middle, robot in electric substation. Right, final robot
</div>


## Technical Stack

<p class="font-semibold mb-2">Robotics Middleware & Software</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">ROS2</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">C++</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Python</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">CAN Bus</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">STM32</span>
</div>

<p class="font-semibold mb-2">Electronics & Power Electronics</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Power Electronics</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Circuit Design</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Safety Circuits</span>
</div>

<p class="font-semibold mb-2">Perception & Computer Vision</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Instance Segmentation</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">PyTorch</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">OpenCV</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">NVIDIA Jetson</span>
</div>

<p class="font-semibold mb-2">Navigation & Localization</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">GNSS-RTK</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Stereo Camera</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Point Cloud</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Coverage Path Planning</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Graphs</span>
</div>

<p class="font-semibold mb-2">Safety & Reliability</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Redundant Sensing</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Fail-Safe Design</span>
</div>

## Role & Impact
 
As lead of the second prototype, I was responsible for taking the platform from a manually validated base into an autonomous system capable of planning and executing its own coverage task in a constrained, safety-critical environment. I also contributed to the electronics and safety-critical circuit design established in the first prototype.
 
---

 <div class="alert alert-secondary d-flex align-items-center mt-4" role="alert">
    <i class="fa-solid fa-lock me-2"></i>
    <div style="font-style: italic;">
         Due to a confidentiality agreement with Innovo, technical designs, schematics, and source code from this project cannot be shared publicly
    </div>
</div>
