---
title: Challenges & Engineering Decisions
---

# Challenges & Engineering Decisions

## Overview

A major portion of the project involved solving real-world industrial integration problems across PLC programming, industrial networking, machine vision communication, SCADA architecture, pneumatic sequencing, and subsystem coordination.

Unlike a purely theoretical or simulation-based project, the system was developed using real industrial hardware and software tools, which introduced communication limitations, integration challenges, hardware delays, and evolving design constraints throughout development.

Many portions of the final architecture evolved significantly as testing exposed new sequencing, communication, and reliability issues that required iterative redesigns and engineering tradeoff decisions.

---

## Original Project Architecture

### Initial System Concept

The original project architecture centered around a fully integrated industrial automation platform combining:

- FANUC robotic handling
- Pneumatic transfer hardware
- Siemens PLC control
- Machine vision integration
- SCADA monitoring
- SQL database logging
- Industrial Ethernet communication

The robotic system was originally intended to perform the primary battery-transfer operations while the PLC coordinated sequence control, scanner communication, subsystem timing, and operator interaction.

The intended architecture represented a more traditional industrial automation workflow involving coordinated robot motion and PLC-controlled peripheral systems.

<!-- ![Original System Concept](Images/OriginalConcept.png) -->

---

## Hardware & Communication Delays

### Robot Integration Limitations

One of the largest project challenges involved the planned integration between the Siemens PLC and the FANUC robotic system.

Although the robot itself supported Ethernet/IP communication, the project architecture required Profinet integration in order to maintain compatibility with the rest of the industrial communication network.

#### Profinet Upgrade Delays

The required Profinet communication hardware experienced repeated delays and ultimately did not arrive within the project timeline.

This created several major impacts:

- Loss of direct PLC-to-robot communication
- Inability to complete planned robot integration
- Reduced integration/testing time
- Required redesign of the automation sequence
- Scope reduction near final integration stages

#### Engineering Response

Rather than abandoning the automation workflow entirely, the project architecture was redesigned around a pneumatic-focused transfer sequence that simulated the intended battery-handling process while preserving the core integration objectives of the project.

This redesign allowed the system to continue demonstrating:

- PLC-controlled automation
- Machine vision communication
- Industrial networking
- SCADA integration
- SQL logging
- Automated sequencing
- Subsystem coordination

despite the loss of robotic communication integration.

---

## Industrial Networking Challenges

### Profinet Integration

Industrial communication integration became one of the largest technical portions of the project.

Several challenges were encountered while integrating:

- Siemens PLC hardware
- Banner vision sensor
- SMC valve manifold
- Ignition SCADA system

#### Device Configuration Issues

Challenges included:

- GSD installation and configuration
- Device naming
- Static IP assignment
- IO module sizing
- Input/output mapping
- Profinet diagnostics

Much of the troubleshooting process involved validating communication settings between the scanner, PLC hardware configuration, and Profinet IO modules inside TIA Portal.

#### Communication Validation

Additional testing was required to:

- Verify cyclic IO behavior
- Confirm scanner trigger timing
- Validate Ethernet communication paths
- Debug incorrect module mappings
- Troubleshoot device-detection issues

The networking subsystem ultimately became one of the most integration-heavy portions of the project.

---

## QR Communication Challenges

### Message Buffer Limitations

One of the most significant technical problems encountered during development involved scanner message-size limitations and Profinet communication buffers.

#### Original Problem

Initial QR payload formats exceeded the configured Profinet input-buffer allocation, causing:

- Truncated scan messages
- Incomplete data fields
- Parsing failures
- Unstable communication behavior

The issue initially appeared to be inconsistent scanner behavior before being traced back to IO module sizing and communication mapping limitations.

#### Engineering Solution

Several engineering decisions were made to improve communication reliability:

- Increasing Profinet input-buffer allocation
- Correcting IO address mapping
- Reducing QR payload size
- Simplifying message formatting
- Optimizing parser structure

The final QR payload format preserved the required traceability information while significantly reducing communication overhead and improving parser reliability.

---

## PLC Architecture Challenges

### State-Machine Development

The PLC sequence architecture evolved significantly throughout development.

Initial implementations became increasingly difficult to maintain as:

- Additional sequence states were added
- Pneumatic timing became more complex
- Reset behavior expanded
- Manual controls were introduced
- Scanner integration requirements increased

#### Sequence Refinement

