# uart_protocol.py

This module defines the UART message protocol for the MQTT subsystem. It provides functions for encoding and decoding 5-byte messages, including full validation of structure, sender/receiver IDs, and data integrity.

---

```python
from machine import UART
import time

START = 0x41
END = 0x42
SENDER = 0x04  # This device’s ID (adjust if needed)

def send_message(uart, receiver, data):
    # Accept int or hex string
    if isinstance(data, str):
        try:
            data = int(data, 0)
        except ValueError:
            raise ValueError("Data must be an integer or hex string")

    if not (0 <= data <= 255):
        raise ValueError("Data must be a single byte (0–255)")

    msg = bytes([START, SENDER, receiver, data, END])
    uart.write(msg)
    print("Sent:", [f"0x{b:02X}" for b in msg])


def read_message(uart):
    while uart.any():
        start = uart.read(1)
        if not start or start[0] != START:
            continue

        rest = b''
        timeout = 1000
        start_time = time.ticks_ms()

        while len(rest) < 4:
            if uart.any():
                rest += uart.read(1)
            elif time.ticks_diff(time.ticks_ms(), start_time) > timeout:
                print("Timed out waiting for full message.")
                return None, None, None, None, False

        msg = bytes([START]) + rest

        if msg[4] != END:
            print("Invalid packet structure:", [hex(b) for b in msg])
            return None, None, None, msg, False

        sender = msg[1]
        receiver = msg[2]
        data = msg[3]

        if sender not in (0x01, 0x02, 0x03, 0x04) or receiver not in (0x01, 0x02, 0x03, 0x04):
            print("Invalid sender or receiver value:", [hex(b) for b in msg])
            return None, None, None, msg, False

        if not (0 <= data <= 255):
            print("Invalid data value (not a byte):", [hex(b) for b in msg])
            return None, None, None, msg, False

        if receiver == SENDER:
            print("Message Received:")
            print(f"  Sender  : 0x{sender:02X}")
            print(f"  Receiver: 0x{receiver:02X}")
            print(f"  Data    : 0x{data:02X} ({chr(data)})")

        return sender, receiver, data, msg, True

    return None, None, None, None, False
