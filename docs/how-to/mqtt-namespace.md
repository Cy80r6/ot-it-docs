# How-to: Návrh MQTT namespace a konvencí pro topic

Tento návod vysvětluje, jak navrhnout a používat konzistentní MQTT namespace (pravidla pro názvy topiců) v OT/IT projektech.

## Proč řešit namespace?
- Umožňuje snadné filtrování a routování zpráv (wildcardy, ACL)
- Usnadňuje správu přístupových práv a bezpečnosti
- Zjednodušuje integraci s dalšími systémy (Node-RED, InfluxDB, Home Assistant)
- Snižuje riziko chyb a refaktoringu v budoucnu

## Doporučený tvar topicu
```
v1/<org>/<site>/<device>/<channel>/<metric>
```
- **v1** – verze schématu (umožní změny bez rozbití starých věcí)
- **<org>** – organizace, např. home, firma
- **<site>** – místo, zóna, např. lab
- **<device>** – např. esp32-01 (kebab-case, bez mezer)
- **<channel>** – tele | state | evt | cmd | cfg | meta
- **<metric>** – např. temperature, humidity, rssi, status

## Příklady
- Telemetrie: `v1/home/lab/esp32-01/tele/temperature` → 22.8
- Stav: `v1/home/lab/esp32-01/meta/status` → "ONLINE" (retained)
- RSSI: `v1/home/lab/esp32-01/state/rssi` → {"value":-61,"unit":"dBm","ts":...}
- Příkaz: `v1/home/lab/esp32-01/cmd/reboot` → {}
- ACK: `v1/home/lab/esp32-01/evt/ack` → {"cmd":"reboot","ok":true,"ts":...}

## Pravidla pojmenování
- Používej malá písmena, čísla, pomlčky (kebab-case)
- Bez mezer a diakritiky
- Délka topicu do 100 znaků
- Device id: např. esp32-<mac3>
- Metric: jednotné číslo, anglicky (temperature, status)

## Význam kanálů
| Kanál | K čemu | Retained | QoS |
|-------|--------|----------|-----|
| tele  | průběžná měření (grafy) | NE | 0 |
| state | pomalé stavy (rssi, battery) | ANO | 0/1 |
| evt   | události (alarm, ack) | NE | 1 |
| cmd   | příkazy k zařízení | n/a | 1 |
| cfg   | konfigurace | ANO | 1 |
| meta  | životní cyklus (ONLINE/OFFLINE) | ANO | 1 |

## Wildcardy a příklady použití
- Všechna tele zařízení: `v1/home/lab/+/tele/#`
- Všechny metriky jednoho zařízení: `v1/home/lab/esp32-01/+/+#`
- Jen teploty: `v1/home/lab/+/tele/temperature`

## Retain & QoS
- Retain = ANO: meta/status, state, cfg
- Retain = NE: tele, evt, cmd
- QoS: tele na 0, cmd/evt/meta/cfg typicky 1

## Payload konvence
- Telemetrie: číslo nebo JSON {"value":22.8,"unit":"°C","ts":...}
- Stav: "ONLINE"/"OFFLINE" (retained)
- Příkaz: prázdný objekt nebo JSON

## Checklist pro Sprint-1
- Zvol org, site, verzi (v1) – zapiš do README
- Device id strategie (esp32-<mac3>)
- Rozhodni payload per metrika: číslo nebo JSON
- Implementuj LWT na .../meta/status (OFFLINE, retained)
- ESP32 publikuje každých 5–10 s na .../tele/<metric>
- Node-RED subscribne v1/home/lab/+/tele/#
- Retained jen na meta/state/cfg, ne na tele

## Další zdroje
- [MQTT topic best practices](https://www.hivemq.com/blog/mqtt-essentials-part-5-mqtt-topics-best-practices/)
