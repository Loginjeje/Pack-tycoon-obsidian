---
tags: [pack-tycoon, juego, interfaz]
actualizado: 2026-11-13
---

# Interfaz, guardado y teclas

Volver a [[00 Inicio]].

## Pantallas

- **Intro** (`intro.gd`, `IntroScreen`, desde 2026-11-12): sale al abrir el juego, antes del menú. Cuento mudo de 5 viñetas estilo *The Binding of Isaac* (~17 s): chaval de noche con el móvil → vídeo vertical tipo TikTok (sobre → carta MITO, "+1.000.000", corazones, comentarios) → ojos de dólar (una S verde de 3×5 con la raya en verde oscuro, dibujada en el pixel art) → rompe la hucha → trastienda llena de sobres. **Clic o tecla = saltar.** El chaval es pixel art escrito con letras en el script (`KID`, clase `Pix`). Solo sale si `Main` es la escena principal (los tests de `dev/` van directos al menú).
- **Menú** (`menu.gd`): JUGAR y AJUSTES, fondo de trastienda. Nada de rebirth aquí.
- **Partida** (`game.gd`): columna de sobres a la izquierda (fondo negro opaco, raya dorada), mesa (`bg_table`, 1344×752), barra de arriba, vitrina abajo, libreta abajo a la izquierda.
- **Menú de inicio**: título «Pack / Tycoon» con aire entre las dos palabras y **ondulando letra a letra** (`menu.gd`, `_process`), subtítulo, chapas Jugar y Ajustes.
- **Movimiento** (`UIKit.bob` / `pop_in` / `dust`): los dibujos flotan cada uno a su ritmo (sobres, tienda, mascotas, vitrinas), los de un panel entran uno detrás de otro al abrirlo, polvo flotando en el menú y la mesa, el dinero sube contando y al vender vuelan monedas hasta el contador.
- **Paneles** (entran con rebote desde el centro): Tienda, Armario, Rebirth/árbol, Libreta, Ajustes, Créditos (desde Ajustes), ADMIN (desde Ajustes, solo en depuración), Ascensión, [[Mascotas]], tarjeta de revelado, mini menú de compra automática, filtro de auto guardar.

### Barra de arriba (izquierda → derecha)

Dinero con su moneda · contador de monedas de rebirth (cerdito dorado pegado al último dígito) · contador de la mesa · iconos de apertura automática, compra automática y ARMARIO · MASCOTAS (solo con HYPER, x 1156) · TIENDA · REBIRTH (mismo tamaño que TIENDA) · ajustes. (El botón VENDER se quitó el 2026-11-12: la mesa ya no tiene objetos.)

### Ajustes (`settings_panel.gd`)

Volumen (MÚSICA y SONIDOS, deslizadores de pixel art con sus buses), pantalla completa, venta automática, fila de AUTO GUARDAR (gris hasta comprarlo en el árbol; luego botón ELEGIR → `AutoStorePanel` con 5 tipos × 10 notas × 4 rarezas, que caben en el panel desde 2026-11-12; por defecto todos los tipos y rarezas y notas 8-10), y FICHA DEL OBJETO (siempre / lo bueno = épico y legendario / nunca). No hay selector de idioma.

Abajo a la derecha, dos botones pequeños uno sobre otro: **AYUDA** (fila del auto guardar) abre `HelpPanel` (`help_panel.gd`): anillo de muestra cargando en bucle con la leyenda turquesa (perfecto ×1,5) / blanco (dorado ×1,25 más), y el juego en 5 pasos + teclas; los números salen de `Gameplay` y `LootDB`. Debajo, **CREDITOS** abre `CreditsPanel` (`credits_panel.gd`): la música de **Stream Cafe** con el enlace del vídeo. El enlace va con la fuente por defecto de Godot (la del juego solo tiene mayúsculas y el id de YouTube distingue mayúsculas) y se puede pulsar para abrirlo. **No quitarlo**: la música es libre solo dando el crédito.

**ADMIN** (`admin_panel.gd`, desde 2026-11-13): botón en Ajustes que **solo sale en depuración** (editor o export con depuración). Trampas para probar: una fila por cosa con su valor y dos botones, rojo **quita** y marrón **da** — dinero (±1000 M), monedas de rebirth (±100), puntos de ascensión (±1), árbol (VACIAR/LLENAR), piezas (A CERO/MAXIMO), mascotas (QUITAR/MAXIMO). No baja de cero; vaciar árbol o piezas apaga lo automático. Cada botón guarda. (Antes los botones medían 600×270 con filas a 120 y se pisaban.)

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
- **Sin ganancias offline**: con el juego cerrado no se gana nada (el pago por el tiempo fuera se quitó el 2026-11-17).

## Sonido

- **Música de fondo** (desde 2026-11-12): `music_streamcafe`, el recopilatorio "1 hour of comfy & relaxing songs" de **Stream Cafe** (https://www.youtube.com/watch?v=TQvXEza4fPc), entero (58:52), en OGG de ~29 MB y en bucle (`Sound._make_loop` pone `loop` al OGG). El nombre está en `Sound.MUSIC`. Crédito obligatorio: "Stream Cafe - <enlace>", en Ajustes > CREDITOS. Cómo se sacó: `pack-tycoon/Sounds/README.md`.
- 11 WAV generados por `tools/make_sounds.py`: `sfx_buy`, `sfx_click`, `sfx_error`, `sfx_open`, `sfx_rare`, `sfx_rip`, `sfx_sell`, `sfx_sell_all`, `sfx_toggle`, `sfx_tool` y la música vieja `music_backroom` (ya no suena). El OGG no lo genera: va en la tabla `EXTERNAL` del generador para que el índice no lo pierda.
