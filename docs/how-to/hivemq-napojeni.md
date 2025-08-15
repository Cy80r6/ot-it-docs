# How-to: HiveMQ napojení

!!! info "Účel"
    Připojit lokální MQTT broker k HiveMQ Cloud a přenášet vybraná témata do cloudu.

## Předpoklady
- Účet na HiveMQ Cloud
- Přístup k lokálnímu MQTT brokeru (Mosquitto)

---

## Kroky
1. Vytvoř účet a instanci na HiveMQ Cloud.
2. Získej přihlašovací údaje (host, port, user, password).
3. Nastav forwarding v Mosquitto (`bridge` sekce v mosquitto.conf).
4. Ověř připojení a přenos zpráv do HiveMQ Cloud.

---

## Ověření
- Zprávy z lokálního MQTT jsou vidět v HiveMQ Cloud klientu.

---

## Rollback
- Odstraň bridge konfiguraci v mosquitto.conf.
- Restartuj Mosquitto.
