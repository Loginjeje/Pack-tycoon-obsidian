---
tags: [pack-tycoon, juego]
actualizado: 2026-11-12
---

# Armario

Volver a [[00 Inicio]]. Código: `locker_panel.gd`, `LootDB.LOOT_KINDS`, `LootDB.bonus_for`, constantes `LOCKER_*` y `KIND_BONUS_*`.

## Qué es

Un trato reversible: un objeto **expuesto** sale del inventario (no se puede vender) y da el bonus de su tipo. Es la única forma de subir los stats con el botín. Necesita el item ARMARIO: **un hueco por nivel, de 1 a 8** (el árbol suma +2 huecos aunque no tengas el item; tope dibujado 8).

| Tipo | Sube | Peso del tipo (`LOCKER_KIND_WEIGHT`) |
|---|---|---|
| CARTAS | daño del golpe | ×1,0 |
| VINILOS | dinero de las ventas | ×0,6 |
| ZAPATILLAS | velocidad de carga | ×1,2 |
| ELECTRÓNICA | monedas de rebirth | ×0,4 |
| JUGUETES | suerte (más raro/épico/legendario) | ×1,5 |

## Cuánto da cada pieza

`bonus = (2 % … 25 % según nota 1…10) × rareza × escalón × peso del tipo`, con:
- Rareza (`LOCKER_RARITY`): común ×1, raro ×1,2, épico ×1,45, legendario ×1,75.
- Escalón del sobre (`LOCKER_TIER`): ×1, ×1,3, ×1,7, ×2,2.
- **Repetir tipo rinde menos** (`LOCKER_SAME_KIND` 0,6): la mejor pieza de cada tipo cuenta entera, la 2.ª al 60 %, la 3.ª al 36 %…

Conclusión de diseño: mezclar tipos es mejor que llenar de uno solo (medido con `BalanceTest armario`, pero con la fórmula de monedas antigua; hay que volver a medirlo, ver [[Pendiente]]).

## Interfaz

- En la partida, la tira de abajo es **la vitrina** (lo expuesto de un vistazo; cada hueco abre el panel). La chapa ARMARIO va en la barra de arriba junto a las automatizaciones. Atajo `A`.
- Panel con el **estilo de cartón de la tienda de mascotas** (desde 2026-11-17): cinta roja «Armario», **inventario a la izquierda** en fichas de papel (50 huecos, scroll; dibujo grande y valor), **vitrinas a la derecha** como fichas con celo (objeto, lo que da y lo que vale; las cerradas, apagadas con «Cerrado»), nota de papel «Lo que te da» con el total por tipo y **Vender todo** en la cabecera.
- Inventario: clic izquierdo = exponer, clic derecho = vender ese objeto. Clic en una vitrina = devolver al inventario.
- **ARMARIO SOLO** (árbol): con el armario lleno, entra la mejor pieza de su tipo y la peor vuelve; nunca empeora el armario.
