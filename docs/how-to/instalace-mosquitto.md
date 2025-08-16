# How-to: Instalace Mosquitto

Tento návod popisuje instalaci MQTT brokeru Mosquitto v Dockeru.

## Předpoklady
- Docker a Docker Compose
- Základní znalost práce s příkazovou řádkou

## Postup
1. Vytvoř `docker-compose.yml` s konfigurací Mosquitto.
2. Spusť broker příkazem:
   ```shell
   docker compose up -d mosquitto
   ```
3. Ověř běh brokera (např. pomocí MQTT Explorer).

## Ověření
- Připojení klienta na port 1883
- Test publish/subscribe

## Rollback
- Zastavení a smazání kontejneru:
   ```shell
   docker compose down
   ```
