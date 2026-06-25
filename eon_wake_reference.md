# Eon Wake — Reference Sheet v3.0.0

---

## VERSION HISTORY

| Version | Summary |
|---------|---------|
| 2.0.0 | Full Madness/Ruin rework, phantom visions, enemy behaviour at high madness |
| 2.1.0 | Jail room type, Iron Grate, isTiny flag, chain rattle |
| 2.2.0 | Corpse system — 75% drop rate, reanimation triggers, destroy/consume mechanic |
| 2.3.0 | Dungeon gen overhaul — 80×50 map, corner cutting, furthest-stair, passive dungeon madness |
| 3.0.0 | BSP dungeon generation, auxiliary rooms, hybrid corridors, connectivity check, new room types, class rework, bump-to-interact, tombstone death records |

---

## CONTROLS

| Input | Action |
|-------|--------|
| Arrow keys / D-pad | Move / Attack / Bump interact |
| · (center D-pad) | Wait. On corpse: destroy/consume (2 turns). On symbol/teleport: progress wait |
| `[ G ] Get` | Pick up item on current tile |
| `[ C ] Close` | Close adjacent open door or gate (appears contextually) |
| `[ > ] Stairs` | Descend stairs / Enter side dungeon |
| `[ I ] Items` | Open inventory |
| `[ F ] Fire` | Ranged attack |
| `[ R ] Reload` | Reload ranged weapon |
| `[ T ] Throw` | Throw equipped throwable (3-turn cooldown) |
| `[ X ] Spell` | Cast equipped spell |
| `[ S ] Save` | Save game (3 slots) |

### Bump Interactions (walk into tile to trigger)
All tile interactions happen on contact — no separate Interact button.

| Tile | Interaction |
|------|------------|
| Door (closed) | Opens |
| Locked Door | Uses Church Key if held |
| Sealed Door | Uses Catacomb Key if held |
| Chest (all tiers) | Opens immediately |
| Offering box | Opens |
| Berry bush | Picks blueberry |
| Foxglove | Picks up |
| Well | Drink (+MaxHP, +10 Madness) |
| Confessional | Hear whisper, −20% Madness (once) |
| Candelabra | Take candle, +1 light radius (once) |
| Bone pile | Search (loot or ambush) |
| Tombstone | Read death record modal |
| Obelisk | Interact (collect spell reward) |

### Wait Interactions (stand on tile, press Wait)
| Tile | Turns | Effect |
|------|-------|--------|
| Corpse | 2 | Destroy (or The Starved consumes) |
| Symbol (inactive) | 2 | Activates, summons guardian |
| Teleport circle | 2 | Teleports |
| Catacomb Altar | 1 | Shrine interaction |
| Church Altar | 1 | Altar interaction |
| Penitent | 1 | Gamble interaction |

---

## STATS

| Stat | Effect |
|------|--------|
| **STR** | Melee damage bonus: `floor((STR-5)/2)` |
| **AGI** | Damage reduction `floor((AGI-4)/3)`. Dodge `AGI×2%` cap 30%. Crit chance `AGI×1.5%` cap 20% (1.5× dmg). |
| **WITS** | Ranged/spell damage: `floor((WITS-5)/2)` |
| **SP** | Regens 1 per 4 turns |

---

## CLASSES

| Class | Sym | HP | SP | STR | AGI | WITS | Start | Unique |
|-------|-----|----|----|-----|-----|------|-------|--------|
| Grave Digger | G | 16 | 2 | 8 | 4 | 1 | Pickaxe | 25% less Madness. Pickaxe: 30% Fracture (−2 STR −1 AGI, stacks 2×, not on boneless/ghosts) |
| Rogue | R | 10 | 8 | 4 | 8 | 4 | Dagger | High AGI/dodge/crit |
| Apostate | A | 10 | 12 | 3 | 4 | 9 | Tome + Dagger | Casting spells restores 1 HP |
| Inquisitor | I | 10 | 10 | 7 | 4 | 5 | Morningstar + Holy Symbol | Undead kills restore 2 SP |
| The Starved | S | 18 | 4 | 9 | 5 | 2 | Claws | +1 HP/kill. Corpse consume (2-turn wait → heal 25% enemy base HP) |
| Ascetic | A | 12 | 6 | 6 | 5 | 4 | Whip + Revolver | 1.5× dmg below 50% HP. Whip 2-tile reach. |

All classes start with Bandage×2, Whiskey×1. 15% chance starting weapon has an affix.

---

## MADNESS & RUIN

### Ruin

