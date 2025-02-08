---
title: Component Selection
---

| MQTT WiFi Server |

| Hardware Selection |
| ------------------ |
| This subsystem has no major hardware components for this section. <br> All components will either be small passive components (resistors, capacitors, etc) or the microcontroller |

On my team, I am responsible for creating the MQTT server, as well as creating the power supply that will feed to the other three subsystems. The main goal of my subsystem will be to connect the project to a computer that will serve not only as the platform for the HMI system and controller but also display diagnostic data from the rest of the system. This data could include battery life, speed, current sensor calculation speed, or current data transfer speed. The exact diagnostics that will be displayed are still undecided by the group as of now, though. I will have UART connections to all three other subsystems, however I will only be sending and receiving data from the HMI subsystem; the other two will only be sending me data.

The determining factors for my microcontroller selection were speed and WiFi capability. The main requirements of the microcontroller for my subsystem are UART communication and WiFi capability. Any other required pins, such as GPIO pins, will be more plentifully available than I need on any board that I choose, so those were not considered in the decision. At a minimum, the subsystem requires 1 UART connection, but more would be easier to use. Both the ESP32 and PIC18 have at least one UART connection, so they both meet my requirements. The PIC does not have internal WiFi capabilities. While I can use external peripherals to supliment that, the ESP32 has the WiFi modules already built into it. The last consideration was speed. The PIC18 has an average clock speed of 40MHz, while the ESP32 has a speed of 240MHz and supports multithreading. This will be helpful when connecting to the MQTT server and handeling data from the other subsystems in real-time.
