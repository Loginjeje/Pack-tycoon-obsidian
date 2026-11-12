---
tags: [pack-tycoon, estado, todo]
actualizado: 2026-11-12
---

# Pendiente

Volver a [[00 Inicio]].

## Falta por hacer (según el README y el diseño)

- [ ] **Ascensión**: un rebirth del rebirth. Para eso se dejó el árbol entero en ~2 h y el camino del rebirth.
- [ ] **Logros permanentes** de la cuenta (los encargos son logros cortos de partida).
- [ ] **Un sonido por material** (hoy el rasgado es igual en cartón que en plástico).
- [ ] **Violín** como objeto nuevo (prompt en `Sprites/PROMPTS.md`). La cámara ya está.
- [ ] **Selector de idioma** (se quitó porque solo hay español).
- [ ] **Botón de empezar de cero** en la interfaz (`Game.hard_reset()` existe, solo lo usan los tests).
- [ ] Los números siguen siendo sprites (`PixelNumber`), no una fuente.

## Equilibrio

- [ ] Las **zapatillas** del armario son flojas: con varios sobres a la vez limita el clic, no la carga. Hay que cambiar qué hacen, no un número.
- [ ] La mediana del jugador normal (13,5 min) está cerca del 12 de abajo de la banda del simulador.
- [ ] Volver a medir `BalanceTest armario`: las cifras del README son de antes de la raíz cuarta y de los juguetes.

## Desfases encontrados al hacer el vault (2026-11-12)

- [ ] Texto del juego: la ficha del ARMARIO (`LootDB.ITEMS`, `detail`) solo nombra cuatro tipos y no dice que los juguetes dan suerte.
- [ ] El comentario de `SPIKE_SHARE` habla de "el 60 %", pero la constante vale 1,0.
- [ ] `Game.FIRST_REBIRTH_COINS = 3` ya no hace nada (con la raíz cuarta ×7, el primer rebirth da 7 como poco).
- Los `.import` del proyecto salen modificados en git (reimportación del editor); no son cambios de juego.

Arreglado el 2026-11-12 en el README del juego: monedas (raíz cuarta ×7), cinco tipos, centro +50 %, coste suave ×1,4, daño +4 %/+35 %, álbum de 7, teclas T/A/R/V, offline 2 h con FÁBRICA, cifras del simulador, recuentos de sprites, "doce" → "veinte" sobres. Y en `scripts/README.md`: ocho items con niveles, cinco tipos, `gameplay.gd` y el árbol completo.
