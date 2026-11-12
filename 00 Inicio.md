---
tags: [pack-tycoon, indice]
actualizado: 2026-11-12
---

# Pack Tycoon — empieza aquí

> **Para Claude (o cualquiera que llegue nuevo):** esta nota y las que enlaza
> bastan para saber cómo va el proyecto sin leer el código. Lee esta entera y
> luego solo la nota del tema que vayas a tocar. Los números salen del código
> y del simulador a fecha de `actualizado`; si el código ha cambiado después,
> manda el código.

## Qué es

Juego **incremental 2D de abrir sobres**, hecho en **Godot 4.7** (GDScript,
renderer GL Compatibility, 1920×1080, pixel art con filtro NEAREST). Todo en
**español**. Compras sobres a ciegas, los golpeas en una mesa hasta que se
abren, sale un objeto con **rareza** y **nota PSA 1-10**, lo vendes o lo
guardas, compras mejoras, expones lo bueno en el **armario** y, al llegar a
750.000, haces **rebirth** para ganar monedas de un **árbol de mejoras**
permanente.

## Dónde está todo

| Qué | Dónde |
|---|---|
| Proyecto Godot | `C:\Users\Usuario\OneDrive\Escritorio\godot stuff\pack-tycoon` |
| Repo del juego | `https://github.com/helo842/pack-tycoon.git` (rama de trabajo actual: `juguetes-mito-balance`) |
| Este vault | `C:\Users\Usuario\OneDrive\Escritorio\godot stuff\Pack tycoon` |
| Repo del vault | `https://github.com/Loginjeje/Pack-tycoon-obsidian.git` |
| Godot | `C:\Users\Usuario\AppData\Local\Programs\Godot\Godot_v4.7.2-stable_win64.exe` |
| README del juego | `pack-tycoon/README.md` (1.100+ líneas, con el *porqué* de cada decisión) |

## Mapa del vault

**Proyecto**
- [[Rutas y comandos]] — cómo abrirlo, jugar, pasar los tests y el simulador
- [[Arquitectura]] — carpetas, capas, autoloads y reglas del código
- [[Historial]] — commits y ramas, qué se hizo cuándo

**El juego**
- [[Bucle principal]] — comprar, golpear, revelar, vender/guardar, la mesa
- [[Sobres]] — los 20 sobres: precio, vida, lo que pagan
- [[Botín y notas PSA]] — objetos, rarezas, notas, valor
- [[Tienda]] — los 10 items y sus fórmulas
- [[Armario]] — exponer objetos y sus bonus
- [[Rebirth y árbol]] — monedas y todos los nodos del árbol
- [[Eventos, habilidades y libreta]] — dorado, VIP, hora feliz, jefe, perfectos, álbum, encargos
- [[Interfaz, guardado y teclas]] — barra, paneles, ajustes, guardado, offline

**Equilibrio y estado**
- [[Números del simulador]] — lo medido con `dev/balance.gd`
- [[Pendiente]] — lo que falta y lo que está roto o desfasado

## Lo esencial en 10 líneas

1. 20 sobres = 5 tipos (SOBRES, VINILOS, ZAPATILLAS, ELECTRÓNICA, JUGUETES) × 4 escalones (normal, ORO, LUJO, MITO). De gratis a 33 M.
2. Golpe: anillo de **1,65 s**, **45** de daño base. Vida del sobre = **21,5 × √precio**.
3. Cada sobre paga de media **×1,35** su precio (×1,32 de ORO para arriba).
4. Nota PSA en pirámide centrada en 3-5; la 10 sale 1 de cada 40 y vale ×2,5.
5. 10 items en la tienda; las **Tijeras** son el daño (+35 %/nivel) y los **Guantes** la velocidad (+15 %/nivel).
6. **Armario**: hasta 8 vitrinas; cada tipo sube un stat (cartas daño, vinilos dinero, zapatillas velocidad, electrónica monedas de rebirth, juguetes suerte).
7. Rebirth a **750.000**; monedas = **7 × (dinero/750.000)^¼** × Electrónica.
8. Árbol: centro (+50 % dinero, empezar con 25.000) y ramas de daño, velocidad, pinchos, armario/rebirth, calidad de vida, habilidades, puntería y eventos, más LEYENDA.
9. Primer rebirth: **~13,5 min** el jugador normal, ~9 el optimizado, ~8 el automático. Árbol entero: ~2 h.
10. La interfaz se monta **en código**: las escenas tienen un solo nodo (el editor se ve vacío y es normal).

## Cómo trabajar en este proyecto (reglas que ya se siguen)

- El usuario escribe en **español**; responder en español.
- Trabaja en la **rama que esté puesta** salvo que pida otra.
- Para reequilibrar: tocar `scripts/data/` (sobre todo `loot_db.gd`) y **medir** con el simulador, no estimar. Ver [[Números del simulador]].
- Tras cambiar código: pasar `SmokeTest` y `InputTest` (ver [[Rutas y comandos]]).
- Los niveles de los items se guardan **por posición** en `LootDB.ITEMS`: un item nuevo va **al final**, nunca en medio.
- Los comentarios del código explican el *porqué* y suelen traer el historial del número; léelos antes de cambiar una constante.
- Al terminar un cambio grande, **actualizar este vault** (y el README) y subirlo.
