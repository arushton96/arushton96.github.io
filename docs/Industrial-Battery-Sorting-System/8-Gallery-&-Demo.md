---
title: Gallery & Demonstrations
---

# Gallery & Demonstrations

## Overview

This section contains screenshots, hardware images, development captures, PLC logic screenshots, SCADA interfaces, pneumatic-system photos, and demonstration media captured throughout development of the Automated Battery Sorting System.

The gallery highlights:
- Final system integration
- PLC development
- SCADA functionality
- Machine vision integration
- Pneumatic automation
- Industrial networking
- Testing and troubleshooting workflows

The images below document both the completed system and the engineering process used throughout subsystem integration and validation.

---

# Final Integrated System

## Completed System Assembly

The completed system integrated:
- Siemens S7-1200 PLC
- Banner ABR3000 vision sensor
- Ignition Perspective SCADA system
- Pneumatic vacuum handling hardware
- SMC valve manifold
- Industrial Ethernet communication
- SQL database logging

The final architecture demonstrated coordinated operation between all major industrial subsystems within a single automated workflow.

![Final System Overview](Images/FinalSystem.png)

---

# System Demonstration

## Automated Sequence Operation

During operation, the system performed:
1. Pneumatic battery pickup and transfer
2. Scanner positioning
3. QR-code acquisition
4. PLC-based message parsing
5. SCADA visualization
6. SQL database logging
7. Sequence completion and reset

The automated sequence demonstrated coordinated interaction between the PLC, scanner, pneumatic hardware, and SCADA system.

### Demonstration Features

The final demonstration included:
- Automatic operation
- Manual operation
- Real-time SCADA monitoring
- Scanner-status handling
- Database logging
- Pneumatic diagnostics
- QR-data visualization

Optional media:
- Demonstration video
- Animated sequence capture
- Integrated system walkthrough

---

# SCADA Interface

## Main Operator Interface

The primary SCADA interface provided:
- Start/stop/reset controls
- Auto/manual mode selection
- Real-time status monitoring
- Pneumatic-state indicators
- Battery scan information
- Scanner diagnostics

The interface was designed to provide centralized visibility into the overall system state during operation.

![Main SCADA Interface](Images/MainSCADA.png)

---

## Manual Control Interface

A dedicated manual-control page was developed to support:
- Pneumatic testing
- Output verification
- Sequence debugging
- Hardware validation
- Scanner integration testing

This interface became one of the primary debugging tools used throughout subsystem integration and testing.

![Manual Control Interface](Images/ManualSCADA.png)

---

## Database Logging

The Ignition SCADA interface integrated directly with a SQL database used for battery traceability logging.

Logged information included:
- Battery serial numbers
- Manufacturer information
- Voltage
- Capacity
- Resistance
- Scan timestamps

The database interface was used to validate:
- Scanner communication
- PLC parsing logic
- Database connectivity
- End-to-end subsystem integration

![Database Logging](Images/DatabaseLogging.png)

---

# PLC Development

## PLC Program Structure

The PLC architecture was organized around:
- State-machine sequencing
- Pneumatic coordination
- QR parsing logic
- SCADA interface mapping
- Industrial communication handling

The project utilized structured PLC organization through:
- Organizational blocks
- Function blocks
- Structured data blocks
- SCL parsing functions

![PLC Program Structure](Images/PLCProgramStructure.png)

---

## State Machine Logic

The automated sequence was controlled through a PLC-based state-machine architecture coordinating:
- Pneumatic outputs
- Scanner timing
- Vacuum handoffs
- Reset behavior
- Sequence-state transitions

The final sequence architecture evolved significantly throughout development as subsystem testing exposed new timing and synchronization challenges.

![PLC State Machine](Images/PLCStateMachine.png)

---

## QR Parsing Logic

Custom SCL parsing functions were developed to process incoming QR messages directly within the PLC.

The parser extracted:
- Battery identification data
- Voltage information
- Capacity values
- Resistance values
- Manufacturing information

