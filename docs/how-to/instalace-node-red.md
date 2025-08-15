# Instalace Node-RED

## Účel
Získat vizuální nástroj pro tvorbu datových flow (sběr, transformace, vizualizace).

## Předpoklady
- Windows 11 (admin práva)
- Node.js LTS nainstalované (`node -v` pro ověření)

## Kroky
1. Instaluj Node-RED:
    ```powershell
    npm install -g --unsafe-perm node-red
    ```
2. Spusť Node-RED:
    ```powershell
    node-red
    ```
3. Otevři v prohlížeči `http://127.0.0.1:1880/`.

## Ověření
- Vidíš editor s prázdným plátnem pro flow.
- Port 1880 není blokován firewall.

## Rollback
```powershell
npm uninstall -g node-red
