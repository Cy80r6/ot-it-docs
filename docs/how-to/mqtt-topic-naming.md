
# MQTT topic naming pro CNC

**Účel:** Zajistit konzistentní názvy témat pro škálování a řízení přístupů.  
**Předpoklady:** Znalost zařízení a výrobní topologie.  
**Odhad času:** 20–30 min.

## Doporučené schéma
```
factory/{line}/{machine}/telemetry/{signal}
factory/{line}/{machine}/event/{type}
factory/{line}/{machine}/lifecycle/status
```
- `telemetry`: pravidelná data
- `event`: alarmy, stavy
- `lifecycle`: online/offline

## Ověření
- Uživatelé v ACL mají pouze potřebné prefixy.

## Rollback
- Pokud změníš schéma, zavést aliasy a přechodné routování.
