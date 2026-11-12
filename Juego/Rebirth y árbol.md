---
tags: [pack-tycoon, juego, equilibrio]
actualizado: 2026-11-12
---

# Rebirth y árbol de mejoras

Volver a [[00 Inicio]]. Código: `LootDB.REBIRTH_*`, `scripts/data/skill_tree.gd` (`SkillTree.NODES`), `rebirth_panel.gd`, `skill_card.gd`.

## El rebirth

- Chapa **REBIRTH** en la barra de la partida, junto a TIENDA (atajo `R`). Siempre se puede pulsar: abre el árbol, que dice cuánto falta. El menú de inicio no sabe nada del rebirth.
- **Requisito: 750.000 en el contador** (lo que tienes, no lo ganado). REBAJA y HUCHA DE ORO lo bajan.
- **Monedas = 7 × (contador / 750.000)^¼ × multiplicador de la Electrónica expuesta** (redondeo al final). 750.000 → 7, 12 M → 14, 192 M → 28. Gastar antes de reiniciar aleja el requisito y baja las monedas.
- El primer rebirth da como mínimo 3 (`Game.FIRST_REBIRTH_COINS`; hoy no influye porque el mínimo real ya es 7).
- **Se pierde**: dinero, items, inventario y armario. **Se queda**: monedas, árbol, álbum, filtro de auto guardar, estadísticas de la cuenta.
- El rebirth se hace **desde dentro del árbol** (barra superior con lo que te llevas y lo que falta). Las monedas sueltas **no dan nada** por sí mismas.

## El árbol

**Cada nodo tiene nombre** (`effect` en `SkillTree.NODES`): sale debajo de la chapa y de título en la ficha; el número va en la ficha (AHORA / AL COMPRARLO). En el juego, sin tildes ni Ñ (la fuente no las tiene).

**Cada nodo tiene su propio dibujo** (desde 2026-11-12): `tree_<id>.png` en `Sprites/tree/`, generado por `tools/tree_designs.py` (el objeto del nombre: NUDILLOS puño americano, MAZA maza con pinchos, MARTILLO mazo, CACTUS cactus, ERIZO erizo, FIESTA matasuegras, LEYENDA corona...). Antes reutilizaban sprites del stat (sobre de cartas, zapatillas...).

Se arrastra con el ratón (los nodos no son botones para no comerse el arrastre); zoom con dos lupas (hasta 1/6). Las líneas entre nodos son **pixel art azul** (escalera de celdas con contorno; azul noche cerrado, azul comprable, celeste comprado). **Pulsar un nodo abre su ficha**; se compra con COMPRAR. Cada nodo pide el anterior (`req`, puede ser lista). Colores: dorado comprado, claro latiendo asequible, apagado caro, cerrado bloqueado.

### Centro

| Nodo | Da | Monedas |
|---|---|---|
| **TRASTIENDA** (`money`) | +50 % de dinero y empezar cada partida con **25.000** (en vez de 500) | 1 |

### Caminos en diagonal (3 anillos + final)

| Camino | Anillo 1 (3) | Anillo 2 (5) | Anillo 3 (8) | Final (x1,25) |
|---|---|---|---|---|
| **Daño** ↖ | NUDILLOS +20 % | PALANCA +25 % | MAZA +35 % | MARTILLO +60 % (10) |
| **Velocidad** ↗ | REFLEJOS +20 % | CARRERILLA +25 % | VELOCISTA +35 % | TURBO +60 % (10) |
| **Pinchos** ↘ | ESPINAS +30 % | CARDO +35 % | CACTUS +50 % | ERIZO +80 % (10) |
| **Armario** ↙ | ESCAPARATE +2 huecos de armario | → sigue en el camino del rebirth | | |

### Camino del rebirth (sigue al armario)

| Nodo | Da | Monedas |
|---|---|---|
| REGATEO | el rebirth pide −15 % | 6 |
| PROPINA | +1 moneda por rebirth | 10 |
| HUCHA DE ORO (final) | +1 moneda más y −10 % más de requisito | 12 |

