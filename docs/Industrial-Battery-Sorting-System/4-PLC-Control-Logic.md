---
title: PLC Control Logic
---

# PLC Control Logic

## PLC Logic Overview

High-level explanation of the PLC’s role as the main controller for the automated system.

Describe:
- Sequence control
- Pneumatic output control
- Vision system triggering
- SCADA command handling
- Status reporting
- Safety and reset behavior

## PLC Program Structure

Overview of the major PLC blocks and how the program was organized.

Topics:
- Main cyclic logic
- Function blocks
- Data blocks
- SCADA interface mapping
- QR parsing blocks
- Pneumatic sequence logic

Possible blocks:
- `OB1`
- `DB_SCADA_Interface`
- `FB_PneuSequence`
- `FB_QR_Parse`
- `FC_SCADA_Interface_Mapping`
- `FC_GetFieldValue`

## State Machine Architecture

Explanation of the automated sequence structure.

Topics:
- Step-based control
- State transitions
- Timers
- Interlocks
- Reset behavior
- Cycle completion

Insert state machine diagram or screenshot.

![PLC State Machine](Images/PLCStateMachine.png)

## Automatic Mode

Description of the automatic pneumatic cycle.

Topics:
- Start cycle command
- Robot vacuum simulation
- Flipper vacuum transfer
- Camera wait state
- Battery handoff sequence
- Cycle complete condition

Example sequence:
1. Start command received
2. Robot vacuum picks battery
3. Battery transfers to flipper vacuum
4. Flipper extends
5. Camera scan occurs
6. Battery transfers back
7. Robot vacuum releases battery
8. Cycle completes

## Manual Mode

Description of manual control logic used for testing and troubleshooting.

Topics:
- Direct valve controls
- Manual SCADA commands
- Auto/manual separation
- Output lockouts
- Safe manual operation

## Mode Management

Explanation of how auto/manual operation was controlled.

Topics:
- Mode selection
- Mode change permissives
- Auto mode allowed conditions
- Manual mode allowed conditions
- Preventing mode changes during active cycles

## Safety and Interlocks

Overview of logic used to prevent unsafe or conflicting outputs.

Topics:
- Stop behavior
- Reset behavior
- Valve exclusivity
- Extend/retract logic
- Vacuum handoff overlap
- Sensor bypass handling
- Safe idle conditions

## Reset Logic

Detailed explanation of how reset behavior was designed.

Topics:
- Returning system to idle
- Preserving battery control during reset
- Reversing sequence safely
- Reset active status
- Manual mode reset behavior

## Vision System Trigger Logic

Explanation of PLC logic used to trigger and monitor the scanner.

Topics:
- Camera trigger command
- Good read / no read bits
- Scan wait state
- Message comparison
- New scan detection

## QR Parsing Logic

Overview of how QR data was handled inside the PLC.

Topics:
- Profinet input buffer
- Message parsing
- Field extraction
- Data validation
- Parsed values sent to SCADA

Detailed parsing can be moved to the Vision System page if needed.

## SCADA Interface Mapping

Explanation of how PLC data was exposed to Ignition.

Topics:
- Command tags
- Status tags
- Value tags
- Read/write separation
- Naming conventions

Example tag groups:
- `usr_` for SCADA/user commands
- `plc_` for internal logic
- `sts_` for SCADA status outputs
- `QR_` for parsed QR data

## Timing and Handoff Logic

Description of timers and coordinated pneumatic handoffs.

Topics:
- Handoff delay
- Camera wait timer
- Vacuum overlap timing
- Step transition timing
- Avoiding dropped battery states

## PLC Debugging and Testing

Description of how the PLC logic was tested.

Topics:
- Watch tables
- Manual command testing
- SCADA test page
- Output verification
- Step-by-step sequence validation

## PLC Logic Challenges

Discussion of major PLC development challenges.

Possible topics:
- State machine restructuring
- Reset logic complexity
- Auto/manual interaction
- Valve output conflicts
- QR parser debugging
- SCADA tag mapping
- Profinet input buffer sizing

## Final Outcome

Summary of completed PLC functionality.

Describe:
- Stable automatic sequence
- Functional manual mode
- SCADA-controlled operation
- Scanner trigger handling
- QR data processing
- Safe reset and stop behavior

```markdown
![PLC State Machine](Images/PLCStateMachine.png)
