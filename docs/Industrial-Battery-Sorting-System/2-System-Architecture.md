---
title: System Architecture
---

# System Architecture

## Architecture Overview

High-level explanation of how the major hardware and software systems interacted within the project.

Describe:
- PLC as the central controller
- Vision system providing battery identification data
- Pneumatic hardware handling battery transfer operations
- SCADA system providing operator interaction and monitoring
- Database system storing scan information
- Industrial Ethernet network connecting devices

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
