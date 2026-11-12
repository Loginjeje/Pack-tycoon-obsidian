---
tags: [pack-tycoon, proyecto]
actualizado: 2026-11-12
---

# Rutas y comandos

Volver a [[00 Inicio]].

## Rutas

- Proyecto: `C:\Users\Usuario\OneDrive\Escritorio\godot stuff\pack-tycoon`
- Godot 4.7.2: `C:\Users\Usuario\AppData\Local\Programs\Godot\Godot_v4.7.2-stable_win64.exe` (hay un acceso directo `Godot.lnk` en `godot stuff`)
- Guardado de la partida: `user://packtycoon.json` (+ copia `.bak`). Las pruebas usan `user://packtycoon_test.json`.
- En Windows `user://` es `%APPDATA%\Godot\app_userdata\Pack Tycoon\`.

## Comandos (desde la carpeta del proyecto, en Git Bash)

```bash
G="/c/Users/Usuario/AppData/Local/Programs/Godot/Godot_v4.7.2-stable_win64.exe"

$G --path . --editor                                  # abrir el editor
$G --path .                                           # jugar
$G --headless --path . --import                       # reimportar (tras añadir escena o class_name)

$G --headless --path . res://dev/SmokeTest.tscn       # test de integración: acaba en "HUMO: todo correcto."
$G --headless --path . res://dev/InputTest.tscn       # clics de verdad: acaba en "RATON: todo correcto."
$G --headless --path . res://dev/BalanceTest.tscn     # simulador: minutos hasta el primer rebirth
$G --rendering-driver opengl3 --path . res://dev/Screenshots.tscn   # PNG de cada pantalla en user://shots/ (con ventana)
```

Modos del simulador (`BalanceTest.tscn <modo>`):

| Modo | Qué mide |
|---|---|
| *(nada)* | minutos hasta el primer rebirth, ritmo de desbloqueos, lo que paga cada sobre |
| `trace` / `trace buys` | la partida paso a paso / cada compra |
| `armario` | qué conviene exponer (30 y 45 min sin reiniciar) |
| `arbol` | minutos hasta el siguiente rebirth con el árbol a medias y entero |
| `final` | cuánto se tarda en tener el árbol entero |
| `curva` | dinero y monedas de una partida larga sin reiniciar |
| `clasico` | apaga todo el gameplay nuevo (eventos, encargos…) para comparar |
| `sin_dorado`, `sin_vip`, `sin_feliz`, `sin_encargos`, `sin_perfecto`, `sin_album`, `sin_combo`, `sin_jefe` | apaga solo esa pieza |

Si un test se cae con `Could not find type "LockerPanel"` (o similar), falta el paso `--import`, no es un fallo del juego.

## Herramientas de Python (`tools/`)

Generan el arte y el sonido: `make_sprites.py` (sprites + `Sprites/index.tsv`), `make_sounds.py`, `make_font.py`, `make_background.py`, `make_screens.py`, `loot_designs.py`, `pack_designs.py`, `font_patterns.py`, maquetas `mockup_*`. Entorno en `.venv/` (ignorado por git).

## Git

- Remoto del juego: `origin = https://github.com/helo842/pack-tycoon.git`
- Ramas locales: `main`, `segunda-entrega`, `dopamina`, `juguetes-mito-balance` (actual).
- Los `.import` **sí** se versionan. Al abrir el editor muchos salen modificados (reimportación); no suelen ser cambios reales.
