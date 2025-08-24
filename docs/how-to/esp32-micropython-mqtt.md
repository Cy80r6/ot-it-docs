# How-to — ESP32 → MQTT v Pythonu (MicroPython)

**Status:** stable  
**Slug:** how-to/esp32-micropython-mqtt

---

## Cíl
Bez Arduino/C++ a bez kouzel. Načteme MicroPython do ESP32 (včetně varianty ESP32‑S3), připojíme Wi‑Fi a pošleme MQTT zprávy do lokálního Mosquitta s LWT a retain tam, kde to dává smysl.

---

## 0) Co budeme potřebovat
- Windows 11 (funguje i macOS/Linux; příklady níže jsou pro Windows).
- USB kabel s daty (ne „jen nabíjecí“).
- Ovladač USB‑UART: nejčastěji CP210x nebo CH340 (nainstaluj, ať se objeví COM port v Správci zařízení).
- Python 3.10+ (kvůli nástrojům) a balíčky:
  ```sh
  py -m pip install --upgrade esptool mpremote
  ```
- Firmware MicroPython pro tvůj čip:
  - ESP32 (classic/ESP32‑D0WD) → soubor esp32-*.bin
  - ESP32‑S3 → soubor esp32s3-*.bin
  - Stáhni stabilní verzi pro svůj čip; soubor ulož třeba do C:\firmware\esp32\.

---

## 1) Flash MicroPython do ESP32

### Varianta A — Jedním klikem přes Thonny (nejjednodušší)
- Nainstaluj Thonny.
- Připoj ESP32 k PC (u S3 často drž BOOT při zasunutí USB; pokud nic, zkus BOOT podržet a krátce ťuknout EN/RST).
- V Thonny: Run → Select interpreter → MicroPython (ESP32).
- Klikni Install or update MicroPython → vyber správné zařízení (COMx) a firmware pro ESP32 nebo ESP32‑S3 → Install.
- Po instalaci bys měl v Thonny dole vidět MicroPython vX.Y on ... a REPL >>>.

### Varianta B — esptool.py (terminál, plná kontrola)
- Zjisti COM port: Správce zařízení → Porty (COM & LPT) (např. COM7).
- Pozn.: ESP32‑S3 – když se nedaří přepnout do bootloaderu, drž BOOT a klikej krátce EN/RST; případně vyzkoušej jiný USB port/kabel.
- Smaž flash (doporučeno při první instalaci):
  ```sh
  py -m esptool --chip esp32s3 --port COM7 --baud 460800 erase_flash
  ```
- Nahraj firmware (nahraď cestu a název .bin):
  ```sh
  py -m esptool --chip esp32s3 --port COM7 --baud 460800 ^
    write_flash -z 0x0 C:\firmware\esp32\esp32s3-2024xxxx-vx.y.z.bin
  ```
- Pro „ne‑S3“ použij --chip esp32 a odpovídající esp32-*.bin.

---

## 2) První připojení a nahrání souborů

Máme dvě rychlé cesty: Thonny (editor + nahrávání) nebo mpremote (CLI).

### A) Thonny
- Vlevo je Files → přepni na zařízení a nahraj soubory boot.py/main.py.
- Spouští se automaticky boot.py a potom main.py.

### B) mpremote (doporučené pro skriptování)
- Najdi port (např. COM7) a spusť:
  ```sh
  # Shell na zařízení (REPL):
  mpremote connect COM7
  # Nebo přímo nakopíruj soubor:
  mpremote connect COM7 fs cp main.py :main.py
  mpremote connect COM7 reset
  ```
- fs cp <lokální> :<cílové> kopíruje na flash zařízení.

---

## 3) MQTT v MicroPythonu (LWT + retain + telemetrie)

Vytvoř main.py (uprav Wi‑Fi/MQTT proměnné):

