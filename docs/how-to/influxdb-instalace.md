# How-to: InfluxDB instalace

!!! info "Účel"
    Nainstalovat a zprovoznit InfluxDB pro ukládání historických dat z Node-RED.

## Předpoklady
- Windows/Linux, admin práva
- Node-RED nainstalovaný

---

## Kroky
1. Stáhni a nainstaluj InfluxDB z oficiálních stránek.
2. Spusť InfluxDB server (default port 8086).
3. Vytvoř databázi (např. `iotdata`).
4. V Node-RED přidej InfluxDB Out node a nastav připojení.
5. Otestuj zápis dat z Node-RED do InfluxDB.

---

## Ověření
- InfluxDB přijímá data z Node-RED (zkontroluj v CLI nebo web UI).
- Historická data jsou dostupná pro dashboard.

---

## Rollback
- Zastav InfluxDB službu.
- Odpoj InfluxDB node v Node-RED.
