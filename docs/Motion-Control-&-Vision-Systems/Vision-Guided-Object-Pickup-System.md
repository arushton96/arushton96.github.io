---
title: Vision Guided Object Pickup System
---

# Vision-Guided Object Pickup System

## Overview

This project focused on integrating machine vision, coordinate transformation, inverse kinematics, and embedded motor control into a complete vision-guided robotic motion system. A Python-based image-processing pipeline detected an object's location within the camera frame, transformed the measured coordinates into the robot workspace coordinate system, calculated the required manipulator joint angles, and transmitted motion commands to a PSOC-based embedded controller over UART communication.

The project combined multiple robotics concepts into a single end-to-end control pipeline, including image segmentation, centroid extraction, homogeneous transformations, planar inverse kinematics, and embedded actuator control.

Unlike industrial automation systems that rely on predefined positions and fixed logic, this project dynamically interpreted visual information from the environment and converted that data into real-time robotic motion commands.

---

## Project Objectives

The primary objectives of the project were:

- Develop a machine-vision-based object localization system
- Detect object position within the camera frame using OpenCV
- Transform camera coordinates into robot workspace coordinates
- Implement planar inverse kinematics for a multi-link manipulator
- Transmit calculated motion data to an embedded controller
- Integrate image processing, robotics mathematics, and actuator control into a unified motion pipeline

The project emphasized the mathematical and algorithmic foundations of robotics while also incorporating practical embedded communication and motor-control implementation.

---

## System Architecture

The system combined machine vision, coordinate transformation, inverse kinematics, serial communication, and embedded motor control into a unified processing pipeline.

### High-Level System Workflow

```text
USB Camera
    ↓
OpenCV Image Processing
    ↓
Object Detection & Centroid Extraction
    ↓
Camera-to-Base Coordinate Transformation
    ↓
Inverse Kinematics Calculation
    ↓
UART Communication
    ↓
PSOC Embedded Controller
    ↓
Servo Motor Actuation
```

The Python control layer handled all image processing and mathematical calculations, while the PSOC microcontroller handled low-level motor actuation and serial communication.

---

## Hardware Configuration

The system used a simple planar robotic manipulator combined with a USB camera and embedded motor controller.

### Major Hardware Components

- USB camera
- PSOC microcontroller
- Dual servo motors
- Planar robotic arm assembly
- Embedded LCD/debug interface
- UART serial communication link

The robotic arm was modeled as a planar multi-link manipulator with fixed link lengths and rotational joints.

---

## Vision Processing Pipeline

The machine vision system was implemented using Python and OpenCV.

Object localization was performed through a multi-stage image-processing pipeline that isolated the target object from the environment and calculated its centroid location within the image frame.

### Image Processing Stages

The processing pipeline included:

- Grayscale image conversion
- Background image capture
- Foreground/background subtraction
- Threshold filtering
- Contour filtering
- Centroid extraction

The system first captured a reference background image and then compared subsequent frames against that reference to isolate newly introduced objects.

### Background Subtraction

```python
Difference = np.absolute(
    np.matrix(np.int16(gray_image_1))
    - np.matrix(np.int16(gray_image_2))
)
```

Absolute-value differencing allowed the system to detect both light and dark objects relative to the original background image.

### Threshold Filtering

```python
BW[BW < 75] = 0
```

Thresholding removed low-intensity noise and simplified the image into a binary representation suitable for contour extraction and centroid calculation.

### Contour Filtering

Contour filtering was then used to isolate valid object regions while rejecting smaller noise artifacts:

```python
contours, _ = cv2.findContours(
    BW,
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)
```

---

## Object Localization

After thresholding and contour filtering, the object's center location was calculated using weighted pixel sums.

The system calculated centroid location independently in both the X and Y image dimensions using intensity-weighted coordinate averaging.

This process allowed the robotic system to determine the object's position within the camera frame before converting that position into robot workspace coordinates.

<!-- ### Planned Diagram Placeholder -->

<!-- Insert image-processing pipeline diagram here -->
<!--
Suggested diagram content:

- Original camera image
- Difference image
- Thresholded image
- Contour-filtered result
- Final centroid extraction
-->
---

## Coordinate Transformation

After detecting the object within the image frame, the measured camera coordinates were transformed into the robot base coordinate system using a homogeneous transformation matrix.

This transformation compensated for:

- Camera orientation
- Coordinate-frame alignment
- Physical displacement between the camera and manipulator origin

### Homogeneous Transformation Matrix

```python
R0_C = np.dot(R_Z, R_Y)

H0_C = np.concatenate((R0_C, D0_C), 1)
H0_C = np.concatenate((H0_C, [[0, 0, 0, 1]]), 0)
```

The transformation matrix combined rotational and translational offsets into a single coordinate transformation system.

