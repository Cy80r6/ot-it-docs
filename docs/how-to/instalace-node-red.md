# How-to: Instalace Node-RED

Tento návod popisuje instalaci Node-RED v Dockeru.

## Předpoklady
- Docker a Docker Compose

## Postup
1. Přidej službu `node-red` do `docker-compose.yml`.
2. Spusť Node-RED:
   ```shell
   docker compose up -d node-red
   ```
3. Otevři webové rozhraní na http://localhost:1880

## Ověření
- Web UI je dostupné
- Lze vytvořit a deploynout flow

## Rollback
- Zastavení a smazání kontejneru:
   ```shell
   docker compose down
   ```
