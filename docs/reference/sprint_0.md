# Sprint 0 — Virtuální prostředí (Docker)

!!! info "Cíl sprintu"
    Mít připravené izolované prostředí pro OT/IT lab, postavené na Dockeru a docker-compose.  
    Všechny další sprinty poběží uvnitř kontejnerů, aby se hostitelský systém nezanášel zbytečnými instalacemi.

---

## Kontext
- **Proč teď:**  
  Před prvním technickým sprintem je vhodné oddělit testovací nástroje a služby od čistého hostitelského systému.  
  Docker poskytuje rychlou a lehkou izolaci bez nutnosti provozovat plnohodnotnou virtuální mašinu.
- **Cíl:**  
  Umět nainstalovat Docker + docker-compose, spouštět základní kontejnery a připravit minimální stack pro Sprint 1 (Mosquitto + Node-RED).
- **Výhody:**  
  - Snadná replikace prostředí na jiném počítači
  - Rychlý úklid (`docker rm`, `docker volume rm`)
  - Možnost verzovat konfiguraci prostředí v Gitu

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

## Architektura

```mermaid
flowchart LR
    HostPC -->|Docker Engine| Containers
    subgraph Containers
        Mosquitto[(MQTT broker)]
        NodeRED[Node-RED]
    end
