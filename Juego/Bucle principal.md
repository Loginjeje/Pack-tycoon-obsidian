---
tags: [pack-tycoon, juego]
actualizado: 2026-11-12
---

# Bucle principal

Volver a [[00 Inicio]].

## Paso a paso

1. **Comprar** — en la columna de sobres de la izquierda (con scroll, 20 filas). Se compra a ciegas y el sobre **cae** en un sitio libre de la mesa (animación de caída, polvo, temblor). Con la mesa llena no se cobra. Ver [[Sobres]].
2. **Golpear** — clic en el sobre: arranca un **anillo de carga de 1,65 s** que se dibuja detrás del sobre. Cada carga completa es un **golpe** de **45** de daño base (ver [[Tienda]] para las mejoras). El sobre se agrieta en tres tramos; no hay barra de vida.
   - **Un clic abre UN sobre**: si otro sobre está a medio abrir, un clic en uno nuevo no hace nada hasta que el primero se rompe. Todos los huecos pueden estar cargando a la vez, pero hay que ir pulsándolos.
   - **Golpe perfecto**: pulsar cuando el anillo pasa del 80 % → golpe inmediato ×1,5 (ver [[Eventos, habilidades y libreta]]).
   - Críticos con el Ojo verde (×2 base, más con el Láser).
3. **Revelado** — al llegar a 0 sale la **tarjeta de revelado**: objeto, rareza, estado, nota PSA y valor. Botones **GUARDAR** (inventario) y **VENDER**. `ESPACIO`/`ENTER`/`ESC` la cierra dejando el objeto en la mesa.
   - La tarjeta solo sale para lo bueno (por defecto): raro o mejor, PSA ≥ 8, o nuevo en la libreta. Lo demás cae en la mesa con un rótulo flotante. Ajustable: siempre / lo bueno / nunca.
   - Medio segundo de bloqueo al aparecer (`RevealCard.arm_delay_ms`) y un clic fuera no la cierra: evita saltársela haciendo spam.
   - Efecto "casi": a veces la rareza sube un momento y vuelve (`NEAR_MISS_CHANCE` 25 %).
4. **Mesa** — el objeto se queda ahí. **Clic = vender**, **arrastrar = moverlo**. Caben **8** cosas (sobres + objetos); el Cajón amplía. Los objetos caen en una rejilla buscando sitio sin solapar (16 px de margen).
5. **Inventario** (50 huecos) — vive dentro del panel del [[Armario]]. Clic izquierdo = exponer, clic derecho = vender ese, **VENDER TODO** vacía.

## Automatizaciones

| Botón (barra de arriba) | Qué hace | Necesita |
|---|---|---|
| Apertura automática | Los Pinchos golpean solos todos los sobres de la mesa | item **Pinchos** |
| Compra automática | Compra el **más barato** de los sobres marcados en su mini menú (`AutoBuyPanel`), cada 0,75 s | item **PC** |
| Venta automática | (Ajustes) todo se vende al salir, sin tarjeta ni mesa | nada |
| Auto guardar | Guarda lo que pase el filtro (tipo × nota × rareza) y vende lo demás | nodo del árbol AUTO GUARDAR |

- Sin su pieza, el botón sale atenuado y avisa de qué falta (`LootDB.AUTO_ITEM_OPEN = "pinchos"`, `AUTO_ITEM_BUY = "pc"`).
- El sobre gratis no aparece en la compra automática (se quedaría en él para siempre).
- **Pinchos**: reparten entre los sobres de la mesa una fracción (`SPIKE_SHARE` = 1,0) del daño por segundo del jugador, +50 % por cada ×10 de dinero (`SPIKE_PER_DECADE`). No se multiplican por el número de sobres.
- MESA LIMPIA (árbol): lo que lleva 12 s quieto en la mesa se vende solo (no corre con la tarjeta abierta).

## Ritmo buscado

Primera mejora a los ~40 s, un escalón nuevo cada 2-3 min, nunca más de ~2 min sin nada que comprar, primer rebirth ~13,5 min. Ver [[Números del simulador]].
