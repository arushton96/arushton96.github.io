---
title: API Documentation
---

# Process and Solved Problems
The MQTT subsystem served as the second node in the UART message chain and took on the primary responsibility for message validation and filtering due to its robust communication protocol. Over the course of development, our team determined that only one type of message—live sensor values—needed to be propagated through the system. While sender and receiver IDs were initially planned as key routing elements, they ultimately became redundant for most subsystems. However, the MQTT system retained full protocol enforcement, which required it to remain between the sensor and motor subsystems to ensure only valid messages were processed. To support this, I adapted the receiver ID field to track whether a message had already circulated through the full UART chain. Upon receiving a valid message, the MQTT system rewrote the receiver ID to its own and filtered out any returning messages marked this way, effectively preventing duplication without altering the message structure.

A separate challenge arose from the high frequency of sensor messages—often 5 to 10 per second—which could overwhelm the system and prevent the motors from restarting once a stop condition was triggered. To manage this, I implemented a timed filter that suppressed duplicate sensor messages during a defined stop interval. After this delay, the MQTT subsystem automatically resumed motor activity and introduced a secondary lockout period to prevent premature retriggering. This approach ensured reliable operation without requiring upstream changes to the sensor logic.

<p>&nbsp;</p>

# Message Overview

|               |   Message 1  |
| ------------- | ------------ |
| Variable Name | Sensor_Value |
| Variable Type |    int8_t    |
|   Min Value   |      100     |
|   Max Value   |      101     |
|    Example    |  0x64 & 0x65 |

As written above, the only value used in the system is the live sensor value.

The Sender and Receiver bytes are 0x01, 0x03, 0x04, and 0x05. The start byte is 0x41 and the end byte is 0x42. Our "data" package of the message is only 1 byte, either 0x64 or 0x65, which represent the only two colors that the sensors were coded to recognize.

Team IDs: Sensor = 0x01 | HMI = 0x03 | Motor = 0x05 | MQTT = 0x04

<p>&nbsp;</p>

# Message Format

This section demonstrates how the MQTT subsystem receives messages differently the first time versus the second time. When a sensor message is received for the first time, it arrives with the original receiver ID (0x05). The MQTT subsystem then rewrites the receiver ID to 0x04 before forwarding it. This change affects how all other subsystems receive the message as well—each sees the modified version with the updated receiver ID. If the message loops back to the MQTT subsystem, the new receiver ID indicates it has already been processed once, allowing it to be identified and discarded as a duplicate.

## First-Time Message Format (Before ID Change)

|               |  Byte 1 |  Byte 2 |  Byte 3  |    Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------ | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Sensor_Value |   End   |
|   Byte Type   |  int8_t |  int8_t |   int8_t |     int8_t   |  int8_t |
| Byte Contents |   0x41  |   0x01  |   0x05   |  Sensor Byte |   0x42  |

## Second-Time Message Format (After ID Change)

|               |  Byte 1 |  Byte 2 |  Byte 3  |    Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------ | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Sensor_Value |   End   |
|   Byte Type   |  int8_t |  int8_t |   int8_t |     int8_t   |  int8_t |
| Byte Contents |   0x41  |   0x01  |   0x04   |  Sensor Byte |   0x42  |
