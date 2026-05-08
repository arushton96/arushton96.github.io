---
title: Vision System
---

# Vision System Integration

## Vision System Overview

Overview of the machine vision subsystem and its role within the project.

Describe:
- QR-code identification of battery cells
- Communication with the PLC
- Scan validation
- Data transfer to SCADA and database systems

## Vision Hardware

### Banner ABR3000 Vision Sensor

Overview of the vision hardware used in the project.

Topics:
- Scanner model and capabilities
- QR-code scanning functionality
- Industrial communication support
- Physical mounting and positioning

Insert images of the scanner and system setup.

![Banner Vision Sensor](Images/BannerScanner.png)

## System Role

Explanation of how the scanner interacted with the automated sequence.

Topics:
- Scan trigger timing
- Battery positioning
- Scan verification
- Good read / no read handling
- Interaction with pneumatic sequence

## Profinet Integration

### PLC Communication Setup

Overview of how the vision system was integrated with the Siemens PLC.

Topics:
- Profinet device configuration
- GSD installation in TIA Portal
- Device naming and IP assignment
- Input/output module configuration
- Industrial Ethernet integration

Discuss:
- Accessible devices configuration
- Online diagnostics
- Hardware compile/download requirements

## Scanner Trigger Logic

Explanation of how the PLC initiated scans.

Topics:
- Trigger command outputs
- One-shot trigger behavior
- Trigger timing
- Scanner acknowledgment signals
- Good read / no read bits

Possible signals:
- `TriggerCmd_Out`
- `Good_Read`
- `No_Read`

## QR Data Structure

### QR Message Format

Overview of the QR-code payload structure.

Topics:
- Original long-form payload
- Reduced short-form payload
- Field formatting
- Delimiters and message structure
- Data organization

Describe:
- Make
- Model
- Serial number
- Date of manufacture
- Voltage
- Capacity
- Resistance

## QR Payload Optimization

Discussion of why the QR payload was reduced and how the final structure was designed.

Topics:
- Profinet buffer limitations
- 128-byte communication limits
- Original message size issues
- Reduced payload implementation
- Improved communication reliability

Discuss:
- Reduction from approximately 130 bytes to approximately 68 bytes
- Maintaining required traceability information
- Buffer headroom considerations

## PLC Parsing Integration

Overview of how scanner data was processed inside the PLC.

Topics:
- Input buffer handling
- STX/CR message validation
- Field extraction
- String handling
- Parsed value storage

Mention:
- `FB_QR_Parse`
- `FC_GetFieldValue`
- Message comparison logic

## Good Read / No Read Handling

Explanation of scan validation logic.

Topics:
- Successful scan behavior
- Failed scan behavior
- PLC status handling
- Scanner output mapping
- Scan retry considerations

Discuss:
- Good read status bit
- No read status bit
- Last-message comparison logic

## Scanner Communication Challenges

Discussion of integration and communication problems encountered during development.

Possible topics:
- Incorrect module sizing
- 32-byte truncation issue
- Input address remapping
- GSD configuration issues
- Device naming problems
- Ethernet communication troubleshooting

## Testing and Validation

Description of how the vision system was tested.

Topics:
- QR-code readability testing
- Lighting condition testing
- Reflective material testing
- Communication validation
- PLC integration testing
- SCADA data verification

## Final Outcome

Summary of completed vision-system functionality.

Describe:
- Reliable QR scanning
- Stable PLC communication
- Successful QR parsing
- SCADA integration
- Database logging support
- Reduced communication overhead
