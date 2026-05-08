---
title: Challenges & Engineering Decisions
---

# Challenges & Engineering Decisions

## Overview

This project involved significant system integration challenges across industrial networking, PLC programming, machine vision, SCADA development, pneumatic sequencing, and hardware coordination.

Many of the final engineering decisions were shaped by real-world constraints including hardware delays, communication limitations, software integration issues, and evolving project scope requirements.

## Project Scope Evolution

### Original System Design

Overview of the original project concept and intended functionality.

Topics:
- FANUC robot integration
- PLC-to-robot communication
- Automated battery transfer
- Vision-based identification
- Full integrated automation workflow

Insert original system concept diagram if available.

![Original System Concept](Images/OriginalConcept.png)

## Hardware Delays and Scope Reduction

### Robot Integration Challenges

Discussion of how hardware availability impacted the project.

Topics:
- Delayed robot communication hardware
- Profinet upgrade limitations
- Inability to complete planned robot integration
- Schedule impact
- Redesign of demonstration scope

Discuss:
- Transition to pneumatic-only automated sequence
- Preserving core automation functionality
- Maintaining system integration goals despite reduced scope

## Industrial Networking Challenges

### Profinet Integration

Discussion of networking and industrial communication issues encountered during development.

Topics:
- Device configuration issues
- GSD installation and setup
- Input/output module sizing
- Device naming and IP assignment
- PLC communication troubleshooting

Discuss:
- Accessible devices configuration
- Communication validation
- Ethernet troubleshooting workflow

## QR Communication Limitations

### Profinet Buffer Constraints

Discussion of QR message-size limitations and communication redesign.

Topics:
- Initial message truncation
- 32-byte vs 128-byte buffer issues
- Scanner module configuration
- Message restructuring

Discuss:
- Reduction of QR payload size
- Communication reliability improvements
- Tradeoffs between data size and traceability information

## PLC Logic Challenges

### State Machine Development

Discussion of difficulties encountered while developing the automated sequence logic.

Topics:
- Sequence restructuring
- Reset handling
- Auto/manual interaction
- Timing coordination
- Interlock management
- Vacuum handoff logic

Discuss:
- Evolution of the sequence architecture
- Debugging process
- Safety considerations

## Pneumatic Coordination Challenges

### Vacuum Transfer Timing

Discussion of challenges related to pneumatic handling.

Topics:
- Battery transfer stability
- Vacuum overlap timing
- Extend/retract synchronization
- Scan-position consistency
- Preventing dropped batteries

Discuss:
- Handoff timing adjustments
- Sequence tuning
- Transfer reliability improvements

## SCADA Integration Challenges

### Ignition Communication & Tag Management

Discussion of SCADA-related integration problems.

Topics:
- OPC UA configuration
- Tag import limitations
- PLC string handling
- Database integration
- Perspective scripting issues

Discuss:
- String-size limitations
- Manual tag mapping
- Gateway troubleshooting
- Interface redesign decisions

## Engineering Tradeoffs

### Design Decisions and Prioritization

Discussion of major engineering decisions made throughout the project.

Possible topics:
- Centralized PLC architecture
- Pneumatic-only fallback operation
- Simplified demonstration scope
- QR payload optimization
- Manual vs automatic operation separation
- Safety-focused reset behavior
- SCADA architecture organization

## System Reliability Improvements

### Iterative Refinement Process

Discussion of how the system was improved throughout development.

Topics:
- Logic refinements
- Sequence stabilization
- Communication improvements
- UI redesigns
- Diagnostic tooling
- Testing workflow

Discuss:
- Incremental debugging process
- Integration testing
- Lessons learned from failed implementations

## Real-World Engineering Constraints

### Adapting to Project Limitations

Discussion of how external constraints shaped the final project.

Topics:
- Shipping delays
- Licensing delays
- Hardware availability
- Schedule compression
- Scope management
- Team coordination

Discuss:
- Maintaining project viability
- Reprioritizing objectives
- Preserving technical value despite constraints

## Lessons Learned

Reflection on technical and engineering lessons gained from the project.

Possible topics:
- Industrial networking experience
- PLC system design
- SCADA integration
- Systems-level troubleshooting
- Communication architecture
- Hardware/software coordination
- Managing evolving requirements

## Final Outcome

Summary of the engineering results achieved despite project constraints.

Describe:
- Successful subsystem integration
- Functional automated sequence
- Reliable vision communication
- Operational SCADA system
- Database logging implementation
- Stable industrial control architecture
- Adaptation to real-world engineering challenges
