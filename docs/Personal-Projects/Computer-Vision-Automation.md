# Computer Vision & Input Automation

## Overview

This project began as a personal experimentation exercise focused on computer vision, event detection, and real-time automation using Python and OpenCV. The goal was to automate a repetitive in-game fishing task by detecting visual splash events on-screen and responding automatically through simulated mouse and keyboard input.

Although the project itself was intentionally lighthearted, the underlying implementation involved many of the same concepts used in practical automation systems:

- Real-time image processing
- Region-of-interest monitoring
- Event detection
- Threshold filtering
- Contour analysis
- Automated input control
- Timing/state management
- Reliability optimization

The project evolved into a small but fully functional computer-vision-based automation system capable of continuously monitoring for visual events, reacting to detected state changes, and managing repetitive interaction loops without user intervention.

---

## Project Motivation

The project originated from a simple operational annoyance:

```text
Repeatedly waiting for visual fishing events
        ↓
Manual reaction input
        ↓
Recasting and repeating
        ↓
Long periods of repetitive idle interaction
```

Rather than treating the problem as a simple macro-recording exercise, the project was approached as a real-time computer-vision problem.

The objective became:

- Detect visual fishing splashes directly from the game window
- Automatically trigger interaction when a splash occurred
- Recast automatically after successful detection
- Maintain continuous unattended operation
- Prioritize reliability over perfect precision

---

## System Workflow

### High-Level Automation Pipeline

```text
Screen Capture
        ↓
Region-of-Interest Isolation
        ↓
Grayscale Conversion
        ↓
Gaussian Blur Filtering
        ↓
Threshold Detection
        ↓
Contour Extraction
        ↓
Splash Event Detection
        ↓
Mouse Movement & Input Trigger
        ↓
Automatic Recast
```

The script continuously monitored a small portion of the screen corresponding to the fishing float location and reacted automatically when a valid splash event was detected.

---

## Detection & Automation Logic

### Region-of-Interest Monitoring

Rather than processing the entire screen, the system monitored a tightly constrained screen region surrounding the fishing float.

#### Example Region Definition

```python
region = {
    "left": 1200,
    "top": 175,
    "width": 180,
    "height": 325
}
```

Restricting the processing area significantly reduced computational overhead while improving detection consistency and minimizing false detections from unrelated screen activity.
<!--
#### Planned Media Placeholder
-->
<!-- Insert screenshot of monitored screen region here -->
<!--
Suggested content:

- Highlighted fishing-float region
- Detection window overlay
- Region-of-interest boundaries
-->
---

### Real-Time Image Processing

The project used OpenCV-based image processing to isolate visual splash events from the monitored screen region.

#### Grayscale Conversion

```python
gray = cv2.cvtColor(frame, cv2.COLOR_BGR2GRAY)
```

#### Noise Reduction

A Gaussian blur filter was applied to reduce image noise and smooth rapid pixel fluctuations:

```python
blur = cv2.GaussianBlur(gray, (5, 5), 0)
```

#### Threshold Filtering

Thresholding converted the processed image into a binary representation suitable for contour extraction:

```python
_, thresh = cv2.threshold(
    blur,
    150,
    255,
    cv2.THRESH_BINARY
)
```

This processing pipeline isolated sudden bright splash events from the surrounding environment.

<!-- #### Planned Media Placeholder -->

<!-- Insert image-processing sequence screenshots here -->
<!--
Suggested content:

- Original screen capture
- Grayscale image
- Thresholded image
- Final contour detection overlay
-->
---

### Contour Detection & Event Recognition

After threshold filtering, the script searched for contours matching expected splash characteristics.

#### Contour Detection

```python
contours, _ = cv2.findContours(
    thresh,
    cv2.RETR_EXTERNAL,
    cv2.CHAIN_APPROX_SIMPLE
)
```

Each detected contour was evaluated based on size and shape constraints:

```python
if 100 < area < 500:
```

This filtering prevented extremely small noise artifacts and excessively large unrelated screen changes from triggering false detections.

#### Centroid Extraction

The system then calculated the center location of the detected splash region:

```python
cX = int(M["m10"] / M["m00"])
cY = int(M["m01"] / M["m00"])
```

The resulting centroid coordinates were converted into screen-space coordinates for automated mouse interaction.

