

# Sprint 0 — Virtuální prostředí (Docker)

!!! info "Cíl sprintu"
  Mít připravené izolované prostředí pro OT/IT lab, postavené na Dockeru a docker-compose. Všechny další sprinty poběží uvnitř kontejnerů, aby se hostitelský systém nezanášel zbytečnými instalacemi.

---

## Backlog (To Do)

| Kategorie      | Úkol | Popis / Akceptační kritéria | Odhad |
|----------------|------|-----------------------------|-------|
| **Technika**   | Instalace Docker Engine + Compose | Docker běží, příkaz `docker ps` funguje | 0,5 dne |
|                | Seznámení se s příkazy (`run`, `ps`, `logs`, `exec`) | Umíš spustit kontejner z obrazu, podívat se dovnitř a zastavit ho | 0,5 dne |
|                | Vytvoření `docker-compose.yml` pro Mosquitto + Node-RED | Soubor definující oba kontejnery, start jedním příkazem | 0,5 dne |
|                | Persistentní svazky pro konfiguraci | Po restartu běží kontejnery se stejným nastavením | 0,5 dne |
| **Dokumentace**| How-to: Instalace Docker + Compose | Struktura: účel, předpoklady, kroky, ověření, rollback | 0,5 dne |
|                | How-to: Základní práce s kontejnery | Příklady příkazů, tipy na čištění | 0,5 dne |
|                | Project page: Docker base env | Kontext, architektura, co pokrývá | 0,5 dne |

---

## Pravidla práce (WIP)

- Nejprve ověř, že Docker a docker-compose fungují.
- Každý krok si ověř na vlastním stroji, až pak pokračuj dál.
- Dokumentační úkol přidej hned po dokončení technického kroku.

---

## Kritéria pro review

- Docker Desktop je nainstalovaný a funkční (ověřeno příkazem `docker run hello-world`).
- docker-compose spustí stack Mosquitto + Node-RED bez chyb.
- Kontejnery běží, lze se připojit na Mosquitto (port 1883) a Node-RED (port 1880).
- V Gitu je commitnutý docker-compose.yml a základní dokumentace.

---

## Rizika

- Problémy s WSL2 nebo Virtual Machine Platform na Windows.
- Kolize portů na hostitelském systému.
- Zapomenuté čištění svazků a obrazů může zabírat místo na disku.

---

## Kanban (vizualizace)

```mermaid
kanban
    section To Do
      Instalace Docker Engine + Compose
      Seznámení se s příkazy (run, ps, logs, exec)
      Vytvoření docker-compose.yml pro Mosquitto + Node-RED
      Persistentní svazky pro konfiguraci
      How-to: Instalace Docker + Compose
      How-to: Základní práce s kontejnery
      Project page: Docker base env
    section Doing
      # (zatím prázdné)
    section Done
      # (zatím prázdné)
