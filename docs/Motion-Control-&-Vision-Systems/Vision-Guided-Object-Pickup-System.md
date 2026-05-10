---
title: Vision Guided Object Pickup System
---

# Vision-Guided Object Pickup System

## Overview

This project focused on integrating machine vision, coordinate transformation, inverse kinematics, and embedded motor control into a vision-guided planar manipulator system. A Python-based vision pipeline detected an object's position within the camera frame, transformed the measured coordinates into the robot base frame, calculated the required joint angles for a two-link arm, and transmitted the resulting motor commands to a PSOC-based embedded controller over UART communication.

The project emphasized the mathematical and algorithmic side of robotics, including image processing, homogeneous transformations, workspace coordinate mapping, and inverse kinematics.

---

## System Workflow

```text
USB Camera
    ↓
OpenCV Image Processing
    ↓
Object Detection & Centroid Extraction
    ↓
Homogeneous Coordinate Transformation
    ↓
Inverse Kinematics Calculation
    ↓
UART Communication
    ↓
PSOC Embedded Controller
    ↓
Servo Motor Actuation
```

---

## Vision Processing Pipeline

The vision system was implemented in Python using OpenCV. Object localization was performed using background subtraction and threshold-based image segmentation.

The process included:

- Grayscale image conversion
- Foreground/background differencing
- Absolute-value image subtraction
- Threshold filtering
- Contour filtering
- Centroid calculation using weighted pixel sums

This approach allowed the system to isolate a target object from the scene and determine its position within the camera frame.

### Example Processing Stages

```python
Difference = np.absolute(
    np.matrix(np.int16(gray_image_1))
    - np.matrix(np.int16(gray_image_2))
)

BW[BW < 75] = 0
```

Contour filtering was then used to remove noise and isolate the target object before calculating the object's centroid location.

---

## Coordinate Transformation

After detecting the object's position within the camera frame, the measured coordinates were transformed into the robot base coordinate system using a homogeneous transformation matrix.

The transformation included:

- Camera rotation compensation
- Base-frame alignment
- Translational offsets between the camera and robot origin

The transformation matrix was constructed using combined rotation matrices and displacement vectors.

```python
R0_C = np.dot(R_Z, R_Y)

H0_C = np.concatenate((R0_C, D0_C), 1)
H0_C = np.concatenate((H0_C, [[0, 0, 0, 1]]), 0)
```

The resulting homogeneous transformation allowed camera-space coordinates to be converted into manipulator workspace coordinates before inverse kinematic calculations were performed.

---

## Inverse Kinematics

The robotic arm used a planar multi-link geometry model. Joint angles were calculated from the transformed target coordinates using trigonometric inverse-kinematic relationships.

The inverse kinematics implementation included:

- Workspace distance calculations
- Link-length geometry
- Quadrant-dependent angle solutions
- Trigonometric joint-angle solving using `atan2()` and `acos()`

```python
phi_1 = math.atan2(y_end, abs_x_end)

phi_2 = math.acos(
    (a1**2 + r1**2 - r2**2) / (2 * a1 * r1)
)
```

The calculated joint angles were then converted into motor commands for the embedded controller.

---

## Embedded Motor Control

A PSOC microcontroller handled low-level motor control and serial communication.

The embedded controller:

- Received position data over UART
- Reconstructed transmitted floating-point values
- Applied offset correction for signed values
- Controlled the servo motors
- Displayed received coordinates on an onboard LCD for debugging

Because UART communication was limited to byte-wise transmission, floating-point values were separated into integer and decimal components before transmission.

This created a reliable communication pipeline between the Python control system and the embedded actuator controller.

---

## Technical Concepts Demonstrated

- OpenCV image processing
- Background subtraction and thresholding
- Object centroid extraction
- Homogeneous coordinate transformations
- Planar inverse kinematics
- Coordinate-frame conversion
- UART serial communication
- Embedded motor control
- Python-to-microcontroller integration

---

## Project Outcome

The completed system successfully detected object locations within the camera frame, transformed those coordinates into robot workspace coordinates, calculated corresponding arm joint angles, and transmitted motor commands to an embedded controller for physical arm actuation.

This project provided hands-on experience with the mathematical foundations of robotics and demonstrated the integration of machine vision, kinematic modeling, and embedded control into a single autonomous motion-control pipeline.
