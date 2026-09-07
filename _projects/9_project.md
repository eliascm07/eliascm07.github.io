---
layout: page
title: Omnidirectional Robot with Manipulator
description: Control of omnidirectional robot for manipulation tasks
img: assets/img/projects/9_fdr_robot/robot_.gif
importance: 6
category: academic
# giscus_comments: false
---


## Overview
 
Mobile manipulators — an omnidirectional mobile base paired with an articulated arm — combine the reach of a mobile platform with the dexterity of an industrial manipulator, extending the effective workspace well beyond what either system offers on its own. This makes them a natural fit for security and demining, space operations, construction, personal assistance, and hazardous-material handling. This project, developed for the Fundamentos de Robótica course at UTEC, targets a more specific case of that same idea: **automating storage and material-handling tasks in a warehouse setting**, using a KUKA KR4 Agilus arm mounted on a Neobotix MPO-500 omnidirectional base.
 
<div class="row justify-content-center">
    <div class="col-sm-12 mt-3 mt-md-0">
        {% include figure.liquid path="assets/img/projects/9_fdr_robot/robot_.gif" title="Mobile manipulator: KUKA KR4 Agilus on a Neobotix MPO-500 base" class="img-fluid rounded z-depth-1" %}
    </div>
</div>
<div class="caption">
    KUKA KR4 Agilus arm mounted on the Neobotix MPO-500 omnidirectional platform, simulated in Gazebo.
</div>

## Hardware Selection
 
Component selection was based on manufacturer datasheets for both platforms, targeting a physically realizable system rather than a purely simulated one.
 
**Manipulator arm — KUKA KR4 Agilus (6 DoF)**
- Brushless servomotors, chosen for zero mechanical friction, lower heat generation, and higher performance
- Position sensing via encoders integrated directly into the brushless motors
- **KR C5 micro** controller, integrated into the robot structure, valued for its compact size, connectivity, and onboard processing

**Mobile base — Neobotix MPO-500 (omnidirectional)**
- Stepper motors for high positioning precision and versatility
- Optical incremental encoders factory-coupled to the stepper motors
- **Raspberry Pi 4** as the central controller, chosen for its native ROS compatibility
- H-bridge power drivers to regulate stepper motor speed and direction
- Four **Mecanum wheels**, with passive rollers mounted at 45° to the wheel's rotation axis, enabling diagonal force application and holonomic motion
- **24V, 50Ah sealed AGM lead-acid battery pack**, selected for high current delivery, fast recharge, and easy replacement

## Modeling & Simulation
 
The official CAD files from KUKA (KR4 Agilus) and Neobotix (MPO-500) were assembled into a single model in **Autodesk Fusion 360**, from which a **URDF** description was generated for simulation. Kinematic and dynamic behavior was visualized in **RViz** and physically simulated in **Gazebo** under ROS. To reproduce the base's holonomic motion correctly in Gazebo, the `ros_planar_move` plugin was used, which models holonomic-system kinematics directly rather than approximating it through a differential-drive plugin.
 
## Kinematic Modeling of the Manipulator
 
### Forward Kinematics (Denavit-Hartenberg)
 
Forward kinematics maps the robot's joint coordinates to the position and orientation of its end effector relative to an inertial base frame. The conventional **Denavit-Hartenberg (D-H)** method was applied using the KUKA KR4 Agilus's workspace dimensions:
 
| Joint ($$i$$) | $$d_i$$ (mm) | $$\theta_i$$ (rad) | $$a_i$$ (mm) | $$\alpha_i$$ (°) |
|:---:|:---:|:---:|:---:|:---:|
| 1 | 330 | $$q_1$$ | 0 | 90° |
| 2 | 0 | $$-q_2 + 90°$$ | 290 | 0° |
| 3 | 0 | $$-q_3$$ | 20 | 90° |
| 4 | 310 | $$-q_4 + 180°$$ | 0 | 90° |
| 5 | 0 | $$-q_5 + 180°$$ | 0 | 90° |
| 6 | 75 | $$-q_6$$ | 0 | 0° |
 
Each link-to-link transformation follows the standard D-H homogeneous matrix:
 
