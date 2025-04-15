**Wavego Bionic Dog Raspberry Pi Setup - Step-by-Step Beginner Guide**

---

This guide documents the entire process of setting up and working with the Wavego 12-DOF Bionic Dog using a Raspberry Pi. This includes connecting, configuring, accessing the camera, running WebSocket servers, troubleshooting, and more. It's tailored for absolute beginners based on our real conversations and hands-on trials.

---

### 🚀 Step 1: Accessing Raspberry Pi (RPi) for the First Time

#### 1. Ping Raspberry Pi
- Connect both your **Raspberry Pi** and **laptop** to the **same WiFi network** (e.g., your mobile hotspot).
- Find the Raspberry Pi's IP address using:
  ```bash
  ping raspberrypi (for ipv4 👉🏼 ping -4 raspberrypi)
  ```
  or use a network scanner app like Fing if that doesn't work.

#### 2. SSH into Raspberry Pi
Once you have the IP address(default) (e.g., `192.168.223.75`), open your terminal and type:
```bash
ssh pi@192.168.223.75
```
Default password:
```text
1234567890
```


---

### 📂 Step 2: Cloning Wavego GitHub Repository
After successful SSH:
```bash
git clone https://github.com/waveshare/Wavego
cd Wavego
```
Run the setup.py file:
```
python3 setup.py
```
### 📷 Step 3: Testing the Camera Module

#### 1. Check camera modules
```bash
libcamera-hello

If you get a series of response, then your camera is working!
```

#### 2. Capture a still image (test):
```bash
libcamera-still -o test.jpg

This command takes a picture using the Raspberry Pi Camera.
```

#### 3. Live Preview with VLC:
Install VLC if not already installed:
```bash
sudo apt-get install vlc
```
Then use the RTSP/HTTP stream address in VLC:
```text
rtsp://<ip>:8554/stream
```

Or test using `libcamera-vid`:
```bash
libcamera-vid -t 0 --inline --listen -o - | cvlc -vvv stream:///dev/stdin --sout '#standard{access=http,mux=ts,dst=:8554}' :demux=h264
```

---

### 🚪 Step 4: Running Web Server (Control Interface)

We used `server.py` from the cloned repo:
```bash
cd ~/Wavego/RPi
python3 webServer.py
```
This runs a **WebSocket** server on `ws://0.0.0.0:8888`, which listens for commands.

#### Notes:
- It has a WiFi hotspot amed `Waveshare Robot`.
- Connect your **laptop/phone to that hotspot**.
- Open your browser and go to:
```text
http://192.168.223.75:5000
```
- It opens the Wavego robot control interface.

---

### 🪧 Step 5: WebSocket Testing

We tested the WebSocket using PieSocket:
```text
https://www.piesocket.com/websocket-tester
```
- Used address:
```text
ws://192.168.223.75:8888
```
- Sent message:
```text
admin:123456
```
- Response:
```text
Connected!
```

---

### 🛠️ Step 6: Fixes and Troubleshooting

#### 1. Missing modules (like websockets)
```bash
pip3 install websockets
```

#### 2. No camera feed showing in browser:
Check if the video stream is actually served via `8000`/`8554` and ensure HTML has proper video rendering logic.

#### 3. Enable VNC for GUI Access
```bash
sudo raspi-config
# Go to Interface Options -> VNC -> Enable
```
Then connect using **VNC Viewer** with IP like `192.168.4.1`.

---

### 🔄 Recap
- Raspberry Pi hosts the robot's brain.
- It broadcasts its own WiFi (Waveshare Robot).
- Laptop/phone should connect to that WiFi to open the UI at `192.168.4.1`.
- WebSocket server handles movement and command processing.
- Camera works with libcamera/VLC.
- Troubleshooting done using SSH, VNC, and logs.

---

