# Sprint 4: Základy bezpečnosti OT ↔ IT (realizace)

Tato stránka popisuje konkrétní realizaci sprintu zaměřeného na základní bezpečnostní opatření v OT/IT laboratoři.

## Cíl
- Zabezpečit komunikaci mezi OT a IT částí pomocí TLS, autentizace a segmentace sítě.

## Realizované kroky

### 1. MQTT broker (Mosquitto) – TLS a hesla
- Vygenerovány certifikáty (self-signed)
- Upraven `mosquitto.conf` pro port 8883, povoleno pouze přihlášení s heslem
- Otestováno připojení klienta s TLS (MQTT Explorer)

### 2. OPC UA SecureChannel
- Na serveru povolena SecurityPolicy `Basic256Sha256`
- Vytvořeny a importovány certifikáty pro server i klienta
- Ověřeno připojení klienta pouze přes zabezpečený kanál

### 3. VLAN segmentace
- Vytvořeny dvě VLAN (OT, IT) na switchi
- Edge gateway nakonfigurována pro routování a firewall
- Ověřeno oddělení provozu (ping, přístup ke službám)

### 4. Firewall na Edge
- Otevřeny pouze nezbytné porty (MQTT, OPC UA, Node-RED dashboard)
- Otestováno blokování ostatních portů

## Dokumentace a návody
- [How-to: Mosquitto Security (TLS, auth, ACL, QoS)](../how-to/mosquitto-security.md)
- [How-to: OPC UA SecureChannel](../how-to/opcua-securechannel.md)
- [Playbook: OT/IT segmentace](../playbooks/ot-it-segmentace.md)

## Výsledky
- MQTT broker a OPC UA server jsou dostupné pouze přes zabezpečené kanály
- IT a OT sítě jsou oddělené, komunikace je řízena pravidly firewallu
- Všechny kroky jsou zdokumentovány v how-to a playbooku
