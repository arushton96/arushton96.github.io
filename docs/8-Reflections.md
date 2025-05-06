---
title: Reflections
---

# Reflections

---

## Lessons Learned

1. **Strong protocols make systems resilient.**
  I learned that having strict filtering and validation rules for UART messages was essential to ensure system stability, especially when other subsystems lacked comparable message checks.

2. **Planning for failure modes early is critical.**
  Building in MQTT fallback functionality for UART failure helped ensure that the system could continue functioning even if direct communication was disrupted.

3. **Documentation should evolve with the project.**
  Early documentation often becomes outdated. Maintaining and updating documentation as the system design changes is essential for clarity and accountability.

4. **Board layout choices have long-term consequences.**
  Design decisions like breakout header placement, silkscreen clarity, and power routing had lasting effects on usability and debugging.

5. **UART communication is more fragile than expected.**
  We encountered numerous issues with missed bytes and synchronization errors, which highlighted the importance of buffering, validation, and consistency.

6. **Test everything in the system, not just subsystems.**
  Testing a working module in isolation is not enough. System-wide integration often reveals timing, communication, and logic issues that weren’t apparent during local testing.

7. **Simpler is often better.**
  Some proposed improvements would have added complexity without meaningful gains. Balancing robustness with simplicity was a recurring theme in decision-making.

8. **Teammate communication can define project success.**
  Technical execution is only part of the challenge. Miscommunication or lack of contribution from team members can result in last-minute stress and duplicated effort.

9. **Debugging tools (like debug LEDs) are lifesavers.**
  Being able to visually confirm where the system was in its boot process or which branches of logic were executing saved significant debugging time.

10. **Embedded systems demand flexibility.**
  I learned to quickly adapt code, protocols, and hardware design to match real-world constraints and teammate limitations without compromising core functionality.

<br>

## Recommendations for Future Students

1. **Use debug LEDs — more than you think you need.**
  Seeing system state visually is invaluable when things go wrong, especially without access to serial output.

2. **Choose UART pins and peripherals very carefully.**
  Not all microcontroller pins are created equal. Cross-reference datasheets before committing to a pinout in your schematic.

3. **Start integration tests early — not just subsystem tests.**
  Subsystems can behave very differently when connected together, especially if timing or shared resources are involved.

4. **Make your code readable, modular, and well-commented.**
  You may need to hand it off to a teammate, or debug something from two weeks ago. Clear code saves everyone time.

5. **Include breakout headers and plan for flexibility.**
  Designing your board with access to spare IO pins, power, and UART connections allows you to run different code, reassign subsystems, or even use your board to rescue someone else’s in a pinch. It’s one of the best safety nets you can build in.

## Version 2.0: Software Improvements

If I were to create a “Version 2.0” of this system’s communication architecture, I would implement a few key improvements.

First, I would transition from the simple 5-byte UART protocol to a more flexible packet-based system using length fields, message IDs, and checksums. This would enable the system to handle variable-length messages and confirm integrity, which is especially useful for future expansion beyond simple stop/go commands.

Second, I would add message queuing and acknowledgment for reliability. Right now, messages are sent and forgotten — if a packet is missed, it’s gone. A retry mechanism with a basic ACK/NACK structure would make the system more robust.

Third, I’d implement heartbeat messages across all systems, not just the MQTT fallback. These periodic “I’m alive” packets would help detect crashed or disconnected subsystems and allow the remaining units to take corrective action.

Fourth, I would modularize the communication logic further, abstracting UART and MQTT handling into shared utility modules that could be reused across subsystems. This would reduce redundant code and make updates easier to propagate across devices.

Finally, I’d enhance logging and state visualization — for example, integrating serial debug output with a timestamp and adding onboard OLED/LED indicators for every major state transition. Combined with the MQTT graph viewer, this would give a comprehensive picture of the system’s runtime behavior.

Overall, Version 2.0 would focus on reliability, scalability, and transparency, while maintaining the simplicity that allowed this version to succeed under real-world testing constraints.
