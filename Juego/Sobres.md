---
tags: [pack-tycoon, juego, equilibrio]
actualizado: 2026-11-12
---

# Sobres

Volver a [[00 Inicio]]. Datos: `LootDB.PACKS` en `scripts/data/loot_db.gd`.

## Los 20 sobres

5 tipos × 4 escalones. En la columna van alternados por precio (SOBRES, VINILOS, ZAPATILLAS, ELECTRÓNICA, JUGUETES y vuelta a empezar en el escalón siguiente). El escalón cambia el material del dibujo: normal, ORO (pan de oro), LUJO (laca con joya), MITO (holográfico).

| Sobre (id) | Precio | Vida (hp) | Paga de media | Golpes sin items | Tiempo sin items |
|---|---:|---:|---:|---:|---:|
| SOBRES (`cards`) | **gratis** | 215 | 166 | 5 | 8 s |
| VINILOS (`vinyls`) | 500 | 481 | 675 | 11 | 18 s |
| ZAPATILLAS (`sneakers`) | 900 | 645 | 1.215 | 15 | 25 s |
| ELECTRÓNICA (`electronics`) | 1.700 | 886 | 2.295 | 20 | 33 s |
| JUGUETES (`toys`) | 3.200 | 1.216 | 4.320 | 28 | 46 s |
| SOBRES ORO (`cards2`) | 6.000 | 1.665 | 7.921 | 37 | 61 s |
| VINILOS ORO | 11.000 | 2.255 | 14.518 | 51 | 84 s |
| ZAPATILLAS ORO | 20.000 | 3.041 | 26.407 | 68 | 112 s |
| ELECTRÓNICA ORO | 38.000 | 4.191 | 50.160 | 94 | 155 s |
| JUGUETES ORO | 70.000 | 5.688 | 92.400 | 127 | 210 s |
| SOBRES LUJO | 130.000 | 7.752 | 171.600 | 173 | 285 s |
| VINILOS LUJO | 240.000 | 10.533 | 316.801 | 235 | 388 s |
| ZAPATILLAS LUJO | 450.000 | 14.423 | 593.994 | 321 | 530 s |
| ELECTRÓNICA LUJO | 830.000 | 19.587 | 1.095.606 | 436 | 719 s |
| JUGUETES LUJO | 1.500.000 | 26.332 | 1.980.019 | 586 | 967 s |
| SOBRES MITO | 2.800.000 | 35.976 | 3.695.993 | 800 | 1.320 s |
| VINILOS MITO | 5.200.000 | 49.028 | 6.863.983 | 1.090 | 1.799 s |
| ZAPATILLAS MITO | 9.600.000 | 66.615 | 12.672.105 | 1.481 | 2.444 s |
| ELECTRÓNICA MITO | 18.000.000 | 91.217 | 23.760.205 | 2.028 | 3.346 s |
| JUGUETES MITO | 33.000.000 | 123.508 | 43.560.409 | 2.745 | 4.529 s |

("Paga de media" y los golpes salen del informe de `BalanceTest` del 2026-11-12.)

## Reglas que no se pueden romper (las comprueba el smoke test)

1. **Escalera de verdad:** ningún sobre de un escalón cuesta menos que el más caro del escalón anterior. Si se cruzan, el de abajo deja de comprarse.
2. **Margen fijo:** cada sobre paga de media **×1,35** su precio (×1,32 de ORO para arriba). El test exige entre +30 % y +45 %. Subir de escalón da *capital*, no rentabilidad.
3. **Vida = 21,5 × √precio** (el básico, gratis, va puesto a mano a 215). El ritmo de la partida lo pone el daño del jugador, no el precio.
4. **×1,85** de un sobre al siguiente, con techo en 33 M (con ×2,5 el dinero crecía sin fin y aguantar siempre ganaba a reiniciar).

## Cómo funciona el botín por escalón

- Los 4 escalones de un tipo **comparten la tabla de botín**: el escalón 1 la escribe en `loot`; los demás ponen `loot_id` y un **`vmult`** que multiplica el valor.
- El `vmult` es distinto en cada sobre (no hay uno por escalón): se calcula para que cada sobre pague su margen con `LootDB.pack_payout()`. **Mover un precio obliga a recalcular su `vmult`.** Tocar `GRADE_WEIGHTS`/`GRADE_MULT` también.
- El **Dólar** abarata todos los sobres un 5 % por nivel. El sobre gratis es el seguro para no quedarse nunca sin nada que abrir.

Ver también [[Botín y notas PSA]].