<!-- #### Planned Media Placeholder -->

<!-- Insert contour detection screenshot here -->
<!--
Suggested content:

- Bounding-box overlay
- Detected splash highlight
- Centroid marker
- Contour visualization
-->
---

### Automated Input Control

When a valid splash event was detected, the system automatically:

1. Moved the cursor to the detected splash location
2. Triggered a right-click interaction
3. Waited for the interaction delay
4. Recast the fishing action automatically

#### Example Automated Interaction Logic

```python
pyautogui.moveTo(screen_x, screen_y)
pyautogui.click(button='right')
```

#### Automatic Recast Logic

```python
pyautogui.press('8')
```

This created a fully continuous automation loop requiring no manual interaction during normal operation.

---

## Reliability & Automation Philosophy

One of the most important design goals of the project was maximizing reliability.

The detection system was intentionally tuned to prioritize avoiding missed splash detections, even if that occasionally produced false-positive triggers.

### Detection Philosophy

```text
False Positive:
    unnecessary recast

False Negative:
    missed fish event
```

Because missed detections carried a higher operational penalty than occasional unnecessary recasts, the system intentionally favored aggressive event triggering.

This resulted in:

- Extremely low missed-detection rates
- Very high operational uptime
- Occasional unnecessary recasts
- Reliable long-duration unattended operation

This design philosophy mirrored many real-world automation systems where avoiding missed events is often more important than eliminating every false trigger.

---

### Timeout & State Handling

The project also implemented timing-based fallback systems to handle edge cases where splash detection failed or interaction timing became desynchronized.

#### Timeout Recovery

```python
elif time.time() - last_cast > timeout:
```

If no splash event occurred within a specified timeout period, the system automatically restarted the fishing cycle to maintain continuous operation.

---

### Automated Maintenance Logic

The script additionally included automated maintenance handling for temporary gameplay enhancements.

#### Automatic Lure Reapplication

```python
if time.time() - last_lure_time > lure_interval:
```

The automation system periodically:

- reapplied the fishing enhancement
- refreshed the interaction state
- resumed automated operation

without requiring manual user intervention.

This transformed the project from a simple event-trigger script into a more complete long-duration automation loop with persistent state handling.

---

## Technical Implementation

### Libraries & Technologies Used

- Python
- OpenCV (`cv2`)
- NumPy
- MSS screen capture
- PyAutoGUI
- Real-time contour analysis

### Major Functional Components

- Screen-region capture
- Real-time image filtering
- Threshold detection
- Contour extraction
- Event classification
- Cursor positioning
- Automated mouse/keyboard control
- Timeout recovery handling
- Periodic maintenance automation

---
<!--
## Demonstration Media

### Planned Demonstration Media

Suggested content:

- Live detection overlay
- Splash-event triggering
- Cursor automation
- Long-duration unattended operation
- False-positive examples
- Timeout recovery demonstrations

---

## Additional Planned Diagrams

### Detection Pipeline Diagram

```text
Screen Capture
        ↓
Image Filtering
        ↓
Threshold Processing
        ↓
Contour Detection
        ↓
Event Validation
        ↓
Input Automation
```

---
-->
### Reliability Tradeoff Diagram

Suggested content:

- False positives vs. false negatives
- Detection sensitivity tuning
- Reliability-focused automation philosophy

---

## Technical Concepts Demonstrated

This project demonstrated practical implementation of:

- Real-time computer vision
- OpenCV image processing
- Region-of-interest optimization
- Threshold-based event detection
- Contour analysis
- Automated input control
- Event-driven automation
- Reliability-focused automation design
- Timeout recovery systems
- Persistent automation loops
- State-management logic

---

## Project Outcome

The completed system successfully automated long-duration repetitive interaction tasks through real-time computer vision and automated input control.

Although originally developed as a personal experimentation project, the implementation evolved into a surprisingly robust event-driven automation system that demonstrated many of the same principles used in practical automation engineering:

- detecting state changes
- filtering noisy inputs
- responding to events
- managing timing and synchronization
- balancing false positives vs. false negatives
- maintaining reliable long-duration operation

This project complements the larger engineering systems elsewhere in this portfolio by demonstrating a more experimental and self-directed approach to automation design while still applying practical computer-vision and real-time event-processing concepts.
