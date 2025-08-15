
## `docs/how-to/instalace-mosquitto.md`
```markdown
# Instalace Mosquitto (lokální broker)

## Účel
Spustit lokální MQTT broker pro testy a lab scénáře.

## Předpoklady
- Windows 11 (admin práva)
- Volný port 1883
- [MQTT Explorer](https://mqtt-explorer.com/) nainstalovaný pro testy

## Kroky
1. Stáhni [Eclipse Mosquitto pro Windows](https://mosquitto.org/download/).
2. Nainstaluj do `C:\mosquitto`.
3. Vytvoř `C:\mosquitto\mosquitto.conf` s obsahem:
    ```
    listener 1883
    allow_anonymous true
    persistence true
    persistence_location C:\mosquitto\data\
    log_type all
    ```
4. Spusť broker:
    ```powershell
    "C:\mosquitto\mosquitto.exe" -c "C:\mosquitto\mosquitto.conf"
    ```

## Ověření
- V MQTT Explorer připoj na `localhost:1883`.
- `Publish` na topic `lab/test` payload `{"hello":"world"}`.
- Přes `Subscribe` na `lab/#` uvidíš zprávu.

## Rollback
- Ukonči běžící proces Mosquitto.
- Smaž instalační složku.
