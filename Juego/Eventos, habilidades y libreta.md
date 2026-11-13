---
tags: [pack-tycoon, juego]
actualizado: 2026-11-13
---

# Eventos, habilidades y libreta

Volver a [[00 Inicio]]. Todo en `scripts/data/gameplay.gd` (`Gameplay`). Los relojes de los eventos **solo corren con la mesa a la vista** (sin tarjeta ni paneles encima).

## Eventos

Anunciados con un cartel que baja (`MainRoot.banner`), destello dorado y una franja bajo la barra de arriba.

| Evento | Cada cuánto | Dura | Qué hace |
|---|---|---|---|
| **Sobre dorado** | 150-240 s (el primero a los 75 s) | espera 10 s el primer golpe | gratis; trae botín de **dos escalones por debajo** del sobre más caro comprado, con nota ≥ 8 |
| **Comprador VIP** | 150-240 s (primero a los 90 s) | 45 s | paga **×2** un tipo con nota mínima (5-8). La tarjeta ya enseña lo que pagaría |
| **Hora feliz** | 150-220 s (primera a los 150 s) | 30 s | un tipo vale **×1,5** |
| **Jefe** | 240-360 s (primero a los 210 s) | 25 s | sobre gigante con vida = 16 golpes tuyos; roto a tiempo suelta 2 objetos (nota ≥ 6) del nivel del dorado. Sin perfectos ni habilidades no se llega |

## Golpe perfecto, racha, zona dorada y combo

- **Perfecto**: pulsar el sobre cargando cuando el anillo pasa del **80 %** (se pone dorado) → golpe inmediato **×1,5**. Pulsar antes penaliza (`EARLY_PENALTY` 0,15).
- **Racha**: perfectos seguidos suman **+8 % por paso**, tope 3 pasos.
- **Zona dorada**: dentro del perfecto, el tramo 92-97 % da **×1,25** más.
- **Tope de la carga** (desde 2026-11-13): la vuelta del anillo **no baja de 0,6 s** (`Gameplay.MIN_CHARGE`). Sin tope, con piezas al máximo y árbol entero duraba 0,14 s (perfecto de 27 ms) y con mascotas 0,05 s: imposible. Con el tope, perfecto 120 ms y zona dorada 30 ms. La velocidad que pasa del tope **se vuelve daño** en la misma proporción (`Game.speed_overflow`, dentro de `damage_multiplier`): mismo daño por segundo, pero se puede apuntar. `Game.raw_charge_time` es la carga sin tope; el simulador aplica la misma regla.
- **Combo de apertura**: romper sobres sin dejar pasar **3 s** entre uno y otro sube un 1 % el valor por paso, hasta 10. Los Pinchos no cuentan.

Todo esto se amplía con las ramas de puntería y eventos del [[Rebirth y árbol]].

## Habilidades (teclas 1, 2, 3)

Se compran en la rama de arriba del árbol: GOLPE CARGADO (×5 el siguiente golpe, 40 s), FRENESÍ (8 s en que toda la mesa se golpea sola, 90 s) y **GOLDEN RAIN** (80 s de recarga). LEYENDA las recarga el doble de rápido.

### GOLDEN RAIN (sustituye a la lupa del tasador desde 2026-11-12)

- Dura **10 s** (`Gameplay.RAIN_TIME`), **20 s** con el nodo **TORMENTA DORADA** (`RAIN_EXTRA`, 3 monedas, cuelga de la golden rain).
- Mientras dura, cada sobre que se **compra** (a mano o con el PC) sale marcado (`TableElement.rain`, se guarda con la mesa) y brilla como holo. El dorado y el jefe no cuentan (no se compran).
- Al romperse un sobre marcado, cada objeto tiene un **30 %** (`RAIN_HOLO_CHANCE`) de salir **HOLO**: `Gameplay.make_holo` pone `holo = true` y **valor ×2** (`HOLO_MULT`).
- El holo **brilla** con un shader arcoíris (`UIKit.holo`) en la tarjeta, el inventario, la vitrina y el sobre marcado; la tarjeta pone "HOLO <NOMBRE>" y "HOLO: VALE EL DOBLE".

## Libreta (botón abajo a la izquierda)

### Álbum (`Gameplay.SETS`) — bonus permanentes, no se pierden con el rebirth

| Colección | Objetos | Bonus |
|---|---|---|
| CARTAS | las 4 cartas | +5 % daño |
| VINILOS | los 4 vinilos | +5 % dinero |
| ZAPATILLAS | las 4 zapatillas | +5 % velocidad |
| CONSOLAS | consola, portátil, cartucho, auriculares, cámara | +3 % críticos |
| TESOROS | reloj, reloj de bolsillo, cable, lingote | +1 hueco de mesa |
| JUGUETES | peluche, robot, figura, cohete | +10 % suerte |
| PERFECCIÓN | sacar notas 10 | +10 % rebirth |

### Encargos (3 a la vez, se cobran solos)

Tipos (`MISSION_TYPES`): abre N sobres · abre N sobres de un tipo · saca una nota ≥ N · haz N golpes perfectos · abre un sobre dorado · véndele al VIP · **guarda N de un tipo con PSA exacto** (también notas bajas; cuenta al entrar en el inventario con `Game.store`).

**Premio según dificultad**: cada encargo estima cuántos sobres cuesta (`mission_packs`, entre 8 y 60) y paga el **2,2 %** (`REWARD_PER_PACK`) del sobre más caro de la partida por cada uno (mínimo 150).
