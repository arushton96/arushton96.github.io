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

My primary responsibility on this project was the development and integration of the industrial control system architecture. This included PLC programming, pneumatic sequence development, machine vision integration, industrial networking configuration, SCADA communication, and overall subsystem coordination.

I designed and implemented the PLC control logic used throughout the project, including the automated pneumatic sequence, manual operating mode, state-machine architecture, reset behavior, vacuum handoff timing, scanner trigger logic, and SCADA interface mapping.

I also handled the integration of the Banner vision system with the Siemens PLC over Profinet communication. This included configuring industrial communication modules, troubleshooting device integration issues, restructuring QR payload formats to fit communication buffer limitations, and developing the PLC-based QR parsing logic used to process battery information.

On the SCADA side, I developed the Ignition Perspective interface used for system control, monitoring, diagnostics, and SQL database logging. This included PLC tag integration through OPC UA, operator interface design, manual testing controls, and database interaction for battery traceability logging.

Additional responsibilities included:

- Industrial Ethernet network configuration
- Static IP addressing and device communication setup
- Pneumatic sequence testing and debugging
- PLC/SCADA integration troubleshooting
- Database logging implementation
- Hardware integration and validation
- Technical documentation and system organization

Throughout development, I also played a major role in adapting the project architecture in response to hardware delays, communication limitations, and evolving project constraints. This included redesigning portions of the automation sequence, restructuring subsystem interactions, and preserving the core functionality of the project despite multiple scope and integration challenges.

## Project Evolution

The original project concept centered around a fully integrated industrial automation system that combined robotic handling, pneumatic transfer hardware, machine vision, industrial networking, and SCADA integration into a single automated workflow.

The planned system architecture included a FANUC robotic arm responsible for battery transfer operations, with the Siemens PLC coordinating robot communication, pneumatic sequencing, scanner interaction, and system monitoring through Profinet-based industrial communication.

During development, the project encountered several constraints that significantly impacted the final system architecture. The most significant issue involved delays related to the robot communication hardware required for Profinet integration. Although the robot itself supported Ethernet/IP communication, the required Profinet upgrade hardware was repeatedly delayed and ultimately unavailable within the project timeline.

As a result, the project scope was restructured to remove direct PLC-to-robot integration while preserving the core automation objectives of the system. The final implementation shifted toward a pneumatic-focused automated sequence that simulated the intended battery transfer process using coordinated vacuum handling, flipper control, machine vision integration, and PLC-controlled sequencing.

This redesign allowed the project to maintain its primary technical goals:

- Industrial device integration
- PLC-based automation control
- Pneumatic sequencing
- Machine vision communication
- SCADA integration
- Database logging
- Industrial networking architecture

Additional project challenges included industrial communication troubleshooting, scanner integration issues, QR message-size limitations, software licensing delays, and hardware shipping delays that compressed the available integration and testing timeline.

Throughout development, the system architecture continued to evolve as testing exposed new timing, communication, and sequencing challenges. Pneumatic handoff logic, scanner communication structures, reset behavior, and SCADA interaction were refined iteratively through subsystem testing and full-system integration.

Although portions of the original architecture were ultimately removed from scope, the final system successfully demonstrated a functional industrial automation platform capable of automated sequencing, QR-code scanning, SCADA interaction, industrial communication, and database logging using real industrial control hardware and software tools.

## Final Outcome

The completed project successfully demonstrated a functional industrial automation platform integrating PLC-based control logic, machine vision communication, pneumatic automation, industrial networking, SCADA monitoring, and database logging into a unified system architecture.

The final implementation operated as a coordinated automated sequence controlled by a Siemens S7-1200 PLC. Pneumatic vacuum hardware transferred battery cells through the handling sequence while a Banner vision sensor scanned QR-coded battery information over Profinet communication. Parsed battery data was transmitted to an Ignition SCADA interface through OPC UA, where operators could monitor system operation, control the sequence, and log scan information to a SQL database.

The completed system demonstrated:

- Automated pneumatic handling and transfer operations
- PLC-based sequence control using a state-machine architecture
- Profinet integration between industrial devices
- QR-code scanning and parsing
- Ignition SCADA integration
- SQL database logging and traceability
- Manual and automatic operating modes
- Real-time operator monitoring and diagnostics
- Industrial Ethernet communication architecture

Beyond the functional system itself, the project provided extensive experience in industrial automation integration, systems-level troubleshooting, communication architecture design, PLC programming, SCADA development, pneumatic sequencing, and industrial networking.

A major outcome of the project was the development of practical engineering problem-solving skills under real-world constraints. Hardware delays, communication limitations, integration issues, and evolving project requirements required continuous redesigns and technical tradeoff decisions throughout development.

Although the final system differed from the original project concept in several areas, the completed implementation successfully preserved the core engineering objectives of the project and demonstrated a fully operational industrial control platform built using professional automation hardware and software tools.