### Calidad de vida (rama de abajo, interruptores)

| Nodo | Da | Monedas | Pide |
|---|---|---|---|
| AUTO GUARDAR | guarda lo que pase el filtro de Ajustes, vende lo demás | 2 | centro |
| SOBRANTE | el daño sobrante de un golpe pasa al siguiente sobre | 2 | auto guardar |
| ARMARIO SOLO | con el armario lleno, entra lo mejor y vuelve lo peor | 4 | auto guardar |
| APRENDIZ | Tijeras y Guantes se compran solos (botón AUTO en la tienda) | 3 | auto guardar |
| AMULETO | Trébol y Ojo verde se compran solos | 4 | aprendiz |
| ENCARGADO | Armario y Láser se compran solos | 5 | armario solo |
| **FÁBRICA** (final) | cada partida empieza con Pinchos y PC, lo automático sigue encendido tras el rebirth, offline hasta 2 h | 8 | AMULETO, SOBRANTE y ENCARGADO (es lo último de la rama, abajo del todo y en medio) |

La rama es simétrica: APRENDIZ → AMULETO bajan por la izquierda, ARMARIO SOLO → ENCARGADO por la derecha, SOBRANTE en el centro, y FÁBRICA abajo en medio (fila 4) colgando de los tres. Detalle en [[Tienda]].

Quitados (devuelven sus monedas al cargar, `SkillTree.REFUND`): ATAJOS (1), SIN FICHA (2), **MESA LIMPIA** (3, la mesa ya no guarda objetos) y **TASADOR** (3, lo sustituye GOLDEN RAIN).

### Habilidades (rama de arriba)

| Nodo | Tecla | Monedas | Recarga | Pide |
|---|---|---|---|---|
| GOLPE CARGADO (siguiente golpe ×5) | `1` | 2 | 40 s | centro |
| FRENESÍ (8 s golpeando toda la mesa) | `2` | 3 | 90 s | golpe cargado |
| **GOLDEN RAIN** (10 s: los sobres que compres pueden traer objetos HOLO) | `3` | 3 | 80 s | golpe cargado |
| **TORMENTA DORADA** (+10 s de golden rain) | — | 3 | — | golden rain |

### Ramas de los lados (horizontales)

| Rama | Nodo | Da | Monedas |
|---|---|---|---|
| Puntería ← | RITMO | la ventana del combo ×2 | 1 |
| | EN RACHA | cada paso de racha de perfectos ×2 | 2 |
| | PUNTO DULCE | la zona dorada del anillo el doble de ancha | 3 |
| | OJO DE HALCÓN (final) | racha +100 % por paso, zona dorada +50 % | 6 |
| Eventos → | FIESTA | la hora feliz dura el doble | 1 |
| | CLIENTE DE ORO | el VIP paga ×3 en vez de ×2 | 2 |
| | PRÓRROGA | reloj del jefe +60 % | 3 |
| | FIEBRE DEL ORO | dorados un 75 % más a menudo y esperan un 75 % más | 4 |
| | IMÁN (final) | dorados +50 %, hora feliz +50 %, VIP +25 %, jefe +40 % | 6 |

### LEYENDA (arriba del todo, x2, oculta)

15 monedas. **Pide el árbol entero** (`SkillTree.reqs_of`) y **no se dibuja hasta que tienes todo lo demás** (`RebirthPanel.node_shown`); va sin líneas, como corona. Da **+100 % de dinero, +50 % de daño, velocidad y Pinchos**, y las habilidades recargan el doble de rápido.

## Cuánto dura

Ver [[Números del simulador]]: árbol entero en ~2,0 h (normal, 18 rebirths), 1,2 h (optimizado), 0,8 h (automático). Pensado así para que la futura **ascensión** (rebirth del rebirth) sea una vuelta corta.
