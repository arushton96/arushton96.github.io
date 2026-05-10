# Servo-Based Shape Drawing System

## Overview

This project focused on embedded motion control, inverse differential kinematics, and trajectory generation using a two-link planar robotic arm driven by coordinated servo motors. The system generated geometric drawing paths directly on a PSOC microcontroller and converted Cartesian motion commands into synchronized joint motion using Jacobian-based kinematic calculations.

Unlike simple point-to-point servo demonstrations, this project emphasized continuous trajectory execution and smooth coordinated motion. The control system generated workspace velocity commands, solved the inverse Jacobian of the manipulator in real time, buffered trajectory points, and executed repeatable geometric drawing operations using embedded motion-control logic.

The completed system was capable of repeatedly drawing geometric shapes with stable and reliable motion using entirely embedded control logic.

---

## Project Objectives

The primary objectives of the project were:

- Develop a planar two-link robotic arm capable of coordinated motion
- Implement embedded inverse differential kinematics
- Generate smooth geometric trajectories in Cartesian space
- Convert workspace motion into joint-space velocity commands
- Execute repeatable drawing operations using buffered motion control
- Develop a standalone embedded robotics platform without external runtime computation

The project served as a practical implementation of robotics kinematics and motion-planning concepts commonly introduced in academic robotics systems.

---

## System Architecture

The system was implemented entirely on a PSOC microcontroller using embedded C. The robotic arm consisted of two servo-driven rotational joints and a pen actuator mounted to the end effector.

### High-Level Motion Pipeline

```text
Shape Selection
    ↓
Path Generation
    ↓
Cartesian Velocity Commands
    ↓
Inverse Jacobian Calculation
    ↓
Joint Angular Velocity Generation
    ↓
Trajectory Buffering
    ↓
Servo Position Commands
    ↓
Physical Arm Motion
```

The embedded controller handled all trajectory generation, motion planning, and servo actuation internally.

---

## Hardware Configuration

The robotic arm used a planar two-link geometry model driven by two servo motors.

### Major Hardware Components

- PSOC microcontroller
- Dual servo motors
- Pen actuator mechanism
- Keypad user interface
- Embedded LCD/debug interface
- Two-link planar arm assembly

The arm geometry was modeled mathematically using fixed link lengths and rotational joints.

---

## Manipulator Geometry

The system modeled the robotic arm as a planar multi-link manipulator with fixed link lengths.

### Planned Diagram Placeholder

<!-- Insert manipulator geometry diagram here -->

Suggested diagram content:

- Link lengths
- Joint angles
- End-effector location
- Workspace coordinates
- Coordinate-frame labeling

The manipulator geometry formed the basis for all inverse kinematic and Jacobian calculations used throughout the project.

---

## Cartesian Motion Generation

Instead of directly commanding joint positions, the system generated motion in Cartesian workspace coordinates.

Desired motion paths were represented as continuous velocity commands:

```c
buffer_straight_path(float xdot, float ydot, ...)
```

This approach allowed the robotic arm to follow smooth geometric trajectories while maintaining coordinated motion between both joints.

The use of workspace velocity commands also simplified the generation of continuous drawing paths such as circles and polygons.

---

## Inverse Differential Kinematics

The core mathematical component of the project was the implementation of inverse differential kinematics using the manipulator Jacobian matrix.

The control system continuously converted desired Cartesian motion into joint angular velocities:

```c
ThetaOneDot = J11_inv * xdot + J12_inv * ydot;
ThetaTwoDot = J21_inv * xdot + J22_inv * ydot;
```

This process allowed the robotic arm to move smoothly through workspace trajectories while dynamically coordinating both servo motors.

The implementation required:

- Manipulator Jacobian derivation
- Matrix inversion
- Joint-space velocity calculations
- Real-time angular updates
- Workspace coordinate handling

This architecture provided significantly smoother motion than discrete point-to-point angle commands.

---

