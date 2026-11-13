---
tags: [pack-tycoon, juego, equilibrio]
actualizado: 2026-11-13
---

# Mascotas (el gachapón)

Volver a [[00 Inicio]]. Código: `scripts/data/pet_db.gd` (`PetDB.MACHINES`), `scripts/screens/pet_panel.gd` (`PetPanel`), `Game.pet_bonus` / `roll_pet` / `can_roll_pet` / `pet_currency`. Dibujos: `tools/tree_designs.py` (`tree_pet_<id>.png`).

## Cuándo sale

Al comprar **HYPER** en la [[Rebirth y árbol#Ascensión (desde 2026-11-12)|ascensión]] sale la chapa **MASCOTAS** en la barra de arriba, al lado de TIENDA (`Game.pet_shop_shown`). Las mascotas **no se pierden nunca**, ni al ascender (solo `hard_reset`).

## Las cuatro máquinas

Una por página; se cambian con las flechas de encima de la máquina. Cada tirada sube **un nivel** a la mascota que sale (máximo **5**, `PetDB.MAX_LEVEL`); las que están al máximo ya no salen. Peso de salir por rareza (`RARITY_WEIGHT`): común 30, rara 15, legendaria 5.

Números **por nivel** (×5 al máximo):

| Máquina | Tirada | Común | Común | Rara | Rara | Legendaria | Legendaria |
|---|---|---|---|---|---|---|---|
| **NORMAL** | 1 moneda de rebirth | PERRO +5 % dinero | GATO +5 % rebirth | RANA +10 % dinero | PEZ +10 % rebirth | ZORRO +25 % dinero | DRAGON +25 % rebirth |
| **ORO** | 10 monedas | HAMSTER +5 % velocidad | CONEJO +10 % suerte | BUHO +1 % crítico | LORO +20 % dinero | TORTUGA +40 % pinchos | OSO +40 % daño |
| **HIELO** | 100 monedas | PINGUINO +10 % velocidad | FOCA +20 % dinero | PULPO +40 % pinchos | LOBO +40 % daño | YETI +2 % crítico | BALLENA +80 % rebirth |
| **COSMICA** | 1 punto de ascensión | ROBOT +20 % velocidad | ALIEN +40 % suerte | FANTASMA +3 % crítico | FENIX +80 % rebirth | UNICORNIO +160 % dinero | GOLEM +160 % daño |

Cada stat se suma entre mascotas (`Game.pet_bonus`) y entra en su multiplicador como un factor más: dinero (`value_multiplier`), monedas de rebirth (`rebirth_multiplier`), daño (`damage_multiplier`), velocidad (`charge_multiplier`), suerte (`luck`) y Pinchos (`spike_dps`) **multiplican**; el crítico **suma** probabilidad (`crit_chance`). La velocidad que pasa del tope de la carga se vuelve daño (ver [[Eventos, habilidades y libreta#Golpe perfecto, racha, zona dorada y combo|tope de la carga]]).

> Los números de ORO, HIELO y COSMICA están **puestos a ojo**, sin simular (`dev/balance.gd` no modela las mascotas). Ver [[Pendiente]].

## Cómo se ve

- **Panel** (estilo cartón de la tienda): a la izquierda la página (nombre de la máquina entre dos flechas y "2/4"), la máquina y **TIRAR n** con el icono de lo que se paga (cerdito o estrella morada); a la derecha seis fichas de papel con la mascota (en silueta si no la tienes), cinco puntos de nivel y lo que da. Arriba a la derecha el saldo (monedas o puntos de ascensión, según la máquina).
- **Dibujos**: las 24 mascotas tienen dibujo propio (el usuario pidió mascotas distintas, no las mismas recoloreadas). Las máquinas de oro, hielo y cósmica sí son la de siempre repintada (`_Recolor`, por luminancia).
- **La tirada** se ve en grande en el **centro de la pantalla**, como la carta de la intro (sin letrero arriba): todo se oscurece, la cápsula cae rebotando, tiembla tres veces cada vez más fuerte y revienta en un destello blanco; sale la mascota con rebote y flotando, con **dos brillos del color de su rareza** girando detrás (en modo suma de luz) y chispas de ese color; debajo "NUEVA MASCOTA!" o el nivel, el nombre, la rareza y lo que da. **Clic, Espacio o Esc** lo cierra.
