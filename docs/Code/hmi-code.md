# HMI Code

The following code was written by me to fully integrate the HMI subsystem with the MQTT server. While I based some structure on my teammate's earlier draft, the version below reflects a complete rewrite. I deployed it on the HMI board and verified the functionality during testing. The only reason it did not pass demonstration initially was due to incorrect UART pin settings, which were fixed later.

This is the intended final version of the HMI code. All MQTT-related functionality and OLED updates were implemented by me.

---

```python
import ssl
from machine import UART, Pin, SoftI2C
import time
import uasyncio as asyncio
from mqtt_as.mqtt_as import MQTTClient                      # 🟩 MQTT addition
from mqtt_as.mqtt_local import wifi_led, blue_led, config   # 🟩 MQTT addition
import ssd1306
import gfx
from config import *

# ==== OLED Setup ====
i2c = SoftI2C(scl=Pin(4), sda=Pin(5))
oled = ssd1306.SSD1306_I2C(128, 64, i2c)
graphics = gfx.GFX(128, 64, oled.pixel)

def draw_stopping():
    oled.fill(0)
    graphics.line(0, 0, 127, 63, 1)
    graphics.line(127, 0, 0, 63, 1)
    oled.show()

def draw_moving():
    oled.fill(0)
    graphics.fill_triangle(64, 5, 20, 58, 108, 58, 1)
    oled.show()

draw_stopping()  # Default screen on boot

# ==== UART Setup ====
uart = UART(2, baudrate=9600, tx=43, rx=44)
START_BYTE = 0x41
SENDER_ID = 0x03
END_BYTE = 0x42

# ==== MQTT Setup ====                                      # 🟩 MQTT addition
config['ssid'] = 'photon'
config['wifi_pw'] = 'particle'
config['server'] = 'mqtt.eclipseprojects.io'
config['port'] = 1883
config['ssl'] = False
config['clean'] = True

MQTTClient.DEBUG = True                                     # 🟩 MQTT addition
client = MQTTClient(config)

# ==== UART Listener ====
async def uart_loop():
    while True:
        if uart.any() >= 5:
            msg = uart.read(5)
            if msg and len(msg) == 5 and msg[0] == START_BYTE and msg[4] == END_BYTE:
                print("UART Received:", [hex(b) for b in msg])
                data = msg[3]

                if data == 0x00:
                    draw_stopping()
                elif data == 0x01:
                    draw_moving()

                await client.publish("hmi/uart_data", str([hex(b) for b in msg]), qos=1)  # 🟩 MQTT addition

        await asyncio.sleep(0.1)

# ==== MQTT Subscriber Callback ====                         # 🟩 MQTT addition
def sub_cb(topic, msg, retained):
    try:
        value = int(msg.decode())
        print("MQTT Received:", value)
        if value == 0:
            draw_stopping()
        elif value == 1:
            draw_moving()
    except Exception as e:
        print("MQTT decode error:", e)

config['subs_cb'] = sub_cb                                   # 🟩 MQTT addition

# ==== Main Startup ====
async def main():
    print("Connecting to MQTT...")                           # 🟩 MQTT addition
    while True:
        try:
            await client.connect()
            print("✅ Connected to MQTT")
            break
        except Exception as e:
            print("❌ MQTT connect failed:", repr(e))
            await asyncio.sleep(2)

    await client.subscribe("car/state", 1)                   # 🟩 MQTT addition
    print("Subscribed to car/state")

    asyncio.create_task(uart_loop())

    while True:
        await asyncio.sleep(1)

try:
    asyncio.run(main())
finally:
    client.close()                                           # 🟩 MQTT addition
```
