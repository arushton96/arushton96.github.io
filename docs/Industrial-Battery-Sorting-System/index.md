# Automated Battery Sorting System

## Project Overview

This project was a PLC-controlled industrial automation system designed to identify, track, and sort battery cells using a combination of machine vision, pneumatic handling, industrial networking, and SCADA integration.

The system was developed as a senior engineering capstone project and focused on creating a modular automation platform capable of scanning QR-coded battery cells, processing identification data, and coordinating automated handling operations through a Siemens PLC and industrial control hardware.

The final system integrated a Banner vision sensor, Siemens S7-1200 PLC, pneumatic vacuum handling system, Ignition SCADA interface, SQL database logging, and Profinet-based industrial communication architecture.

## My Role

My primary responsibility on this project was the development and integration of the industrial control system architecture. This included PLC programming, Profinet device integration, pneumatic sequencing logic, SCADA communication, QR data processing, and system-level troubleshooting.

I designed and implemented the majority of the PLC control logic, including the automated pneumatic sequence, manual control architecture, safety/reset handling, communication mapping, and SCADA interface structure.

I also handled the integration of the Banner vision system over Profinet, developed the QR parsing logic used to process battery identification data, and created the Ignition SCADA interface used for system monitoring, control, and database logging.

## Key Contributions

- Developed PLC logic for automated pneumatic sequencing and state-machine control
- Integrated Banner vision hardware with Siemens PLC using Profinet communication
- Designed QR parsing and message-handling logic for battery identification
- Created Ignition SCADA interface for system control, monitoring, and diagnostics
- Implemented SQL database logging for scanned battery information
- Configured industrial Ethernet and static-IP network architecture
- Designed manual and automatic operating modes with safety interlocks and reset handling
- Troubleshot Profinet communication, string handling, and SCADA integration issues
- Reworked project scope and system architecture to adapt to hardware limitations and shipping delays

## Technologies Used

- Siemens S7-1200 PLC
- Ignition SCADA (Perspective)
- Profinet industrial communication
- Banner ABR3000 vision sensor
- SQL database integration
- OPC UA communication
- Industrial Ethernet networking
- Pneumatic vacuum handling systems
- SMC valve manifolds
- Structured Text (SCL) and Ladder Logic
- TIA Portal
- State-machine control architecture

## System Features

The completed system demonstrated:

- Automated pneumatic handling and transfer of battery cells
- QR-code scanning and identification using machine vision
- Real-time PLC control and sequencing
- SCADA-based operator control and monitoring
- SQL database logging of scanned battery information
- Manual and automatic operating modes
- Industrial network communication between PLC, vision hardware, and SCADA systems

## Engineering Challenges

A significant portion of the project involved solving real-world industrial integration challenges, including communication limitations, hardware delays, industrial network configuration, timing coordination, and system redesigns caused by unavailable hardware.

The project required adapting the original system architecture to maintain functionality despite multiple hardware and scheduling constraints, including the removal of the robotic handling subsystem from the final integrated scope.

These challenges resulted in a stronger emphasis on industrial controls integration, pneumatic automation, vision-system communication, and robust PLC/SCADA architecture design.

## Project Documentation

The pages in this section provide a technical breakdown of the system architecture, PLC logic, SCADA implementation, pneumatic control system, industrial networking configuration, vision-system integration, engineering decisions, and supporting technical documentation developed throughout the project.
