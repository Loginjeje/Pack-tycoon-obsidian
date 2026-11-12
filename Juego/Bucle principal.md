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
3. **Revelado** — al llegar a 0 sale la **tarjeta de revelado**: objeto, rareza, estado, nota PSA y valor. Botones **GUARDAR** (inventario) y **VENDER**. **Solo se cierra eligiendo**: `ESPACIO`/`ENTER`/`ESC` = GUARDAR.
   - La tarjeta solo sale para lo bueno (por defecto): raro o mejor, PSA ≥ 8, o nuevo en la libreta. **Lo demás se vende en el momento** con un rótulo flotante (nombre, PSA, lo que paga). Ajustable: siempre / lo bueno / nunca.
   - Medio segundo de bloqueo al aparecer (`RevealCard.arm_delay_ms`) y un clic fuera no la cierra: evita saltársela haciendo spam.
   - Efecto "casi": a veces la rareza sube un momento y vuelve (`NEAR_MISS_CHANCE` 25 %).
4. **Mesa: SOLO SOBRES.** Desde 2026-11-12 ningún objeto se queda en la mesa ("sin limbo"): todo acaba en el inventario o vendido al salir. Se quitaron el botón VENDER de la barra, la tecla `V` y la mejora MESA LIMPIA. Los sobres se pueden **arrastrar**; caben **8** (el Cajón amplía) y caen en una rejilla buscando sitio sin solapar.
5. **Inventario** (50 huecos) — vive dentro del panel del [[Armario]]. Clic izquierdo = exponer, clic derecho = vender ese, **VENDER TODO** vacía.

## Automatizaciones

| Botón (barra de arriba) | Qué hace | Necesita |
|---|---|---|
| Apertura automática | Los Pinchos golpean solos todos los sobres de la mesa | item **Pinchos** |
| Compra automática | Compra el **más barato** de los sobres marcados en su mini menú (`AutoBuyPanel`), cada 0,75 s | item **PC** |
| Venta automática | (Ajustes) todo se vende al salir, sin tarjeta ni mesa | nada |
| Auto guardar | Guarda lo que pase el filtro (tipo × nota × rareza) y vende lo demás (inventario lleno → vende) | nodo del árbol AUTO GUARDAR |
| Compra automática de la tienda | Sube sola el item más barato con el botón AUTO puesto | nodos APRENDIZ / AMULETO / ENCARGADO, ver [[Tienda]] |

- Sin su pieza, el botón sale atenuado y avisa de qué falta (`LootDB.AUTO_ITEM_OPEN = "pinchos"`, `AUTO_ITEM_BUY = "pc"`).
- El sobre gratis no aparece en la compra automática (se quedaría en él para siempre).
- **Pinchos**: reparten entre los sobres de la mesa una fracción (`SPIKE_SHARE` = 1,0) del daño por segundo del jugador, +50 % por cada ×10 de dinero (`SPIKE_PER_DECADE`). No se multiplican por el número de sobres.

## Ritmo buscado

Primera mejora a los ~40 s, un escalón nuevo cada 2-3 min, nunca más de ~2 min sin nada que comprar, primer rebirth ~13,5 min. Ver [[Números del simulador]].
