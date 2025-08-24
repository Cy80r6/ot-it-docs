# How-to — ESP32 → MQTT (PlatformIO/Arduino)

**Status:** stable  
**Slug:** how-to/platformio-esp32-mqtt

---

## Cíl
Rychlý start s PlatformIO (Arduino framework) pro ESP32: Wi-Fi, MQTT, LWT, telemetrie každých 5 s, sjednocené topics.

---

## platformio.ini
```ini
[env:esp32-s3]
platform = espressif32
board = esp32-s3-devkitc-1
framework = arduino
monitor_speed = 115200
lib_deps = knolleary/PubSubClient@^2.8
build_flags = -D ORG=\"home\" -D SITE=\"lab\" -D DEV=\"esp32-01\"
```

---

## src/main.cpp
Použij mini-sketch z [How-to: ESP32 → MQTT (mini-sketch)](../how-to/esp32-mqtt.md), sjednoť topics podle konvence:
- LWT: `v1/<org>/<site>/<device>/meta/status` (ONLINE/OFFLINE, retained)
- Telemetrie: `v1/<org>/<site>/<device>/tele/temperature` (každých 5 s, QoS 0, retain false)

---

## Build & upload
- Připoj ESP32 přes USB.
- Spusť v rootu projektu:
  ```sh
  pio run -t upload
  pio device monitor
  ```

---

## Ověření
- MQTT Explorer: subscribe na `v1/home/lab/esp32-01/#`.
- Na topicu `.../meta/status` uvidíš ONLINE/OFFLINE (retained).
- Každých 5 s se publikuje teplota na `.../tele/temperature`.

---

## Troubleshooting
- Pokud se ESP32 nepřipojí, zkontroluj Wi-Fi údaje a MQTT broker.
- Pro detailní logy použij PlatformIO Monitor.
