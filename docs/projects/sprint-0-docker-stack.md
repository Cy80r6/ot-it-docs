
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


### Ověření funkčnosti docker-compose a GPU

#### 1. Ověření orchestrátoru (Hello world)

Nejprve si vytvoř složku pro testování Docker Compose, například:

```powershell
mkdir ~/docker-tests
cd ~/docker-tests
```

V této složce vytvoř soubor `docker-compose.yml` s tímto obsahem:

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

Pokud se zobrazí zpráva z hello-world kontejneru, Docker Compose funguje správně.

> **Poznámka:** docker compose hledá soubor `docker-compose.yml` v aktuálním adresáři, odkud příkaz spouštíš. Pro každý projekt je vhodné mít vlastní složku.

#### 2. Ověření GPU podpory (volitelné)

Pokud máš v PC NVIDIA GPU a chceš ji využívat v kontejnerech:

- Nainstaluj NVIDIA Container Toolkit (viz sekce výše ve WSL distribuci).
- Restartuj Docker Desktop.
- Spusť testovací kontejner:

```powershell
docker run --rm --gpus all nvidia/cuda:12.6.2-base-ubuntu22.04 nvidia-smi
```

Pokud se ve výstupu objeví tvoje karta (např. GeForce RTX 4070 Ti), Docker správně používá lokální GPU.

---

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
  curl -fsSL https://nvidia.github.io/libnvidia-container/gpgkey \
   | sudo gpg --dearmor -o /usr/share/keyrings/nvidia-container-toolkit.gpg

  curl -fsSL https://nvidia.github.io/libnvidia-container/stable/deb/nvidia-container-toolkit.list \
   | sed 's#deb https://#deb [signed-by=/usr/share/keyrings/nvidia-container-toolkit.gpg] https://#' \
   | sudo tee /etc/apt/sources.list.d/nvidia-container-toolkit.list >/dev/null

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


### Ověření běhu Mosquitto a Node-RED

1. **Zkontroluj běžící kontejnery:**

   ```powershell
   docker ps
   ```
   Měl bys vidět oba kontejnery, např.:
   
   | CONTAINER ID | IMAGE                  | STATUS | PORTS                       | NAMES                       |
   |--------------|------------------------|--------|-----------------------------|-----------------------------|
   | ...          | eclipse-mosquitto:2    | Up ... | 0.0.0.0:1883->1883/tcp      | ...mosquitto...             |
   | ...          | nodered/node-red:latest| Up ... | 0.0.0.0:1880->1880/tcp      | ...nodered...                |

2. **Ověření Mosquitta (MQTT broker):**

   - Nainstaluj si MQTT klient (např. mosquitto-clients ve WSL nebo MQTT Explorer ve Windows).
   - Otevři dvě terminálová okna:

     V prvním spusť subscriber:
     ```bash
     mosquitto_sub -h localhost -t test
     ```
     (čeká na zprávy na topicu "test")

     Ve druhém okně publikuj zprávu:
     ```bash
     mosquitto_pub -h localhost -t test -m "ahoj"
     ```
     Pokud se zpráva objeví v prvním okně, Mosquitto běží správně.

   - Pokud Mosquitto neběží, zkontroluj logy:
     ```powershell
     docker compose logs mosquitto
     ```


     Nejčastější problém: chybějící nebo nefunkční config soubor v `./mosquitto/config`. Co se děje:

     - Pokud máš volume `./mosquitto/config:/mosquitto/config` a adresář je prázdný, Mosquitto image nenajde žádný config a broker se nespustí (i když kontejner v `docker ps` vypadá jako "Up").
     - V logu pak uvidíš chybu `Error: Unable to open config file` nebo "Starting in local only mode..." (povoleno jen pro localhost).

     **Možnosti řešení:**

     **a) Rychlý start bez vlastního configu:**
     - Dočasně zakomentuj nebo odstraň řádek s volume pro config v `docker-compose.yml`:
       ```yaml
       # - ./mosquitto/config:/mosquitto/config
       ```
     - Spusť stack znovu:
       ```powershell
       docker compose down
       docker compose up -d
       ```
     - Mosquitto pak použije vestavěný default config (ale povolí jen přístup z localhost uvnitř kontejneru).

     **b) Vytvoř si vlastní minimální config:**
     - Vytvoř složku `mosquitto/config` vedle svého `docker-compose.yml`.
     - Do souboru `mosquitto/config/mosquitto.conf` vlož například:
       ```
       # Poslouchej na všech rozhraních na portu 1883
       listener 1883

       # Povolit připojení bez autentizace (jen na testy!)
       allow_anonymous true

       # Ulož data a logy (adresáře mapuješ v docker-compose.yml)
       persistence true
       persistence_location /mosquitto/data/

       log_dest file /mosquitto/log/mosquitto.log
       ```
     - Ujisti se, že v `docker-compose.yml` je volume pro config odkomentovaný:
       ```yaml
       - ./mosquitto/config:/mosquitto/config
       ```
     - Spusť stack znovu:
       ```powershell
       docker compose down
       docker compose up -d
       ```
     - V logu už by neměla být hláška "local only mode" a MQTT Explorer se připojí z hosta na `localhost:1883`.

     > **Poznámka:** V základním image Mosquitta není žádné výchozí jméno/heslo. Pokud chceš zabezpečit přístup, musíš si v configu nastavit password_file a další parametry (viz dokumentace Mosquitta).


