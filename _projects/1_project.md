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
 
**Robotics Middleware & Software**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">ROS2</span>
    <span class="badge rounded-pill bg-secondary">C++</span>
    <span class="badge rounded-pill bg-secondary">Python</span>
    <span class="badge rounded-pill bg-secondary">CAN Bus</span>
    <span class="badge rounded-pill bg-secondary">STM32</span>
</div>

**Electronics & Power Electronics**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">Power Electronics</span>
    <span class="badge rounded-pill bg-secondary">Circuit Design</span>
    <span class="badge rounded-pill bg-secondary">Safety Circuits</span>
</div>

**Perception & Computer Vision**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">Instance Segmentation</span>
    <span class="badge rounded-pill bg-secondary">PyTorch</span>
    <span class="badge rounded-pill bg-secondary">OpenCV</span>
    <span class="badge rounded-pill bg-secondary">NVIDIA Jetson</span>
</div>

**Navigation & Localization**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">GNSS-RTK</span>
    <span class="badge rounded-pill bg-secondary">Stereo Camera</span>
    <span class="badge rounded-pill bg-secondary">Point Cloud</span>
    <span class="badge rounded-pill bg-secondary">Coverage Path Planning</span>
    <span class="badge rounded-pill bg-secondary">Graphs algorithms</span>
</div>

**Safety & Reliability**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">Redundant Sensing</span>
    <span class="badge rounded-pill bg-secondary">Fail-Safe Design</span>
</div>



## Role & Impact
 
As lead of the second prototype, I was responsible for taking the platform from a manually validated base into an autonomous system capable of planning and executing its own coverage task in a constrained, safety-critical environment. I also contributed to the electronics and safety-critical circuit design established in the first prototype.
 
---

 <div class="alert alert-secondary d-flex align-items-center mt-4" role="alert">
    <i class="fa-solid fa-lock me-2" style="font-style: italic;"></i>
    <div>
         Due to a confidentiality agreement with Innovo, technical designs, schematics, and source code from this project cannot be shared publicly
    </div>
</div>
