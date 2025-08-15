# Runbook: Start/Stop labu (ESP32 → MQTT → Node‑RED)

!!! info "Účel"
    Opakovatelné spuštění a zastavení labu bez přemýšlení. Jedna stránka pro každodenní provoz.

## Předpoklady
- Windows 11, admin práva
- Mosquitto nainstalovaný v `C:\mosquitto`
- Node‑RED nainstalovaný (`npm install -g node-red`)
- MQTT Explorer

---

## Start labu (cca 60–90 s)
1. **Mosquitto – start**
   ```powershell
   "C:\mosquitto\mosquitto.exe" -c "C:\mosquitto\mosquitto.conf"
   ```
   Kontrola: MQTT Explorer → Connect localhost:1883.

2. **Node‑RED – start**
   ```powershell
   node-red
   ```
   Kontrola: Otevři http://127.0.0.1:1880/ a /ui.

3. **Test payload (volitelné)**
   - Topic: lab/esp32/telemetry/temperature
   - Payload:
     ```json
     {"ts": 1734300000, "value": 23.7, "unit": "C"}
     ```

4. **ESP32 – připojit napájení**
   - Očekávej publish každých 5 s do tématu výše.

---

## Stop labu (cca 15 s)
- Zavři konzoli s Mosquittem nebo ukonči proces ve Správci úloh.
- V konzoli s Node‑RED stiskni Ctrl+C (dvakrát potvrď).
- Odpoj ESP32.

---

## Rychlé trouble‑shooting
- Nevidím data v Node‑RED: zkontroluj topic, přidej debug node.
- MQTT Explorer se nepřipojí: běží Mosquitto? není blokován port 1883?
- ESP32 neposílá: IP brokeru, Wi‑Fi, kolize clientId.

---

## Odkazy
- [How‑to: Instalace Mosquitto](../how-to/instalace-mosquitto.md)
- [How‑to: Instalace Node‑RED](../how-to/instalace-node-red.md)
- [How‑to: Node‑RED: základní flow](../how-to/node-red-zakladni-flow.md)
- [How‑to: ESP32 → MQTT](../how-to/esp32-mqtt.md)
- [Project: Sprint 1 – Lokální MQTT tok](../projects/sprint-1-lokalni-mqtt-tok.md)