The resulting matrix allowed the system to convert image-space coordinates into manipulator workspace coordinates before performing inverse kinematic calculations.

### Coordinate Transformation Step

```python
P0 = np.dot(H0_C, PC)
```

This transformation process represented one of the most important mathematical components of the project because it linked the vision system directly to the robotic workspace.

---

## Manipulator Geometry & Inverse Kinematics

The robotic arm was modeled as a planar multi-link manipulator with fixed link lengths.

After transforming the object location into workspace coordinates, the system calculated the required joint angles needed to move the manipulator toward the target location.

### Inverse Kinematic Solution

The inverse kinematics implementation used trigonometric geometry relationships including:

- Workspace distance calculations
- Link-length geometry
- Quadrant-dependent angle handling
- Trigonometric angle solving using `atan2()` and `acos()`

### Example Calculations

```python
phi_1 = math.atan2(y_end, abs_x_end)

phi_2 = math.acos(
    (a1**2 + r1**2 - r2**2)
    / (2 * a1 * r1)
)
```

The system then generated the corresponding joint angles:

```python
theta_1
theta_2
```

Quadrant-dependent handling logic was used to ensure correct manipulator motion throughout the workspace.

<!-- ### Planned Diagram Placeholder -->

<!-- Insert manipulator geometry diagram here -->
<!--
Suggested diagram content:

- Link lengths
- Joint angles
- Workspace coordinates
- End-effector location
- Coordinate-frame labels
-->
---

## Embedded Communication & Motor Control

The calculated joint-angle values were transmitted from Python to the PSOC microcontroller using UART serial communication.

The embedded controller handled:

- Serial data reception
- Coordinate reconstruction
- Offset correction
- Servo actuation
- LCD-based debugging display

### UART Transmission Strategy

Because the serial communication system was limited to byte-wise transmission, floating-point values were separated into integer and decimal components before transmission.

Negative values were handled by applying offsets prior to transmission and removing those offsets on the embedded controller.

### Example Embedded Reconstruction

```c
float xReceived = xReceiveD1D
                + xDecReceived
                - 11;
```

This communication pipeline allowed reliable transmission of signed floating-point coordinate data between the Python vision system and the embedded actuator controller.

---

## Engineering Challenges

Several engineering challenges were encountered throughout development.

### Vision Noise & Object Isolation

Lighting conditions and image noise created instability during early object-detection testing. Threshold filtering and contour cleanup routines were introduced to improve object isolation reliability.

### Coordinate-Frame Alignment

Accurately mapping camera coordinates into robot workspace coordinates required careful calibration of rotational offsets and translational displacement values.

Small calibration errors significantly affected manipulator positioning accuracy.

### Workspace & Quadrant Handling

Inverse kinematic calculations required special handling across multiple workspace quadrants to ensure the manipulator produced physically correct motion solutions.

### Embedded Communication Constraints

UART communication constraints required custom handling for signed floating-point transmission between Python and the embedded controller.

The communication pipeline was redesigned to separate whole-number and decimal components into individual byte transmissions.

---
<!--
## Demonstration Media

### Planned Demonstration Media
-->
<!-- Add demonstration images/videos here -->
<!--
Suggested content:

- Camera view screenshots
- Thresholded image examples
- Coordinate-overlay images
- Manipulator motion demonstrations
- Embedded LCD debugging display

---

## Additional Planned Diagrams

### Vision-to-Motion Pipeline Diagram

Suggested content:

```text
Camera Image
    ↓
Image Processing
    ↓
Centroid Extraction
    ↓
Coordinate Transformation
    ↓
Inverse Kinematics
    ↓
Joint Angle Commands
    ↓
Embedded Motor Control
```

---

### Coordinate Transformation Diagram

Suggested content:

- Camera coordinate frame
- Robot base frame
- Translation offsets
- Rotation alignment
- Workspace coordinate conversion

---

## Technical Concepts Demonstrated

This project demonstrated practical implementation of several robotics and machine-vision concepts:

- OpenCV image processing
- Background subtraction
- Threshold filtering
- Contour extraction
- Object centroid calculation
- Homogeneous transformations
- Coordinate-frame conversion
- Planar inverse kinematics
- UART serial communication
- Embedded motor control
- Python-to-microcontroller integration

---
-->
## Project Outcome

The completed system successfully detected object locations within the camera frame, transformed those measurements into robot workspace coordinates, calculated corresponding manipulator joint angles, and transmitted motion commands to an embedded actuator controller.

The project provided hands-on experience with machine vision, coordinate transformation, robotics mathematics, embedded communication, and manipulator control while demonstrating the integration of sensing, computation, and physical robotic motion into a unified control system.

This project complements the more industrial automation-focused systems elsewhere in this portfolio by emphasizing the algorithmic, mathematical, and perception-oriented side of robotics engineering.
