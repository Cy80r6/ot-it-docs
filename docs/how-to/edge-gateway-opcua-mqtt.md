
# Edge gateway: OPC UA → MQTT v Node‑RED
--8<-- "includes/abbr.md"

**Účel:** Přenést data ze simulovaného OPC UA serveru do MQTT jako základ OT→IT integrace.  
**Předpoklady:** Nainstalovaný Node‑RED, přístup k OPC UA serveru (Prosys), MQTT broker (Mosquitto).  
**Odhad času:** 45–60 min.

## Kroky
1. Přidej uzel **opcua-client** a připoj se na `opc.tcp://<server>:4840`.
2. Přihlaš se, vyber proměnné a spusť **subscribe**.
3. Mapuj na JSON: `{ts, value, unit, quality, source}`.
4. Pošli na MQTT téma `factory/line1/cnc01/telemetry/spindle_speed` s QoS 1.
5. Ověř v MQTT Exploreru, že zprávy přichází.

## Ověření
- V Node‑RED dashboardu vidíš živou hodnotu + graf.
- V MQTT Exploreru chodí zprávy každých X sekund.

## Rollback
- Zastav flow, odpoj MQTT a OPC UA, vypni Node‑RED.
