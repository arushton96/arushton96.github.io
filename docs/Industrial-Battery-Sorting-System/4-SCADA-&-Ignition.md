---
title: SCADA & Ignition
---

# SCADA / Ignition Interface

## SCADA Overview

Overview of the SCADA system and its role within the project.

Describe:
- Purpose of the operator interface
- Real-time monitoring and control
- Communication with the PLC
- Database interaction
- Diagnostic and testing functionality

## Ignition Platform

### Ignition Perspective

Overview of:
- Ignition Gateway setup
- Perspective module usage
- Browser-based interface design
- Tag-based architecture

Topics:
- Perspective vs Vision
- Gateway configuration
- Project structure
- Device connections

## PLC Communication

### OPC UA Integration

Overview of communication between Ignition and the Siemens PLC.

Topics:
- OPC UA device configuration
- S7-1200 communication setup
- Tag browsing and mapping
- Manual DB tag configuration
- Read/write operations

## SCADA Page Layout

### Main Operator Interface

Overview of the primary control screen.

Topics:
- System status indicators
- Start/stop/reset controls
- Auto/manual mode selection
- Battery information display
- Cycle monitoring

Insert screenshots of the main interface.

![Main SCADA Page](Images/MainSCADA.png)

## Manual Control Interface

### Pneumatic Testing & Diagnostics

Overview of the manual control page used during development and testing.

Topics:
- Direct valve control
- Pneumatic testing
- Sensor/status verification
- Debugging workflow
- Manual operation safeguards

Insert screenshots of the manual page.

![Manual Control Page](Images/ManualSCADA.png)

## Tag Architecture

### PLC to SCADA Mapping

Overview of the tag structure used between the PLC and SCADA system.

Topics:
- `usr_` command tags
- `sts_` status tags
- Internal PLC tags
- Database logging tags
- Camera/QR data tags

Describe:
- Naming conventions
- Read/write separation
- Organization strategy

## User Controls

### System Commands

Overview of implemented operator controls.

Topics:
- Start cycle
- Stop cycle
- Reset logic
- Manual controls
- Mode switching

Discuss:
- Safety considerations
- Operator restrictions
- Mode lockouts

## Status & Diagnostics

### System Monitoring

Overview of status indicators and diagnostic functionality.

Topics:
- Cycle state indicators
- Vacuum status
- Scanner status
- Auto/manual mode indicators
- Error handling
- Communication diagnostics

## Database Integration

### Battery Logging Interface

Overview of how Ignition interacted with the SQL database.

Topics:
- Database connection setup
- Insert queries
- Logged battery information
- Query browser testing
- Table display configuration

Insert screenshots of database tables or logging results.

![Database Logging](Images/DatabaseLogging.png)

## UI Design Decisions

Discussion of interface design choices and engineering considerations.

Possible topics:
- Button-based controls vs switches
- Status visibility
- Simplified operator interaction
- Readability improvements
- Leading-zero handling for serial numbers

## SCADA Challenges

Discussion of development and integration challenges.

Possible topics:
- OPC tag import limitations
- String handling issues
- Database configuration
- Tag path troubleshooting
- Perspective scripting issues
- Gateway communication troubleshooting

## Final Outcome

Summary of the completed SCADA functionality and final capabilities.

Describe:
- Fully operational controls
- Real-time monitoring
- Database logging
- Stable PLC communication
- Manual and automatic control support
