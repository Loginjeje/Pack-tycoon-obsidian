---
tags: [pack-tycoon, proyecto, codigo]
actualizado: 2026-11-12
---

# Arquitectura

Volver a [[00 Inicio]].

## Carpetas

```
project.godot   1920x1080 (ventana 1280x720), stretch canvas_items, NEAREST, GL Compatibility
                main_scene = res://scenes/Main.tscn
                autoloads: Game = scripts/core/game_state.gd, Sound = scripts/core/audio.gd
scenes/         un .tscn por pantalla, cada uno con UN solo nodo + su script
  ui/game_theme.tres   tema: fuente font_pack.fnt y colores
scripts/
  core/         main.gd (MainRoot: cambia de pantalla, velo, overlays, banner, confeti)
                game_state.gd (autoload Game: dinero, inventario 50, armario, monedas,
                ajustes, guardado, offline)  ~1.630 líneas
                audio.gd (autoload Sound: sfx, música, 3 buses)
                palette.gd (tokens de color, también los usa el generador de Python)
  data/         loot_db.gd   (LootDB: sobres, botín, PSA, items, armario, rebirth) ~1.250
                skill_tree.gd (SkillTree: nodos del árbol)                         ~730
                gameplay.gd   (Gameplay: eventos, perfectos, combo, jefe, habilidades,
                               álbum, encargos)                                    ~390
  ui/           ui_kit.gd (fábrica de controles + juice/punch/shake), sprites.gd,
                pixel_number.gd, damage_popup.gd, pixel_slider.gd, timer_ring.gd
  screens/      game.gd (la partida, ~2.250 líneas), menu, shop_panel, item_card,
                auto_buy_panel, auto_store_panel, locker_panel, rebirth_panel (~960),
                skill_card, reveal_card, settings_panel, credits_panel, help_panel, intro, libreta_panel,
                table_element (~720)
Sprites/        ~500 PNG en subcarpetas (tree/ = nodos del árbol); Sprites/index.tsv = nombre corto -> ruta
Sounds/         11 WAV (10 sfx + 1 música vieja) + music_streamcafe.ogg (la de fondo, 1 h);
                Sounds/index.tsv
dev/            SmokeTest, InputTest, BalanceTest (balance.gd), Screenshots, probes
tools/          generadores de Python (sprites, sonidos, fuente, fondos, maquetas;
                tree_designs.py = un dibujo por nodo del árbol)
source_art/     arte de partida, no lo usa el juego
```

Unas 12.600 líneas de GDScript en total.

## Reglas de diseño del código

- **Capas:** `data/` no depende de la interfaz; `ui/` no sabe nada del juego; `screens/` es lo único que mezcla las dos. Se puede reequilibrar sin abrir la interfaz.
- **Escenas de un nodo:** toda la UI se construye en `_ready()` con `UIKit`. El editor se ve vacío a propósito; para ver algo hay que ejecutar (F5).
- **Recursos por nombre corto:** `Sprites.tex("loot_vinyl_psa7")`, `Sound.play("sfx_sell")`. Las rutas salen de los `index.tsv` que escribe Python. Nunca rutas a mano.
- **Quien decide es la pantalla, no la tarjeta:** `RevealCard`, `ItemCard` y `SkillCard` solo **piden** (señal `resolved("store"/"sell"/"later")`, comprar…); `game.gd` / la tienda / el árbol mueven el dinero y suenan.
- **El audio no conoce el juego:** `Sound` reproduce nombres; `game.gd` decide qué suena.
- **Clases por `class_name`** (globales), no por ruta.
- **Números grandes con sprites** (`PixelNumber` compone `num_*.png`); el resto del texto usa la fuente de mapa de bits `font_pack.fnt` (escala solo en múltiplos de 7 px).
- **El anillo de carga** (`TimerRing`) se pinta celda a celda y va **detrás** del sobre.
- **La mesa solo tiene sobres:** `TableElement` ya no tiene modo "botín" (se quitaron `setup_loot`, `sell_instant`, la señal `sold`…). Lo que sale de un sobre pasa por `_deliver` y acaba en el inventario o vendido.
- **Animaciones:** `MainRoot.push_veil/push_overlay` (paneles con rebote), `MainRoot.banner` (carteles de eventos), `MainRoot.confetti`, `UIKit.juice()` (ya dentro de todos los botones), `UIKit.punch` y `UIKit.shake`.
- **Guardado por posición:** los niveles de los items se leen por índice de `LootDB.ITEMS`; items nuevos siempre al final.
- **Nodos retirados del árbol** devuelven sus monedas al cargar (`SkillTree.REFUND`).

## Funciones clave

| Qué | Dónde |
|---|---|
| Daño por golpe | `LootDB.damage_per_hit(levels)` |
| Tiempo de carga | `LootDB.charge_time(levels)` |
| Precio de un item | `LootDB.item_cost(index, level)` |
| Monedas de rebirth | `LootDB.rebirth_coins_for(amount)` |
| Bonus de una pieza expuesta | `LootDB.bonus_for(...)` |
| Media de lo que paga un sobre | `LootDB.pack_payout()` |
| Bonus del árbol | `Game.skill_bonus(stat)`, `SkillTree.node_bonus(n, stat)` |
| Encargos | `Gameplay.make_mission`, `mission_step`, `mission_reward`, `mission_packs` |
| Guardar en inventario | `Game.store(...)` |
| Pago offline | `game_state.gd` (constantes `OFFLINE_*`) |
| Atajos de teclado | `game.gd::_hotkey` |
| Qué pasa con un objeto al salir | `game.gd::_deliver` (auto guardar → venta automática → venta sin ficha → tarjeta) |
| Golden rain / holo | `Game.rain_left`, `Game.rain_time()`, `Gameplay.make_holo`, `UIKit.holo` (shader) |
| Compra automática de la tienda | `SkillTree.AUTO_ITEMS`, `Game.item_auto_on`, `Game.auto_item_pick`, `game.gd::_auto_items_step` |