$$
^{i-1}T_i = \begin{bmatrix} \cos\theta_i & -\cos\alpha_i\sin\theta_i & \sin\alpha_i\sin\theta_i & a_i\cos\theta_i \\ \sin\theta_i & \cos\alpha_i\cos\theta_i & -\sin\alpha_i\cos\theta_i & a_i\sin\theta_i \\ 0 & \sin\alpha_i & \cos\alpha_i & d_i \\ 0 & 0 & 0 & 1 \end{bmatrix}
$$
 
with the full pose given by $$^0T_6 = \prod_{i=1}^{6} {}^{i-1}T_i$$. The result was validated in RViz for a zero configuration and a random configuration ($$q = [0.9, 0.8, 0.5, -1.3, -0.4, 0.4]$$), confirming that a control marker placed at the computed pose lands exactly on the simulated end effector.
 
### Inverse Kinematics (Newton-Raphson)
 
Joint configurations for a desired end-effector pose $$x_d$$ were computed using the **Newton-Raphson method**, recursively updating joint positions through the arm's Jacobian $$J$$:
 
$$
q_{k+1} = q_k + J^{-1}(q_k)\big(x_d - f(q_k)\big)
$$
 
This was implemented in Python and validated in RViz across several target poses, converging quickly — the position error norm dropped to zero within a small number of iterations in all tested cases.
 
## Kinematic Control Strategies
 
### Arm Kinematic Control
 
Motion control of the arm was based on an exponential position-error control law:
 
$$
\dot{e}^{*} = -ke
$$
 
Joint velocities are then obtained through the Moore-Penrose pseudo-inverse of the Jacobian:
 
