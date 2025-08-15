# How-to: OPC UA → MQTT bridge

!!! info "Účel"
    Přenést data z OPC UA serveru do MQTT pomocí Node-RED.

## Předpoklady
- Nainstalovaný Node-RED s OPC UA a MQTT pluginy
- Přístup k OPC UA serveru (např. Prosys)
- MQTT broker (Mosquitto)

---

## Kroky
1. Přidej do Node-RED uzel OPC UA Client a připoj se na OPC UA server.
2. Vyber proměnné k odběru (např. teplota, rychlost vřetena).
3. Přidej uzel MQTT Out a nastav téma (např. factory/line1/cnc01/telemetry/).
4. Propoj OPC UA Client → funkce (případně transformace) → MQTT Out.
5. Ověř v MQTT Exploreru, že data z OPC UA chodí do MQTT.

---

## Ověření
- V MQTT Exploreru vidíš zprávy z OPC UA serveru.
- Node-RED loguje úspěšné přenosy.

---

## Rollback
- Odpoj OPC UA Client v Node-RED.
- Zastav flow nebo vypni Node-RED.
