

# How-to: MQTT Security (Mosquitto: TLS, auth, ACL, QoS)

Tento návod popisuje, jak zabezpečit Mosquitto broker pomocí TLS/SSL certifikátů, uživatelských hesel a ACL.

## Předpoklady
- Běžící Mosquitto broker (např. v Dockeru)
- Základní znalost práce s příkazovou řádkou a TLS certifikátů

## Postup
1. **Vytvoř uživatele a hesla**
	```shell
	mosquitto_passwd -c mosquitto.passwd uzivatel
	```
	V konfiguraci nastav:
	```
	password_file /mosquitto/config/mosquitto.passwd
	allow_anonymous false
	```
2. **Vygeneruj TLS certifikáty** (self-signed nebo od CA):
	```shell
	openssl req -x509 -newkey rsa:4096 -keyout mosquitto.key -out mosquitto.crt -days 365 -nodes
	```
	Ulož certifikáty do složky, kterou mountuješ do kontejneru Mosquitto (např. `./config/certs/`).
3. **Uprav konfiguraci Mosquitto** (`mosquitto.conf`):
	```
	listener 8883
	cafile /mosquitto/config/certs/mosquitto.crt
	certfile /mosquitto/config/certs/mosquitto.crt
	keyfile /mosquitto/config/certs/mosquitto.key
	require_certificate false
	```
4. **Vytvoř ACL** (Access Control List):
	- Definuj role (např. `publisher-edge`, `viewer-dashboard`) a povolené topiky v souboru `aclfile`.
	- V konfiguraci přidej:
	```
	acl_file /mosquitto/config/aclfile
	```
5. **Restartuj Mosquitto** a ověř, že port 8883 je otevřený a připojení vyžaduje TLS a heslo.
6. **Otestuj připojení klienta** (např. MQTT Explorer) s TLS a správnými přihlašovacími údaji.

## Ověření
- Klient se připojí pouze přes TLS (port 8883) a s platným uživatelským jménem/heslem
- Neautorizované topiky vrací chybu podle ACL
- Nezabezpečené připojení na 1883 lze zakázat v konfiguraci

## Rollback
- Zálohuj původní `mosquitto.conf` a vrať jej zpět
- Smaž nebo obnov soubory s hesly a certifikáty
