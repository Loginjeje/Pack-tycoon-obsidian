---
tags: [pack-tycoon, equilibrio]
actualizado: 2026-11-12
---

# Números del simulador

Volver a [[00 Inicio]]. `dev/balance.gd` juega la partida entera con los datos de verdad (comprar, golpear, cobrar, subir de escalón, comprar mejoras, eventos, encargos, álbum, perfectos). **Los números del equilibrio se miden, no se estiman.** Comandos en [[Rutas y comandos]].

## Jugadores del simulador

- **Normal**: un clic cada 0,35 s, lee la tarjeta, paga mejoras cuando le caben; acierta ~1 de cada 3 perfectos.
- **Optimizado**: más rápido, gasta a tirones (~20 % del dinero por mejora), más de la mitad de perfectos.
- **Automático**: como el optimizado pero con PC y Pinchos.

## Medido el 2026-11-12 (`BalanceTest.tscn`, 25 partidas por jugador)

| Jugador | Mediana hasta el 1.er rebirth | 80 % central | Sin dado |
|---|---:|---|---:|
| Normal | **13,5 min** | 10,0 – 17,5 | 20,7 min |
| Optimizado | **9,2 min** | 7,8 – 11,8 | 11,3 min |
| Automático | **8,2 min** | 5,8 – 10,2 | 9,2 min |

- El simulador da por buena la mediana del normal entre **12 y 19** (OK). El smoke test exige al normal sin dado entre **12 y 26**.
- Primer sobre de cada escalón (mediana, min): vinilos 0,3 · zapatillas 1,0 · electrónica 2,5 · juguetes 2,6 · SOBRES ORO 3,5 · VINILOS ORO 5,3 · ZAPATILLAS ORO 6,4 · ELECTRÓNICA ORO 7,1 · JUGUETES ORO 8,4 · SOBRES LUJO 10,1 · VINILOS LUJO 11,0 · ZAPATILLAS LUJO 13,5.
- Primera mejora: 0,7 min. Rato más largo sin comprar mejora: 1,5 min de mediana, 2,7 el peor.
- Normal, por partida: ~61 sobres, 38 niveles de tienda, 3 dorados, 5 encargos, 2,5 colecciones.
- Sobres más comprados por el normal: SOBRES ORO 11 %, VINILOS ORO 11 %, JUGUETES 10 %, ZAPATILLAS ORO 10 %.

## Del comentario del código (no re-medido hoy)

- Árbol entero (`BalanceTest final`): **2,0 h** normal (18 rebirths), **1,2 h** optimizado, **0,8 h** automático. Objetivo pedido: ~2 h decente, ~1,5 h bueno.
- `BalanceTest arbol` (minutos hasta el siguiente rebirth): sin árbol 12,8/8,4/8,1 · centro + 4 caminos 8,3/5,9/5,4 · + puntería y eventos 6,9/5,1/5,0 · árbol entero 1,2/0,5/0,5.

## Palancas del equilibrio

- `LootDB.REBIRTH_REQUIRE` (750.000), `REBIRTH_COIN_POWER` (¼), `REBIRTH_COIN_SCALE` (7).
- `VALUE_SCALE` (2,06) y los `vmult` → margen de los sobres (×1,35/×1,32).
- La constante 21,5 de la vida (hp = 21,5 × √precio).
- `DAMAGE_PER_LEVEL` 0,04, `TIJERAS_DAMAGE_PER_LEVEL` 0,35, `GLOVES_SPEED_PER_LEVEL` 0,15, `CHARGE_TIME` 1,65, `BASE_DAMAGE` 45.
- `Gameplay`: racha (0,08, tope 3), zona dorada (×1,25), eventos, `REWARD_PER_PACK` 0,022.
- Margen de gasto del optimizado: ~20 % del dinero por mejora es lo óptimo.
