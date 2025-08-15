# Sprint 4 — Základy bezpečnosti OT ↔ IT

!!! info "Cíl sprintu"
    Přidat základní bezpečnostní opatření (TLS, autentizace, VLAN segmentace) do současné architektury.

---

## Backlog → To Do (Sprint 4)

| Kategorie      | Úkol                         | Popis / Akceptační kritéria | Odhad |
|----------------|------------------------------|-----------------------------|-------|
| **Technika**   | MQTT TLS + hesla             | Broker vyžaduje TLS a auth pro přístup | 1 den |
|                | OPC UA SecureChannel         | Povolit šifrování a auth v OPC UA serveru | 0,5 dne |
|                | VLAN segmentace (lab)        | Oddělené VLAN pro OT/IT, ping test | 1 den |
|                | Firewall na Edge             | Otevřené jen nezbytné porty | 0,5 dne |
| **Dokumentace**| How-to: MQTT TLS              | Účel, kroky, ověření, rollback | 0,5 dne |
|                | How-to: OPC UA SecureChannel | Účel, kroky, ověření, rollback | 0,5 dne |
|                | Playbook: OT/IT segmentace   | Postup, schéma, testy | 0,5 dne |

---

## Review kritéria
- MQTT Explorer připojí jen klient s certifikátem/heslem.
- OPC UA klient se připojí jen přes zabezpečený kanál.
- VLAN funguje (OT → IT jen přes Edge).
- Web má 3 nové bezpečnostní návody.

---
