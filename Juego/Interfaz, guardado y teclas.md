---
tags: [pack-tycoon, juego, interfaz]
actualizado: 2026-11-12
---

# Interfaz, guardado y teclas

Volver a [[00 Inicio]].

## Pantallas

- **Menú** (`menu.gd`): JUGAR y AJUSTES, fondo de trastienda. Nada de rebirth aquí.
- **Partida** (`game.gd`): columna de sobres a la izquierda (fondo negro opaco, raya dorada), mesa (`bg_table`, 1344×752), barra de arriba, vitrina abajo, libreta abajo a la izquierda.
- **Paneles** (entran con rebote desde el centro): Tienda, Armario, Rebirth/árbol, Libreta, Ajustes, tarjeta de revelado, mini menú de compra automática, filtro de auto guardar.

### Barra de arriba (izquierda → derecha)

Dinero con su moneda · contador de monedas de rebirth (cerdito dorado pegado al último dígito) · contador de la mesa · iconos de apertura automática, compra automática y ARMARIO · TIENDA · REBIRTH (mismo tamaño que TIENDA) · ajustes. (El botón VENDER se quitó el 2026-11-12: la mesa ya no tiene objetos.)

### Ajustes (`settings_panel.gd`)

Volumen (MÚSICA y SONIDOS, deslizadores de pixel art con sus buses), pantalla completa, venta automática, fila de AUTO GUARDAR (gris hasta comprarlo en el árbol; luego botón ELEGIR → `AutoStorePanel` con 4 tipos × 10 notas × 4 rarezas; por defecto todos los tipos y rarezas y notas 8-10), y FICHA DEL OBJETO (siempre / lo bueno / nunca). No hay selector de idioma.

## Teclas (`game.gd::_hotkey`, solo con la mesa despejada)

| Tecla | Acción |
|---|---|
| `1` `2` `3` | golpe cargado, frenesí, golden rain (si están compradas) |
| `T` | tienda |
| `A` | armario |
| `R` | árbol del rebirth |
| `ESPACIO` / `ENTER` / `ESC` (con tarjeta) | GUARDAR el objeto de la tarjeta |
| `ESC` | cerrar panel; sin panel, volver al menú (la mesa se guarda tal cual) |

## Animaciones y "jugo"

Paneles con rebote, sobres que caen y aplastan la mesa, botones que crecen al pasar y se hunden al pulsar (`UIKit.juice`), `punch` al comprar y `shake` al no poder, dinero que salta, carteles de eventos con destello, confeti al comprar mejoras, fundido al cambiar de pantalla.

## Guardado

- La mesa guardada son **solo sobres** (`{"pack", "hp", "rain"}`). Un objeto que aparezca en `table` (la tarjeta abierta al cerrar, o un guardado viejo con objetos en la mesa) va **al inventario** al cargar (si está lleno, se vende).
- `user://packtycoon.json`, automático **cada 30 s si cambió algo** y al salir. Antes de guardar copia el anterior a `.bak`; si el principal falla, carga la copia.
- Las escenas de `dev/` guardan en `packtycoon_test.json` (el smoke test hace `hard_reset()`; el 2026-11-10 se llevó una partida real por delante, por eso existe esto).
- **Offline**: con el juego cerrado se gana el **10 %** de lo que se ganaba por segundo (`lifetime_earned / play_seconds`), mínimo 60 s fuera, máximo **1 h (2 h con FÁBRICA)**, y **solo con los Pinchos comprados**. Aviso "MIENTRAS NO ESTABAS". Constantes `Game.OFFLINE_*`.

## Sonido

11 WAV generados por `tools/make_sounds.py`: `sfx_buy`, `sfx_click`, `sfx_error`, `sfx_open`, `sfx_rare`, `sfx_rip`, `sfx_sell`, `sfx_sell_all`, `sfx_toggle`, `sfx_tool` y la música `music_backroom` en bucle.
