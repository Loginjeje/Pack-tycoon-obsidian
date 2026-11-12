---
tags: [pack-tycoon, juego]
actualizado: 2026-11-12
---

# Botín y notas PSA

Volver a [[00 Inicio]]. Datos: `LootDB.PACKS[*].loot`, `GRADE_WEIGHTS`, `GRADE_MULT`, `VALUE_SCALE`.

## Objetos por tipo (valores base del escalón 1, antes de nota y `vmult`)

| Tipo | Común | Raro | Épico | Legendario |
|---|---|---|---|---|
| **Cartas** (`cards`) | CARTA COMÚN 60 (w55) | EDICIÓN LIMITADA 85 (w30) | CARTA VIEJA 135 (w12) | HOLOGRÁFICA 210 (w3) |
| **Vinilos** (`vinyls`) | VINILO 170 (w52) | DOBLE LP 235 (w31) | EDICIÓN NUMERADA 380 (w14) | PRENSADO EN COLOR 600 (w3) |
| **Zapatillas** (`sneakers`) | BLANCA 620 (w48) | DE PIEL 860 (w32) | A RESTAURAR 1.400 (w16) | LIMITADA 2.200 (w4) |
| **Juguetes** (`toys`) | PELUCHE 3.000 (w50) | ROBOT DE HOJALATA 4.200 (w30) | FIGURA EN CAJA 6.500 (w15) | COHETE DE HOJALATA 10.000 (w5) |

**Electrónica** (`electronics`) es la tabla grande: CONSOLA, AURICULARES, CÁMARA, CARTUCHO y CONSOLA PORTÁTIL en las cuatro rarezas (1.200–8.000), más CABLE (común), RELOJ (raro), LINGOTE DE ORO (épico) y RELOJ DE BOLSILLO (legendario).

`w` = peso en el sorteo. El **Trébol** y la suerte de los juguetes suben el peso de lo no común; **no tocan la nota**.

## Nota PSA (1-10)

- Pesos (`GRADE_WEIGHTS`): 6, 9, 13, 15, 14, 10, 7, 4, 3, 2 → pirámide centrada en **3-5**. Un tercio sale 1-3; la 9 sale ~1 de cada 28 y la **10, 1 de cada 40**.
- Multiplicador de valor (`GRADE_MULT`): 0,70 · 0,82 · 0,94 · 1,08 · 1,25 · 1,45 · 1,68 · 1,95 · 2,22 · **2,50**.
- Valor final ≈ `valor base × GRADE_MULT[nota] × VALUE_SCALE (2,06) × vmult del sobre` (+ eventos, combo, árbol, vinilos expuestos…).
- **Estado** sale de la nota (no es otra tirada): 9-10 como nuevo, 7-8 buen estado, 4-6 gastado, 1-3 roto.
- **El desgaste va en el sprite**: cada objeto tiene 10 dibujos (`loot_x_psa1` … `psa10`). Las cartas tienen su propio desgaste (roce, humedad, esquina doblada, pliegue, mancha).

## Arte

25 diseños de botín × 10 notas = 250 PNG en `Sprites/loot/`. Las cartas son 4 dibujos distintos (común, editada, vintage, foil), no uno teñido.
