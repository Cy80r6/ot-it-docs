# Playbook: OT/IT segmentace

Návod na segmentaci sítě mezi OT a IT částí v laboratorním prostředí.

## Cíl
- Oddělit provoz OT a IT pomocí VLAN a firewallu na edge zařízení.

## Postup
1. Vytvoř dvě VLAN (např. VLAN 10 pro OT, VLAN 20 pro IT).
2. Nakonfiguruj switch a edge gateway pro správné routování mezi VLAN.
3. Omez komunikaci mezi VLAN pouze na nezbytné porty (firewall).
4. Ověř funkčnost (ping, přístup ke službám).

## Schéma
- [Zde doplň síťový diagram]

## Testy
- OT zařízení nevidí IT síť (kromě povolených služeb)
- IT síť má přístup pouze na vybrané služby v OT
