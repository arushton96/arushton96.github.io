---
title: Component Selection
---
## My Subsystem

I was responsible for developing the MQTT subsystem, which provided fault-tolerant communication reliability and system monitoring. The ESP32-S3-WROOM module served as the backbone for this design due to its native Wi-Fi capabilities, flexible UART support, and sufficient GPIO for expansion and debugging features.

This subsystem was responsible for relaying messages between other boards, detecting UART failure, and forwarding valid data via MQTT. It also coordinated a system-wide response to sensor input, managed a timed lockout to prevent false stop signals, and published MQTT data that could be visualized using a Python-based local graph viewer. Debug LEDs were incorporated for status indication during startup, message processing, and system activity.


<br>

## Major Hardware Selection

This subsystem relied primarily on the ESP32-S3-WROOM, which integrated all the features required for MQTT communication and UART relaying. No external microcontroller or Wi-Fi module was necessary due to its built-in capabilities. The only other significant component was a switching voltage regulator (buck converter) used to step down 5V USB power to the 3.3V required by the ESP32.

While passive components like resistors and capacitors were also included, they were part of standard best practices rather than explicit design decisions. Because the ESP32 handled all logic, message processing, and communication tasks, no additional sensors or specialized ICs were required.


<br>

## Design Decisions and Justification

The MQTT board’s design revolved around ensuring reliable message routing, fallback communication, and monitoring. The ESP32-S3-WROOM was selected because it could handle all UART communication and MQTT publishing without external hardware. This selection enabled robust integration and flexibility while minimizing the component count.

A previously validated buck converter (MP1584) was reused to provide regulated 3.3V power. Although not extensively researched during this project, the module was well-known from earlier coursework and was verified to meet voltage and current demands. Other hardware choices, such as debug LEDs, were added to support troubleshooting and state visibility throughout the development process.

Originally, this subsystem was also meant to handle the power supply and HMI coordination, but its role was later focused specifically on system communication. Final implementation included UART parsing and filtering, MQTT publishing of valid messages, automatic fallback when UART became unresponsive, and visualization of system activity using a local Python graphing tool. These decisions resulted in a stable and modular foundation for the entire system’s message flow.


<br>

# Microcontroller Selection