Significant engineering effort was spent optimizing:
- Message structure
- Buffer handling
- String parsing
- Communication reliability

![QR Parsing Logic](Images/QRParsing.png)

---

# Vision System

## Banner Scanner Integration

The Banner ABR3000 vision sensor was integrated with the Siemens PLC through Profinet communication and served as the primary battery-identification subsystem.

The scanner architecture included:
- PLC-triggered acquisition
- Good-read/no-read handling
- QR-code parsing
- Industrial Ethernet communication
- SCADA integration

![Banner Vision Sensor](Images/BannerScanner.png)

---

## QR Scan Results

The scanner transmitted raw QR payloads to the PLC, where the message was parsed into structured battery information fields for display and logging.

The final implementation optimized QR payload structure to improve:
- Communication reliability
- Parser stability
- Profinet compatibility
- Scan consistency

![QR Scan Results](Images/QRResults.png)

---

# Pneumatic System

## Pneumatic Hardware

The pneumatic subsystem utilized:
- Vacuum generators
- SMC valve manifold control
- Pneumatic transfer hardware
- PLC-controlled sequencing

The pneumatic architecture coordinated:
- Battery transfer
- Vacuum overlap timing
- Scanner positioning
- Sequence-state synchronization

![Pneumatic Assembly](Images/PneumaticAssembly.png)

---

## Flipper Transfer Mechanism

A pneumatic flipper assembly coordinated battery transfer between handling stages and maintained stable positioning during scanning operations.

The mechanism relied heavily on:
- Vacuum overlap timing
- Extend/retract coordination
- PLC-controlled sequencing
- Deterministic transfer behavior

![Flipper Mechanism](Images/FlipperMechanism.png)

---

## Pneumatic Testing

Extensive subsystem testing was performed throughout development to validate:
- Vacuum timing
- Transfer stability
- Reset behavior
- Sequence reliability
- Scanner synchronization

Manual testing interfaces were heavily used during subsystem integration and troubleshooting.

![Pneumatic Testing](Images/PneumaticTesting.png)

---

# Industrial Networking

## Ethernet & Communication Architecture

The system utilized an industrial Ethernet architecture connecting:
- Siemens PLC
- Banner scanner
- SMC valve manifold
- Ignition SCADA workstation

Communication technologies included:
- Profinet IO
- OPC UA
- Industrial Ethernet
- SQL database integration

![Network Architecture](Images/NetworkSetup.png)

---

# Hardware Integration

## Industrial Control Hardware

The completed system integrated several industrial hardware platforms into a unified automation architecture.

Major hardware components included:
- Siemens S7-1200 PLC
- SITOP power supply
- Banner scanner
- SMC valve manifold
- Ethernet switch
- Pneumatic hardware

![Control Hardware](Images/ControlHardware.png)

---

# Development Process

## Integration & Troubleshooting

Development involved extensive subsystem testing and iterative troubleshooting across:
- PLC logic
- Scanner communication
- Pneumatic timing
- SCADA integration
- Database logging
- Industrial networking

The final implementation evolved significantly throughout development as testing exposed new sequencing and communication challenges.

![Development Process](Images/DevelopmentProcess.png)

---

## Project Evolution

The project architecture evolved considerably throughout development due to:
- Communication hardware delays
- Industrial integration challenges
- Pneumatic redesigns
- Sequence refinements
- Real-world engineering constraints

The final implementation preserved the core industrial automation objectives of the project while adapting the physical handling architecture to maintain a stable and demonstrable system.

![Project Evolution](Images/ProjectEvolution.png)

---

# Final Demonstration Summary

## Completed System Demonstration

The completed project successfully demonstrated:
- PLC-controlled automation
- Industrial Ethernet integration
- Machine vision communication
- Pneumatic sequencing
- Ignition SCADA functionality
- SQL database logging
- Manual and automatic operation
- Real-time subsystem coordination

The final implementation served as a fully integrated industrial automation platform combining multiple industrial hardware and software systems into a coordinated working system.
