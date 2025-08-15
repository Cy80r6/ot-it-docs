# How-to: Node-RED dashboard (historie)

!!! info "Účel"
    Zobrazit historická data z InfluxDB v Node-RED dashboardu.

## Předpoklady
- Node-RED běží
- InfluxDB obsahuje historická data

---

## Kroky
1. Přidej v Node-RED InfluxDB In node.
2. Nastav dotaz na historická data (např. posledních 24h).
3. Propoj výstup na dashboard widget (graf, text).
4. Ověř zobrazení dat v dashboardu.

---

## Ověření
- Dashboard zobrazuje historická data z InfluxDB.

---

## Rollback
- Odpoj nebo deaktivuj InfluxDB In node v Node-RED.
