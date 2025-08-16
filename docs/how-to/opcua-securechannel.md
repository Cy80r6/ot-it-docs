# How-to: OPC UA SecureChannel

Návod na zabezpečení komunikace OPC UA serveru pomocí SecureChannel (šifrování a autentizace).

## Předpoklady
- Běžící OPC UA server (např. Prosys, open62541, nebo v Node-RED)
- OPC UA klient (např. UaExpert)

## Postup
1. **Povolit šifrování na serveru** – v konfiguraci serveru nastav SecurityPolicy (např. `Basic256Sha256`).
2. **Vytvoř nebo importuj certifikáty** pro server i klienta.
3. **Nastav režim komunikace** na `Sign & Encrypt`.
4. **Přidej certifikát klienta do důvěryhodných na serveru** (a naopak, pokud je vyžadováno).
5. **Restartuj server a klienta**.
6. **Připoj klienta** a ověř, že komunikace probíhá přes zabezpečený kanál.

## Ověření
- Klient se připojí pouze přes zabezpečený kanál (Sign & Encrypt)
- Nezabezpečené spojení je odmítnuto

## Rollback
- V konfiguraci serveru nastav zpět SecurityPolicy na `None` a restartuj server
