---
title: Overview
---

# System Overview

## Project Objective

The objective of this project was to design and integrate an industrial automation system capable of identifying, tracking, and handling battery cells through a combination of machine vision, PLC-based control logic, pneumatic automation, and SCADA integration.

The original system concept centered around a fully integrated automated handling process using a FANUC robotic arm, pneumatic transfer hardware, QR-code scanning, and industrial networking. Battery cells would be transferred through the system, scanned using a machine vision sensor, and logged through a SCADA interface connected to a SQL database.

As the project evolved, hardware availability and communication limitations required portions of the system architecture to be redesigned. The final implementation shifted toward a pneumatic-focused automation platform while preserving the core goals of industrial device integration, automated sequencing, vision-system communication, operator interaction, and data logging.

The completed system successfully demonstrated:
- PLC-controlled automated sequencing
- QR-code scanning and parsing
- Profinet industrial communication
- Ignition SCADA integration
- SQL database logging
- Manual and automatic operating modes
- Pneumatic battery handling and transfer

## System Summary

The completed system operated as a coordinated industrial automation platform centered around a Siemens S7-1200 PLC.

During operation, a battery cell would enter the handling sequence and be transferred through a pneumatic vacuum system controlled by the PLC. The PLC coordinated vacuum actuation, flipper movement, sequence timing, and communication between all major subsystems.

Once positioned within the scanning area, the PLC triggered a Banner vision sensor over Profinet to read the QR code attached to the battery cell. The scanner returned identification data to the PLC, where the message was parsed and processed into individual battery information fields such as serial number, voltage, capacity, and manufacturing data.

The PLC then transmitted system status and parsed battery information to an Ignition SCADA interface through OPC communication. The SCADA system provided operator controls, real-time monitoring, diagnostic visibility, and SQL database logging for scanned battery information.

The final system integrated:
- Siemens S7-1200 PLC
- Banner ABR3000 vision sensor
- Ignition Perspective SCADA interface
- SQL database logging
- Pneumatic vacuum handling hardware
- Profinet industrial networking
- Manual and automatic sequence control

The project emphasized real-world industrial integration challenges including industrial networking, PLC programming, machine vision communication, pneumatic coordination, SCADA architecture, and system-level troubleshooting.

Example flow:
1. Battery enters system
2. Vision system scans QR code
3. PLC processes identification data
4. Pneumatic sequence transfers battery
5. SCADA displays/logs information
6. Database records scan results

## Final System Architecture

System-level block diagram or architecture image.

![System Architecture](Images/SystemArchitecture.png)

## Major Subsystems

### PLC Control System

The Siemens S7-1200 PLC served as the central controller for the entire automation system. All automated sequencing, pneumatic output control, scanner communication, SCADA interaction, and status management were coordinated through the PLC.

The PLC logic was structured around a state-machine architecture that controlled both automatic and manual operating modes. It managed sequence timing, vacuum handoff coordination, scanner triggering, reset behavior, and communication between the various industrial devices connected throughout the system.

In addition to direct control responsibilities, the PLC also handled QR-code parsing and mapped system information to the SCADA interface for operator monitoring and database logging.

### Vision System

The vision subsystem used a Banner ABR3000 vision sensor to scan QR-coded battery cells during operation. The scanner communicated with the PLC over Profinet and returned identification data that was processed and parsed inside the PLC logic.

The system was designed to trigger scans automatically during the pneumatic handling sequence and verify successful reads using dedicated good-read and no-read status signals.

A significant portion of the vision-system development focused on industrial communication reliability, including Profinet configuration, message-size optimization, and QR payload restructuring to fit within communication buffer limitations while preserving required traceability information.

### Pneumatic Handling System

The pneumatic subsystem handled the physical transfer and positioning of battery cells throughout the automated sequence. Vacuum generators, pneumatic solenoids, and a flipper transfer mechanism were used to coordinate battery movement between handling stages.

The system used multiple vacuum channels to maintain controlled handoffs during battery transfer operations. Timing coordination between vacuum outputs, flipper movement, and scanner positioning was managed directly by the PLC.

The final implementation focused heavily on reliable sequencing, vacuum overlap timing, reset behavior, and safe operation during both automatic and manual modes.

### SCADA / HMI System

The SCADA subsystem was developed using Ignition Perspective and provided the primary operator interface for the project. The interface allowed users to control the automated sequence, monitor system status, manually actuate pneumatic outputs, and view scanned battery information in real time.

Communication between the PLC and SCADA system was handled through OPC UA, allowing Ignition to monitor PLC tags and exchange system commands and status information.

The SCADA interface also integrated with a SQL database to log battery scan information for traceability and system monitoring purposes. Additional diagnostic and testing pages were created throughout development to support troubleshooting and subsystem validation.

### Networking & Communications

The project used an industrial Ethernet architecture to connect the PLC, vision system, SCADA workstation, and pneumatic communication hardware.

Profinet communication was used for real-time industrial device integration between the Siemens PLC and the Banner scanner as well as the pneumatic valve manifold. Static IP addressing and industrial network configuration were used to maintain reliable communication between all major subsystems.

The networking architecture also supported OPC UA communication between the PLC and Ignition SCADA system, allowing real-time monitoring, control, and database logging throughout operation.

## My Contributions

Short summary of the portions you personally designed or implemented.

## Project Evolution

Short discussion of how the system changed throughout development.

Topics:
- Original robot integration plan
- Hardware delays
- Scope reduction
- Pneumatic-only automated sequence
- Communication redesigns
- Integration challenges

## Final Outcome

Short concluding section describing:
- what was successfully demonstrated
- what subsystems were operational
- what engineering goals were achieved
