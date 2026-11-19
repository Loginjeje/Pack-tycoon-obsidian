---
tags: [pack-tycoon, juego, equilibrio]
actualizado: 2026-11-19
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

**Cada nodo tiene su propio dibujo** (desde 2026-11-12): `tree_<id>.png` en `Sprites/tree/`, generado por `tools/tree_designs.py` (el objeto del nombre: BATE bate de madera, MAZA maza con pinchos, MARTILLO mazo, CACTUS cactus, ERIZO erizo de mar, FIESTA globos, FIEBRE DEL ORO lingote, SOBRANTE bumerán, LEYENDA corona...). Antes reutilizaban sprites del stat (sobre de cartas, zapatillas...).

Se arrastra con el ratón (los nodos no son botones para no comerse el arrastre); zoom con dos lupas (hasta 1/6), que van dentro de la franja de abajo. Las líneas entre nodos son **pixel art azul** (escalera de celdas con contorno; azul noche cerrado, azul comprable, celeste comprado). **Dos clics, como en la telaraña** (desde 2026-11-19): el primero **elige** el nodo (marco dorado) y su ficha sale en la **franja de abajo** (nombre y precio o qué falta, lo que hace, "Ahora X; después, Y"; la explicación larga en su tooltip); **el segundo lo compra**, igual que el botón COMPRAR de la franja. Se abre sin nada elegido. La ficha a pantalla completa (`SkillCard`) se borró. Cada nodo pide el anterior (`req`, puede ser lista). Colores: dorado comprado, claro latiendo asequible, apagado caro, cerrado bloqueado.

### Centro

| Nodo | Da | Monedas |
|---|---|---|
| **TRASTIENDA** (`money`) | +50 % de dinero y empezar cada partida con **25.000** (en vez de 500) | 1 |

### Caminos en diagonal (3 anillos + final)

| Camino | Anillo 1 (3) | Anillo 2 (5) | Anillo 3 (8) | Final (x1,25) |
|---|---|---|---|---|
| **Daño** ↖ | BATE +20 % | PALANCA +25 % | MAZA +35 % | MARTILLO +60 % (10) |
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
| **FÁBRICA** (final) | cada partida empieza con Pinchos y PC, lo automático sigue encendido tras el rebirth | 8 | AMULETO, SOBRANTE y ENCARGADO (es lo último de la rama, abajo del todo y en medio) |

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

## Ascensión (desde 2026-11-12)

Con el árbol **entero** comprado sale el botón morado **ASCENSION** abajo a la izquierda del árbol (`Game.ascension_shown`). Abre una pantalla como la del árbol pero morada, con partículas moradas subiendo (`AscensionPanel`). **ASCENDER** (dos pulsaciones) lo borra **todo**: partida, monedas y árbol del rebirth, contador de rebirths, álbum y lo automático (`Game.do_ascension`). Se quedan los ajustes, las estadísticas y la ascensión. Da **1 punto de ascensión**.

Los puntos se gastan en **la telaraña** (`AscensionTree`, desde 2026-11-19): **HYPER** en el centro (persona morada, 1 punto, **×3 de dinero**, exagerado a propósito para que la segunda ascensión llegue antes) y **seis hilos** de cuatro anillos, unidos por los hilos de la red. Cada nodo pide el de dentro de su hilo. Anillo 1 = 1 punto, 2 = 2, 3 = 3, 4 = 4 (`RING_COST`). Tras ascender una vez el botón sale siempre, para poder entrar a gastar los puntos.

| Hilo | Anillo 1 (1) | Anillo 2 (2) | Anillo 3 (3) | Anillo 4 (4) |
|---|---|---|---|---|
| **Dinero** → | Bolsa ×1,5 | Cofre ×2 | Supernova ×3 | Galaxia ×5 |
| **Rebirth** ↘ | Eco ×1,5 monedas | Atajo: requisito −30 % | Big bang ×2 monedas | Multiverso ×3 monedas |
| **Automático** ↙ | Piloto: habilidades solas | Jardinero: compra sola el nodo más barato del árbol | Auto rebirth: rebirth solo con 16× el requisito (el doble de monedas), solo mientras falte algo del árbol del rebirth | Perfección: todos los golpes perfectos solos (zona dorada) |
| **Herencia** ← | Recuerdo: no se pierde el álbum | Herencia: se guardan calidad de vida y habilidades del árbol | Semilla: cada partida empieza con 250.000 de dinero más (por debajo del requisito con todas las rebajas, ~394.000) | Legado: rebirth y ascensión no quitan el armario |
| **Estrellas** ↖ | Estrella doble: +1 punto por ascensión | Collar: +1 hueco de mascota | Constelación: +2 puntos por ascensión | Nebulosa: +3 puntos por ascensión |
| **Golpe** ↗ | Fuerza ×1,5 daño | Rayo ×1,5 velocidad | Cometa ×2 daño y pinchos | Meteorito ×3 daño |

- Efectos: `"mult"` multiplica (`Game.asc_mult`), `"add"` suma (`Game.asc_add`), `"flag"` (`Game.asc_flag`). Los automáticos son **interruptores** (`Game.toggle_asc`, apagados en `asc_off`, se guarda). Corren una vez por segundo con la mesa despejada (`_asc_auto_step` en `game.gd`). Jardinero gasta las monedas que irían a las mascotas: por eso se puede apagar.
- Pantalla: la telaraña es más grande que la vista y **se navega** (arrastrar, rueda o lupas; zoom 0,3 a 1, abre a 0,65). Nombres bajo cada hexágono y de cada hilo fuera. Pulsar un hexágono lo **elige** (marco lila) y su ficha sale en la franja de abajo (nombre, lo que da, precio y qué pide, botón COMPRAR / ENCENDER / APAGAR); pulsar otra vez el elegido lo compra. Cada hexágono tiene su dibujo `tree_asc_*`.
- Costes a ojo: `dev/balance.gd` no simula la ascensión.

Con HYPER sale la tienda de [[Mascotas]] (gachapón), cuya máquina COSMICA se paga con puntos de ascensión.

## Cuánto dura

Ver [[Números del simulador]]: árbol entero en ~2,0 h (normal, 18 rebirths), 1,2 h (optimizado), 0,8 h (automático). Pensado así para que la **ascensión** (rebirth del rebirth) sea una vuelta corta.
