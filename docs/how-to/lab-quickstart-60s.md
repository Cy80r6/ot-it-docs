# Lab Quickstart (60 s)

!!! info "Jednou větou"
    Spusť Mosquitto → Node‑RED → otestuj payload → otevři dashboard.

## Kroky
1. **Mosquitto**
   ```powershell
   "C:\mosquitto\mosquitto.exe" -c "C:\mosquitto\mosquitto.conf"
   ```

2. **Node‑RED**
   ```powershell
   node-red
   ```

3. **Test payload (MQTT Explorer)**
   - Topic: lab/esp32/telemetry/temperature
   - Payload:
     ```json
     {"ts": 1734300000, "value": 23.7, "unit": "C"}
     ```

4. **Dashboard**
   - Otevři http://127.0.0.1:1880/ui a zkontroluj graf + text.

---

## Co dělat, když to neukáže data
- Přidej v Node‑RED debug node a koukej na msg.payload.
- Zkontroluj, že MQTT in má správný topic (lab/+/telemetry/#).
