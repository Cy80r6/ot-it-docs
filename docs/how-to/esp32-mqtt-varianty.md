# ESP32 → MQTT — varianty a checklist

**Status:** stable  
**Slug:** how-to/esp32-mqtt-varianty

---

## Přehled variant

| Varianta         | Výhody                        | Odkaz na návod |
|------------------|-------------------------------|---------------|
| MicroPython      | Rychlý start, jednoduchý kód  | [MicroPython](./esp32-micropython-mqtt.md) |
| ESPHome          | YAML, OTA, bez kódu           | [ESPHome](./esphome-esp32-mqtt.md) |
| Tasmota          | WebUI, pravidla, OTA          | [Tasmota](./tasmota-esp32-mqtt.md) |
| PlatformIO/Arduino | Plná kontrola, knihovny      | [PlatformIO/Arduino](./platformio-esp32-mqtt.md) |

---

## Checklist pro realizaci
- [ ] Zvolená varianta je nahraná na ESP32
- [ ] MQTT broker běží (viz Mosquitto LAB profil)
- [ ] ESP32 publikuje LWT (`.../meta/status`, retained)
- [ ] ESP32 publikuje telemetrii (`.../tele/temperature`, každých 5 s, QoS 0, retain false)
- [ ] Node-RED dashboard zobrazuje data do 2 s

---

## Ready-to-run odkazy
- [How-to: ESP32 → MQTT (MicroPython)](./esp32-micropython-mqtt.md)
- [How-to: ESP32 → MQTT (ESPHome)](./esphome-esp32-mqtt.md)
- [How-to: ESP32 → MQTT (Tasmota)](./tasmota-esp32-mqtt.md)
- [How-to: ESP32 → MQTT (PlatformIO/Arduino)](./platformio-esp32-mqtt.md)