```python
# main.py — MicroPython (ESP32 / ESP32‑S3)
import network, time
from umqtt.simple import MQTTClient
import ubinascii, machine

# === Nastavení (uprav) ===
WIFI_SSID   = "<wifi-ssid>"
WIFI_PASS   = "<wifi-pass>"
MQTT_HOST   = "<ip-nebo-host-brokera>"   # např. "192.168.1.100" nebo "raspberrypi"
MQTT_PORT   = 1883
ORG, SITE, DEV = "home", "lab", "esp32-01"
CLIENT_ID   = b"esp32-01"  # musí být unikátní

# === Topics ===
tele = lambda metric: f"v1/{ORG}/{SITE}/{DEV}/tele/{metric}".encode()
meta = lambda key:    f"v1/{ORG}/{SITE}/{DEV}/meta/{key}".encode()

# === Wi‑Fi připojení ===
sta = network.WLAN(network.STA_IF)
sta.active(True)
if not sta.isconnected():
    sta.connect(WIFI_SSID, WIFI_PASS)
    t0 = time.ticks_ms()
    while not sta.isconnected():
        if time.ticks_diff(time.ticks_ms(), t0) > 15000:
            raise RuntimeError("Wi‑Fi timeout")
        time.sleep_ms(200)
print("Wi‑Fi:", sta.ifconfig())

# === MQTT klient ===
status_topic = meta("status")
client = MQTTClient(client_id=CLIENT_ID, server=MQTT_HOST, port=MQTT_PORT, keepalive=60)
client.set_last_will(status_topic, b"OFFLINE", retain=True, qos=0)
client.connect()
client.publish(status_topic, b"ONLINE", retain=True, qos=0)

# === Hlavní smyčka — každých 5 s pošli „teplotu“ ===
try:
    t0 = time.ticks_ms()
    while True:
        # dummy hodnota 20.00–29.99
        v = 20.0 + (time.ticks_ms() % 10000) / 1000.0
        payload = f"{v:.2f}".encode()
        client.publish(tele("temperature"), payload, retain=False, qos=0)
        # udržuj spojení při životě
        client.ping()
        time.sleep(5)
except Exception as e:
    # při pádu necháme broker doručit LWT=OFFLINE
    print("ERR:", e)
    machine.reset()
```

Pozn.: umqtt.simple je standardní lehký klient v MicroPythonu. Má publish(topic, msg, retain=False, qos=0) a set_last_will(topic, msg, retain, qos).

---

## 4) Ověření (MQTT Explorer)
- Připoj se na localhost:1883 (bez TLS, bez user/pass, pokud jedeš LAB profil).
- Subscribe na v1/# nebo přímo v1/home/lab/esp32-01/tele/temperature.
- Po resetu ESP32 uvidíš na .../meta/status hodnotu ONLINE (retained). Když zařízení odpojíš (nebo spadne), broker zveřejní OFFLINE (retained).

---

## 5) Troubleshooting (zkušenosti z praxe)

| Problém | Co zkusit |
|---------|-----------|
| Nevidím COM port | Doinstaluj CP210x/CH340 ovladač; vyměň USB kabel/port; u S3 podrž BOOT při připojení. |
| esptool hází sync chyby | Sniž baud (115200), drž BOOT a ťukni EN/RST, zkontroluj správný --chip a bin pro S3 vs. non‑S3. |
| Thonny neinstaluje firmware | Zvol správný port a typ desky (ESP32 vs. ESP32‑S3), zavři ostatní seriové programy. |
| Wi‑Fi „connected“ ale bez IP | Zkontroluj SSID/heslo a kanál; některé 5 GHz sítě ESP32 neumí (přejdi na 2.4 GHz). |
| MQTT publish „nic nedělá“ | Ověř IP/hostname brokera, port 1883, témata musí sedět; začni s QoS 0. |
| Dvojí připojení → odpojování | Unikátní CLIENT_ID pro každé zařízení. |

---

## 6) Alternativa: ESPHome (YAML, bez kódu)

Když nechceš psát Python, použij ESPHome:
- Vytvoř YAML s Wi‑Fi a mqtt: blokem.
- Přidej senzor (např. sensor: - platform: template ...) a mqtt publikační název.
- První flash přes USB, další už OTA.
- ESPHome je skvělé na rychlé prototypy a domácí automatizaci; MicroPython je svobodnější pro vlastní logiku.

---

## Rychlá karta rozhodnutí

| Chci... | Doporučený postup |
|---------|-------------------|
| Nejrychlejší start bez IDE | Thonny + MicroPython (tento návod) |
| Nižší overhead a přesnou kontrolu nad pamětí | Arduino/C++ |
| Zero‑code konfiguraci a OTA | ESPHome |
