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

### Siemens PLC

Overview of:
- Siemens S7-1200 PLC
- Centralized control responsibilities
- Sequence execution
- I/O handling
- Communication coordination

Topics:
- PLC as system controller
- Real-time control responsibilities
- Device coordination

## Vision System Architecture

### Banner QR Scanner

Overview of:
- Banner ABR3000 vision sensor
- QR scanning process
- Profinet communication with PLC
- Data transfer structure
- Trigger and acknowledgment behavior

Topics:
- Scan initiation
- Good read / no read handling
- QR message structure

## Pneumatic System Architecture

### Vacuum Handling System

Overview of:
- Pneumatic vacuum generators
- SMC valve manifold
- Vacuum transfer logic
- Flipper mechanism operation

Topics:
- Robot vacuum
- Flipper vacuum A/B
- Extend/retract motion
- Battery handoff timing

## SCADA Architecture

### Ignition Perspective Interface

Overview of:
- Ignition Gateway
- Perspective-based interface
- PLC communication through OPC UA
- Real-time monitoring and controls

Topics:
- Manual controls
- Automatic controls
- Status indicators
- Database interaction

## Database Architecture

### SQL Logging System

Overview of:
- Database structure
- Logged battery information
- Scan history storage
- SCADA query/display interaction

Topics:
- Battery scan logging
- Data persistence
- Traceability functionality

## Networking Architecture

### Industrial Ethernet Network

Overview of:
- Static IP configuration
- Unmanaged Ethernet switch
- Profinet device communication
- SCADA communication path

Topics:
- PLC IP configuration
- Vision system addressing
- SCADA connectivity
- OPC communication

## Communication Flow

Step-by-step description of how information moved through the system.

Example:
1. PLC triggers scanner
2. Scanner reads QR code
3. Scanner sends data over Profinet
4. PLC parses message
5. PLC updates SCADA tags
6. Ignition logs battery information to database

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