| Ruin | Overflow message | Madness resets to | Effects |
|------|-----------------|-------------------|---------|
| 0 | — | — | None |
| 1 | "Something tore. You felt it." | 20 | Phantoms. Atmospheric messages. |
| 2 | "The edges of things no longer hold their shape." | 40 | Phantoms 2× frequent. |
| 3 | "You cannot remember what silence sounded like." | 60 | HP display scrambled ±25%. Hysteria at 50. Cultist chanting at 30+. |
| 4 | "There is almost nothing left of you." | 80 | Phantoms deal 1 HP. Random enemy +5 HP every 8 turns. |
| 5 | "You are no longer here." | — | Death. |

### Madness Tiers

| Tier | Normal | Ruin 3+ | Effects |
|------|--------|---------|---------|
| Uneasy | 0–40 | 0–20 | None |
| Fractured | 41–74 | 21–50 | +1–2 enemy dmg. Atmospheric logs. Cultist chanting. |
| Hysteria | 75–99 | 51–99 | +2–3 enemy dmg. All enemies aware. Dark One double shot. Cultists summon worms. |

**Passive dungeon madness:** +1 every 2 turns while in any dungeon (not surface).
**Stair descent / dungeon exit:** resets Madness to 0.
**Serum:** 18% chance to reduce Ruin by 1.

### Madness Gain Sources (selected)

| Source | Amount |
|--------|--------|
| Pale Watcher sight | +30 |
| Well drink | +10 |
| Enter Catacombs / Slime Pits | +8 |
| Mutagen / Primer use | +12 |
| Corpse reanimates | +4 |
| Symbol activation | +4 |
| Passive dungeon | +1/2 turns |
| Withdrawal | +2/turn |

---

## MUTATIONS

### Standard

| Mutation | Symbol | Effects |
|----------|--------|---------|
| Compounding Muscles | ☠ | Crits deal 3×. Bleed on crit. +1 STR/level-up. |
| Mutant Health | ♥ | 2× HP. |
| Quickened Neurons | ⚡ | Attack twice. Dodge AGI×3% cap 40%. Counter-ready on dodge. |
| Chitinous Hide | ◆ | Flat dmg reduction floor(lvl/2) max 5. Doubled by Iron Vessel. |
| Void-Touched | ○ | −2 light radius. See enemies ≤4 tiles through walls. |

### Transformations (Primers)

| Primer | Transformation | Effects |
|--------|---------------|---------|
| Rat King's Ichor | Iron Vessel ⚙ | Madness immune. No biologicals (uses Grease Tube instead). Chitin doubled. |
| Warden's Marrow | Amalgamation ≈ | Acid immune. 18% slow. 1.5× fire damage. |
| Pale Draught | Void Walker ◎ | Phase Step. 20% Unreadable. 30% Terrifying. +1 SP/kill. −25% MaxHP. |

---

## WEAPONS

### Melee

| Weapon | Base | +1 | +2 | Special |
|--------|------|----|----|---------|
| Fists | 2 | — | — | — |
| Claws (The Starved) | 4 | — | — | — |
| Dagger | 3 | 5 | 7 | — |
| Whip | 4 | 6 | 8 | 2-tile reach (Ascetic) |
| Pickaxe | 5 | 7 | 9 | Grave Digger: 30% Fracture |
| Bone Club | 6 | 8 | 11 | — |
| Morningstar | 5 | 7 | 9 | 20% Stagger (−2 STR −1 AGI, 2 turns) |
| Annihilator | 50 | — | — | 1% from any chest |

### Ranged

| Weapon | Base | +1 | +2 | Ammo |
|--------|------|----|----|------|
| Pistol | 4 | 6 | 8 | 6 |
| Revolver | 5 | 7 | 9 | 6 |
| Rifle | 7 | 9 | 12 | 3 |
| Shotgun | 10 | 13 | 16 | 2 |
| Infiltrator | 10 | — | — | 4 |

### Weapon Affixes

Melee: Sharp (Bleed 25%), Weighted (+2 always), Serrated (+1 above 50% HP), Vampiric (drain 3t), Venomous (poison 4t), Cruel (+50% vs full HP).
Ranged: Penetrating (pass-through 50%), Explosive (20% blast), Silenced (reset LKP), Hollow Point (+2 AP), Vampiric, Venomous.

Affix rates: 15% starting, 22% enemy drop, 35% iron chest, 50% gilded chest.

---

## SPELLS

| Spell | Cost | Effect |
|-------|------|--------|
| Forbidden Tome | 3 SP | 4+WITS dmg all visible. 1.5× ghosts. |
| Holy Symbol | 60% MaxSP min 4 | Banishes undead. −40% Madness. |
| Hymn of Unmaking | 5 SP | floor(Mad/10) dmg all visible. 1.5× ghosts. |

---

## THROWABLES (3-turn cooldown)

| Item | Effect |
|------|--------|
| Dynamite | 12 dmg, 5×5 |
| Molotov | 5 dmg + Burn, 3×3 |
| Vial of Slime | 8 dmg + Acid, 3×3 |

---

## CONSUMABLES

