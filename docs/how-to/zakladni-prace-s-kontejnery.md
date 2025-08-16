# How-to: Základní práce s kontejnery (Docker)

## Spuštění kontejneru
```powershell
docker run -it --rm ubuntu bash
```

## Seznam běžících kontejnerů
```powershell
docker ps
```

## Výpis logů kontejneru
```powershell
docker logs <jméno|ID>
```

## Vstup do běžícího kontejneru
```powershell
docker exec -it <jméno|ID> sh
```

## Zastavení a odstranění kontejnerů
```powershell
docker compose down
```

## Odstranění svazků (dat)
```powershell
docker volume rm <název>
```

## Další tipy
- Pro každý projekt používej vlastní složku.
- Logy a data mapuj do volumes.
- Po změně docker-compose.yml vždy stack restartuj:
  ```powershell
  docker compose down
  docker compose up -d
  ```
