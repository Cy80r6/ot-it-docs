# Sprint 1 — Lokální MQTT tok + první dokumentace

!!! info "Jednou větou"
    První funkční řetězec **ESP32 → MQTT broker → Node-RED dashboard**  
    a začátek jednotné dokumentace v OT/IT Docs webu.

---

## Kontext
- **Výchozí stav:** Web běží na MkDocs Material + GitHub Pages, CI/CD hotové.
- **Cíl:** Naučit se základy sběru a zobrazení dat (MQTT, Node-RED, ESP32) a u toho zavést standard psaní How-to.
- **Proč teď:** To, co vybudujeme, je základ pro všechny další sprinty – bez funkčního toku dat nemá analytika ani bezpečnost na co navazovat.

---

## Architektura

```mermaid
flowchart LR
    ESP32 -->|MQTT publish| Mosquitto[(Mosquitto broker)]
    Mosquitto -->|subscribe| NodeRED[Node-RED]
    NodeRED -->|dashboard| WebUI[Dashboard /ui]


---

## Postup (hlavní kroky)

| Krok | Popis | Odkaz na How-to |
|------|-------|-----------------|
| 1 | Studijní blok: TCP/IP & VLAN – základní pojmy, topologie, návrh topic namespace | – |
| 2 | Instalace Mosquitto – lokální broker na portu 1883 | Instalace Mosquitto |
| 3 | Instalace Node-RED – spuštění editoru a UI | Instalace Node-RED |
| 4 | Základní flow v Node-RED – MQTT in → Function → ui_chart/ui_text | Node-RED: základní flow |
| 5 | ESP32 → MQTT – jednoduchý sketch s PubSubClient | ESP32 → MQTT |
| 6 | Dokumentace – How-to, ADR 0001, Project page | ADR 0001 – Volba stacku |

---

## Výsledek

- MQTT Explorer zobrazuje data z ESP32 v reálném čase.
- Node-RED dashboard má 1 graf a 1 textový widget.
- Web obsahuje 3 nové stránky: How-to Mosquitto, ADR stack, Project Sprint 1.

---

## Rizika / Lessons learned

- Síťové nastavení: začít v jedné LAN, VLAN řešit až později.
- MQTT bezpečnost: pro lab stačí heslo, TLS přijde ve Sprintu 4.
- Disciplína v dokumentaci: každý krok má vlastní How-to, odkazy z Project page.

---

## Další kroky

- Sprint 2: OPC UA simulátor, bridge do MQTT.
- Rozšířit dashboard o historická data z InfluxDB.