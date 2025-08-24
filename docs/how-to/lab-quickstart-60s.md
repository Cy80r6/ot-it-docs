
# Lab Quickstart (60 s) — Docker Compose

!!! info "Jednou větou"
    Spusť Mosquitto a Node‑RED v Dockeru → otestuj payload → otevři dashboard.

> Tento postup vychází z [ADR 0002: Docker stack jako výchozí prostředí](../adr/adr-0002-docker-stack.md).

## Předpoklady
- Nainstalovaný Docker Desktop + WSL2 ([návod zde](instalace-docker-compose.md))
- Připravený adresář s `docker-compose.yml` (viz [Sprint 0](../projects/sprint-0-docker-stack.md))

## Kroky
1. **Spusť lab stack**
   V adresáři s `docker-compose.yml` spusť:
   ```powershell
   docker compose up -d
   ```
   (Spustí Mosquitto a Node-RED v kontejnerech.)

2. **Ověř běh služeb**
   ```powershell
   docker ps
   ```
   Měl bys vidět oba kontejnery (mosquitto, nodered) a porty 1883, 1880.

3. **Test payload (MQTT Explorer nebo WSL2)**
   - Připoj se v MQTT Exploreru na `localhost:1883`.
   - Subscribe na topic: `v1/home/lab/esp32-01/tele/temperature`
   - Publish na stejný topic, payload:
     ```json
     {"ts": 1734300000, "value": 23.7, "unit": "C"}
     ```
   - Alternativně z WSL2:
     ```bash
   mosquitto_pub -h localhost -t v1/home/lab/esp32-01/tele/temperature -m '{"ts": 1734300000, "value": 23.7, "unit": "C"}'
     ```

4. **Dashboard**
   - Otevři [http://localhost:1880/ui](http://localhost:1880/ui) a zkontroluj graf + text.

---

## Co dělat, když to neukáže data
- Přidej v Node‑RED debug node a sleduj `msg.payload`.
- Zkontroluj, že MQTT in má správný topic (`lab/+/telemetry/#`).
- Ověř logy kontejnerů:
  ```powershell
  docker compose logs mosquitto
  docker compose logs nodered
  ```
- Ověř, že v `mosquitto.conf` je `listener 1883` a `allow_anonymous true` (viz [Sprint 0](../projects/sprint-0-docker-stack.md)).

---

> Ruční spouštění Mosquitto a Node-RED na hostitelském systému je legacy varianta a není doporučeno.
