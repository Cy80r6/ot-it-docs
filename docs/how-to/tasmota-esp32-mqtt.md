# How-to — ESP32 → MQTT (Tasmota)

**Status:** stable  
**Slug:** how-to/tasmota-esp32-mqtt

---

## Cíl
Rychlý start s Tasmota na ESP32: Wi-Fi, MQTT, LWT, telemetrie každých 5 s, vše přes webové rozhraní a Console.

---

## Flash a základní nastavení
- Flashni firmware `tasmota32.factory.bin` (Tasmotizer nebo esptool).
- Připoj ESP32 k Wi-Fi (WebUI → Configure Wi‑Fi).
- Nastav MQTT (WebUI → Configure MQTT):
  - Broker: IP adresa Mosquitto
  - Port: 1883
  - Client: esp32-01
  - FullTopic: `v1/home/lab/%topic%/%prefix%/`

---

## Pravidla pro LWT a telemetrii
V Tasmota Console zadej:
```
Backlog Rule1 ON Tele-LWT#LWT DO Publish v1/home/lab/esp32-01/meta/status %value% ENDON; Rule1 1
TelePeriod 5
Rule2 ON Time#Minute DO ADD1 1; Publish v1/home/lab/esp32-01/tele/temperature %var1% ENDON; Rule2 1
```
- Pokud potřebuješ `retain`, použij `Publish topic payload 1`.

---

## Ověření
- MQTT Explorer: subscribe na `v1/home/lab/esp32-01/#`.
- Na topicu `.../meta/status` uvidíš ONLINE/OFFLINE (retained).
- Každých 5 s se publikuje teplota na `.../tele/temperature`.

---

## Troubleshooting
- Pokud se ESP32 nepřipojí, zkontroluj Wi-Fi údaje a MQTT broker.
- Pravidla lze upravit v Console podle potřeby.
- Pro detailní logy použij WebUI → Console.
