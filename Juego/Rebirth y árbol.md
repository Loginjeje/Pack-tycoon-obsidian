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

Se arrastra con el ratón (los nodos no son botones para no comerse el arrastre); zoom con dos lupas (hasta 1/4). **Pulsar un nodo abre su ficha**; se compra con COMPRAR. Cada nodo pide el anterior (`req`, puede ser lista). Colores: dorado comprado, claro latiendo asequible, apagado caro, cerrado bloqueado.

### Centro

| Nodo | Da | Monedas |
|---|---|---|
| **DINERO** (`money`) | +50 % de dinero y empezar cada partida con **25.000** (en vez de 500) | 1 |

### Caminos en diagonal (3 anillos + final)

| Camino | Anillo 1 (3) | Anillo 2 (5) | Anillo 3 (8) | Final (x1,25) |
|---|---|---|---|---|
| **Daño** ↖ | +20 % | +25 % | +35 % | MARTILLO +60 % (10) |
| **Velocidad** ↗ | +20 % | +25 % | +35 % | TURBO +60 % (10) |
| **Pinchos** ↘ | +30 % | +35 % | +50 % | ERIZO +80 % (10) |
| **Armario** ↙ | +2 huecos de armario | → sigue en el camino del rebirth | | |

### Camino del rebirth (sigue al armario)

| Nodo | Da | Monedas |
|---|---|---|
| REBAJA | el rebirth pide −15 % | 6 |
| MONEDA EXTRA | +1 moneda por rebirth | 10 |
| HUCHA DE ORO (final) | +1 moneda más y −10 % más de requisito | 12 |

### Calidad de vida (rama de abajo, interruptores)

| Nodo | Da | Monedas | Pide |
|---|---|---|---|
| AUTO GUARDAR | guarda lo que pase el filtro de Ajustes, vende lo demás | 2 | centro |
| SOBRANTE | el daño sobrante de un golpe pasa al siguiente sobre | 2 | auto guardar |
| MESA LIMPIA | lo que lleva 12 s quieto en la mesa se vende solo | 3 | auto guardar |
| ARMARIO SOLO | con el armario lleno, entra lo mejor y vuelve lo peor | 4 | auto guardar |
| **FÁBRICA** (final) | cada partida empieza con Pinchos y PC, lo automático sigue encendido tras el rebirth, offline hasta 2 h | 8 | las tres anteriores |

ATAJOS y SIN FICHA se quitaron; si estaban comprados devuelven sus monedas (`SkillTree.REFUND`).

### Habilidades (rama de arriba)

| Nodo | Tecla | Monedas | Recarga | Pide |
|---|---|---|---|---|
| GOLPE CARGADO (siguiente golpe ×5) | `1` | 2 | 40 s | centro |
| FRENESÍ (8 s golpeando toda la mesa) | `2` | 3 | 90 s | golpe cargado |
| LUPA DEL TASADOR (en la tarjeta, vuelve a tirar la nota) | `3` | 3 | 75 s | golpe cargado |

### Ramas de los lados (horizontales)

| Rama | Nodo | Da | Monedas |
|---|---|---|---|
| Puntería ← | COMBO | la ventana del combo ×2 | 1 |
| | RACHA | cada paso de racha de perfectos ×2 | 2 |
| | ZONA ORO | la zona dorada del anillo el doble de ancha | 3 |
| | OJO DE HALCÓN (final) | racha +100 % por paso, zona dorada +50 % | 6 |
| Eventos → | HORA FELIZ | dura el doble | 1 |
| | PAGO VIP | el VIP paga ×3 en vez de ×2 | 2 |
| | TIEMPO JEFE | reloj del jefe +60 % | 3 |
| | DORADOS | dorados un 75 % más a menudo y esperan un 75 % más | 4 |
| | IMÁN (final) | dorados +50 %, hora feliz +50 %, VIP +25 %, jefe +40 % | 6 |

### LEYENDA (arriba del todo, x1,5)

15 monedas. Pide el anillo 3 de daño y de velocidad, ZONA ORO y DORADOS. Da **+100 % de dinero, +50 % de daño, velocidad y Pinchos**, y las habilidades recargan el doble de rápido.

## Cuánto dura

Ver [[Números del simulador]]: árbol entero en ~2,0 h (normal, 18 rebirths), 1,2 h (optimizado), 0,8 h (automático). Pensado así para que la futura **ascensión** (rebirth del rebirth) sea una vuelta corta.