## Trajectory Buffering

One of the primary design goals of the project was improving motion smoothness and repeatability.

To accomplish this, trajectory points were generated and stored in motion buffers before execution:

```c
Theta1_Buffer[i]
Theta2_Buffer[i]
```

Buffered playback provided several advantages:

- Reduced discontinuities in motion
- Smoother path transitions
- More stable servo movement
- Deterministic execution timing
- Improved geometric consistency

Rather than recalculating motion during execution, the system precomputed trajectory data and replayed it at controlled timing intervals.

This architecture significantly improved the quality and repeatability of the resulting motion.

---

## Shape Generation

The project supported multiple predefined geometric drawing operations generated directly within the embedded controller.

Implemented path types included:

- Straight-line trajectories
- Square paths
- Circular paths
- Triangular paths

Each shape was generated through coordinated Cartesian velocity commands and translated into joint-space motion using the inverse Jacobian system.

Trajectory parameters such as:

- drawing scale
- motion speed
- segment timing
- execution duration

could be adjusted through the embedded control interface.

---

## Embedded Interface & Runtime Control

The project included a keypad-driven user interface for selecting motion routines and controlling the robotic arm during operation.

Additional embedded features included:

- Servo calibration routines
- Motion timing adjustments
- Pen actuation control
- Runtime shape selection
- Buffered trajectory execution
- Position scaling

The entire system operated independently on embedded hardware without requiring external processing software during runtime.

---

## Engineering Challenges

Several engineering challenges were encountered during development.

### Motion Smoothness

Early motion implementations produced discontinuous or unstable arm movement. Buffered trajectory playback and velocity-based motion generation were introduced to improve path smoothness and maintain consistent motion timing.

### Coordinated Joint Motion

Generating smooth workspace trajectories required careful synchronization between both servo joints. Jacobian-based motion calculations were implemented to maintain coordinated end-effector movement throughout path execution.

### Geometric Accuracy

Maintaining recognizable geometric shapes required tuning motion timing, scaling, and servo calibration values. Small positional errors accumulated quickly during continuous motion and required iterative adjustment.

### Embedded Resource Constraints

All trajectory generation and motion control calculations were executed directly on the PSOC microcontroller. Motion-planning routines had to be implemented efficiently while maintaining stable runtime performance.

---

## Demonstration Media

### System Demonstration Video

<!-- Replace with actual video path -->

<video controls preload="metadata" width="100%">
  <source src="../Images/shape_drawing_demo.mp4" type="video/mp4">
</video>

---

## Additional Planned Diagrams

### Motion-Control Pipeline Diagram

Suggested content:

```text
Shape Generator
    ↓
Cartesian Velocity Commands
    ↓
Inverse Jacobian
    ↓
Joint Velocity Calculation
    ↓
Trajectory Buffer
    ↓
Servo Outputs
```

---

### Workspace Coordinate Visualization

Suggested content:

- Cartesian coordinate system
- Workspace boundaries
- End-effector trajectory examples
- Joint-angle relationships

---

## Technical Concepts Demonstrated

This project demonstrated practical implementation of several robotics and embedded motion-control concepts:

- Embedded robotics control
- Planar manipulator kinematics
- Inverse differential kinematics
- Jacobian matrix implementation
- Cartesian-to-joint coordinate conversion
- Trajectory generation
- Buffered motion execution
- Coordinated servo control
- Real-time embedded motion planning
- Embedded HMI design
- PSOC-based actuator control

---

## Project Outcome

The completed system successfully generated and executed smooth geometric drawing paths using coordinated dual-servo motion and embedded Jacobian-based control logic.

The project demonstrated practical implementation of robotics mathematics directly within an embedded environment and provided hands-on experience with trajectory planning, kinematic modeling, motion synchronization, and real-time actuator control.

This project complements the more industrial automation-focused systems elsewhere in this portfolio by emphasizing the mathematical and controls-oriented side of robotics engineering.
