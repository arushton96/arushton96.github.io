---
title: System Architecture
---

# System Architecture

## Architecture Overview

The Automated Battery Sorting System was designed as an integrated industrial automation platform combining PLC-based control, machine vision, pneumatic handling, industrial networking, SCADA monitoring, and database logging into a unified system architecture.

At the center of the system was a Siemens S7-1200 PLC, which acted as the primary controller for all automated operations. The PLC coordinated pneumatic sequencing, scanner communication, operator commands, timing control, status monitoring, and communication between all major subsystems.

The pneumatic handling subsystem was responsible for transferring and positioning battery cells throughout the automated sequence. Vacuum generators, pneumatic solenoids, and a flipper transfer mechanism were used to coordinate battery movement between handling stages and maintain stable positioning during scanning operations.

Battery identification was handled by a Banner ABR3000 vision sensor connected to the PLC over Profinet communication. During operation, the PLC triggered the scanner to read QR-coded battery information and receive the resulting message data through industrial Ethernet communication. The PLC then parsed the received message into structured battery information fields used throughout the rest of the system.

An Ignition Perspective SCADA interface provided the primary operator interaction layer for the project. Through OPC UA communication, Ignition monitored PLC status tags, issued operator commands, displayed scanned battery information, and provided manual testing and diagnostic controls during development and operation.

The SCADA subsystem also integrated with a SQL database used to log scanned battery information for traceability and system monitoring purposes. Parsed battery data transmitted from the PLC was recorded through the Ignition interface and stored in a structured database table for later review and validation.

Industrial Ethernet networking connected all major hardware devices throughout the system. Static IP addressing and Profinet communication were used to maintain reliable communication between the PLC, vision system, pneumatic communication hardware, and SCADA workstation.

The final architecture emphasized:
- Centralized PLC-based control
- Industrial communication integration
- Modular subsystem coordination
- Real-time operator monitoring
- Reliable pneumatic sequencing
- Vision-based identification and traceability
- Scalable industrial automation design principles

Although the final implementation evolved significantly throughout development, the completed system successfully demonstrated the integration of multiple industrial automation technologies into a coordinated and functional control platform.

## Overall System Diagram

Insert a full system architecture or communication diagram.

![System Architecture Diagram](Images/SystemArchitecture.png)

## Control System Architecture

### Siemens S7-1200 PLC

The Siemens S7-1200 PLC served as the central controller for the entire automation system. All automated sequencing, pneumatic output control, scanner communication, SCADA interaction, timing coordination, and system-state management were coordinated through the PLC.

The control architecture was designed around a state-machine sequence structure that managed both automatic and manual operating modes. The PLC handled communication between all major subsystems while maintaining deterministic control over pneumatic sequencing, scan timing, reset behavior, and operator interaction.

The PLC also acted as the primary communication hub for the system by:
- Triggering the vision system
- Parsing incoming QR-code data
- Mapping tags to the SCADA interface
- Coordinating pneumatic outputs
- Managing system status information
- Supporting SQL database logging through Ignition

---

## Vision System Architecture

### Banner ABR3000 Vision Sensor

The vision subsystem used a Banner ABR3000 vision sensor to scan QR-coded battery cells during operation. The scanner communicated directly with the PLC using Profinet industrial communication and returned battery identification data through configured input modules.

During the automated sequence, the PLC triggered the scanner once the battery reached the correct scan position. The scanner then processed the QR code and transmitted the resulting message back to the PLC for parsing and validation.

The vision-system architecture included:
- Profinet-based industrial communication
- Trigger-based scan control
- Good-read and no-read status handling
- PLC-based QR parsing
- Reduced QR payload optimization
- Real-time integration with SCADA and database logging

A major portion of the scanner integration work involved configuring communication modules, troubleshooting Profinet integration issues, and restructuring QR payload formatting to fit within communication buffer limitations while preserving required battery traceability information.

---

## Pneumatic System Architecture

### Vacuum Handling & Transfer System

The pneumatic subsystem was responsible for the physical handling and transfer of battery cells throughout the automated sequence. Vacuum generators, pneumatic solenoids, and a flipper transfer mechanism were coordinated by the PLC to move batteries between handling stages.

The system used multiple vacuum channels to maintain controlled battery handoffs during transfer operations. Extend/retract motion and vacuum timing were synchronized through PLC-controlled sequencing to maintain stable battery positioning during scanner operations.

The pneumatic architecture included:
- SMC valve manifold control
- Vacuum generation hardware
- Robot vacuum simulation
- Dual flipper vacuum channels
- Extend/retract motion control
- Timed vacuum handoff coordination

