# Eon Wake — Reference Sheet v2.3.0

---

## CONTROLS

| Input | Action |
|-------|--------|
| Arrow keys / D-pad | Move / Attack |
| · (center D-pad) | Wait one turn. On a corpse tile: destroy/consume (2 turns) |
| `E` / Interact btn | Interact with adjacent tile |
| `>` / Stairs btn | Descend stairs / Enter side dungeon |
| `I` / Inv btn | Open inventory |
| `F` / Fire btn | Ranged attack |
| `R` / Reload btn | Reload ranged weapon |
| `T` / Throw btn | Throw equipped throwable (3-turn cooldown) |
| `X` / Spell btn | Cast equipped spell |
| `S` / Save btn | Save game (3 slots) |

---

## STATS

| Stat | Effect |
|------|--------|
| **STR** | Melee damage bonus: `floor((STR-5)/2)` |
| **AGI** | Damage reduction `floor((AGI-4)/3)`. Dodge `AGI×2%` cap 30%. Ranged evasion `floor(AGI/4)%`. Crit chance `AGI×1.5%` cap 20% (1.5× dmg). |
| **WITS** | Ranged/spell damage: `floor((WITS-5)/2)` |
| **SP** | Regens 1 per 4 turns |

---

## CLASSES

| Class | Sym | HP | SP | STR | AGI | WITS | Melee | Ranged | Special |
|-------|-----|----|----|-----|-----|------|-------|--------|---------|
| War Veteran | V | 14 | 6 | 7 | 4 | 3 | Fists | Pistol | Tough fighter |
| Rogue | R | 10 | 8 | 4 | 8 | 4 | Dagger | — | High AGI/dodge/crit |
| Occultist | O | 8 | 14 | 3 | 4 | 9 | Fists | — | Forbidden Tome |
| Exorcist | E | 10 | 10 | 5 | 4 | 7 | Fists | — | Holy Symbol |
| Ghoul | G | 18 | 4 | 9 | 5 | 2 | Claws | — | +1 HP/kill. Consumes corpses (2-turn wait → heal 25% of enemy's base HP) |
| Flagellant | F | 12 | 6 | 6 | 5 | 4 | Whip (2-tile) | Revolver | 1.5× dmg below 50% HP |

All classes start with Bandage×2, Whiskey×1. 15% chance starting weapon has an affix.

---

## MADNESS & RUIN

### Ruin (permanent, never decreases)

| Ruin | Overflow message | Madness resets to | Permanent effects |
|------|-----------------|-------------------|------------------|
| 0 | — | — | None |
| 1 | "Something tore. You felt it." | 20 | Phantoms appear. Atmospheric messages begin. |
| 2 | "The edges of things no longer hold their shape." | 40 | HP display scrambled ±25%. Phantoms 2× frequent. |
| 3 | "You cannot remember what silence sounded like." | 60 | Hysteria threshold drops to 50. Cultist chanting at 30+. |
| 4 | "There is almost nothing left of you." | 80 | Phantoms deal 1 HP on tile entry. Random enemy +5 HP every 8 turns. |
| 5 | "You are no longer here." | — | Death. No reprieve. |

Ruin bar hidden at Ruin 0, appears crimson on first overflow.

### Madness Tiers

| Tier | Normal | Ruin 3+ | Active effects |
|------|--------|---------|----------------|
| Uneasy | 0–40 | 0–20 | None |
| Fractured | 41–74 | 21–50 | +1–2 enemy damage. Atmospheric logs. Cultist chanting. |
| Hysteria | 75–99 | 51–99 | +2–3 enemy damage. All enemies permanently aware. Dark One double shot (50%). Cultists summon 2 worms. |

### Passive Madness Gain
+1 Madness every 2 turns while in any dungeon (main dungeon, Rat's Nest, Catacombs, Slime Pits). Not on the surface.

### Madness Reset
Descending stairs or exiting an optional dungeon resets Madness to 0 with a unique flavour message.

### Phantom Visions
Appear at Ruin 1+ or Madness 75+. Dim purple in FOV. Fade after 4–7 turns. Max 2 at once. Standing on one: +1 Madness/turn (Ruin 0–3) or +1 HP damage (Ruin 4). Cannot be attacked.

### Madness Gain Sources

| Source | Amount |
|--------|--------|
| Pale Watcher sight | +30 |
| Enter Slime Pits | +8 |
| Enter Catacombs | +8 |
| Mutagen / Primer use | +12 |
| Dark Reflection sight | +6 |
| Unshackle Bound Cultist | +6 |
| Well drink | +10 |
| Elder Thing sight | +4 |
| Symbol activation (obelisk) | +4 |
| Corpse reanimates | +4 |
| Mi-Go sight | +4 |
| The Hollow visible | +4/turn |
| Dark One sight | +3 |
| Watcher in Yellow visible (≤6 tiles) | +2/turn |
| Pale Messenger visible | +2/turn |
| Crawling Choir within 8 tiles | +1/turn |
| Starblood Vial active | +1/turn |
| Phantom vision tile | +1/turn |
| Eldest Hunger melee hit | +2/hit |
| Passive dungeon presence | +1/2 turns |
| Withdrawal (addicted, no pills) | +2/turn |

### Madness Reduction on Kill

| Enemy | Reduction |
|-------|-----------|
| Dark Reflection | −20% current |
| Crawling Choir | −15 |
| The Hollow | −15 |
| Mi-Go | −10 |
| Elder Thing | −10 |
| The Penitent | −10 |
| Eldest Hunger | −10 |
| Mummified Knight | −8 |
| Dark One | −8 |
| The Frayed | −8 |
| The Unravelling | −8 |
| Wraith | −8 |
| Corrosive Horror | −8 |
| Burial Shade | −6 |
| Floating Horror | −6 |
| Pit Lurker | −6 |
| Watcher in Yellow | −6 |
| Spawn of Deep Tongue | −5 |
| Ravenous Zombie | −5 |
| Bone Archer | −4 |
| Reanimated Corpse | −4 |
| Zombie | −3 |
| Plague Rat | −3 |
| Giant Rat | −2 |
| Sewer Rat | −2 |
| Slime | −2 |
| Flesh Scrap | −2 |
| Rat | −1 |

---

## MUTATIONS

### Standard Mutations

| Mutation | Symbol | Effects |
|----------|--------|---------|
| Compounding Muscles | ☠ | AGI crits deal 3× dmg (instead of 1.5×). Bleed on crit (not skeletal/ghost). +1 STR per level-up. |
| Mutant Health | ♥ | 2× starting HP. Double HP on level-up. |
| Quickened Neurons | ⚡ | Attack twice. Dodge = AGI×3% cap 40%. Dodging sets counter-ready (+2 next melee, no HP penalty). |
| Chitinous Hide | ◆ | Flat dmg reduction floor(lvl/2) max 5. Doubled by Iron Vessel. |
| Void-Touched | ○ | Light radius −2. See enemies within 4 tiles through walls. Ranged miss 25% outside FOV. |

### Transformation Primers (mutually exclusive)

| Primer | Transformation | Effects |
|--------|---------------|---------|
| Rat King's Ichor | Iron Vessel ⚙ | Immune to Madness and Infected. Cannot use biologicals. Chitin doubled. |
| Warden's Marrow | Amalgamation ≈ | Acid immune. 18% melee slow. Vulnerable to fire 1.5×. |
| Pale Draught | Void Walker ◎ | Phase Step. 20% Unreadable. 30% Terrifying. +1 SP/kill. −25% MaxHP. |

### Crit System
All players: AGI × 1.5% crit chance (cap 20%) → 1.5× damage.
Compounding Muscles: crits deal 3× damage + Bleed on non-skeletal/ghost targets.
Quickened Neurons: dodging sets counter-ready (+2 bonus on next melee attack).

---

## WEAPONS

### Melee

| Weapon | Base | +1 | +2 |
|--------|------|----|----|
| Fists | 2 | — | — |
| Claws (Ghoul) | 4 | — | — |
| Dagger | 3 | 5 | 7 |
| Whip | 4 | 6 | 8 |
| Pickaxe | 5 | 7 | 9 |
| Bone Club | 6 | 8 | 11 |
| Annihilator | 50 | — | — |

### Ranged

| Weapon | Base | +1 | +2 | Ammo |
|--------|------|----|----|------|
| Pistol | 4 | 6 | 8 | 6 |
| Revolver | 5 | 7 | 9 | 6 |
| Rifle | 7 | 9 | 12 | 3 |
| Shotgun | 10 | 13 | 16 | 2 |
| Infiltrator | 10 | — | — | 4 |

Whip has 2-tile reach (Flagellant). Rifle near-zero miss ≤8 tiles. Shotgun falls off beyond 1 tile. Pistol 6% miss/tile beyond 2. All cap 70%. AGI evasion stacks multiplicatively.

### Weapon Affixes

Melee: Sharp (Bleed 25%, not skeletal), Weighted (+2 always), Serrated (+1 above 50% HP + blood pool), Vampiric (drain +1HP/turn 3t), Venomous (poison 4t), Cruel (+50% vs full-HP).

Ranged: Penetrating (pass-through 50%), Explosive (20% blast), Silenced (reset LKP), Hollow Point (+2 AP), Vampiric, Venomous.

Affix rates: 15% starting, 22% enemy drop, 35% iron chest, 50% gilded chest.

---

## SPELLS

| Spell | Cost | Effect |
|-------|------|--------|
| Forbidden Tome | 3 SP | 4+WITS dmg to all visible. 1.5× vs ghosts. |
| Holy Symbol | 60% MaxSP (min 4) | Banishes undead. −40% current Madness. |
| Hymn of Unmaking | 5 SP | floor(Madness/10) dmg to all visible. 1.5× vs ghosts. |

---

## THROWABLES (3-turn cooldown)

| Item | Effect |
|------|--------|
| Dynamite | 12 dmg, 5×5 blast |
| Molotov | 5 dmg + Burn, 3×3 |
| Vial of Slime | 8 dmg + Acid, 3×3 |

---

## CONSUMABLES

| Item | Symbol | Effect | Stack |
|------|--------|--------|-------|
| Bandage | ! | +4 HP | 5 |
| MedKit | + | +8 HP | 5 |
| Whiskey Flask | j amber | +6 SP | 5 |
| Bourbon | j dark-red | +10 SP | 5 |
| Strange Elixir | ? | Random effect | 5 |
| Hearty Meal | % | +10 HP, −10 Madness | 5 |
| Serum | ! | Purge status, −20 Madness | 5 |
| Mutagen | ! | 33% mutation, +12 Madness | 5 |
| Blueberry | , | +1 HP, −1 Madness, Confused | 25 |
| Foxglove | f | Melee poison until floor change | 5 |
| Trapezohedron Shard | ◆ | −25 Madness + random teleport | — |
| Starblood Vial | ♥ | +4 WITS 8t, +1 Madness/turn | — |
| Happy Pill | o teal | −5 Madness + Sedated 12–15t. 20% addiction. Ruin 1+ only. | 15 |

### Happy Pill System
- **Sedated** ○ — −1 Madness/turn for 12–15 turns.
- **Addiction** — 20% per use, permanent.
- **Withdrawal** ◙ — +2 Madness/turn when Sedated expires on addicted player. Only cured by another pill. Not curable by Serum.

---

## STATUS EFFECTS

| Status | Symbol | Source | Effect |
|--------|--------|--------|--------|
| Infected | ☣ | Zombies, Rats | Stacks duration. 1–2 dmg/turn. Serum cures. |
| Confused | ✦ | Blueberry | Random movement. |
| Burning | 🔥 | Molotov | Damage/turn. 1.5× Amalgamation. |
| Foxglove Toxin | ✿ | Foxglove | Poisons melee hits. |
| Bleed | — | Sharp/Serrated/CM crit | DoT. Not skeletal/ghost. |
| Poisoned | — | Venomous/rat spit | DoT. |
| Sedated | ○ | Happy Pill | −1 Madness/turn. |
| Withdrawal | ◙ | Addiction expiry | +2 Madness/turn. |
| Starblood | — | Starblood Vial | +4 WITS, +1 Madness/turn, 8 turns. |

---

## CHEST TIERS

| Type | Symbol | Contents | Notes |
|------|--------|----------|-------|
| Standard | □ | Consumables, ammo, Happy Pills (Ruin 1+) | Any floor |
| Iron | ▣ steel-blue | 1× +1 weapon + 50% quality bonus | Floors 3–7, 35% affix |
| Gilded | ◈ gold | 1× +2 weapon + 2 guaranteed bonus + 50% third | Floors 5–10, 50% affix |

Iron/Gilded bonus pool: MedKit, Serum, Hearty Meal, Elixir, Mutagen, Bourbon. Gilded also: Starblood Vial, Trapezohedron Shard. No bandages in Iron/Gilded.
All chests: 15% Mass of Worms spawn. 1% Annihilator. Gilded: 5% Primer added to pool.

---

## CORPSE SYSTEM

- **Drop rate:** 75% of eligible kills drop a corpse `%`
- **Reanimation triggers** (either condition):
  - Player moves 7+ tiles from corpse
  - 8–10 turns pass since corpse dropped (random on spawn)
- **Warning:** One turn before reanimating — *"Something stirs near the [name] corpse..."*
- **Destroy:** Stand on corpse and Wait (·) for 2 turns → corpse is permanently destroyed
- **Ghoul consume:** Same 2-turn wait → corpse consumed, heal `floor(enemy base HP × 0.25)`
- Reanimated corpse deals +4 Madness on appearance

Corpses do not drop from: Worm Mass, Dark Reflection, Flesh Scraps, Crawling Choir, The Unravelling, Zombies (undead), Ghoul, Wraith, Burial Shade, Bone Archer, Mummified Knight, Deathless Warden, Slime, Small Slime, Corrosive Horror, Pit Lurker, Ravenous Zombie.

---

## DUNGEON GENERATION (Main Dungeon)

- **Map size:** 80×50 tiles
- **Rooms:** 50 placement attempts, accepted rooms 5–13×4–10
- **Corner cutting:** Each room corner independently rolls a diagonal cut (0 to floor(min(w,h)/3)), 30% chance any corner stays square — no two rooms look alike
- **Corridors:** L-shaped, connecting rooms in sequence
- **Doors:** Placed at genuine corridor-to-room entry points only. Must have walls on N+S OR E+W. One per wall side per room. No door placed without a connecting corridor.
- **Stairs:** Always placed in the room furthest from the player start room (Manhattan distance)
- **Items:** 4–7 per floor, max 1 per room
- **Enemy count:** `4 + level × 2`

### Special Room Types
**Jail Room** — 20% chance per eligible room (≥8×8), floor 2+. Iron walls, jail floor (dark blue-grey), iron grates on cell fronts. Cells contain 1–2 enemies (50% chance). Chains decoration on walls (15% per eligible tile). Passing through chains alerts all enemies within 6 tiles and plays a metallic rattle sound.

---

## ENEMY STATS & SCALING

Base stats shown. Main dungeon scales at spawn: HP +floor(l×1.8), DMG +floor(l×0.4), STR +floor(l×0.2) where l=floor−1. XP never scales. Obelisk guardians scale with G.dl at summon time.

### Main Dungeon

| Enemy | Unlocks | HP | DMG | AGI | XP | Ranged | Special |
|-------|---------|----|----|-----|----|--------|---------|
| Mass of Worms | 1 | 3 | 1 | 2 | 3 | No | Splits 25%/3t cap 8. isTiny (passes doors/grates). |
| Cultist | 1 | 6 | 2 | 3 | 10 | No | Chants at Fractured+ (summons worms) |
| Zombie | 1 | 12 | 3 | 1 | 15 | No | 40% Infect (stacks). Drops gun. |
| Wolf | 2 | 8 | 3 | 6 | 12 | No | High AGI |
| Deep One | 3 | 12 | 5 | 4 | 22 | No | High dmg melee |
| Dark One | 4 | 14 | 4 | 3 | 25 | Yes | Double shot at Hysteria (50%) |
| Floating Horror | 5 | 10 | 3 | 7 | 20 | Yes | Very high AGI |
| Wraith | 6 | 13 | 5 | 5 | 30 | No | Ghost type |
| Mi-Go | 7 | 18 | 7 | 6 | 50 | Yes | — |
| Ghoul | 8 | 16 | 7 | 5 | 35 | No | — |
| Elder Thing | 9 | 22 | 9 | 4 | 60 | Yes | — |

### Cosmic Horror Encounters (50% per floor, floor 2+)

| Encounter | Floors | Notes |
|-----------|--------|-------|
| Dark Reflection | All | Mirrors player. 30% Trapezohedron Shard drop. |
| Ravenous Zombies ×2 | 3–10 | 35% Infect, 1–2 dmg/turn. Scaled. |
| Bound Cultist → The Hollow | 2–6 | Interact: +6 Madness. Spawns The Hollow. |
| Pale Messenger | 4–8 | Flees. +2 Madness/turn visible. |
| Grave Robber | 1–5 | Flees. Drops item. |
| Corrupted Offering | 2+ | Fake chest, rare loot pool. |
| The Unravelling | 3+ | Ranged. Splits into 2 Flesh Scraps on death. Scaled. |
| The Penitent | 2+ | Kneeling gamble: 25% buff / 40% +12 Mad / 35% hostile. Scaled. |
| Crawling Choir | 4+ | +1 Mad/turn ≤8 tiles. Alerts all on damage. 25% Wound-Script drop. Scaled. |

### Rat's Nest (fixed difficulty)

| Enemy | HP | DMG | AGI | XP | Special |
|-------|----|-----|-----|----|---------|
| Rat | 4 | 1 | 6 | 4 | 40% Infect (stacks). isTiny. |
| Giant Rat | 10 | 3 | 5 | 12 | 40% Infect (stacks). |
| Plague Rat | 5 | 1 | 7 | 15 | 100% Infect, 1–2 dmg/turn, virulent. |
| Sewer Rat | 3 | 1 | 5 | 8 | 40% Infect. Alerts all rats when hit. |
| Rat King | 40 | 7 | 4 | 80 | Spawns 2 rats (Rat/Plague/Sewer) every 2 turns. |

### Catacombs (fixed difficulty)

| Enemy | HP | DMG | AGI | XP | Special |
|-------|----|-----|-----|----|---------|
| Burial Shade | 8 | 3 | 7 | 22 | Ghost. Walks through walls. |
| Bone Archer | 7 | 4 | 4 | 18 | Ranged. Skeletal. |
| Mummified Knight | 20 | 7 | 1 | 40 | Skeletal. |
| Deathless Warden | 40 | 9 | 2 | 150 | Skeletal. Resurrects once. Drops Warden's Marrow + 25% Hymn of Unmaking. |

### Slime Pits (fixed difficulty)

| Enemy | HP | DMG | AGI | XP | Special |
|-------|----|-----|-----|----|---------|
| Slime | 6 | 2 | 2 | 8 | Splits into Small Slimes |
| Small Slime | 3 | 1 | 2 | 4 | — |
| Corrosive Horror | 14 | 4 | 3 | 30 | Acid |
| Pit Lurker | 12 | 5 | 4 | 25 | Hidden until approached |

### Obelisk Guardians (scale with floor level at summon time)

| Guardian | Base HP | Base DMG | XP | Special |
|----------|---------|----------|----|---------|
| Watcher in Yellow | 18 | 4 | 45 | +2 Madness/turn within 6 tiles. Ranged. |
| Spawn of Deep Tongue | 12 | 3 | 30 | Blood pool on death. Not on undead/ghost. |
| The Frayed | 22 | 6 | 55 | 20% hit deflection. Immune physical status. |
| Eldest Hunger | 35 | 10 | 90 | +2 Madness/hit. AGI 1. −20% Madness on death. |

---

## OBELISK EVENT

30% chance floor 2+, in a large room with no existing chest. All 4 diagonal tiles from room center must be floor — if any are walls the event skips entirely. Interact (E or bump) each symbol to summon its guardian (+4 Madness). When all 4 activated and all 4 guardians dead, interact/bump obelisk for a spell (whichever of Tome/Holy/Hymn you don't own). Music plays from first activation until reward.

Standing on an inactive symbol shows a flavour description of the guardian behind it.

---

## SPECIAL TILE RULES

### Iron Grate `≡`
Blocks player and enemy movement. Transparent — FOV and ranged attacks pass through. Ghosts and walksThrough entities pass freely. Tiny entities (worms) pass freely.

### Iron Wall `#` (steel-blue)
Impassable. Blocks FOV. Found in jail rooms.

### Jail Floor `.` (dark blue-grey)
Passable. Distinctive colour differentiates jail interiors from standard dungeon floors.

### Chains `¥` (rust-brown)
Passable decoration. Stepping through triggers a metallic rattle sound and alerts all enemies within 6 tiles to the rattle position.

### Ghost Type (walksThrough)
Wraith, Burial Shade, The Hollow, Dark Reflection. Pass through all terrain freely. Melee: half dmg (min 1). Ranged: 75% dmg. Spells: 1.5× dmg. Immune: Bleed, Poison, Burning, Slow, Serrated blood pool.

### Skeletal Type
Bone Archer, Mummified Knight, Deathless Warden, Skeleton, Giant Skeleton. Immune: Bleed (Sharp), Venomous, Foxglove poison. Holy Symbol banishes.

### isTiny (Tiny entities)
Mass of Worms, Rat (future swarms). Can move through T.DOOR, T.LOCKED_DOOR, T.IRON_GRATE, T.FENCE_GATE. Cannot pass solid walls.

---

## AREA GUIDE

### Surface (dl:0)
Church interior (altar, offerings ×3, confessional −20% Madness one-time, locked vestry with MedKit + Bandages — item TBD). Graveyard (8 tombstones, 1 loot cache, 4 skeleton guards — catacomb entry unlocks after all dead). Forest (berry bushes, foxglove max 4, test dungeon entrances).

### Main Dungeon (floors 1–10)
80×50 map. Corner-cut rooms. Stairs always in furthest room. Iron chests floors 3–7. Gilded chests floors 5–10. Obelisk event 30% chance floor 2+. Jail rooms 20% chance per eligible room floor 2+.

### Rat's Nest
Fixed center den. Hidden chamber west of den — contains Rat Caller's Pipe (6 SP, summons Giant Rat ally with melee bite + venomous spit at range 2–3). Guaranteed corridor from den to stair. Rat King in center spawns 2 rats every 2 turns.

### Catacombs
Fixed layout. Spine 1 (north): 5 alcoves with bone piles (65% chance, 30% skeletal ambush). Spine 2 (south): shrine altars (50% skeleton / 20% −Madness / 15% Holy Symbol / 15% Burial Shades). Ossuary approach corridor. Sealed door (Catacomb Key) → ossuary chest. Deathless Warden guards ossuary.

### Slime Pits
Cellular automaton cavern. Flood-fill guaranteed. Guaranteed spine from stair to center. Acid pools deal damage/turn (Amalgamation immune).

---

## ITEM OBTAINABILITY

| Item | Sources |
|------|---------|
| Forbidden Tome | Obelisk event · Occultist start |
| Holy Symbol | Obelisk event · Catacomb shrine 15% · Floor drop 1–3 (6%) · Exorcist start |
| Hymn of Unmaking | Obelisk event · Deathless Warden true death (25%) |
| Rat Caller's Pipe | Rat's Nest hidden chamber |
| Bone Caller's Staff | Catacombs ossuary chest |
| Trapezohedron Shard | Dark Reflection drop 30% · Gilded chest · Corrupted Offering |
| Wound-Script | Crawling Choir drop 25% · Gilded chest |
| Starblood Vial | Standard chest · Floor drop 5+ (10%) · Corrupted Offering |
| Bourbon | Iron/Gilded chest bonus · Floor drop 4+ (8%) |
| Happy Pill | Floor drop alongside loose ammo (Ruin 1+) · Standard chest (Ruin 1+) |
| Mutagen | Standard/Gilded chest · Floor drops |
| Warden's Marrow | Deathless Warden true death (guaranteed) |
| Rat King's Ichor | Rat King death (guaranteed) |
| Pale Draught | Gilded chest (5% Primer pool) |
| Annihilator | Any chest (1%) |

---

*v2.3.0 — Test dungeon entrances (Rat's Nest burrow, Slime Pit entry) remain in surface forest. Vestry item TBD. Remove test entrances before release.*
