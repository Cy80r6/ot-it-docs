
# Runbook: Start/Stop labu (Docker Compose)

!!! info "Účel"
    Opakovatelné spuštění a zastavení labu (Mosquitto + Node-RED) v Dockeru. Jedna stránka pro každodenní provoz.

## Předpoklady
- Windows 10/11 s Docker Desktop a WSL2
- Připravený `docker-compose.yml` (viz Sprint 0)
- MQTT Explorer

---

## Start labu (cca 30 s)
1. **Spusť stack**
   ```powershell
   docker compose up -d
   ```
2. **Ověření běhu**
   - `docker ps` — oba kontejnery běží
   - Mosquitto: MQTT Explorer → Connect localhost:1883
   - Node-RED: http://localhost:1880
3. **ESP32 – připojit napájení**
   - Očekávej publish každých 5 s do tématu např. `v1/home/lab/esp32-01/tele/temperature`

---

## Stop labu (cca 10 s)
- Vypni stack:
  ```powershell
  docker compose down
  ```
- Odpoj ESP32.

---

## Správa a troubleshooting
- `docker ps` — seznam běžících kontejnerů
- `docker logs <jméno>` — výpis logů (např. mosquitto, nodered)
- `docker exec -it <jméno> sh` — vstup do kontejneru
- Pokud nevidíš data v Node-RED, zkontroluj topic a přidej debug node.
- MQTT Explorer se nepřipojí: běží Mosquitto? není blokován port 1883?
- ESP32 neposílá: IP brokeru, Wi-Fi, kolize clientId.

---

## Odkazy
- [How‑to: Instalace Dockeru na Windows + WSL2](../projects/sprint-0-docker-stack.md)
   # ...existing code...
- [How‑to: Instalace Node‑RED](../how-to/instalace-node-red.md)
- [How‑to: Node‑RED: základní flow](../how-to/node-red-zakladni-flow.md)
- [How‑to: ESP32 → MQTT](../how-to/esp32-mqtt.md)
- [Project: Sprint 0 – Docker stack](../projects/sprint-0-docker-stack.md)
