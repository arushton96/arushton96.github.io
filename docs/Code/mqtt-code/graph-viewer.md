# mqtt_graph_viewer.py

This Python script was developed to run on a PC for real-time monitoring and visualization of MQTT data during testing and demonstration.

It subscribes to key MQTT topics and plots:
- Robot car movement state (start/stop)
- Count of terminated messages (loopback detected)
- Count of redundant messages (excess sensor triggers)

This is not embedded code — it was designed as a local tool to support development and debugging of the MQTT subsystem.

---

```python
import matplotlib.pyplot as plt
import matplotlib.animation as animation
from collections import deque
import paho.mqtt.client as mqtt

# MQTT Configuration
BROKER = "mqtt.eclipseprojects.io"
PORT = 1883
TOPICS = [("car/state", 0), ("counter/terminated", 0), ("counter/redundant", 0)]

# Plot data
MAX_POINTS = 100
movement_data = deque([0]*MAX_POINTS, maxlen=MAX_POINTS)
terminated_count = deque([0]*MAX_POINTS, maxlen=MAX_POINTS)
redundant_count = deque([0]*MAX_POINTS, maxlen=MAX_POINTS)

# Current values to plot
current_terminated = 0
current_redundant = 0

def on_connect(client, userdata, flags, rc):
    if rc == 0:
        print("Connected to MQTT broker.")
        for topic, qos in TOPICS:
            client.subscribe(topic)
    else:
        print(f"Failed to connect, return code {rc}")

def on_message(client, userdata, msg):
    global current_terminated, current_redundant

    topic = msg.topic
    payload = msg.payload.decode()

    if topic == "car/state":
        if payload == "1":
            movement_data.append(1)
        elif payload == "0":
            movement_data.append(-1)
        else:
            movement_data.append(0)
    elif topic == "counter/terminated":
        try:
            current_terminated = int(payload)
        except ValueError:
            pass
    elif topic == "counter/redundant":
        try:
            current_redundant = int(payload)
        except ValueError:
            pass

def update_plot(frame):
    terminated_count.append(current_terminated)
    redundant_count.append(current_redundant)

    ax1.clear()
    ax1.plot(movement_data, label="Car State")
    ax1.set_ylim([-1.5, 1.5])
    ax1.set_title("Robot Car State (1 = Moving, -1 = Stopped)")
    ax1.grid(True)
    ax1.legend(loc="upper right")

    ax2.clear()
    ax2.plot(terminated_count, label="Terminated Messages")
    ax2.plot(redundant_count, label="Redundant Messages")
    ax2.set_title("MQTT Message Counters")
    ax2.set_xlabel("Time")
    ax2.grid(True)
    ax2.legend(loc="upper left")

# MQTT Setup
client = mqtt.Client()
client.on_connect = on_connect
client.on_message = on_message
client.connect(BROKER, PORT, 60)
client.loop_start()

# Matplotlib Setup
fig, (ax1, ax2) = plt.subplots(2, 1, figsize=(10, 6))
ani = animation.FuncAnimation(fig, update_plot, interval=500)
plt.tight_layout()
plt.show()
