---
title: API Documentation
---

# Introduction
The MQTT subsystem is at the end of the UART daisy chain, so my main process will be to pass on the messages I receive back to the beginning of the chain. I will be using the data I receive on the WiFi server, but the MQTT communication will be outlined on a different page than this one. This page will only focus on the UART communications.
<br>
<br>
# Message Overview

|               |   Message 1  |   Message 2   |   Message 3   |
| ------------- | ------------ | ------------- | ------------- |
| Variable Name | Sensor_Value | Motor_1_Value | Motor_2_Value |
| Variable Type |    uint8_t   |    uint8_t    |    uint8_t    |
|   Min Value   |       0      |       0       |       0       |
|   Max Value   |      255     |      255      |      255      |
|    Example    |      52      |       77      |      153      |

I will be receiving three different data values from upstream: The sensor values and the values of motor 1 and motor 2. The values will be uploaded to the WiFi server and then passed along the chain to the HMI subsystem. That means that all of these messages will be both received and transmitted messages.

The Sender and Receiver bytes will be between 0x01 and 0x04. The start byte is 0x41 and the end byte is 0x42. Currently, our "data" package of each message will only be 1 byte between 0 and 255, written in hex.

Team IDs: Sensor = 0x01 HMI = 0x02 Motor = 0x03 MQTT = 0x04


<br>
<br>
# Sensor Value
## Receive Format

|               |  Byte 1 |  Byte 2 |  Byte 3  |    Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------ | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Sensor_Value |   End   |
|   Byte Type   | uint8_t | uint8_t |  uint8_t |    uint8_t   | uint8_t |
| Byte Contents |   0x41  |   0x03  |   0x05   |  Sensor Byte |   0x42  |

## Transmit Format

|               |  Byte 1 |  Byte 2 |  Byte 3  |    Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------ | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Sensor_Value |   End   |
|   Byte Type   | uint8_t | uint8_t |  uint8_t |    uint8_t   | uint8_t |
| Byte Contents |   0x41  |   0x05  |   0x04   |  Sensor Byte |   0x42  |

# Motor 1
## Receive Format

|               |  Byte 1 |  Byte 2 |  Byte 3  |     Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------- | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Motor_1_Value |   End   |
|   Byte Type   | uint8_t | uint8_t |  uint8_t |     uint8_t   | uint8_t |
| Byte Contents |   0x41  |   0x03  |   0x05   |  Motor 1 Byte |   0x42  |

## Transmit Format

|               |  Byte 1 |  Byte 2 |  Byte 3  |     Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------- | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Motor_1_Value |   End   |
|   Byte Type   | uint8_t | uint8_t |  uint8_t |     uint8_t   | uint8_t |
| Byte Contents |   0x41  |   0x05  |   0x04   |  Motor 1 Byte |   0x42  |

# Motor 2
## Receive Format

|               |  Byte 1 |  Byte 2 |  Byte 3  |     Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------- | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Motor_2_Value |   End   |
|   Byte Type   | uint8_t | uint8_t |  uint8_t |     uint8_t   | uint8_t |
| Byte Contents |   0x41  |   0x03  |   0x05   |  Motor 2 Byte |   0x42  |

## Transmit Format

|               |  Byte 1 |  Byte 2 |  Byte 3  |     Byte 4    |  Byte 5 |
| ------------- | ------- | ------- | -------- | ------------- | ------- |
|   Byte Name   |   Start |  Sender | Receiver | Motor_2_Value |   End   |
|   Byte Type   | uint8_t | uint8_t |  uint8_t |     uint8_t   | uint8_t |
| Byte Contents |   0x41  |   0x05  |   0x04   |  Motor 2 Byte |   0x42  |
