# Code Overview

This section documents the code I developed for the MQTT subsystem, along with support code I wrote or modified for the Sensor and HMI subsystems to enable UART-to-MQTT fallback functionality.

---

## MQTT Subsystem

This subsystem was responsible for UART message filtering, MQTT communication, and system visualization. It includes three main files:

- **main.py** – Core loop for UART parsing, MQTT publishing, and state control
- **UART_Protocol.py** – Functions to encode and decode 5-byte UART messages
- **mqtt_graph_viewer.py** – Python script for PC-based graphing of MQTT topic data.

[📄 View MQTT Subsystem Code](mqtt-code/index.md)

---

## Sensor Subsystem (UART Backup)

The following code reflects the final integrated version used for the Sensor subsystem. While the original structure was written by a teammate, I incorporated all MQTT-related functionality and UART fallback logic shown here.

This version was tested and ready for final demonstration, and accurately represents my contribution to the subsystem. All MQTT-related sections are clearly marked in the code below. The remaining core sensor logic and I2C implementation were provided by the original developer.

[📄 View Sensor Code](sensor-code.md)

---

## HMI Subsystem (MQTT Visualization)

To support UART backup functionality and MQTT-based system visualization, I modified the HMI subsystem code to publish messages over MQTT for my subsystems use. The base structure was written by a teammate; I incorporated MQTT logic and display rendering to support testing and integration.

The code shown here does not exactly match the final version submitted by the teammate responsible for the HMI subsystem. I rewrote portions of the code for my at-home testing environment and have highlighted the sections that reflect my contributions. These highlighted areas represent the full scope of my work on this subsystem and were functionally validated during integration.

[📄 View HMI Code](hmi-code.md)
