---
layout: page
title: DTA-VALVE
description: Digital twin prototype for automated pre-assembly of thermo-expansion valves at Parker Hannifin. Project developed @ TEC de Monterrey.
img: assets/img/projects/3_parker/plant_process.jpg
# redirect: https://www.wikipedia.org/
importance: 2
category: academic
---

## Overview
 
Parker Hannifin's Valve Division (SVD) relies on a manual, high-complexity process to assemble and test thermo-expansion valves — one that involves delicate manipulation of small components such as brazing rings. This project developed a **digital twin prototype for the automated pre-assembly stage** of that process, aiming to reduce human involvement, keep investment cost low, and preserve flexibility to adapt the line to other valve types.
 
The scope covers the assembly sequence from component press-fitting through to furnace entry: pressing the inlet, outlet, and external fittings onto the valve body, followed by transfer toward the oven — the stages prior to gas charging and batch testing, which were left for future work.
 
<div class="row justify-content-sm-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/3_parker/process_view2.jpg" title="process parker" class="img-fluid rounded z-depth-1" %}
    </div>
    <div class="col-sm-4 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/3_parker/template.jpeg" title="template parker valve" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    Left, automated process design. Right, layout of the process
</div>

## Proposed Solution
 
The redesign runs two processes in parallel that converge onto a single conveyor feeding the furnace:
 
- **Indexing table** — performs the press-fitting of the inlet, outlet, and external components (with their respective rings) onto the valve body, using horizontal cylinders and slides.
- **Intelligent tray supply** — in parallel, positions the lower components and their rings into each available slot on the trays advancing toward the furnace.
- **Delta robot integration** — a delta robot removes the assembled parts from the indexing table and executes two pick-and-place cycles: the first places the part over the fluxing zone, and the second places it onto the tray positions moving toward the furnace.
## My Contribution
 
This was a team project with shared contributions across requirements analysis, layout definition, and CAD design. **I led the PLC ladder logic development**, translating the mechanical and process design into the control logic that sequenced the indexing table, cylinders, and robot handoffs.
 
- **PLC programming (Ladder Logic)** — led
- **CAD design** — shared contribution across the team
- **Layout and solution search** — shared contribution across the team
- **Simulation** — shared contribution across the team

## Simulation & Validation
 
The proposed line was validated in simulation before any physical implementation:
 
- **Process Simulate** — used to validate robot reachability, cycle sequencing, and station-level interactions
- **Plant Simulation** — used to model line throughput and compare the proposed automated process against the current manual baseline
<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        <div class="ratio ratio-16x9">
            <iframe src="https://www.youtube.com/embed/a8Jq7TYmHYY" title="Digital Twin — Automated Valve Pre-Assembly" frameborder="0" allow="accelerometer; autoplay; clipboard-write; encrypted-media; gyroscope; picture-in-picture" allowfullscreen></iframe>
        </div>
    </div>
</div>
<div class="caption">
    Simulated process walkthrough of the automated pre-assembly line.
</div>

## Full Report
 
The complete report (in Spanish), covering requirements analysis, layout evaluation, mechanical design, industrial component selection, process design, PLC logic, simulation, and comparison against the current manual process, is available below.
 
<div class="d-flex justify-content-center mt-3 mb-3">
    <a href="assets/pdf/projects/3_parker_report.pdf" class="btn btn-outline-secondary" target="_blank">
        <i class="fa-solid fa-file-pdf me-1"></i> Download Full Report (PDF, Spanish)
    </a>
</div>

## Technical Stack
 
**Mechanical Design**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">CAD Design</span>
    <span class="badge rounded-pill bg-secondary">Industrial Component Selection</span>
    <span class="badge rounded-pill bg-secondary">Solidworks</span>
</div>

**Automation & Control**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">PLC Programming</span>
    <span class="badge rounded-pill bg-secondary">Ladder Logic</span>
    <span class="badge rounded-pill bg-secondary">Unifiliar Diagram</span>
</div>

**Process Engineering**
<div class="mb-3">
    <span class="badge rounded-pill bg-secondary">Layout Design</span>
    <span class="badge rounded-pill bg-secondary">Requirements Analysis</span>
</div>
