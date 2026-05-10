---
title: Vision System
---

# Vision System

## Vision System Overview

The vision subsystem was responsible for identifying and processing QR-coded battery cells during operation. A Banner ABR3000 industrial vision sensor was integrated with the Siemens S7-1200 PLC over Profinet to provide real-time scan results and battery identification data to the control system.

The scanner acted as the primary source of battery information within the system. Once a battery was positioned at the scanning station, the camera captured and decoded the QR label, transmitted the raw scan data to the PLC, and allowed the control logic to process the battery information for display, logging, and system tracking.

The vision system was designed around:

- Industrial Ethernet communication
- PLC-based QR parsing
- Real-time scan verification
- Structured battery data extraction
- SCADA/database integration

---

# Vision Hardware

## Banner ABR3000 Vision Sensor

The system utilized a Banner ABR3000 industrial barcode and vision sensor configured for Profinet communication.

### Scanner Responsibilities

The scanner was responsible for:

- Detecting QR-coded battery labels
- Capturing scan data
- Transmitting scan results to the PLC
- Indicating successful or failed reads
- Supporting repeated automated scans during operation

### Hardware Integration

The scanner was connected to the industrial Ethernet network alongside the Siemens PLC and pneumatic valve manifold.

System connections included:

- Ethernet / Profinet communication
- 24 VDC industrial power
- PLC-controlled trigger signals
- PLC status feedback handling

### Industrial Network Configuration

The camera was assigned a static IP address within the system network and configured as a Profinet IO device within TIA Portal.

Topics:

- Static IP configuration
- Device naming
- Profinet device assignment
- GSD installation
- IO module configuration

---

# Profinet Integration

## PLC Communication

The Banner scanner communicated directly with the Siemens PLC through Profinet IO communication rather than serial or TCP socket-based messaging.

This allowed the scanner to behave as a cyclic industrial IO device inside the PLC hardware configuration.

### Communication Architecture

The scanner exchanged:

- Trigger commands from the PLC
- Good read / no read status bits
- Raw scan message data
- Device status information

The PLC handled all higher-level message processing and parsing logic internally.

### GSD Configuration

A Banner Profinet GSD file was imported into TIA Portal in order to configure the scanner as an industrial IO device.

Challenges included:

- Correct module selection
- IO length configuration
- Address allocation
- Matching scanner-side communication settings

---

# Scanner Triggering Logic

## PLC-Controlled Acquisition

The scanner was configured for externally triggered acquisition. The PLC generated a trigger pulse once the pneumatic sequence positioned a battery at the scan station.

### Trigger Sequence

The scanning process followed this general sequence:

1. Pneumatic system positions the battery
2. PLC enables scan state
3. PLC issues trigger pulse
4. Scanner captures QR code
5. Scanner transmits decoded message
6. PLC verifies read result
7. Parsed data becomes available to SCADA/database systems

### Trigger Output

The scanner trigger was controlled using a dedicated PLC output bit mapped to the Profinet device output area.

Topics:

- One-shot trigger logic
- Scan timing coordination
- Synchronization with pneumatics
- Read verification timing

---

# QR Message Processing

## Raw Data Handling

The scanner transmitted raw QR message data into the PLC through the Profinet input buffer.

The PLC received:

- Status bytes
- Message control characters
- QR payload data
- Read status indicators

The incoming message was processed entirely within PLC logic using SCL string parsing functions.

### Message Buffer Structure

The message buffer contained:

- Header/status bytes
- Start-of-text characters
- QR payload content
- End-of-message delimiters

The parser monitored the input buffer and extracted usable text data from the incoming byte stream.

---

# QR Parsing Logic

## PLC-Based String Parsing

A custom SCL parsing routine was developed to process incoming QR messages directly within the PLC.

The parser extracted:

- Battery make
- Model information
- Serial number
- Date information
- Voltage
- Capacity
- Resistance data

### Parsing Architecture

Custom PLC functions were developed for:

- Field extraction
- Delimiter detection
- String assembly
- Message validation
- Data organization

Topics:

- SCL string handling
- Character-by-character parsing
- Delimiter-based extraction
- Structured data formatting
- Parse validation

### Scan Verification

The parser validated:

- Proper message structure
- Expected delimiters
- Valid scan formatting
- Successful read conditions

The system rejected invalid or incomplete messages.

---

# Buffer Size Optimization

## Profinet Message Limitations

One of the major engineering challenges during development involved Profinet message buffer sizing and scanner communication limits.

Initial scanner configurations produced QR payloads that exceeded the configured PLC input buffer size, resulting in truncated scan messages.

### Original Problem

Issues encountered included:

- Partial message reads
- Truncated QR payloads
- Inconsistent parsing
- Lost data fields
- PLC/scanner IO mismatches

### Solution

The final solution involved:

- Increasing Profinet input buffer allocation
- Correcting IO address mapping
- Reducing QR payload length
- Optimizing transmitted data formatting

The QR format was redesigned into a shorter structured format that preserved required information while reducing message size significantly.

### Final Result

The final communication implementation:

- Fit reliably within the PLC buffer
- Maintained scan consistency
- Improved parser reliability
- Reduced communication overhead

---

# Read Status Handling

## Good Read / No Read Logic

The scanner transmitted dedicated status bits indicating successful or failed scans.

These signals were integrated into PLC logic and SCADA monitoring systems.

### Status Indicators

The system monitored:

- Good read conditions
- No read conditions
- Trigger acknowledgement
- Scanner activity states

### PLC Integration

Read status bits were used for:

- Scan validation
- State transitions
- Error handling
- Retry logic
- Operator feedback

The SCADA system displayed scanner state information in real time during operation.

---

# SCADA Integration

## Vision Data in Ignition

Parsed battery information was transferred from the PLC into the Ignition SCADA system for display and logging.

### Displayed Information

The SCADA interface displayed:

- Battery serial number
- Voltage
- Capacity
- Resistance
- Scan status
- Current battery information

### Database Logging

Battery scan information was also logged into the SQL database system through Ignition.

Logged information included:

- Battery identification data
- Scan timestamps
- System status information
- Operator-visible scan results

---

# Vision System Challenges

## Engineering Challenges

Several technical challenges were encountered during development of the vision subsystem.

### Major Challenges

Key development issues included:

- Profinet module configuration mismatches
- Scanner IO sizing limitations
- PLC string handling limitations
- Truncated scan data
- Trigger synchronization
- Message parsing reliability
- Industrial Ethernet configuration

### Troubleshooting Process

Development required:

- Packet sizing verification
- Address remapping
- Scanner-side configuration changes
- PLC parser redesign
- IO module testing
- Real-time scan validation

The majority of troubleshooting focused on ensuring reliable communication between the scanner and PLC while maintaining deterministic industrial control behavior.

---

# Final Outcome

## Completed Vision System

The final vision subsystem successfully provided:

- Reliable QR scanning
- Real-time Profinet communication
- PLC-based message parsing
- Structured battery data extraction
- SCADA integration
- Database logging support

The completed implementation demonstrated reliable industrial machine vision integration using PLC-controlled industrial Ethernet communication and custom structured-text parsing logic.
