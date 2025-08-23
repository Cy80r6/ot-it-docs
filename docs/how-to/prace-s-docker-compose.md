# How-to: Práce s docker-compose a Docker Desktop

Tento návod vysvětluje, jak správně pracovat s docker-compose.yml v kombinaci s Docker Desktop, jak udržet synchronizaci a jaké soubory verzovat.

## Základní workflow
- Měj projektovou složku s docker-compose.yml a konfiguračními soubory.
- Stack vždy spouštěj z terminálu:
  ```shell
  docker compose up -d
  ```
- Pokud změníš docker-compose.yml, spusť znovu `docker compose up -d` (nebo `--build` při změně Dockerfile).
- Docker Desktop UI (Play/Stop) pouze spouští/vypíná existující kontejnery, nečte automaticky nové YAML.

## Jak udržet pořádek
- Verzuj pouze docker-compose.yml, konfigy a případné init skripty.
- Ignoruj data, logy, volume a runtime soubory pomocí .gitignore.
- Pro čistý start použij:
  ```shell
  docker compose down
  docker compose up -d
  ```

## Doporučený .gitignore
```gitignore
**/data/
**/log/
**/logs/
**/tmp/
**/node_modules/
mosquitto/data/
mosquitto/log/
nodered/data/
nodered/node_modules/
nodered/.config.runtime.json
nodered/.sessions.json
.vscode/
.idea/
.DS_Store
Thumbs.db
*.bak
*.swp
*.tmp
```

## Tipy
- Pokud chceš mít projekt v Docker Desktop UI „klikací“, použij v menu Containers → Create → From Compose file a nahraj svůj YAML.
- Po větších změnách vždy stack shodit (`down`) a znovu spustit (`up -d`).
- Pro větší projekty používej pojmenované kontejnery a sítě v docker-compose.yml.