| Item | Effect | Stack |
|------|--------|-------|
| Bandage | +4 HP | 5 |
| MedKit | +8 HP | 5 |
| Grease Tube | +6 HP (Iron Vessel only) | 5 |
| Whiskey Flask | +6 SP | 5 |
| Bourbon | +10 SP | 5 |
| Strange Elixir | Random | 5 |
| Hearty Meal | +10 HP −10 Madness | 5 |
| Serum | Purge status −20 Madness 18% −1 Ruin | 5 |
| Mutagen | 33% mutation +12 Madness | 5 |
| Blueberry | +1–4 HP −1–4 Madness Confused | 25 |
| Foxglove | Melee poison until floor change | 5 |
| Trapezohedron Shard | −25 Madness + teleport | — |
| Starblood Vial | +4 WITS 8t +1 Madness/turn | — |
| Happy Pill | −5 Madness + Sedated 12–15t. 20% addiction. Ruin 1+. | 15 |

**Happy Pill system:** Sedated −1 Mad/turn. Addiction: +2 Mad/turn on expiry (Withdrawal) until next pill.
**Iron Vessel:** MedKit and Bandage drops replaced by Grease Tube everywhere.

---

## CORPSE SYSTEM

- **Drop rate:** 75% of eligible kills
- **Reanimation:** player moves 7+ tiles away OR 8–10 turns pass → 1-turn warning → reanimates (+4 Madness)
- **Destroy:** Wait on corpse 2 turns
- **The Starved consume:** same 2 turns → heals floor(enemy base HP × 0.25)

---

## CHEST TIERS

| Type | Symbol | Contents | Floors |
|------|--------|----------|--------|
| Standard □ | Consumables, ammo | Any |
| Iron ▣ | +1 weapon + quality bonus | 3–7 |
| Gilded ◈ | +2 weapon + 2–3 bonuses | 5–10 |

All chests: 15% worm mass, 1% Annihilator. Gilded: 5% Primer.

---

## DUNGEON GENERATION v3.0.0

### BSP Partition
Map (80×50) recursively divided into partitions. Split chance: depth 1=50%, depth 2=67%, depth 3=33%. Minimum partition: 16×12. Produces 4–16 leaf partitions per floor — varies every run.

### Main Rooms
One per partition. Occupies 45–75% of partition area. Corner cutting applied independently per corner. Room offset is randomised within the partition.

### Auxiliary Rooms
Each main room tries to sprout up to 3 aux rooms (max), one per side, each at 75% chance. Aux rooms are 3–8 × 3–6 tiles. Connection to parent: 50% direct open archway, 50% doorway. Aux rooms go through the special room replacement pass.

### Corridors
Main room pairs connected by: direct wall opening (if adjacent, gap ≤2 tiles) OR L-shaped corridor (if separated). L-corridors: 15% chance to be 2 tiles wide.

### Connectivity
BFS flood fill from start room after generation. Any unreachable room receives an emergency corridor carved to the nearest reachable floor tile (no door).

### Door Placement
Perimeter scan of every room. Door placed at corridor-to-room entry if: tile is a corridor (not inside a room), has walls on N+S OR E+W, no adjacent door. One door per wall side per room.

### Special Room Types (priority order, main + aux eligible)

| Room | Chance | Min size | Floor | Contents |
|------|--------|---------|-------|----------|
| Ritual Room | 25%, once | 10×8 | 2+ | Stone floor, blood/gore scatter, Obelisk + 4 symbols, 2 Cultists |
| Armoury | 25%, once | 7×6 | 2+ | Stone floor, guaranteed iron chest, 1–2 melee enemies |
| Guard Post | 30%, multiple | 8×6 | 2+ | Partial wall chokepoint with 1-tile gap, 1–2 enemies on far side |
| Crypt Room | 20%, once | 8×6 | 2+ | Crypt floor, bone piles on walls, Catacomb Entry, 2 Burial Shades |
| Slime Chamber | 20%, once | 8×6 | 3+ | Mold floor, slime walls, acid pools, Slime Pit Entry, 2 Slimes |
| Jail Room | 20%, multiple | 8×8 | 2+ | Iron walls, jail floor, iron grate cells, chains, enemies in cells |

### Stairs
Always placed in the room with greatest Manhattan distance from start room.

### Items
4–7 per floor, max 1 per room.

### Enemy Count
`4 + level × 2`

### Enemy Floor Ranges

| Enemy | Floors |
|-------|--------|
| Mass of Worms | 1–4 |
| Cultist | 1–5 |
| Zombie | 1–6 |
| Wolf | 2–6 |
| Deep One | 3–7 |
| Dark One | 4–8 |
| Floating Horror | 4–8 |
| Wraith | 5–9 |
| Mi-Go | 6–10 |
| Ghoul | 7–10 |
| Elder Thing | 8–10 |

