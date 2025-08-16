
# How-to: Základní práce s Node-RED

Tento návod shrnuje základní práci s Node-RED: instalaci, orientaci v editoru, vytváření a správu flow.

## Předpoklady
- Windows, Linux nebo macOS
- Nainstalovaný Node.js (doporučeno LTS)

## Instalace Node-RED
1. Otevři terminál a spusť:
	```shell
	npm install -g --unsafe-perm node-red
	```
2. Spusť Node-RED:
	```shell
	node-red
	```
3. Otevři editor v prohlížeči: [http://localhost:1880](http://localhost:1880)

## Základní pojmy
- **Node**: základní stavební blok (vstup, výstup, zpracování dat)
- **Flow**: propojení více nodů do logického celku
- **Dashboard**: webové rozhraní pro vizualizaci dat (volitelný doplněk)

## Práce s editorem
1. Přetáhni nody z levého panelu do pracovní plochy
2. Propoj nody tažením myši
3. Dvojklikem otevři nastavení nodu
4. Klikni na "Deploy" pro spuštění flow

## Import/export flow
- Menu (vpravo nahoře) → Import/Export
- Flow lze sdílet jako JSON

## Správa flow
- Flow lze pojmenovat, duplikovat, mazat
- Pro větší projekty používej více záložek (tabs)

## Tipy
- Instaluj další nody přes "Manage palette"
- Pravidelně zálohuj flow (menu → Export)
