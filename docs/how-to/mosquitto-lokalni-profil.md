# How-to — Mosquitto: lokální vývojový profil

**Status:** stable  
**Slug:** how-to/mosquitto-lokalni-profil

---

## Cíl
Lokální broker akceptuje připojení z hostitele i ESP32 v LAN bez TLS, volitelně s heslem. Minimální nastavení: listener 1883, persistence on, log do souboru, allow_anonymous (jen pro lab). Vzor i typické chybové stavy navazují na Sprint‑0.

---

## Předpoklady
- Běží Docker Desktop + WSL2 (viz Sprint‑0 – Docker stack).
- Složka projektu obsahuje `docker-compose.yml` pro Mosquitto + Node‑RED.

### Struktura adresářů
```
.
├─ docker-compose.yml
└─ mosquitto/
   ├─ config/
   │  └─ mosquitto.conf
   ├─ data/
   └─ log/
```

Vytvoř složky:
```sh
mkdir -p mosquitto/config mosquitto/data mosquitto/log
```

---

## Minimální mosquitto.conf (LAB)
```conf
# Poslouchej na všech rozhraních na portu 1883
listener 1883

# Povolit anonymní přístup jen pro lokální vývoj / lab!
allow_anonymous true

# Perzistence a logy (dirs mapují volumes v docker-compose)
persistence true
persistence_location /mosquitto/data/
log_timestamp true
log_dest file /mosquitto/log/mosquitto.log

# (Volitelné) Delší keepalive pro chatrné Wi‑Fi
max_keepalive 120
```

Bezpečnější profil (TLS, ACL, password_file) přidáš ve Sprintu 4.

---

### (Volitelně) Heslo uživatele
```conf
# místo allow_anonymous: false a password_file
allow_anonymous false
password_file /mosquitto/config/passwords
```

Vytvoření souboru hesel (uvnitř kontejneru nebo lokálně a namapovat):
```sh
docker exec -it <mosquitto-container> sh -c "mosquitto_passwd -c /mosquitto/config/passwords dev && chmod 600 /mosquitto/config/passwords"
```

---

## docker-compose.yml — relevantní část
```yaml
services:
  mosquitto:
    image: eclipse-mosquitto:2
    ports:
      - "1883:1883"
    volumes:
      - ./mosquitto/config:/mosquitto/config
      - ./mosquitto/data:/mosquitto/data
      - ./mosquitto/log:/mosquitto/log
```

Pokud config chybí nebo je prázdný, broker padá do local only mode nebo nenastartuje.

---

## Start a ověření
```sh
docker compose down
docker compose up -d
docker compose logs -f mosquitto
```
V logu nesmí být „Starting in local only mode…“.

Na Windows otestuj klientem z WSL i z hostitele.

---

## Rychlé testy (vyber jeden)

### A) MQTT Explorer (Windows app)
- Subscribe na `test/topic` → 2) Publish na `test/topic` s payloadem `hello`.

### B) WSL klienty
```sh
sudo apt-get update && sudo apt-get install -y mosquitto-clients
mosquitto_sub -h localhost -t test/topic &
mosquitto_pub -h localhost -t test/topic -m "ahoj z WSL"
```

### C) Z vedlejšího kontejneru
```sh
docker run --rm --network $(basename "$PWD")_default eclipse-mosquitto:2 \
  mosquitto_pub -h mosquitto -t test/topic -m "ahoj z kontejneru"
```

---

## Typické chybové stavy (rychlá diagnostika)
| Příznak | Pravděpodobná příčina | Oprava |
|---------|----------------------|--------|
| Starting in local only mode v logu | Nenalezen validní config, mapuješ prázdný ./mosquitto/config | Vytvoř mosquitto.conf, restartuj stack |
| Error: Unable to open config file | Špatná cesta/volume, chybí soubor | Zkontroluj volume a existenci souboru |
| ESP32 se připojí, ale zprávy „nikde“ | Subscribe na jiný topic než publikuješ; QoS 1/2 bez ACK | Sjednoť topic, začni na QoS 0 |
| Address already in use při startu | Port 1883 už používá jiný proces | Změň mapování portu nebo ukonči kolizní službu |
| MQTT Explorer se nepřipojí | TLS zapnuto v klientu, ale broker běží bez TLS | Vypni TLS/validate v klientu |

---

## Cleanup

Stačí zastavit stack a smazat data/logy podle potřeby.