---

## SPECIAL TILE RULES

| Tile | Symbol | Colour | Rule |
|------|--------|--------|------|
| Iron Grate | ≡ | Gunmetal | Impassable (ghosts/walksThrough free). Transparent. |
| Iron Wall | # | Steel blue | Impassable. Blocks FOV. Jail perimeter. |
| Jail Floor | . | Dark blue-grey | Passable. |
| Chains | ¥ | Rust brown | Passable. Step-on: metallic rattle, alert enemies ≤6 tiles. |
| Gore | % | Dark crimson | Passable decoration. Ritual rooms only. |
| Crypt Floor | . | Dark grey-green | Passable. Crypt rooms. |
| Mold Floor | . | Very dark green | Passable. Slime chambers. |
| Slime Wall | # | Wet dark green | Impassable. Blocks FOV. Slime chamber perimeter. |

**Ghost type (walksThrough):** pass all terrain freely. Melee half dmg. Ranged 75%. Spells 1.5×. Immune Bleed/Poison/Burn.
**Skeletal type:** Immune Bleed, Venomous, Foxglove. Holy Symbol banishes.
**isTiny:** Worms. Pass through doors, locked doors, grates, gates.

---

## GRAVEYARD — TOMBSTONE SYSTEM

Death records saved to localStorage (max 20, newest first). Each death records: name, class, floor/area, level, Ruin, turns, kills, cause, and a one-liner death quip.

**Tombstones** fill the left half of the graveyard in a uniform 3-tile spacing grid. Empty slots show an unmarked grave message. One tombstone holds a loot cache.

**Interaction:** Bump tombstone → death record modal appears (name, class, floor, level, Ruin, turns, kills, quip, cause). Close modal → tombstone collapses → cache items drop.

**Name entry:** Optional text input at character creation. If blank, uses class name.

---

## ENEMY STATS & SCALING

Scaling: HP +floor(l×1.8), DMG +floor(l×0.4), STR +floor(l×0.2) where l=floor−1. XP never scales.

### Main Dungeon (base stats)

| Enemy | HP | DMG | AGI | XP | Special |
|-------|----|-----|-----|----|---------|
| Mass of Worms | 3 | 1 | 2 | 3 | Splits. isTiny. |
| Cultist | 6 | 2 | 3 | 10 | Chants at Fractured+ |
| Zombie | 12 | 3 | 1 | 15 | 40% Infect (stacks) |
| Wolf | 8 | 3 | 6 | 12 | High AGI |
| Deep One | 12 | 5 | 4 | 22 | — |
| Dark One | 14 | 4 | 3 | 25 | Ranged. Double shot at Hysteria. |
| Floating Horror | 10 | 3 | 7 | 20 | Ranged. Very high AGI. |
| Wraith | 13 | 5 | 5 | 30 | Ghost. |
| Mi-Go | 18 | 7 | 6 | 50 | Ranged. |
| Ghoul | 16 | 7 | 5 | 35 | — |
| Elder Thing | 22 | 9 | 4 | 60 | Ranged. |

### Obelisk Guardians (scale with floor)

| Guardian | HP | DMG | XP | Special |
|----------|----|-----|----|---------|
| Watcher in Yellow | 18 | 4 | 45 | +2 Madness/turn ≤6 tiles |
| Spawn of Deep Tongue | 12 | 3 | 30 | Blood pool on death |
| The Frayed | 22 | 6 | 55 | 20% deflect. Immune physical status. |
| Eldest Hunger | 35 | 10 | 90 | +2 Madness/hit. −20% Madness on death. |

---

## OBELISK EVENT

Appears in Ritual Room (25% chance, floor 2+). Wait 2 turns on each SYMBOL_INACTIVE tile to activate. When all 4 active and guardians dead, bump/wait on obelisk to claim a spell.

---

## ITEM SOURCES

| Item | Sources |
|------|---------|
| Forbidden Tome | Obelisk · Apostate start |
| Holy Symbol | Obelisk · Catacomb shrine 15% · Floor 1–3 (6%) · Inquisitor start |
| Hymn of Unmaking | Obelisk · Deathless Warden true death (25%) |
| Warden's Marrow | Deathless Warden (guaranteed) |
| Rat King's Ichor | Rat King (guaranteed) |
| Pale Draught | Gilded chest (5%) |
| Annihilator | Any chest (1%) |
| Trapezohedron Shard | Dark Reflection (30%) · Gilded chest · Corrupted Offering |
| Wound-Script | Crawling Choir (25%) · Gilded chest |
| Grease Tube | Standard/Iron/Gilded chest · Enemy drops (Iron Vessel only — replaces MedKit/Bandage) |

---

*v3.0.0 — Test dungeon entrances remain in surface forest. Vestry item TBD. Catacomb surface access planned for removal.*
