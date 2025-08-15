
---

## `docs/how-to/node-red-zakladni-flow.md`
```markdown
# Node-RED: základní flow

## Účel
Postavit minimální flow: MQTT → transformace → dashboard.

## Předpoklady
- Funkční MQTT broker (viz [Instalace Mosquitto](instalace-mosquitto.md))
- Node-RED běží (viz [Instalace Node-RED](instalace-node-red.md))

## Kroky
1. V Node-RED otevři **Manage palette** a přidej:
   - `node-red-dashboard`
2. Přidej **MQTT in** node:
   - Server: `localhost:1883`
   - Topic: `lab/+/telemetry/#`
3. Přidej **Function** node:
    ```js
    let p = msg.payload;
    try { p = (typeof p === 'string') ? JSON.parse(p) : p; } catch(e){}
    msg.payload = p.value ?? p;
    return msg;
    ```
4. Přidej **ui_chart** a **ui_text** (skupina „Telemetry“).
5. Propoj: MQTT in → Function → (chart + text).
6. Deploy.

## Ověření
- V MQTT Explorer `Publish` na `lab/esp32/telemetry/temperature` s payloadem `{"ts":123,"value":23.7,"unit":"C"}`.
- Dashboard (`/ui`) zobrazuje číslo a graf.

## Rollback
- Flow disable nebo smazání z editoru.
