# How-to: ESP32 → MQTT

Návod na odesílání dat ze senzoru ESP32 do MQTT brokeru.

## Předpoklady
- ESP32 s firmwarem (např. ESPHome, MicroPython)
- MQTT broker

## Postup
1. Nakonfiguruj ESP32 pro připojení k WiFi a MQTT brokeru.
2. Odesílej data (např. teplotu) do tématu `test/sensor/temperature`.

## Ověření
- Data z ESP32 jsou vidět v MQTT Exploreru
