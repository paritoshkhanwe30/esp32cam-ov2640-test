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






#CODE
#include "esp_camera.h"
#include <WiFi.h>

// ✅ Brownout fix
#include "soc/soc.h"
#include "soc/rtc_cntl_reg.h"

// ===================
// Camera Model
// ===================
#define CAMERA_MODEL_AI_THINKER
#include "camera_pins.h"

// ===================
// WiFi Credentials
// ===================
const char* ssid = "pari";
const char* password = "12345678";

void startCameraServer();

// ===================
// Setup
// ===================
void setup() {
  Serial.begin(115200);
  Serial.setDebugOutput(false);
  Serial.println();

  // 🔥 Disable brownout reset
  WRITE_PERI_REG(RTC_CNTL_BROWN_OUT_REG, 0);

  camera_config_t config;
  config.ledc_channel = LEDC_CHANNEL_0;
  config.ledc_timer = LEDC_TIMER_0;

  config.pin_d0 = Y2_GPIO_NUM;
  config.pin_d1 = Y3_GPIO_NUM;
  config.pin_d2 = Y4_GPIO_NUM;
  config.pin_d3 = Y5_GPIO_NUM;
  config.pin_d4 = Y6_GPIO_NUM;
  config.pin_d5 = Y7_GPIO_NUM;
  config.pin_d6 = Y8_GPIO_NUM;
  config.pin_d7 = Y9_GPIO_NUM;

  config.pin_xclk = XCLK_GPIO_NUM;
  config.pin_pclk = PCLK_GPIO_NUM;
  config.pin_vsync = VSYNC_GPIO_NUM;
  config.pin_href = HREF_GPIO_NUM;

  config.pin_sccb_sda = SIOD_GPIO_NUM;
  config.pin_sccb_scl = SIOC_GPIO_NUM;

  config.pin_pwdn  = PWDN_GPIO_NUM;
  config.pin_reset = RESET_GPIO_NUM;

  config.xclk_freq_hz = 20000000;
  config.pixel_format = PIXFORMAT_JPEG;

  // 🔥 ULTRA LOW LAG SETTINGS
  config.frame_size = FRAMESIZE_QQVGA;   // 160x120
  config.jpeg_quality = 25;              // faster
  config.fb_count = 2;
  config.grab_mode = CAMERA_GRAB_LATEST;

  // Init camera
  esp_err_t err = esp_camera_init(&config);
  if (err != ESP_OK) {
    Serial.printf("Camera init failed 0x%x", err);
    return;
  }

  // 🔥 Sensor tuning
  sensor_t * s = esp_camera_sensor_get();
  s->set_framesize(s, FRAMESIZE_QQVGA);
  s->set_quality(s, 25);
  s->set_brightness(s, 0);
  s->set_contrast(s, 0);
  s->set_saturation(s, 0);

  // WiFi connection
  WiFi.begin(ssid, password);
  WiFi.setSleep(false); // 🔥 reduces lag

  Serial.print("Connecting...");
  while (WiFi.status() != WL_CONNECTED) {
    delay(300);
    Serial.print(".");
  }

  Serial.println("\nWiFi connected");

  // Start streaming server
  startCameraServer();

  Serial.print("Camera Ready: http://");
  Serial.println(WiFi.localIP());
}

// ===================
// Loop
// ===================
void loop() {
  delay(10000);
}
