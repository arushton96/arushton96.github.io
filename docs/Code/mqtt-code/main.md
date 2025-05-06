# main.py — MQTT Subsystem Firmware

This file contains the full firmware for the MQTT subsystem. It handles UART communication, message filtering, and MQTT publishing, as well as timed control logic for forwarding and managing stop/start signals to the motor system.

---

```python
import ssl
from mqtt_as.mqtt_as import MQTTClient
from mqtt_as.mqtt_local import wifi_led, blue_led, config
import uasyncio as asyncio
from machine import UART, Pin
import time
from config import *

START_BYTE = 0x41
END_BYTE = 0x42
PACKET_SIZE = 5

# Debug LEDs
led_start  = Pin(39, Pin.OUT)
led_wifi   = Pin(40, Pin.OUT)
led_mqtt   = Pin(41, Pin.OUT)
led_alive  = Pin(42, Pin.OUT)

async def flash_led(led, duration=0.2):
    led.on()
    await asyncio.sleep(duration)
    led.off()

config['ssid'] = 'photon'
config['wifi_pw'] = 'particle'
config['server'] = 'mqtt.eclipseprojects.io'
config['port'] = 1883
config['ssl'] = False
config['clean'] = True
config['subs_cb'] = lambda t, m, r: print("MSG:", m.decode())
MQTTClient.DEBUG = True
client = MQTTClient(config)

uart = UART(1, baudrate=9600, tx=43, rx=44)

while uart.any():
    uart.read()

async def uart_listener():
    buffer = bytearray()
    reader = asyncio.StreamReader(uart)

    lockout = False
    grace_period = False
    repeat_counter = 0

    asyncio.create_task(alive_flasher())

    while True:
        byte = await reader.read(1)
        await asyncio.sleep(0.01)

        if byte:
            buffer.append(byte[0])
            print(f"Received byte: 0x{byte[0]:02X}")

            if len(buffer) == PACKET_SIZE:
                start, sender, receiver, data, end = buffer
                print("Full UART message:", [f"0x{b:02X}" for b in buffer])

                if start == START_BYTE and end == END_BYTE and sender in (0x01, 0x03, 0x04, 0x05) and receiver in (0x01, 0x03, 0x04, 0x05):

                    # Handle STOP triggers (0x64 or 0x65)
                    if data in (0x64, 0x65):
                        if not lockout and not grace_period:
                            print("STOP received — initiating lockout")
                            lockout = True
                            repeat_counter = 0

                            # Send STOP
                            stop_msg = bytes([START_BYTE, 0x04, 0x05, 0x00, END_BYTE])
                            uart.write(stop_msg)
                            await client.publish("car/state", b"0", qos=1)

                            await asyncio.sleep(5)

                            # Send GO
                            go_msg = bytes([START_BYTE, 0x04, 0x05, 0x01, END_BYTE])
                            uart.write(go_msg)
                            await client.publish("car/state", b"1", qos=1)

                            # Grace period begins
                            grace_period = True
                            lockout = False
                            await asyncio.sleep(2)
                            grace_period = False

                            while uart.any():
                                uart.read()

                        else:
                            repeat_counter += 1
                            print(f"Ignored repeat stop (count = {repeat_counter})")

                    elif receiver == 0x05:
                        print("Message terminated at MQTT")
                        await client.publish("car/terminated", b"1", qos=0)

                    else:
                        buffer[2] = 0x05
                        uart.write(buffer)
                        print("Forwarded message with receiver updated to 0x05")

                else:
                    print("Invalid message structure or IDs")
                    await client.publish("car/invalid", b"1", qos=0)

                buffer[:] = b''

async def alive_flasher():
    while True:
        led_alive.on()
        await asyncio.sleep(0.1)
        led_alive.off()
        await asyncio.sleep(9.9)

async def main():
    print("Step 1: Starting script")
    await flash_led(led_start)

    print("Step 2: Connecting to MQTT...")
    mqtt_attempts = 0
    while True:
        try:
            await client.connect()
            print("Connected to MQTT")
            await flash_led(led_wifi)
            await flash_led(led_mqtt)
            break
        except Exception as e:
            mqtt_attempts += 1
            print(f"MQTT connect failed ({mqtt_attempts}):", repr(e))
            await asyncio.sleep(2)

    print("Step 3: MQTT ready, starting UART...")
    asyncio.create_task(uart_listener())

    while True:
        await asyncio.sleep(1)

try:
    asyncio.run(main())
finally:
    client.close()
