---
layout: page
title: Road marking maintenance robot
description: Final Year Project @ UTEC - Implementation of autonomy algorithms for a road marking maintenance robot
img: assets/img/projects/2_road_maintenance/robot_in_car_park.jpeg
importance: 1
category: academic
related_publications: true
# giscus_comments: true
---

## Overview
 
**Implementation of Autonomy Algorithms for the Maintenance of Flat Pavement Markings Using a Mobile Robot** {% cite cabeza2025pavementmarking %} — Bachelor's Thesis  (Trabajo de Investigación) in Mechatronics Engineering, UTEC.
 
Horizontal traffic markings are essential for road safety and traffic organization, but they wear down over time and require routine maintenance. Traditional maintenance methods face recurring problems: subjective visual evaluation, occupational safety risks for workers, and traffic disruption during inspection. This project integrates detection, evaluation, localization, and control algorithms into an existing mobile robot platform to automate the monitoring and maintenance of pavement markings, building on it with hardware upgrades and a full autonomy stack developed from scratch.
 

<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/2_road_maintenance/robot_2.jpg" title="Mobile robot platform for pavement marking maintenance" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     Mobile robot platform used for pavement marking evaluation and maintenance.
</div>

## Motivation
 
Automating routine pavement marking maintenance offers clear advantages over manual inspection: it reduces the need for human intervention in traffic-exposed conditions, lowering occupational risk while increasing operational efficiency. An autonomous system can also operate continuously, including during low-traffic night hours, improving maintenance frequency and standardizing how wear is evaluated — replacing subjective human judgment with consistent, repeatable criteria. Beyond maintenance itself, the ability to continuously monitor pavement conditions and generate real-time data contributes to broader smart-city infrastructure planning, enabling maintenance to be scheduled proactively rather than reactively.
 
## Objectives
 
**General objective:** Implement autonomy algorithms for a mobile robot that performs maintenance of linear pavement markings.
 
**Specific objectives:**
- Adapt the robot's electrical system to support marking maintenance via an electric water dispenser
- Implement a localization algorithm and environment mapping to support autonomous navigation
- Develop a recognition and evaluation model to assess the wear condition of horizontal markings
- Design and implement a trajectory generation algorithm and kinematic control based on the position of degraded markings
- Validate the algorithms and navigation through simulation and real-world testing
## Methodology
 
The system was developed in three sequential stages, each handling a distinct part of the autonomy pipeline.

<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/2_road_maintenance/metodologia.jpg" title="Methodology" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
     Methodology of algorithms used in robot (Diagram in Spanish).
</div>
 
### Stage 1 — Recognition & Mapping
 
Two standard cameras in a stereo configuration provide depth information, on top of which ORB-SLAM is applied for visual localization and mapping. This is fused with encoder and IMU measurements through an Extended Kalman Filter (EKF) for robot state estimation. In parallel, morphological filtering on the stereo/mapping output produces an occupancy grid used for obstacle-aware navigation.
 
<div class="row justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/2_road_maintenance/etapa1.jpg" title="Etapa 1: Reconocimiento y mapeo" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Stage 1 module diagram: stereo-based mapping, ORB-SLAM, EKF fusion, and occupancy grid generation (diagram in Spanish).
</div>

### Stage 2 — Processing & Interest Zone Computation
 
A YOLO11 model, trained for instance segmentation, classifies pavement lines as good or worn directly from the camera feed. Inverse perspective mapping is then applied to the detections, followed by instance sampling. Using only the information from the detected lines, points of interest are identified by evaluating the worn area against an adaptive threshold.
 
 <div class="row justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/2_road_maintenance/etapa2.jpg" title="Etapa 2: Procesamiento y cálculo de zonas de interés" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Stage 2 module diagram: YOLO11 instance segmentation, inverse perspective mapping, and adaptive-threshold interest point extraction (diagram in Spanish).
</div>

### Stage 3 — Trajectory Planning & Execution
 
The points of interest from Stage 2 are clustered using DBSCAN to isolate the line segments that require maintenance. A line fit is applied to each cluster, and the most efficient route between segments is computed using a branch-and-bound approach over the resulting graph. The robot then tracks this route with a Model Predictive Control (MPC) scheme for kinematic control, using the occupancy grid from Stage 1 as a navigation constraint, while marking execution is simulated through the robot's water-dispenser mechanism. Designing and manufacturing this dispensing mechanism was outside the scope of the project, which focused on the autonomy algorithms rather than the marking hardware itself.
 
  <div class="row justify-content-center">
    <div class="col-sm-10 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/2_road_maintenance/etapa3.jpg" title="Etapa 3: Planificación y ejecución de trayectoria" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Stage 3 module diagram: DBSCAN clustering, line fitting, branch-and-bound route optimization, and MPC trajectory tracking (diagram in Spanish).
</div>

## Scope & Constraints
 
- The project reuses an existing differential-drive mobile robot platform, upgraded at the hardware level, rather than developing a new mechanical/electrical base from scratch
- Perception relies solely on cameras (no LiDAR), balancing cost against accuracy while accounting for natural and artificial lighting conditions
- Validation was carried out in controlled environments, assuming blocks free of vehicular or pedestrian traffic
- The painting mechanism was replaced with an electric water dispenser to avoid unauthorized alteration of the pavement during testing
- The markings addressed are limited to berm, center, and lane lines

## Technical Stack
 
<p class="font-semibold mb-2">Perception & Localization</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Extended Kalman Filter</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">ORB-SLAM3</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Stereo Camera</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">IMU</span>
</div>

<p class="font-semibold mb-2">Computer Vision</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">YOLO11</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Instance Segmentation</span>
</div>

<p class="font-semibold mb-2">Planning & Control</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">DBSCAN</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Branch-and-Bound</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Model Predictive Control</span>
</div>

<p class="font-semibold mb-2">Hardware</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Differential Drive Platform</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Electric Water Dispenser</span>
</div>

## Team & Contribution
 
This thesis was developed together with <a href="https://www.linkedin.com/in/manuelcarita/" target="_blank">Manuel Carita</a>, with a comparable overall workload split across the three stages:
 
- **Stage 2 (Processing & Interest Zone Computation)** — fully led by me
- **Stage 3 (Trajectory Planning & Execution)** — led by me, excluding the MPC trajectory tracking
- **Stage 1 (Recognition & Mapping) and MPC control** — led by Manuel Carita

## Full Report, Code & Presentation
 
The complete thesis document (in Spanish), the presentation slides from the defense, and a portion of the codebase — focused on my own contribution to the system — are available below.
 
<div class="flex flex-wrap justify-center gap-3 my-4">
    <a href="../../assets/pdf/projects/2_informe.pdf" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-solid fa-file-pdf"></i> Download Full Thesis (PDF, Spanish)
    </a>
    <a href="../../assets/pdf/projects/2_presentacion.pdf" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-solid fa-file-powerpoint"></i> Download Presentation (PDF, Spanish)
    </a>
    <a href="https://github.com/eliascm07/roadmarking-maintenance-robot" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-brands fa-github"></i> View Code on GitHub
    </a>
</div>
 