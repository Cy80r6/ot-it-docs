# How-to — ESP32 → MQTT (ESPHome)

**Status:** stable  
**Slug:** how-to/esphome-esp32-mqtt

---

## Cíl
Rychlý start s ESPHome pro ESP32: Wi-Fi, MQTT, LWT, telemetrie každých 5 s, vše v YAML.

---

## Základní konfigurace (esp32-01.yaml)
```yaml
esphome:
  name: esp32-01
  platform: ESP32
  board: esp32dev
wifi:
  ssid: !secret wifi_ssid
  password: !secret wifi_pass
ota:
logger:
  level: INFO
mqtt:
  broker: 192.168.1.100
  port: 1883
  client_id: esp32-01
  topic_prefix: v1/home/lab/esp32-01
  birth_message:
    topic: v1/home/lab/esp32-01/meta/status
    payload: ONLINE
    retain: true
  will_message:
    topic: v1/home/lab/esp32-01/meta/status
    payload: OFFLINE
    retain: true
sensor:
  - platform: template
    name: "Dummy temperature"
    id: dummy_temp
    unit_of_measurement: "°C"
    update_interval: 5s
    lambda: |-
      return 20.0 + (millis() % 10000) / 1000.0;
    on_value:
      then:
        - mqtt.publish:
            topic: v1/home/lab/esp32-01/tele/temperature
            payload: !lambda |-
              char buf[16];
              snprintf(buf, sizeof(buf), "%.2f", id(dummy_temp).state);
              return std::string(buf);
            qos: 0
            retain: false
```

---

## První flash (USB) → OTA
- Připoj ESP32 přes USB.
- Spusť:
  ```sh
  esphome run esp32-01.yaml
  ```
- První flash proběhne přes USB, další už můžeš dělat OTA.

---

## Ověření
- MQTT Explorer: subscribe na `v1/home/lab/esp32-01/#`.
- Na topicu `.../meta/status` uvidíš ONLINE/OFFLINE (retained).
- Každých 5 s se publikuje teplota na `.../tele/temperature`.

---

## Troubleshooting
- Pokud se ESP32 nepřipojí, zkontroluj Wi-Fi údaje a MQTT broker.
- Pro OTA musí být zařízení ve stejné síti.
- Pokud potřebuješ `retain` pro jiný topic, přidej `retain: true` do publish.
