---
title: API Documentation
---

# Introduction
The MQTT subsystem is at the end of the UART daisy chain, so my main process will be to pass on the messages I receive back to the beginning of the chain. I will be using the data I receive on the WiFi server, but the MQTT communication will be outlined on a different page than this one. This page will only focus on the UART communications.
<br>
<br>
# Message Structure
## Messages Overview

|               |    Byte 1    |     Byte 2    |     Byte 3    |
| ------------- | ------------ | ------------- | ------------- |
| Variable Name | Sensor_Value | Motor_1_Value | Motor_2_Value |
| Variable Type |    uint8_t   |    uint8_t    |    uint8_t    |
|   Min Value   |       0      |       0       |       0       |
|   Max Value   |      255     |      255      |      255      |
|    Example    |      52      |       77      |      153      |

I will be receiving three different data values from upstream: The sensor values and the values of motor 1 and motor 2. The values will be uploaded to the WiFi server and then passed along the chain to the HMI subsystem. That means that all of these messages will be both received and transmitted messages.
<br>
<br>
## Sensor Value
