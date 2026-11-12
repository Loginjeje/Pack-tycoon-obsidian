---
tags: [pack-tycoon, proyecto, historial]
actualizado: 2026-11-12
---

# Historial

Volver a [[00 Inicio]].

## Commits (del más reciente al más antiguo)

| Fecha | Commit | Qué |
|---|---|---|
| 2026-11-11 | `49b9c36` | JUGUETES (5.º tipo), escalón MITO y equilibrio para completar el árbol en ~2 h (monedas con raíz cuarta ×7) |
| 2026-11-11 | `f9db91f` | Atajos de teclado gratis (T/A/R/V) y barras de volumen nuevas |
| 2026-11-10 | `dc94807` | Árbol: nodos nuevos, finales de camino, LEYENDA y FÁBRICA |
| 2026-11-06 | `b43797d` | "Dopamina": racha, zona dorada, combo, suspense, jefe y precios bien pintados |
| 2026-11-06 | `28bb4ca` | Auto guardar, ficha sin spam, encargos por dificultad, animaciones y confeti |
| 2026-11-06 | `2c16d6a` | Segunda entrega |
| 2026-11-03 | `b8ed597` | Gameplay nuevo: eventos, golpe perfecto, habilidades y libreta |
| 2026-11-03 | `5eb8566`…`d093210` | GRATIS con cifras de precios, reloj, doble LP, revelado con escudo PSA, zoom del árbol |
| 2026-10-30 | `24b21d9`…`e111a10` | Rediseño de sobres, fuente legible, bugs, balance del primer rebirth (~15 min), medir el ritmo |
| 2026-10-29 | `f6dfb3d`…`6cd61d7` | El rebirth pasa a ser un árbol de mejoras; rama de calidad de vida; tres anillos |
| 2026-10-23 | `223f736` | Balance, recursos y pruebas |
| 2026-10-09 | `b26bb8b`…`e31f41a` | Armario con bonus por tipo, hucha a la barra de partida |
| 2026-10-06 | `f0b2338` | Sobres de lujo, guantes, colecciones |
| 2026-09-30 | `2b6646b` | Primera versión jugable |

## Ramas

- Locales: `main`, `segunda-entrega`, `dopamina`, `juguetes-mito-balance` (la actual).
- Solo remotas: `3d-15oct`, `arbol-rebirth-2d`, `gameplay`, `claude/bugs-y-balance`, `claude/propuesta-objetos-revelado`, `claude/rediseno-sobres`.

## Cambios de diseño importantes (por qué las cosas son como son)

- **Monedas de rebirth:** línea recta (1 por cada 1,5 M) → raíz cúbica (árbol en 11,7 h) → **raíz cuarta ×7** (árbol en ~2 h). Aguantar sin reiniciar no debe compensar más que reiniciar.
- **Daño:** antes cualquier nivel daba +15 % (se compraba lo más barato sin pensar). Ahora +4 % por nivel cualquiera y +35 % las Tijeras.
- **Las monedas no dan dinero solas:** antes cada una daba +5 % al valor de venta; ahora solo valen lo que compras en el árbol.
- **Requisito del rebirth:** 80.000 → 120.000 → 150.000 → **750.000**, siempre medido con el simulador.
- **Escalera de sobres:** ningún sobre de un escalón es más barato que los del anterior; ×1,85 entre sobres y un techo (con ×2,5 el dinero crecía sin fin).
- **Nota PSA:** el desgaste va en el sprite (10 dibujos por objeto) y el estado se deduce de la nota.

## Registro del vault

- 2026-11-12: vault creado desde el README y el código. README del juego corregido (monedas, tipos, álbum, teclas, números medidos de nuevo).
- 2026-11-12 (2): GOLDEN RAIN sustituye a la lupa del tasador (+ nodo TORMENTA DORADA); la mesa ya no guarda objetos (fuera MESA LIMPIA, el botón VENDER y la tecla V); nodos APRENDIZ / AMULETO / ENCARGADO con botón AUTO en la tienda; nombre propio para todos los nodos del árbol. Smoke test (777 checks) e input test en verde. Sin commitear en `juguetes-mito-balance`.
- 2026-11-12 (3): el panel AUTO GUARDAR ya cabe con los cinco tipos (chapas de 216 cada 236 desde x 480; el smoke test mide las chapas); la ficha en "lo bueno" sale solo con épico y legendario, sin mirar la nota PSA.
- 2026-11-12 (4): FÁBRICA al fondo de su rama (cuelga de ENCARGADO); LEYENDA x2, arriba, sin líneas y oculta hasta tener todo el árbol; líneas del árbol en pixel art azul; zoom con pasos 1/5 y 1/6.
- 2026-11-12 (5): rama de abajo simétrica: ENCARGADO pasa a la derecha (pide ARMARIO SOLO en vez de AMULETO) y FÁBRICA sube a la fila 4, colgando de AMULETO, SOBRANTE y ENCARGADO.
- 2026-11-12 (6): música de fondo nueva, el recopilatorio de una hora de **Stream Cafe** (OGG, `Sound.MUSIC`), y botón **CREDITOS** en Ajustes con su crédito y enlace. Smoke test e input test en verde.
- 2026-11-12 (7): anillo de carga de 320 a 400 px para que el sobre no tape el tramo del perfecto; **intro** estilo Isaac antes del menú (clic para saltar) y botón **AYUDA** en Ajustes encima de CREDITOS. Smoke test e input test en verde. Sin commitear en `juguetes-mito-balance`.
- 2026-11-12 (8): en la intro los ojos del chaval se vuelven signos de dólar dibujados en su pixel art (antes, monedas puestas encima).
- 2026-11-12 (9): un dibujo propio para cada uno de los 38 nodos del árbol (`tools/tree_designs.py` → `Sprites/tree/tree_<id>.png`); ojos de dólar de la intro más bajos (5 px en vez de 7). Smoke e input test en verde.
