
# Sprint 0 — Docker stack: Mosquitto + Node-RED

## 1. Instalace Dockeru na Windows + WSL2

!!! info "Účel"
    Připravit Windows 10/11 systém na běh Docker Engine pomocí Windows Subsystem for Linux verze 2 (WSL2) a nainstalovat Docker Desktop s podporou docker-compose.

### Předpoklady
- Windows 10 (build 19041 a vyšší) nebo Windows 11
- Administrátorský přístup
- Stabilní internetové připojení


### Kroky (rychlá varianta)
1. Pro většinu uživatelů stačí spustit v PowerShellu jediný příkaz, který provede kroky 1–8 najednou:
   ```powershell
   wsl --install
   ```
   Tento příkaz automaticky aktivuje potřebné Windows komponenty, stáhne a nastaví WSL2, nainstaluje výchozí Linux distribuci (např. Ubuntu) a provede restart počítače.

2. Po restartu spusť nově nainstalovanou Linux distribuci (např. Ubuntu) a nastav uživatelské jméno a heslo.

3. Stáhni a nainstaluj Docker Desktop:
   - https://www.docker.com/products/docker-desktop/
   - Během instalace zaškrtni "Use the WSL 2 based engine".
   - Po instalaci spusť Docker Desktop a v Settings → Resources → WSL Integration povol svou Linux distribuci.

4. Ověření instalace:
   ```powershell
   docker version
   docker compose version
   ```
   - Příkaz `docker run hello-world` by měl zobrazit uvítací zprávu z Dockeru.

---

> **Poznámka:** Pokud potřebujete detailní ruční postup (například pro firemní prostředí nebo při chybě automatické instalace), použijte rozšířený návod v předchozích verzích dokumentace nebo na webu Microsoftu.

### Ověření funkčnosti docker-compose
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

### Rollback / Odinstalace
- Odinstaluj Docker Desktop z Nastavení → Aplikace
- Odinstaluj Linux distribuci přes Microsoft Store → Moje knihovna
- Vypni WSL a Virtual Machine Platform:
   ```powershell
   dism.exe /online /disable-feature /featurename:Microsoft-Windows-Subsystem-Linux
   dism.exe /online /disable-feature /featurename:VirtualMachinePlatform
   ```


### GPU podpora v Dockeru (volitelné)

Pokud máš v PC NVIDIA GPU a chceš ji využít v kontejnerech (např. pro AI nebo video processing), je potřeba:

1. Mít nainstalované aktuální NVIDIA ovladače (pro Windows 11).
2. Používat Docker Desktop s WSL2 backendem.
3. Ve WSL distribuci (např. Ubuntu) nainstalovat **NVIDIA Container Toolkit**:
  ve windows v powershellu:
  ```powershell
  wsl
  ```
  v ubuntu v bash:
  ```bash
  sudo apt update
  sudo apt install -y nvidia-container-toolkit
  sudo systemctl restart docker
  ```
4. V Docker Desktop v Settings → Resources → GPU povolit použití karty.
5. Otestovat pomocí:
  ```bash
  docker run --rm --gpus all nvidia/cuda:12.6.2-base-ubuntu22.04 nvidia-smi
  ```

> **Poznámka:** Pro detailní návod viz oficiální dokumentaci Dockeru a NVIDIA (https://docs.nvidia.com/cuda/wsl-user-guide/index.html).

### Lessons learned
- WSL2 je rychlejší než starší Hyper-V backend a má nižší nároky na RAM.
- Pokud máš více Linux distribucí, v Docker Desktop nastav, která má běžet s Dockerem.
- Na slabších strojích pomůže snížení přidělené RAM a CPU v Settings → Resources.

---

## 2. Zprovoznění Mosquitto + Node-RED stacku v Dockeru

!!! info "Jednou větou"
    Zprovoznění základního stacku Mosquitto a Node-RED v Dockeru na Windows s WSL2.

### Kontext
- **Výchozí stav:** Docker Desktop a WSL2 jsou nainstalované a funkční.
- **Cíl:** Mít připravený a ověřený základ pro další sprinty (MQTT, Node-RED) v kontejnerech.
- **Proč teď:** Docker umožní rychlé testování, čisté prostředí a snadné rozšiřování stacku.

### Postup (hlavní kroky)

| Krok | Popis | Odkaz na How-to |
|------|-------|-----------------|
| 1 | Vytvoření docker-compose.yml | (viz níže) |
| 2 | Spuštění stacku | docker compose up -d |
| 3 | Ověření běhu Mosquitto a Node-RED | – |
| 4 | Správa kontejnerů | docker ps, docker logs, docker exec |
| 5 | Čištění a rollback | docker compose down, docker volume rm |

### Ukázka docker-compose.yml

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

### Ověření
- Mosquitto běží na portu 1883
- Node-RED UI je na http://localhost:1880

### Správa kontejnerů
- `docker ps` — seznam běžících kontejnerů
- `docker logs <jméno>` — výpis logů
- `docker exec -it <jméno> sh` — vstup do kontejneru

### Čištění
- `docker compose down` — zastavení a odstranění kontejnerů
- `docker volume rm <název>` — odstranění svazků (smazání dat)

### Výsledek
- Jedním příkazem (`docker compose up -d`) spustíš Mosquitto + Node-RED.
- Konfigurace je uložená ve svazcích a přežije restart.
- Dokumentace obsahuje instalaci a základy práce s Dockerem.

### Rizika / Lessons learned
- Docker na Windows potřebuje WSL2 (nutná aktivace ve Windows Features).
- Při kolizi portů je potřeba změnit mapování v docker-compose.yml.
- Vyčištění starých obrazů (`docker image prune`) a svazků (`docker volume prune`) je nutné čas od času provést.

---

## Další kroky
- Sprint 1: Lokální MQTT tok (ESP32 → Mosquitto → Node-RED dashboard)
- Přidat InfluxDB do stacku ve Sprintu 3.
