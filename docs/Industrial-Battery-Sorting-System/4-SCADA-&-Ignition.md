---
title: SCADA & Ignition
---

# SCADA / Ignition Interface

## SCADA Overview

The SCADA subsystem was developed using Ignition Perspective and served as the primary operator interface for the Automated Battery Sorting System. The interface provided real-time monitoring, operator controls, manual testing functionality, and database interaction throughout development and final system operation.

The SCADA system acted as the primary bridge between the operator and the PLC-controlled automation sequence. Through OPC UA communication, Ignition exchanged commands and status information with the Siemens S7-1200 PLC while displaying system-state information, battery data, pneumatic status, and scanner results in real time.

The interface was designed to support both normal system operation and subsystem debugging throughout development. In addition to standard operator controls, multiple diagnostic and testing interfaces were developed to simplify troubleshooting and validate communication between the PLC, scanner, pneumatic hardware, and SQL database.

The completed SCADA system supported:

- Real-time system monitoring
- Automatic and manual operating modes
- Pneumatic output diagnostics
- Scanner-status monitoring
- Battery information display
- SQL database logging
- PLC communication diagnostics
- Development and testing workflows

---

## Ignition Platform

### Ignition Perspective

The SCADA system was developed using Ignition Perspective running through an Ignition Gateway environment. Perspective was selected due to its browser-based architecture, modern interface capabilities, and strong support for industrial communication through OPC UA integration.

The project architecture separated the PLC control logic from the operator interface layer, allowing the Siemens PLC to maintain deterministic control over the automation sequence while Ignition handled visualization, operator interaction, and database communication.

The Ignition project was organized around:

- PLC device connections
- OPC UA tag structures
- Perspective views and pages
- SQL database integration
- Real-time status bindings
- Operator-control scripts

The Perspective interface was accessible through a browser-based client connected to the Ignition Gateway and provided centralized access to system controls and diagnostics throughout development and testing.

A major advantage of using Ignition Perspective was the ability to rapidly iterate on interface layouts and create dedicated diagnostic pages during subsystem integration and troubleshooting.

---

## PLC Communication

### OPC UA Integration

Communication between Ignition and the Siemens S7-1200 PLC was handled through OPC UA using Ignition’s built-in Siemens device driver configuration.

The PLC exposed command tags, status values, parsed QR data, and sequence-state information to Ignition through dedicated SCADA interface data blocks. Ignition then mapped these values into Perspective tags used throughout the operator interface.

The communication architecture supported:

- Real-time monitoring
- Operator command handling
- Status visualization
- Manual output control
- Database logging
- Diagnostic feedback

During development, several challenges were encountered related to PLC string handling and tag importing. User-defined PLC data structures did not always browse correctly through OPC UA, requiring portions of the tag architecture to be mapped manually using direct DB-offset addressing.

Additional work was required to properly handle PLC string structures and prevent invalid tag states caused by oversized string allocations within the SCADA interface mapping blocks.

Despite these challenges, the final OPC UA integration provided stable and reliable communication between the PLC and Ignition throughout operation.

---

## SCADA Page Layout

### Main Operator Interface

The primary operator interface was designed to provide centralized control and monitoring for the automated sequence while maintaining a simple and readable layout.

The main page displayed:

- System operating mode
- Cycle-state information
- Scanner status
- Pneumatic status indicators
- Battery scan information
- Operator command controls
- Sequence activity indicators

Operators could start, stop, and reset the automation sequence directly from the interface while monitoring real-time system behavior throughout operation.

The page layout emphasized readability and quick status visibility, allowing important system information to remain visible without overwhelming the interface with unnecessary controls or diagnostics.

![Main SCADA Page](Images/MainSCADA.png)

---

## Manual Control Interface

### Pneumatic Testing & Diagnostics

A dedicated manual-control page was developed to support subsystem testing and pneumatic debugging throughout development.

The manual interface allowed operators to directly actuate pneumatic outputs independently of the automatic sequence and was heavily used during:

- Pneumatic testing
- Vacuum tuning
- Sequence debugging
- Output verification
- Scanner integration
- Hardware validation

The page exposed direct controls for:

- Vacuum outputs
- Flipper extension/retraction
- Scanner triggering
- Pneumatic diagnostics
- Sequence-state testing

Additional safeguards and lockouts were implemented to prevent conflicts between automatic and manual operation while maintaining safe output behavior during testing.

The manual-control architecture became one of the most useful debugging tools throughout subsystem integration and final sequence tuning.

![Manual Control Page](Images/ManualSCADA.png)

