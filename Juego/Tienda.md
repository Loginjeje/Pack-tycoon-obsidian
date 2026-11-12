---
tags: [pack-tycoon, juego, equilibrio]
actualizado: 2026-11-12
---

# Tienda (los 10 items)

Volver a [[00 Inicio]]. Datos: `LootDB.ITEMS` y constantes de `loot_db.gd`. Panel: `shop_panel.gd` + ficha `item_card.gd`.

> ⚠️ El orden de `ITEMS` es el del guardado (niveles por posición). Guantes y Dólar van al final por eso. Un item nuevo, **siempre al final**.

## Items

| # | Item | Precio base | Niveles | Efecto |
|---|---|---:|---|---|
| 0 | **TIJERAS** | 300 | 8 | **+35 % de daño por nivel** (más el +4 % común = 39 %) |
| 1 | **TRÉBOL** | 600 | 8 | +2 % de peso a lo no común por nivel |
| 2 | **CAJÓN** | 1.100 | 8 | +2 huecos de mesa por nivel; +5 por nivel desde el 5 |
| 3 | **OJO VERDE** | 1.800 | 8 | +5 % de crítico por nivel (40 % al 8) |
| 4 | **LÁSER** | 2.600 | 8 | +5 % de daño crítico por nivel (crítico base ×2) |
| 5 | **PC** | 24.000 | compra única | enciende la compra automática |
| 6 | **PINCHOS** | 40.000 | compra única | enciende la apertura automática |
| 7 | **ARMARIO** | 35.000 | 8 | un hueco de vitrina por nivel (1-8), ver [[Armario]] |
| 8 | **GUANTES** | 400 | 8 | **+15 % de velocidad de carga por nivel** |
| 9 | **DÓLAR** | 900 | 8 | −5 % al precio de todos los sobres por nivel (−40 % al 8) |

## Fórmulas

- **Daño por golpe** = `45 × (1 + 0,04 × niveles_totales + 0,35 × nivel_tijeras)` × bonus de armario, árbol, álbum… (`damage_per_hit`).
- **Carga** = `1,65 s / (1 + 0,03 × niveles_totales + 0,15 × nivel_guantes)` (`charge_time`).
- "niveles_totales" cuenta los niveles de **todos** los items: cualquier compra da un poco (+4 % daño, +3 % velocidad).
- **Precio por nivel** (`item_cost`):
  - Normal: `base × 1,6^nivel`.
  - Tijeras, Guantes y Dólar en dos tramos: las 3 primeras compras suben ×1,4 (`SOFT_COST_GROWTH`) y a partir de ahí ×2,8 (Tijeras y Guantes) o ×2,3 (Dólar) sobre el último precio barato. Dólar: de 900 a ~113.500.
  - PC y Pinchos: un solo pago.

## Interfaz

Caja de cartón claro (`ui_panel_carton`) con fichas de papel crema y el precio en cinta roja; TIENDA en cinta roja. Puntos de nivel en rojo; al tope pone AL TOPE; las de compra única llevan COMPRA ÚNICA y luego COMPRADO. Un precio que no llega sale en gris (`Palette.PRICE_OFF`). Clic en una ficha abre su `ItemCard` con el efecto actual y el siguiente; la ficha **pide** la compra, la tienda cobra.
