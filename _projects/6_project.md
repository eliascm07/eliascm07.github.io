---
layout: page
title: Impedance Control for Open Manipulator
description: Advanced Robotics Course Project @ UTEC. Implementation of impedance control for Open Manipulator
img: assets/img/projects/6_impedance/control.gif
importance: 4
category: academic
---

## Overview
 
Compliance — a robot's ability to react to external forces rather than rigidly rejecting them — is central to safe and effective physical human-robot interaction. It underlies collaborative manufacturing, delicate manipulation in healthcare, and robots operating in unstructured, contact-rich environments. This project, developed for the Advanced Robotics course at UTEC, implements **model-based impedance control** on the **Open Manipulator-X**, a 4-DoF manipulator built on Dynamixel servomotors.
 
<div class="row justify-content-center">
    <div class="col-sm-8 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/6_impedance/control.gif" title="Impedance control on the Open Manipulator-X" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    The Open Manipulator-X end effector responding compliantly to external disturbances under impedance control.
</div>

## Motivation
 
Position or torque control alone perform poorly the moment a manipulator makes contact with its environment: any deviation from the reference trajectory is treated purely as error to reject, which produces stiff, unsafe interaction forces. Impedance control instead regulates the *dynamic relationship* between the end-effector's motion and the force applied to it — effectively imposing a programmable virtual spring-damper-mass behavior at the end effector, rather than tracking a rigid trajectory. This makes it a natural fit anywhere a robot must both move accurately and yield safely on contact, from assembly tasks to legged locomotion, where a compliant response to ground reaction forces is what keeps contact stable and absorbs impact.
 
## Objectives
 
**General objective:** Implement and validate model-based impedance control on a manipulator that lacks force/torque sensing, using its known dynamic model in place of direct force feedback.
 
**Specific objectives:**
- Derive the impedance control law from the manipulator's dynamic model and Jacobian, for the case where no F/T sensor is available
- Implement the control law as a ROS controller on the Open Manipulator-X
- Characterize the closed-loop response of the end effector under different virtual stiffness and damping settings
- Validate compliant behavior experimentally against external disturbances applied to the end effector

## Methodology
 
### Impedance Control
 
Mechanical impedance relates the force at the end effector, $$F(s)$$, to its velocity, $$V(s)$$:
 
$$
Z(s) = \frac{F(s)}{V(s)} = \frac{F(s)}{sX(s)}
$$
 
This relationship can be shaped to behave like a mass-spring-damper system, with inertia $$m$$, damping $$b$$, and stiffness $$k$$:
 
$$
ms^2X(s) + bsX(s) + kX(s) = F(s) \quad \Rightarrow \quad Z(s) = sm + b + \frac{k}{s}
$$
 
Generalizing this to a desired impedance around a reference trajectory $$x_d$$ gives the target closed-loop dynamics:
 
$$
M_d(\ddot{x}-\ddot{x}_d) + B_d(\dot{x}-\dot{x}_d) + K_d(x-x_d) = F_A
$$
 
where $$M_d$$, $$B_d$$, and $$K_d$$ are the desired (virtual) inertia, damping, and stiffness matrices, and $$F_A$$ is the external force applied at the end effector.
 
### Model-Based Formulation (No Force/Torque Sensor)
 
The Open Manipulator-X's rigid-body dynamics are:
 
$$
M\ddot{x} + C\dot{x} + g = \tau
$$
 
with $$M$$ the inertia matrix, $$C$$ the Coriolis term, $$g$$ the gravity vector, and $$\tau$$ the joint torques. Combining this with the target impedance dynamics through inverse dynamics yields the general impedance control law:
 
$$
\tau = M_A J_A^{-1}\left[\ddot{x} - \dot{J}_A\dot{q} + M_d^{-1}\big(B_d(\dot{x}_d-\dot{x}) + K_d(x_d-x)\big)\right] + C\dot{q} + g + J_A^{T}(M_xM_d^{-1}-I)F_A
$$
 
The Open Manipulator-X has no force/torque sensing at the end effector, which normally rules out this kind of law. However, because its full URDF and dynamic parameters are open source, $$F_A$$ can be eliminated analytically by choosing the special case $$M_d = M_x = J_A^{-T}MJ_A^{-1}$$, which cancels the force-feedback term entirely and leaves a purely model-based control law:
 
