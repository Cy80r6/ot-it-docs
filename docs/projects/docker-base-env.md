# Projekt: Docker base env

Tato stránka shrnuje architekturu a základní informace k výchozímu Docker stacku pro OT/IT lab.

## Architektura

- Docker Desktop s WSL2 (Windows 11)
- docker-compose orchestruje služby (Mosquitto, Node-RED, případně další)
- Volitelné: podpora GPU (NVIDIA)

## Odkazy
- [How-to: Instalace Docker + Compose](../how-to/instalace-docker-compose.md)
- [How-to: Základní práce s kontejnery](../how-to/zakladni-prace-s-kontejnery.md)
- [ADR 0001: Docker stack jako výchozí prostředí](../adr/adr-0001-docker-stack.md)
- [Sprint 0 — Docker stack: Mosquitto + Node-RED](sprint-0-docker-stack.md)

## Co pokrývá
- Základní infrastruktura pro všechny další sprinty
- Ověřený běh Mosquitto a Node-RED v kontejnerech
- Možnost rozšíření o další služby (InfluxDB, OPC UA, ...)
