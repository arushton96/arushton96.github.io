# Sensor Code

The following code integrates the original sensor functionality written by my teammate with MQTT fallback logic that I implemented. This version reflects the final, fully functional integration that was intended to run during demonstration. Due to a hardware issue on the sensor board (USB wiring), I was unable to upload and test this version on the actual board — but it is the correct working integration.

MQTT-related sections are clearly marked to indicate my specific contributions.

---

```python
import time
import uasyncio as asyncio
from machine import SoftI2C, Pin, UART
# 🟩 MQTT addition
from mqtt_as.mqtt_as import MQTTClient
# 🟩 MQTT addition
from mqtt_as.mqtt_local import config

# I2C and sensor setup
i2c = SoftI2C(scl=Pin(9), sda=Pin(8), freq=100000)
OPT4060_ADDR = 0x44

# LED
led = Pin(15, Pin.OUT)

# UART2 on GPIO 47 (TX), GPIO 46 (RX)
uart = UART(2, baudrate=9600, tx=Pin(47), rx=Pin(46))

# Protocol constants
START = 0x41
END = 0x42
SENDER_ID = 0x01
RECEIVER_ID = 0x03
RED_MSG = 0x64
BLUE_MSG = 0x65

# 🟩 MQTT addition
# ==== MQTT Configuration ====
config['ssid'] = 'phpton'
config['wifi_pw'] = 'particle'
config['server'] = 'mqtt.eclipseprojects.io'
config['port'] = 1883
config['ssl'] = False
config['clean'] = True
config['subs_cb'] = None
MQTTClient.DEBUG = True
client = MQTTClient(config)

# 🟩 MQTT addition
# ==== Fallback Logic ====
uart_last_seen = time.ticks_ms()
FALLBACK_TIMEOUT = 15000  # 15 seconds
mqtt_fallback_active = False

def send_message(receiver_id, data_byte):
    msg = bytes([START, SENDER_ID, receiver_id, data_byte, END])
    uart.write(msg)
    print("Sent to UART:", [hex(b) for b in msg])

def read_uart():
    global uart_last_seen
    if uart.any() >= 5:
        msg = uart.read(5)
        if msg and len(msg) == 5 and msg[0] == START and msg[4] == END:
            print("UART Received:", [hex(b) for b in msg])
            uart_last_seen = time.ticks_ms()
            return msg
    return None

# 🟩 MQTT addition
# ==== MQTT Fallback Watchdog ====
async def check_mqtt_fallback():
    global mqtt_fallback_active
    while True:
        if time.ticks_diff(time.ticks_ms(), uart_last_seen) > FALLBACK_TIMEOUT:
            if not mqtt_fallback_active:
                mqtt_fallback_active = True
                print("MQTT fallback mode ENABLED")
                await client.subscribe('hmi/uart_data', 1)
        await asyncio.sleep(1)

# 🟩 MQTT addition
# ==== MQTT Message Handler ====
async def handle_mqtt_messages(topic, msg, retained):
    global uart_last_seen
    if not mqtt_fallback_active:
        return

    try:
        values = eval(msg.decode())
        if isinstance(values, list) and len(values) == 5 and values[0] == '0x41' and values[4] == '0x42':
            parsed = [int(v, 16) for v in values]
            print("Forwarding MQTT message:", values)
            uart.write(bytes(parsed))
            uart_last_seen = time.ticks_ms()
    except Exception as e:
        print("Error parsing MQTT message:", e)

# 🟩 MQTT addition
config['subs_cb'] = handle_mqtt_messages

# ==== Sensor Main Loop ====
async def sensor_loop():
    while True:
        read_uart()  # Check for incoming UART messages

        # Color sensor capture
        try:
            i2c.writeto(OPT4060_ADDR, bytes([0x0A, 0x30, 0x78]))
            time.sleep_ms(100)
            i2c.writeto(OPT4060_ADDR, bytes([0x00]))
            data = i2c.readfrom(OPT4060_ADDR, 16)

            red   = ((data[0] & 0x0F) << 8) | data[1]
            green = ((data[4] & 0x0F) << 8) | data[5]
            blue  = ((data[8] & 0x0F) << 8) | data[9]

            print("Red:", red, "Green:", green, "Blue:", blue)

            if red > green and red > blue and red > 200:
                print("🔴 Red detected")
                led.on()
                await asyncio.sleep(0.2)
                led.off()
                await asyncio.sleep(0.2)
                send_message(RECEIVER_ID, RED_MSG)

            elif blue > red and blue > green and blue > 200:
                print("🔵 Blue detected")
                led.on()
                await asyncio.sleep(0.2)
                led.off()
                await asyncio.sleep(0.2)
                send_message(RECEIVER_ID, BLUE_MSG)

            else:
                led.off()
                await asyncio.sleep(0.5)

        except Exception as e:
            print("Sensor read error:", e)
            await asyncio.sleep(1)

# 🟩 MQTT addition
# ==== Main Entry Point ====
async def main():
    print("Connecting to MQTT...")
    while True:
        try:
            await client.connect()
            print("MQTT Connected")
            break
        except Exception as e:
            print("MQTT connect failed:", e)
            await asyncio.sleep(2)

    asyncio.create_task(check_mqtt_fallback())
    await sensor_loop()

try:
    asyncio.run(main())
finally:
    client.close()
# 🟩 MQTT addition
```