$$
\tau = M_AJ_A^{-1}\left[\ddot{x} - \dot{J}_A\dot{q} + M_d^{-1}\big(B_d(\dot{x}_d-\dot{x}) + K_d(x_d-x)\big)\right] + C\dot{q} + g
$$
 
This is what makes impedance control achievable on hardware with no dedicated F/T sensor: compliant behavior is imposed entirely through the known dynamic model, the manipulator Jacobian, and the choice of virtual $$B_d$$, $$K_d$$ gains.
 
### Implementation
 
The controller was implemented in C++ as a ROS node, extending ROBOTIS's own gravity-compensation controller for the Open Manipulator-X rather than building a controller from scratch:
 
```cpp
// Inverse Jacobian
J_pos_pinv_ = J_pos_.completeOrthogonalDecomposition().pseudoInverse();
 
// Desired inertia matrix (special case: Md = Mx)
Md_ = J_pos_pinv_.transpose() * M_.data * J_pos_pinv_;
 
auto imposicion = Md_.inverse() * (Bd_ * (desired_x_dot_ - x_dot_eigen_) +
                                    Kd_ * (desired_x_ - x_eigen_));
 
auto dinamica = desired_x_dot_dot_ - J_pos_dot_ * q_dot_.data + imposicion;
 
tau_.data = M_.data * J_pos_pinv_ * dinamica + C_.data + G_.data;
```
 
The node runs alongside the stock `open_manipulator_controller`, publishing the computed torques to the Dynamixel joints in place of the default position/gravity-compensation behavior.
 
### Running the Controller
 
The impedance controller builds on top of ROBOTIS's own gravity-compensation controller package for the Open Manipulator-X. To run it:
 
1. Clone the base controller package into your catkin workspace:
```bash
$ cd ~/catkin_ws/src/
$ git clone https://github.com/ROBOTIS-GIT/open_manipulator_controls.git
$ cd ~/catkin_ws && catkin_make
```
 
2. Locate the gravity-compensation controller source file inside the cloned package's `/src` directory, and insert the impedance control code shown above in place of (or alongside) the default gravity-compensation logic.
3. Launch the standard Open Manipulator controller together with the modified gravity-compensation/impedance controller:
```bash
roslaunch open_manipulator_controller open_manipulator_controller.launch
roslaunch open_manipulator_controllers gravity_compensation_controller.launch
```

## Results
 
The controller was tested under different virtual stiffness ($$K_d$$) and damping ($$B_d$$) settings, applying external forces by hand to the end effector and observing the resulting compliant motion. Lower stiffness produced a visibly softer, more yielding response to contact, while higher stiffness kept the end effector closer to its reference trajectory under the same disturbance — the expected trade-off between compliance and tracking accuracy that impedance control is designed to expose as a tunable parameter rather than a fixed compromise.
 
<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        <iframe src="https://www.youtube.com/embed/r9iU3OYlKqc" style="width: 100%; aspect-ratio: 16/9; border: 0;" allowfullscreen></iframe>
    </div>
</div>
<div class="caption">
    Compliant response of the Open Manipulator-X end effector under different impedance gains.
</div>

## Scope & Constraints
 
- The written article frames impedance control's relevance partly through legged locomotion, where compliant contact with the ground is essential — but the implementation and all experimental validation in this project were carried out on the Open Manipulator-X arm, not on a legged platform
- No force/torque sensor is available on the hardware, so all compliant behavior comes from the model-based reformulation above rather than direct force feedback
- Gains ($$M_d$$, $$B_d$$, $$K_d$$) were tuned experimentally rather than derived from a formal stability or performance criterion

## Technical Stack
 
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Impedance Control</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Inverse Dynamics</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">ROS</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">C++</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Open Manipulator-X</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Dynamixel</span>
</div>

## Team & Contribution
 
This project was developed for the Advanced Robotics course at UTEC together with <a href="https://www.linkedin.com/in/manuelcarita/" target="_blank">Manuel Carita</a>, César Delgado, and Jairo Custodio.
 
## Full Report & Code
 
The complete article (in Spanish) and the controller implementation are available below.
 
<div class="flex flex-wrap justify-center gap-3 my-4">
    <a href="../../assets/pdf/projects/6_final.pdf" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-solid fa-file-pdf"></i> Download Article (PDF, Spanish)
    </a>
    <a href="https://github.com/manul30/ImpedanceControl_OpenManipulator" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-brands fa-github"></i> View Code on GitHub
    </a>
</div>