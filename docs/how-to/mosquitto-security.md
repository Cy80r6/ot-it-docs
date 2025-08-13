
# Mosquitto: TLS, auth, ACL, QoS
--8<-- "includes/abbr.md"

**Účel:** Zabezpečit MQTT broker pro provozní data.  
**Předpoklady:** Mosquitto, základní znalost TLS certifikátů.  
**Odhad času:** 60–90 min.

## Kroky
1. Vytvoř uživatele a hesla (`mosquitto_passwd`).
2. Vygeneruj TLS certifikáty (CA, server, klient volitelně).
3. Nastav `mosquitto.conf` pro port 8883, `cafile`, `certfile`, `keyfile`, `allow_anonymous false`.
4. Vytvoř **ACL**: role `publisher-edge`, `viewer-dashboard` se specifickými topiky.
5. Otestuj připojení klienta a publikaci/subscribe.

## Ověření
- Klient se přihlásí jen přes TLS a správnou roli.
- Neautorizované topiky vrací chybu.

## Rollback
- Zálohuj původní `mosquitto.conf` a vrať jej zpět.
