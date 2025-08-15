
# Playbooks
--8<-- "includes/abbr.md"

Osvědčené postupy a checklisty.

## Lab workflow (rychlý start)

1) Mosquitto:
```powershell
"C:\mosquitto\mosquitto.exe" -c "C:\mosquitto\mosquitto.conf"
```

Node‑RED:
```powershell
node-red
```

Test payload (MQTT Explorer) → topic lab/esp32/telemetry/temperature
```json
{"ts": 1734300000, "value": 23.7, "unit": "C"}
```

Otevři dashboard: http://127.0.0.1:1880/ui

ℹ️ Detailní verze: Playbooks → Runbook: Start/Stop labu.
