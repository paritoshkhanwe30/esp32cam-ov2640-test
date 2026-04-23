# esp32cam-ov2640-test
This repository contains test code for the ESP32-CAM module using the OV2640 camera.

# ESP32-CAM (OV2640) + ESP32-CAM-MB Web Server 📷

This project uses the **CameraWebServer example** to stream video from ESP32-CAM using the ESP32-CAM-MB (USB programmer board).

---

## 🧰 Hardware Required

* ESP32-CAM (AI Thinker)
* OV2640 Camera
* ESP32-CAM-MB (Micro USB Board)
* Micro USB Cable

---

## ⚙️ Setup

### 1. Install Arduino IDE

https://www.arduino.cc/en/software

---

### 2. Add ESP32 Board URL

Go to:
**File → Preferences → Additional Board Manager URLs**

Paste:

```id="boardurl2"
https://raw.githubusercontent.com/espressif/arduino-esp32/gh-pages/package_esp32_index.json
```

Then:

* Go to **Tools → Board → Board Manager**
* Search **ESP32**
* Install **esp32 by Espressif Systems**

---

## 📚 Libraries

No extra libraries required ✅

Built-in:

* WiFi.h
* esp_camera.h

---

## 📂 Open Example Code

Go to:

**File → Examples → ESP32 → Camera → CameraWebServer**

---

## 🔌 Connection

* Insert ESP32-CAM into **ESP32-CAM-MB board**
* Connect USB cable to PC

✅ No manual wiring required

---

## ⚡ Board Settings

* Board: **AI Thinker ESP32-CAM**
* Port: Select correct COM port
* Upload Speed: **115200**

---

## ▶️ Upload Steps

1. Press and hold **BOOT button** on MB board
2. Click **Upload**
3. Release BOOT after upload starts
4. Wait for upload to finish
5. Press **RESET button**

## 📡 Run

1. Open Serial Monitor (**115200 baud**)
2. You will see an IP address:

```id="ip2"
http://192.168.1.xxx
```

3. Open it in browser
4. Click **Start Stream**


## ❗ Notes

* Make sure WiFi SSID & Password are correct in code
* Use good USB cable (data cable, not charging only)
* If upload fails, press BOOT again


## 👨‍💻 Author
Paritosh Khanwe
