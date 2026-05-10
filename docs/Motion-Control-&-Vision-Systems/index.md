# Motion Control & Vision Systems

This section contains two robotics-focused projects centered around machine vision, embedded control, inverse kinematics, and motion-planning algorithms. These projects were developed as part of advanced robotics coursework and focused heavily on the mathematical and controls-oriented side of robotics engineering.

While the Battery Sorting System elsewhere in this portfolio emphasized industrial automation, PLC integration, SCADA development, and systems engineering, the projects in this section focused more heavily on robotics mathematics, coordinate transformation, trajectory generation, and embedded motion control.

Together, these projects demonstrate practical implementation of:

- Machine vision and object localization
- Coordinate-frame transformation
- Inverse kinematics and Jacobian-based motion control
- Embedded robotics programming
- Cartesian-to-joint coordinate conversion
- Servo synchronization and trajectory generation
- PSOC-based actuator control

---

## Projects

### Vision-Guided Object Pickup System

A machine-vision-based robotic control system that detected object locations using OpenCV image processing, transformed camera-frame coordinates into robot workspace coordinates using homogeneous transformations, calculated manipulator joint angles through inverse kinematics, and transmitted motion commands to a PSOC-based embedded controller over UART communication.

The project combined computer vision, robotics mathematics, and embedded control into a single end-to-end motion pipeline.

#### Technical Highlights

- OpenCV image processing
- Background subtraction and thresholding
- Object centroid extraction
- Homogeneous coordinate transformation
- Planar inverse kinematics
- Camera-to-base frame conversion
- UART communication
- Embedded servo control

#### System Workflow

```text
Camera Capture
    ↓
OpenCV Vision Processing
    ↓
Object Localization
    ↓
Coordinate Transformation
    ↓
Inverse Kinematics
    ↓
UART Communication
    ↓
PSOC Motor Control
```

[View project](Vision-Guided-Object-Pickup-System.md)

---

### Servo-Based Shape Drawing System

A fully embedded motion-control system that generated and executed geometric drawing trajectories using a two-link planar robotic arm driven by coordinated servo motion. The project implemented inverse differential kinematics and Jacobian-based trajectory control directly on a PSOC microcontroller.

Rather than using simple point-to-point servo commands, the system generated continuous Cartesian motion trajectories, converted workspace velocities into joint angular velocities, buffered trajectory points, and executed smooth repeatable drawing operations entirely within embedded firmware.

#### Technical Highlights

- Embedded motion planning
- Inverse differential kinematics
- Jacobian matrix implementation
- Cartesian velocity generation
- Trajectory buffering
- Coordinated dual-servo control
- Real-time path execution
- Embedded robotics control

#### Motion-Control Pipeline

```text
Shape Generation
    ↓
Cartesian Velocity Commands
    ↓
Inverse Jacobian Solution
    ↓
Joint Velocity Calculation
    ↓
Trajectory Buffering
    ↓
Servo Motor Execution
```

[View project](Servo-Based-Shape-Drawing-System.md)

---

## Technical Focus

These projects focused on robotics concepts that are mathematically and algorithmically intensive, including:

- Workspace coordinate transformation
- Manipulator kinematics
- Jacobian-based motion control
- Trajectory generation
- Coordinate-system alignment
- Embedded actuator control
- Continuous motion execution

The projects also provided practical experience implementing robotics concepts directly in software and embedded hardware rather than relying solely on simulation environments.

Together, they complement the larger industrial automation systems within this portfolio by demonstrating experience with both high-level robotics mathematics and low-level embedded motion control implementation.
