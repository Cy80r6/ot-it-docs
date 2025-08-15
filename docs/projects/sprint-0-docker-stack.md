# Sprint 0 — Docker stack: Mosquitto + Node-RED

!!! info "Jednou větou"
    Zprovoznění základního stacku Mosquitto a Node-RED v Dockeru na Windows s WSL2.

---

## Kontext
- **Výchozí stav:** Čistý Windows 10/11, bez Dockeru.
- **Cíl:** Mít připravený a ověřený základ pro další sprinty (MQTT, Node-RED) v kontejnerech.
- **Proč teď:** Docker umožní rychlé testování, čisté prostředí a snadné rozšiřování stacku.

---

## Postup (hlavní kroky)

| Krok | Popis | Odkaz na How-to |
|------|-------|-----------------|
| 1 | Instalace Dockeru a Compose | Instalace Dockeru na Windows + WSL2 |
| 2 | První kontejner | docker run hello-world |
| 3 | Vytvoření docker-compose.yml | (viz níže) |
| 4 | Spuštění stacku | docker compose up -d |
| 5 | Ověření běhu Mosquitto a Node-RED | – |
| 6 | Správa kontejnerů | docker ps, docker logs, docker exec |
| 7 | Čištění a rollback | docker compose down, docker volume rm |

---

## Ukázka docker-compose.yml

```yaml
version: "3.9"
services:
  mosquitto:
    image: eclipse-mosquitto:2
    ports:
      - "1883:1883"
    volumes:
      - ./mosquitto/config:/mosquitto/config
      - ./mosquitto/data:/mosquitto/data
      - ./mosquitto/log:/mosquitto/log

  nodered:
    image: nodered/node-red:latest
    ports:
      - "1880:1880"
    volumes:
      - ./nodered/data:/data
```

---

## Ověření
- Mosquitto běží na portu 1883
- Node-RED UI je na http://localhost:1880

---

## Správa kontejnerů
- `docker ps` — seznam běžících kontejnerů
- `docker logs <jméno>` — výpis logů
- `docker exec -it <jméno> sh` — vstup do kontejneru

---

## Čištění
- `docker compose down` — zastavení a odstranění kontejnerů
- `docker volume rm <název>` — odstranění svazků (smazání dat)

---

## Výsledek
- Jedním příkazem (`docker compose up -d`) spustíš Mosquitto + Node-RED.
- Konfigurace je uložená ve svazcích a přežije restart.
- Dokumentace obsahuje instalaci a základy práce s Dockerem.

---

## Rizika / Lessons learned
- Docker na Windows potřebuje WSL2 (nutná aktivace ve Windows Features).
- Při kolizi portů je potřeba změnit mapování v docker-compose.yml.
- Vyčištění starých obrazů (`docker image prune`) a svazků (`docker volume prune`) je nutné čas od času provést.

---

## Další kroky
- Sprint 1: Lokální MQTT tok (ESP32 → Mosquitto → Node-RED dashboard)
- Přidat InfluxDB do stacku ve Sprintu 3.

---

# How-to: Instalace Dockeru na Windows + WSL2

!!! info "Účel"
    Připravit Windows 10/11 systém na běh Docker Engine pomocí Windows Subsystem for Linux verze 2 (WSL2) a nainstalovat Docker Desktop s podporou docker-compose.

## Předpoklady
- Windows 10 (build 19041 a vyšší) nebo Windows 11
- Administrátorský přístup
- Stabilní internetové připojení

---

## Kroky
1. Aktivace WSL2 a Virtual Machine Platform
   ```powershell
   dism.exe /online /enable-feature /featurename:Microsoft-Windows-Subsystem-Linux /all /norestart
   dism.exe /online /enable-feature /featurename:VirtualMachinePlatform /all /norestart
   ```
2. Restart počítače
3. Stažení a instalace jádra WSL2
   - Stáhni z oficiální stránky Microsoftu: WSL2 Linux kernel update package
   - Nainstaluj .msi soubor.
4. Nastavení WSL2 jako výchozí verze
   ```powershell
   wsl --set-default-version 2
   ```
5. Instalace distribuce Linuxu
   - Z Microsoft Store stáhni například Ubuntu 22.04 LTS.
   - Po prvním spuštění nastav uživatelské jméno a heslo.
6. Instalace Docker Desktop
   - Stáhni z https://www.docker.com/products/docker-desktop/
   - Během instalace zaškrtni Use the WSL 2 based engine
   - Po instalaci spusť Docker Desktop a v Settings → Resources → WSL Integration povol svou Linux distribuci.
7. Ověření instalace
   ```powershell
   docker version
   docker compose version
   ```
   - Příkaz `docker run hello-world` by měl zobrazit uvítací zprávu z Dockeru.

---

## Ověření funkčnosti docker-compose
Vytvoř soubor docker-compose.yml:

```yaml
version: "3.9"
services:
  hello:
    image: hello-world
```

Spusť:
```powershell
docker compose up
```
Očekávej výpis „Hello from Docker!“.

---

## Rollback / Odinstalace
- Odinstaluj Docker Desktop z Nastavení → Aplikace
- Odinstaluj Linux distribuci přes Microsoft Store → Moje knihovna
- Vypni WSL a Virtual Machine Platform:
   ```powershell
   dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux
   dism.exe /online /disable-feature /featurename:VirtualMachinePlatform
   ```

---

## Lessons learned
- WSL2 je rychlejší než starší Hyper-V backend a má nižší nároky na RAM.
- Pokud máš více Linux distribucí, v Docker Desktop nastav, která má běžet s Dockerem.
- Na slabších strojích pomůže snížení přidělené RAM a CPU v Settings → Resources.
