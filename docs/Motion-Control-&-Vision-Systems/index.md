# Motion Control & Vision Systems

This section contains two academically focused robotics and motion-control projects developed using Python, OpenCV, inverse kinematics, and PSOC-based embedded motor control.

These projects are smaller in scope than the Battery Sorting System, but they demonstrate a different side of my engineering work: mathematical modeling, coordinate transformation, camera-based sensing, and low-level actuator control.

---

## Projects

### Vision-Guided Object Pickup System

A camera-based object localization system that used Python and OpenCV to detect an object's position, transform camera-frame coordinates into the robot base frame, calculate joint angles using inverse kinematics, and send motion commands to a PSOC controller over UART.

**Key topics:**

- OpenCV image processing
- Background subtraction
- Centroid extraction
- Homogeneous coordinate transformation
- Planar inverse kinematics
- UART communication
- Embedded motor control

[View project](vision-guided-object-pickup.md)

---

### Servo-Based Shape Drawing System

A two-servo drawing arm that used Python-generated motion commands and PSOC-based motor control to trace predefined shapes. The project focused on inverse kinematics, coordinated servo motion, and translating geometric paths into actuator movement.

**Key topics:**

- Python motion generation
- Two-link arm kinematics
- Servo control
- Coordinate-based drawing
- Embedded control through PSOC
- Physical motion validation

[View project](servo-shape-drawing.md)

---

## Technical Focus

Together, these projects demonstrate the academic and mathematical side of robotics and motion control. While the Battery Sorting System emphasized industrial controls, PLC programming, SCADA integration, and system-level implementation, these projects focused more heavily on robotics fundamentals:

- Mapping camera or path coordinates into actuator motion
- Solving position-based motion problems mathematically
- Translating calculated values into embedded motor commands
- Testing physical motion against expected geometric behavior

They serve as supporting projects within this portfolio, showing experience with robotics concepts alongside the larger industrial automation work.