$$
\dot{q} = J^{\#}\dot{e}^{*} \qquad J^{\#} = (J^TJ)^{-1}J^T
$$
 
Joint positions were updated numerically in Python via forward Euler integration:
 
$$
q_k = q_{k-1} + \Delta t\,\dot{q}_k
$$
 
The Jacobian determinant was also modeled symbolically in MATLAB to analyze kinematic **singularities**, which were found to occur concurrently when:
 
$$
q_4 = 0 \text{ or } \pm\pi \quad\land\quad q_1 = 0 \text{ or } \pm\pi/2 \quad\land\quad q_2 = 0 \text{ or } \pm\pi/2
$$
 
### Omnidirectional Platform Kinematic Control
 
The base was controlled in the XY plane with a proportional loop over Cartesian pose error. A Python control node (`controlP`) subscribes to the simulator's `/odom` topic and publishes linear/angular velocity commands to `/cmd_vel`, following:
 
$$
\dot{x} = K_p(x_d - x)
$$
 
Mapping inertial Cartesian velocities to the platform's own body-frame velocities ($$v, w$$) through the rotation/kinematic-influence matrix $$S$$ gives the holonomic base's control inputs:
 
$$
\begin{bmatrix} v \\ w \end{bmatrix} = S^{\#}K_p(x_d - x)
$$
 
Gazebo simulations confirmed the platform reaches a desired Cartesian target (e.g. $$[3, 2]$$) along a direct trajectory, as expected of a holonomic base.
 
### Combined Whole-Body Control
 
To reach targets outside the arm's static workspace, both controllers were integrated into a **hierarchical scheme**: for a target $$[x_d, y_d]$$ beyond the arm's reach, the mobile base drives toward the target first; once the Cartesian error norm falls below **0.65 m** — the KUKA KR4's effective operating radius — the base autonomously stops, and the arm's own kinematic controller takes over to complete the precise final positioning of the end effector.
 
## Dynamic Modeling and Control of the Arm
 
### Dynamic Model
 
The arm's dynamics follow the standard rigid-body equation of motion:
 
$$
M(q)\ddot{q} + C(q,\dot{q})\dot{q} + g(q) = u
$$
 
where $$M(q)$$ is the inertia matrix, $$C(q,\dot{q})$$ the Coriolis/centrifugal matrix, $$g(q)$$ the gravity vector, and $$u$$ the generalized input torques. The KUKA KR4's dynamic coefficients were computed numerically using **RBDL (Rigid Body Dynamics Library)**, decoupling the terms by first zeroing joint velocity and acceleration to isolate $$g(q)$$, then zeroing only acceleration to obtain $$C(q,\dot{q})\dot{q}$$, and iterating to progressively estimate the columns of $$M(q)$$.
 
### Inverse Dynamics Control — Joint Space
 
An inverse-dynamics control law was implemented in joint space, feeding back the computed dynamic model to precompensate the required forces. A proportional gain of $$K_p = 0.3$$ per joint was chosen, with critical damping $$K_d = 2\sqrt{K_p} \approx 1.0954$$ to obtain an overdamped transient response free of oscillation. Simulations confirmed clean convergence of both the individual joints and the end-effector's final Cartesian position to their desired values.
 
### Inverse Dynamics Control — Operational Space
 
A second inverse-dynamics controller was designed to act directly on the end effector's Cartesian coordinates, precomputing the geometric Jacobian. Under this scheme, individual joints do not settle at fixed setpoints — they keep moving, since the controller's only priority is the end effector's Cartesian position. Testing showed the end effector does reach the desired 3D target $$[X, Y, Z]$$, but oscillates while doing so, because **only Cartesian position was controlled, not end-effector orientation**. Two fixes were identified:
1. Add a control loop that also accounts for end-effector orientation
2. Add a stopping criterion that disables the controller and joint torques once the position-error norm falls below a small threshold

## Key Takeaways
 
- Mecanum-wheel kinematic control gives markedly better maneuverability than non-holonomic platforms, thanks to decoupled, simultaneous motion along both local Cartesian axes
- Joint-space dynamic control keeps both the individual joints and the end effector converging cleanly to their targets; operational-space control, by only constraining Cartesian position, leaves the joints oscillating for lack of orientation control
- Combining a mobile omnidirectional base with a multi-axis arm substantially extends coverage area and unlocks more complex, versatile tasks than either system alone
- Mecanum wheels, while holonomic, are prone to slippage and inefficiency on uneven ground — real-world use should be restricted to flat, regular surfaces
- Real-time, obstacle-aware trajectory generation would be a natural next step for safer, more autonomous operation in dynamic warehouse environments

## Technical Stack
 
<p class="font-semibold mb-2">Hardware</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">KUKA KR4 Agilus</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Neobotix MPO-500</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Mecanum Wheels</span>
</div>
<p class="font-semibold mb-2">Simulation & Modeling</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">ROS</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Gazebo</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">RViz</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">URDF</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Fusion 360</span>
</div>
<p class="font-semibold mb-2">Kinematics & Control</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Denavit-Hartenberg</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Newton-Raphson IK</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">RBDL</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Inverse Dynamics Control</span>
</div>
<p class="font-semibold mb-2">Languages & Tools</p>
<div class="flex flex-wrap gap-2 mb-4">
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">Python</span>
    <span class="border border-divider text-text text-xs rounded-full px-3 py-1">MATLAB</span>
</div>

## Team & Contribution
 
This project was developed for the Fundamentos de Robótica course at UTEC together with <a href="https://www.linkedin.com/in/manuelcarita/" target="_blank">Manuel Carita</a>, Jairo Custodio, and Rosario Quispe.
 
## Resources
 
The full simulation, kinematics, and control implementation, along with the symbolic Jacobian derivation, are available below.
 
<div class="flex flex-wrap justify-center gap-3 my-4">
    <a href="../../assets/pdf/projects/9_proyecto_fundamentos.pdf" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-solid fa-file-pdf"></i> Download Full Article (PDF, Spanish)
    </a>
    <a href="https://github.com/manul30/fdr_manipulator_movil_robot" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-brands fa-github"></i> View Code on GitHub
    </a>
    <a href="https://colab.research.google.com/drive/1Dsg7hCRUdB_NAdd-jc3W0AYWZl4_VIot?usp=sharing" target="_blank" class="inline-flex items-center gap-2 border border-divider text-text text-sm rounded-full px-4 py-2 hover:bg-divider/20 transition-colors">
        <i class="fa-solid fa-square-root-variable"></i> Jacobian Derivation (Colab)
    </a>
</div>