| ESP Info                                      | Answer                                                                                                                               |
| --------------------------------------------- | ------------------------------------------------------------------------------------------------------------------------------------ |
| Model                                         | ESP32-S3-WROOM-1                                                                                                                     |
| Product Page URL                              | [Product Page](https://www.espressif.com/en/products/modules/page#ESP32-S3)                                                          |
| ESP32-S3-WROOM-1-N4 Datasheet URL             | [Data Sheet](https://www.espressif.com/sites/default/files/documentation/esp32-s3-wroom-1_wroom-1u_datasheet_en.pdf)                 |
| ESP32 S3 Datasheet URL                        | [ESP32-S3 Series Datasheet](https://www.espressif.com/sites/default/files/documentation/esp32-s3_datasheet_en.pdf)                   |
| ESP32 S3 Technical Reference Manual URL       | [Technical Reference Manual](https://www.espressif.com/sites/default/files/documentation/esp32-s3_technical_reference_manual_en.pdf) |
| Vendor link                                   | [Digikey Link](https://www.digikey.com/en/products/detail/espressif-systems/ESP32-S3-WROOM-1-N4/16162639)                            |
| Code Examples                                 | [PubSubLibrary](https://github.com/knolleary/pubsubclient) <br> [WiFi Library](https://github.com/arduino-libraries/WiFi)            |
| External Resources URL(s)                     | [ESP32 MicroPython: MQTT](https://youtu.be/ugEnE7XSR5I?si=Fv3zHxQ3zeP0jUnl) <br> [How to setup MQTT for Raspberry Pi and ESP32](https://youtu.be/ebsXSCKsHeQ?si=SOfk5tIESVuPY_7s)                                                                                                                                                                   |
| Unit cost                                     | $2.95                                                                                                                                |
| Absolute Maximum Current for entire IC        | 1500 mA                                                                                                                              |
| Supply Voltage Range                          | Min: 3.0V <br> Typical: 3.3V <br> Max: 3.6V                                                                                          |
| Maximum GPIO current <br> (per pin)           | Not explicitly stated in datasheet, however in general around 40mA max and 20mA typical.                                             |
| Supports External Interrupts?                 | Supports external interrupts which can also be used to wake from light sleep mode                                                    |
| Required Programming Hardware, Cost, URL      | [USD to Serial converter 5-10$](https://www.amazon.com/IZOKEE-CP2102-Converter-Adapter-Downloader/dp/B07D6LLX19/ref=sr_1_3?adgrpid=1330409641990384&dib=eyJ2IjoiMSJ9.qroPT-fyHbCHJ3tcPCCTQfWRI8aVGF1Xa7ZxFaJF9LZMTgBqYg3YnMxqbubd7viDdw_T94MoKF_7UtWKuCCOebeoGVe5et2rTnfrh9iC_hn_snBwX5FbfEboSq0eX1q9MR1r8YCT-GcYlrxQgXiivg2L_gIOq_3L4baNmX-jjSPmZemAlGkHT9GRgYIJJ9vUZtfyOIzaqS0kOh4-z1Vm7fHMl2-8sjURz31spK3cVGs.EzKKHT1QePqvJUjzSOqcp0mnXalQBORKEvpsg7AKnl8&dib_tag=se&hvadid=83150817082162&hvbmt=be&hvdev=c&hvlocphy=77892&hvnetw=o&hvqmt=e&hvtargid=kwd-83150962142855%3Aloc-190&hydadcr=19132_13351602&keywords=usb+to+uart+converter&mcid=c9ec1fe3b12d3f0e82d616b6cc8bb95d&qid=1738966734&sr=8-3)                                            |
<br>

| Module         | # Available | Needed |                        Associated Pins (or * for any)                        |
| -------------- | ----------- | ------ | ---------------------------------------------------------------------------- |
| UART           | 2           | 2      | Pins 10 & 37 (TX) <br> Pins 11 & 36 (RX)                                     |
| external SPI\* | 1           | 0      | SPI 0 & 1 (Reserved) <br> SPI 2 (Pins 18-21) <br> SPI 3 (Pins 27-30)         |
| I2C            | 2           | 0      | *                                                                            |
| GPIO           | 36          | 0      | Any except 1, 2, 3, 40 & 41                                                  |
| ADC            | 20          | 0      | ADC1 (Pins 4-7, 12, 15, 17, 18, 38, 39) <br> ADC2 (Pins 8-11, 13, 14, 19-22) |
| LED PWM        | 36          | 4      | *                                                                            |
| Motor PWM      | 36          | 0      | *                                                                            |
| USB Programmer | 1           | 1      | Pins (13 & 14 +VCC/Ground)                                                   |

<p>&nbsp;</p>

## Final Major Components Summary

| Component              | Part Number         | Function                           |
|------------------------|---------------------|------------------------------------|
| ESP32-S3-WROOM         | ESP32-S3-WROOM-1-N8R8 | Main microcontroller for MQTT, UART, Wi-Fi |
| Buck Converter Module  | LM2596 Module       | 5V voltage regulation for board power |

<p>&nbsp;</p>

### Power Budget

| **Component Name** | **Part Number**          | **Supply Voltage Range** | **# Used** | **Absolute Max Current (mA)** | **Total Current (mA)** |
|--------------------|--------------------------|---------------------------|------------|-------------------------------|-------------------------|
| Buck Converter     | LM2575-3.3               | 7.0V – 40.0V              | 1          | **1000**                      | 1000                    |
| ESP32-S3-WROOM     | ESP32-S3-WROOM-1-N4R2    | 3.0V – 3.6V               | 1          | **500**                       | 500                     |

This power budget outlines the expected current draw and supply capability of the major components in the system. The LM2575-3.3 voltage regulator supplies up to 1000 mA at 3.3V, which is sufficient to power the ESP32-S3-WROOM module, which has a maximum current draw of approximately 500 mA. This margin ensures stable operation even under peak load conditions. Estimating power requirements like this was essential for confirming regulator selection and ensuring reliability during testing.

<p>&nbsp;</p>

# ESP32 Diagram and Wiring

![Block Diagram](Images/Diagram.png)

![Block Diagram](Images/Wiring.png)