The final state-machine architecture focused heavily on:

- Deterministic behavior
- Sequence readability
- Timing coordination
- Safe state transitions
- Reset reliability
- Manual/automatic separation

Large portions of the sequence logic were repeatedly restructured throughout development as subsystem integration exposed new timing and synchronization problems.

---

## Pneumatic Coordination Challenges

### Vacuum Handoff Timing

Reliable battery transfer required significantly more timing coordination than originally expected.

#### Transfer Stability Issues

Early implementations encountered:

- Unstable handoffs
- Inconsistent battery positioning
- Vacuum release timing problems
- Reset-state complications
- Transfer synchronization issues

#### Engineering Improvements

The final implementation introduced:

- Overlapping vacuum states
- Dedicated handoff delays
- Structured transfer sequencing
- Improved reset behavior
- Deterministic motion coordination

These changes significantly improved transfer reliability and overall sequence stability.

---

## SCADA Integration Challenges

### Ignition & OPC UA

Several integration challenges were encountered while connecting Ignition to the Siemens PLC through OPC UA communication.

#### Major Issues

Challenges included:

- PLC string handling limitations
- Invalid tag states
- OPC browsing limitations
- Manual DB-offset mapping
- Perspective scripting issues
- Database configuration troubleshooting

#### String Handling Problems

One particularly difficult issue involved PLC string allocations and OPC communication reliability.

Oversized PLC string declarations caused:

- Invalid OPC tag states
- Broken sibling tags
- Unstable SCADA communication behavior

The final solution involved restructuring portions of the PLC data architecture and reducing string-size allocations to improve OPC stability.

---

## Real-World Project Constraints

### Schedule Compression

A major challenge throughout development involved adapting the project architecture while operating under compressed integration timelines.

Several external constraints affected development time, including:

- Hardware shipping delays
- Communication hardware availability
- Software licensing delays
- Integration troubleshooting
- Subsystem redesigns

These issues reduced the amount of available time for:

- Full-system integration
- Final validation
- Sequence refinement
- Hardware testing

#### Engineering Adaptation

The project required continuous reprioritization and scope management throughout development in order to preserve the most technically valuable portions of the system architecture.

The final implementation prioritized:

- Industrial integration
- PLC control architecture
- Machine vision communication
- SCADA functionality
- Pneumatic sequencing
- Database logging
- Networking reliability

while removing or simplifying portions of the original scope that could no longer be realistically completed within the available timeline.

---

## Engineering Decisions

### Architectural Priorities

Several major engineering decisions shaped the final system architecture.

#### Centralized PLC Control

The project used the Siemens PLC as the centralized controller for:

- Sequence logic
- Scanner communication
- Pneumatic coordination
- SCADA interaction
- System-state management

This simplified subsystem coordination and improved deterministic control behavior.

#### Separation of Control & Interface Layers

The PLC retained all real-time automation logic while Ignition handled:

- Visualization
- Operator interaction
- Diagnostics
- Database logging

This separation improved system stability and simplified troubleshooting.

#### Pneumatic-Only Fallback Architecture

The redesign away from robotic integration preserved the majority of the industrial automation architecture while simplifying the physical handling process enough to maintain a stable and demonstrable final system.

---

## Lessons Learned

### Engineering Experience

The project provided extensive experience in:

- Industrial automation integration
- PLC programming
- Machine vision communication
- Industrial networking
- SCADA development
- Pneumatic sequencing
- Real-world troubleshooting
- Systems-level debugging

#### Systems Integration

One of the largest lessons learned throughout development involved the complexity of integrating multiple industrial subsystems into a coordinated and reliable automation platform.

Many of the project challenges were not isolated programming problems, but instead involved:

- Communication architecture
- Timing coordination
- Hardware/software interaction
- State management
- Integration reliability
- Real-world engineering constraints

---

## Final Outcome

### Completed System Architecture

Despite multiple communication challenges, hardware limitations, and evolving project constraints, the final implementation successfully demonstrated:

- PLC-controlled automation
- Industrial Ethernet communication
- Machine vision integration
- Pneumatic sequencing
- SCADA monitoring
- SQL database logging
- Manual and automatic operation
- Real-time subsystem coordination

The completed project ultimately served as a fully integrated industrial automation platform demonstrating practical industrial control-system design, subsystem communication, and engineering problem-solving under real-world development constraints.
