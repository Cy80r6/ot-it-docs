# How-to: Node-RED → InfluxDB

!!! info "Účel"
    Ukládat data z Node-RED do InfluxDB pro historickou analýzu.

## Předpoklady
- Nainstalovaný Node-RED
- InfluxDB server běží

---

## Kroky
1. Přidej v Node-RED InfluxDB Out node.
2. Nastav připojení na InfluxDB (host, port, databáze).
3. Propoj data z MQTT/OPC UA na InfluxDB Out node.
4. Otestuj zápis dat a ověř v InfluxDB.

---

## Ověření
- Data z Node-RED jsou uložená v InfluxDB.
- Historie je dostupná pro dashboard.

---

## Rollback
- Odpoj nebo deaktivuj InfluxDB Out node v Node-RED.
