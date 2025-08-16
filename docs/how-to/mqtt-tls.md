# How-to: MQTT TLS (Mosquitto)

Tento návod popisuje, jak zabezpečit Mosquitto broker pomocí TLS/SSL certifikátů.

## Předpoklady
- Běžící Mosquitto broker (např. v Dockeru)
- Základní znalost práce s příkazovou řádkou

## Postup
1. **Vygeneruj certifikáty** (self-signed nebo od CA):
   ```shell
   openssl req -x509 -newkey rsa:4096 -keyout mosquitto.key -out mosquitto.crt -days 365 -nodes
   ```
2. **Ulož certifikáty** do složky, kterou mountuješ do kontejneru Mosquitto (např. `./config/certs/`).
3. **Uprav konfiguraci Mosquitto** (`mosquitto.conf`):
   ```
   listener 8883
   cafile /mosquitto/config/certs/mosquitto.crt
   certfile /mosquitto/config/certs/mosquitto.crt
   keyfile /mosquitto/config/certs/mosquitto.key
   require_certificate false
   ```
4. **Restartuj Mosquitto** a ověř, že port 8883 je otevřený.
5. **Připoj klienta** (např. MQTT Explorer) s TLS povoleným.

## Ověření
- Klient se připojí pouze přes TLS (port 8883)
- Nezabezpečené připojení na 1883 lze zakázat v konfiguraci

## Rollback
- Vrať změny v `mosquitto.conf` a restartuj broker
