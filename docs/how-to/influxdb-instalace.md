# How-to: InfluxDB instalace

Návod na instalaci a základní konfiguraci InfluxDB v Dockeru.

## Předpoklady
- Docker a Docker Compose

## Postup
1. Přidej službu `influxdb` do `docker-compose.yml`.
2. Spusť InfluxDB:
   ```shell
   docker compose up -d influxdb
   ```
3. Otevři web UI na http://localhost:8086 a nastav organizaci a bucket.

## Ověření
- Lze zapisovat a číst data přes API nebo UI