3. **Ověření Node-RED:**

   - Otevři prohlížeč a přejdi na:
     [http://localhost:1880](http://localhost:1880)
   - Pokud se načte Node-RED UI, vše funguje.

---

### Praktický testovací flow: MQTT Explorer, WSL2 a Docker klient

#### Co je co?
- **Broker (Mosquitto)** běží v Dockeru a poslouchá na localhost:1883 (díky `ports: 1883:1883`).
- **MQTT Explorer (Windows app)** je klient. Připojuje se na localhost:1883.
- Publikovat můžeš buď přímo z MQTT Exploreru, nebo z WSL2 (nebo z dalšího kontejneru).

#### Nejkratší funkční testy (vyber si jeden):

**A) Všechno jen v MQTT Exploreru (nejjednodušší):**
1. V MQTT Exploreru klikni na Subscribe na topic `test/topic`.
2. Vpravo dole Publish na stejný topic `test/topic`, payload třeba `hello`.
3. Zpráva se hned objeví vlevo. Hotovo.

**B) Publikace z WSL2 (když chceš mít jistotu „z jiného místa“):**
1. Ve WSL spusť:
   ```bash
   sudo apt-get update
   sudo apt-get install -y mosquitto-clients
   mosquitto_pub -h localhost -t test/topic -m "ahoj z WSL"
   ```
2. V MQTT Exploreru (subscribed na `test/topic`) uvidíš zprávu.
3. (Volitelně) Otevři druhé WSL okno a spusť subscriber:
   ```bash
   mosquitto_sub -h localhost -t test/topic
   ```
   a v Exploreru klikni Publish → uvidíš to v terminálu.

**C) Publikace z Dockeru (bez instalace klientů do WSL):**
1. Z adresáře s docker-compose.yml spusť:
   ```powershell
   docker run --rm --network mqtt_node-red_default eclipse-mosquitto:2 \
     mosquitto_pub -h mosquitto -t test/topic -m "ahoj z vedlejšího kontejneru"
   ```
   (Síť `mqtt_node-red_default` se jmenuje podle tvé složky; ověř příkazem `docker network ls`.)
2. V MQTT Exploreru zprávu uvidíš.

#### Když to stále neukáže zprávy:
- V `mosquitto.conf` musíš mít aspoň:
  ```
  listener 1883
  allow_anonymous true
  ```
- Po úpravě configu vždy restartuj stack:
  ```powershell
  docker compose down
  docker compose up -d
  ```
- Ověř porty a stav:
  ```powershell
  docker ps
  docker compose logs mosquitto
  ```
- V logu nesmí být „local only mode…“.
- V MQTT Exploreru: Host `localhost`, Port `1883`, TLS/Validate vypnuto, bez user/pass.

---

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