---

## Tag Architecture

### PLC to SCADA Mapping

The PLC-to-SCADA interface was organized using a structured tag-mapping architecture designed to separate operator commands, PLC-controlled values, status indicators, and parsed battery information into clearly defined groups.

A naming convention was implemented throughout the project to improve readability and simplify troubleshooting.

Major tag groups included:

- `usr_` tags for user and SCADA commands
- `plc_` tags for internal PLC logic
- `sts_` tags for system status outputs
- `QR_` tags for parsed battery information
- Camera and scanner status tags
- Database logging values

This structure simplified:

- Tag browsing
- OPC mapping
- Debugging
- Interface development
- Database integration

Read/write separation was also maintained throughout the architecture to prevent unintended operator interaction with internal PLC-controlled values.

---

## User Controls

### System Commands

The SCADA interface provided operators with direct access to the major system commands required during operation and testing.

Primary controls included:

- Start cycle
- Stop cycle
- Reset sequence
- Automatic/manual mode selection
- Manual pneumatic controls
- Scanner trigger controls

A significant portion of the interface design focused on preventing unsafe or misleading operator interaction. Mode changes and manual controls were restricted using PLC-side permissive logic to prevent invalid state transitions during active sequences.

Additional consideration was given to:

- Operator readability
- Control visibility
- Sequence-state feedback
- Simplified troubleshooting
- Safe interaction during testing

The final interface emphasized predictable and straightforward operator interaction throughout both automatic and manual operation.

---

## Status & Diagnostics

### System Monitoring

Real-time status monitoring was integrated throughout the SCADA interface to provide visibility into system operation and subsystem behavior.

Displayed status information included:

- Current sequence step
- Operating mode
- Pneumatic output states
- Scanner status
- Good-read/no-read results
- Vacuum activity
- Reset and stop conditions
- Communication status indicators

Additional diagnostic indicators were developed throughout integration testing to simplify troubleshooting and validate subsystem communication during development.

The status architecture became especially important during:

- Pneumatic timing adjustments
- Scanner communication debugging
- Reset-sequence validation
- OPC communication testing
- Full-system integration

---

## Database Integration

### Battery Logging Interface

Ignition was used to interface with a SQL database for battery traceability logging throughout operation.

Parsed battery information received from the PLC was inserted into a structured database table using Perspective scripting and SQL query execution.

Logged information included:

- Battery serial numbers
- Manufacturer information
- Voltage
- Capacity
- Resistance
- Scan timestamps
- Additional QR-derived values

The database interface was used to:

- Demonstrate traceability functionality
- Validate scanner communication
- Verify QR parsing logic
- Store scan-history information

Additional testing and validation were performed using Ignition’s built-in database query browser throughout development.

![Database Logging](Images/DatabaseLogging.png)

---

## UI Design Decisions

Several design decisions were made throughout development to improve usability, readability, and system visibility within the SCADA interface.

One major decision involved using button-based controls rather than toggle switches for many operator interactions. Buttons provided more deterministic command behavior and reduced the likelihood of mismatched interface states during operation.

Additional UI priorities included:

- Clear operating-state visibility
- Simplified control layouts
- Readable battery information displays
- Real-time feedback
- Minimal operator confusion
- Fast troubleshooting access

Special consideration was also given to preserving leading-zero formatting for serial-number displays and improving readability of parsed QR data throughout the interface.

---

## SCADA Challenges

Several challenges were encountered during SCADA development and subsystem integration.

Major challenges included:

- OPC UA tag-import limitations
- PLC string-handling issues
- Invalid tag states caused by oversized strings
- Manual DB-offset mapping requirements
- Perspective scripting issues
- Database configuration troubleshooting
- Gateway communication debugging

Additional complexity was introduced by the evolving PLC architecture and changing subsystem requirements throughout development.

Many portions of the final interface evolved iteratively as testing exposed new communication, readability, and troubleshooting requirements during subsystem integration.

---

## Final Outcome

The completed SCADA system successfully provided real-time monitoring, operator interaction, diagnostic visibility, and database integration for the Automated Battery Sorting System.

The final implementation demonstrated:

- Stable OPC UA communication
- Real-time PLC monitoring
- Manual and automatic operating modes
- Pneumatic diagnostics
- Scanner-status visualization
- SQL database logging
- Integrated operator controls
- Development and troubleshooting support

The SCADA subsystem ultimately became one of the primary integration layers connecting the PLC, operator interface, machine vision system, and database architecture into a unified industrial automation platform.
