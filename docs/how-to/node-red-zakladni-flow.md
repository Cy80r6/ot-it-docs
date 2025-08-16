# How-to: Node-RED – základní flow

Tento návod ukazuje, jak vytvořit základní flow v Node-RED pro příjem dat z MQTT a jejich zobrazení na dashboardu.

## Předpoklady
- Běžící Node-RED (např. v Dockeru)
- MQTT broker (např. Mosquitto)
- Přístup do webového rozhraní Node-RED

## Postup
1. Otevři Node-RED v prohlížeči (obvykle http://localhost:1880).
2. Přidej node `mqtt in` a nakonfiguruj připojení na broker (např. localhost:1883, téma např. `lab/esp32/telemetry/temperature`).
3. Přidej node `debug` pro zobrazení příchozích zpráv.
4. Přidej node `ui_chart` (z balíčku node-red-dashboard) pro vizualizaci dat.
5. Propoj nody: `mqtt in` → `ui_chart` a/nebo `debug`.
6. Deployni flow a sleduj data na dashboardu.

## Ověření
- Data z MQTT se zobrazují v debug okně a na dashboardu v grafu.

## Rollback
- Flow lze smazat nebo deaktivovat v editoru Node-RED.

## Tipy
- Pro testování můžeš použít MQTT Explorer nebo příkazový řádek k odesílání zpráv do zvoleného tématu.
- Přidej další vizualizační nody podle potřeby (gauge, text, atd.).