The final sequence architecture emphasized reliable transfer timing, safe reset behavior, and stable operation during both automatic and manual operating modes.

---

## SCADA Architecture

### Ignition Perspective Interface

The SCADA subsystem was developed using Ignition Perspective and provided the primary operator interface for the system. The interface allowed operators to monitor system status, control automated operation, manually actuate pneumatic outputs, and view scanned battery information in real time.

Communication between Ignition and the PLC was handled through OPC UA, allowing the SCADA system to exchange commands and status information with the control system.

The SCADA architecture included:
- Real-time operator controls
- System status monitoring
- Manual and automatic operating modes
- Pneumatic diagnostics
- QR-data visualization
- SQL database logging
- Development and testing interfaces

Additional diagnostic pages were developed throughout integration and testing to support subsystem validation and troubleshooting.

---

## Database Architecture

### SQL Logging System

The database subsystem was used to store scanned battery information for traceability and system monitoring purposes. Parsed QR-code data received by the PLC was transmitted to Ignition and inserted into a SQL database table through the SCADA interface.

The logging architecture allowed scanned battery information to be:
- Recorded automatically during operation
- Viewed through Ignition
- Queried for testing and validation
- Used for traceability demonstrations

The database structure included fields for:
- Battery serial number
- Manufacturer information
- Voltage
- Capacity
- Resistance
- Date information
- Scan timestamps

The database subsystem demonstrated integration between industrial control hardware, SCADA software, and persistent data storage systems.

---

## Networking Architecture

### Industrial Ethernet Communication

The system used an industrial Ethernet architecture to connect all major devices throughout the project. Static IP addressing and Profinet communication were used to maintain reliable communication between the PLC, scanner, pneumatic communication hardware, and SCADA workstation.

The PLC acted as the primary communication coordinator for the industrial network while Ignition interfaced with the control system through OPC UA communication.

The networking architecture included:
- Static IP device configuration
- Industrial Ethernet communication
- Profinet device integration
- OPC UA communication
- Scanner communication mapping
- SCADA connectivity
- SQL communication support

The network structure was designed to support reliable subsystem communication while maintaining separation between industrial device communication and SCADA-level monitoring and control functionality.

## Communication Flow

The Automated Battery Sorting System operated through coordinated communication between the PLC, vision system, pneumatic hardware, SCADA interface, and SQL database. The Siemens S7-1200 PLC acted as the central communication controller and coordinated all major subsystem interactions throughout operation.

During automatic operation, the communication flow followed the sequence below:

1. A start command was issued through the Ignition SCADA interface or directly through PLC logic.

2. The PLC initiated the automated pneumatic sequence and controlled the vacuum handling hardware through the SMC valve manifold over Profinet communication.

3. Once the battery reached the scan position, the PLC triggered the Banner ABR3000 vision sensor using a dedicated trigger output.

4. The vision sensor scanned the QR code attached to the battery cell and transmitted the resulting message data back to the PLC through Profinet input modules.

5. The PLC validated the received message and parsed the QR payload into individual battery information fields including serial number, manufacturer information, voltage, capacity, resistance, and manufacturing data.

6. Parsed battery information and system-status values were mapped to the SCADA interface through OPC UA communication between the Siemens PLC and Ignition Gateway.

7. The Ignition SCADA interface displayed the battery information in real time while simultaneously logging the scan results into a SQL database for traceability and monitoring purposes.

8. The PLC completed the remainder of the pneumatic transfer sequence and returned the system to a ready state for the next cycle.

Throughout operation, communication between subsystems remained continuous. The PLC continuously exchanged:
- Status information with Ignition
- Control commands with pneumatic hardware
- Scanner trigger and acknowledgment signals with the vision system
- Sequence-state information with the SCADA interface
- Parsed battery data for database logging

The communication architecture combined:
- Profinet industrial communication
- OPC UA SCADA communication
- Industrial Ethernet networking
- SQL database integration

This layered communication structure allowed the system to coordinate real-time automation control, operator interaction, machine vision processing, and persistent data logging within a single integrated industrial automation platform.

## Design Decisions

Discussion of major architectural decisions and why they were made.

Possible topics:
- Centralized PLC architecture
- Profinet device integration
- Pneumatic-only fallback operation
- Separation of SCADA and control logic
- Database logging implementation

## Architecture Challenges

Overview of integration and system-level challenges.

Possible topics:
- Robot communication limitations
- Profinet compatibility issues
- QR message size limitations
- Device integration troubleshooting
- Hardware delays and redesigns
