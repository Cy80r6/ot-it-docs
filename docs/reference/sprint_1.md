

# Sprint 1 — Lokální MQTT tok (šablona)

!!! info "Cíl sprintu"
    Mít funkční lokální MQTT tok (senzor → broker → Node-RED dashboard) a k tomu první sadu dokumentace ve webu.

---

## Backlog (To Do)

| Kategorie      | Úkol                         | Popis / Akceptační kritéria | Odhad |
|----------------|------------------------------|-----------------------------|-------|
| **Technika**   | Studijní blok: TCP/IP a VLAN | Umíš vysvětlit IP adresu, masku, gateway, VLAN segmentaci; nakreslíš jednoduchý topologický diagram | 0,5 dne |
|                | MQTT broker (Mosquitto) v Dockeru | Nainstalovaný a běžící broker, test přes MQTT Explorer (publish/subscribe) | 0,5 dne |
|                | Instalace Node-RED v Dockeru | Node-RED běží v kontejneru, přístup přes web UI | 0,5 dne |
|                | Node-RED základní flow      | Input → transformace → výstup (MQTT subscribe → dashboard widget) | 1 den |
|                | Test ESP32 → MQTT           | Senzor (např. teplota) posílá JSON do tématu `test/sensor/temperature` | 1 den |
| **Dokumentace**| [How-to: Instalace Mosquitto](../how-to/instalace-mosquitto.md)  | Struktura: účel, předpoklady, kroky, ověření, rollback | 0,5 dne |
|                | [How-to: Instalace Node-RED](../how-to/instalace-node-red.md) | Struktura: účel, předpoklady, kroky, ověření, rollback | 0,5 dne |
|                | [How-to: MQTT Explorer](../how-to/mqtt-explorer.md) | Testování publish/subscribe | 0,5 dne |
|                | ADR: Volba stacku           | Proč Docker Compose, alternativy, důsledky | 0,5 dne |
|                | Project page: OT-IT Docs Web| Kontext, architektura, dosavadní postup, co je hotovo | 0,5 dne |

---

## Pravidla práce (WIP)

- Max 2 technické úkoly rozdělané najednou.
- Dokumentační úkol přidat hned po dokončení technického kroku, dokud máš detaily čerstvé.

---

## Kritéria pro review

- MQTT Explorer ukazuje zprávy z ESP32 v reálném čase.
- Node-RED dashboard má min. 1 graf a 1 textový widget s daty z MQTT.
- Na webu jsou min. 3 nové stránky (How-to, ADR, Project) a prošly rychlou kontrolu (obsah, formát, odkazy fungují).
- Vše běží v Docker Compose stacku.

---

## Rizika

- Zaseknutí na síťové konfiguraci → drž se jednoduchého topologie (vše v jedné LAN) pro první test.
- MQTT bezpečnost zatím řešit jen heslem, TLS až v dalším sprintu.

---

## Kanban (vizualizace)

```mermaid
kanban
    section To Do
      Studijní blok: TCP/IP a VLAN
      MQTT broker (Mosquitto) v Dockeru
      Instalace Node-RED v Dockeru
      Node-RED základní flow
      Test ESP32 → MQTT
      How-to: Instalace Mosquitto
      How-to: Instalace Node-RED
      How-to: MQTT Explorer
      ADR: Volba stacku
      Project page: OT-IT Docs Web
    section Doing
      # (zatím prázdné)
    section Done
      # (zatím prázdné)
```

---

> Konkrétní realizace sprintu najdeš v sekci [Projects](../projects/).
