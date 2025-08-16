
# How-to: MQTT topic naming (CNC, PLC, OT/IT)

Doporučení pro pojmenovávání MQTT topiců v prostředí průmyslové automatizace (CNC, PLC, OT/IT).

## Zásady
- Hierarchická struktura: např. `factory/line1/machineA/temperature` nebo `plant/area1/plc1/input/temperature`
- Nepoužívat mezery, speciální znaky
- Používat malá písmena a oddělovat lomítkem

## Příklady pro CNC
- `cnc/obrabeci-stroj1/teplota`
- `cnc/obrabeci-stroj2/vibrace`
- `cnc/obrabeci-stroj3/stav`

## Příklady pro PLC
- `plc/linka1/vstup/teplota`
- `plc/linka2/vystup/tlak`
- `plc/linka3/alarm/porucha`

## Tipy
- Pro každou strojní buňku nebo PLC používej unikátní prefix
- Stavové a alarmové zprávy odděluj do samostatných větví
- Odděluj vstupy, výstupy a alarmy do samostatných větví
