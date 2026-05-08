# MQTT Subsystem Code

This section contains all code related to the MQTT subsystem, including both embedded logic running on the ESP32 and a separate Python script used for real-time visualization on a local computer.

---

## Files

- [main.py](main.md): Core firmware logic for UART filtering, MQTT publishing, and system control.
- [uart_protocol.py](uart-protocol.md): Utility functions to encode and decode 5-byte UART messages.
- [mqtt_graph_viewer.py](graph-viewer.md): Python script for PC-based graphing of MQTT topic data.
