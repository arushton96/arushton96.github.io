---
title: MQTT WiFi Server
---

| Hardware Selection |
| ------------------ |
| This subsystem has no major hardware components for this section. <br> All components will either be small passive components (resistors, capacitors, etc) or the microcontroller) |

On my team, I am responsible for creating the MQTT server, as well as creating the power supply that will feed to the other three subsystems. The main goal of my subsystem will be to connect the project to a computer that will serve not only as the platform for the HMI system and controller but also display diagnostic data from the rest of the system. This data could include battery life, speed, current sensor calculation speed, or current data transfer speed. The exact diagnostics that will be displayed are still undecided by the group as of now, though. I will have UART connections to all three other subsystems, however I will only be sending and receiving data from the HMI subsystem; the other two will only be sending me data.
