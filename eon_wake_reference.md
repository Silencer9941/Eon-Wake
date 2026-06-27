# Eon Wake — Complete Reference Sheet v3.2.0

---

## VERSION HISTORY

| Version | Summary |
|---------|---------|
| 3.0.0 | BSP generation, weighted room system, new room types, plant faction, lighting, settings |
| 3.1.0 | Catacomb rework, Warden's Sword + follower system, mutation upgrades, new enemies, new rooms |
| 3.2.0 | Spear + Shortsword weapons, multi-affix system, Bone Club knockback, Infiltrator rework, Crimson Tide + Consuming Mass + Clockwork Body mutations |

---

## ARCHITECTURE

Single-file HTML5 roguelike. Lovecraftian horror. Mobile-first D-pad + keyboard. Turn-based ASCII canvas. Save/load (3 slots, localStorage). Web Audio API music and SFX. Map `MW=80 MH=50`. `VW=42 VH=20` desktop, `VW=20 VH=12` mobile (overridable via settings). Cell size scales via `getMaxCellSize()`.

---

## CLASSES

| Class | Sym | HP | SP | STR | AGI | WITS | Unique |
|-------|-----|----|----|-----|-----|------|--------|
| Grave Digger | G | 16 | 2 | 8 | 4 | 1 | 25% less Madness. Pickaxe 30% Fracture. |
| Rogue | R | 10 | 8 | 4 | 8 | 4 | High dodge/crit. |
| Apostate | A | 10 | 12 | 3 | 4 | 9 | Casting spells restores 1 HP. |
| Inquisitor | I | 10 | 10 | 7 | 4 | 5 | Undead kills restore 2 SP. |
| The Starved | S | 18 | 4 | 9 | 5 | 2 | +1 HP/kill. Corpse consume. |
| Ascetic | A | 12 | 6 | 6 | 5 | 4 | 1.5× dmg below 50% HP. Whip 2-tile reach. |

All classes start with Bandage ×2, Whiskey ×1. 15% chance starting weapon has an affix.

---

## STATS

| Stat | Effect |
|------|--------|
| STR | Melee dmg bonus `floor((STR-5)/2)` |
| AGI | Dmg reduction `floor((AGI-4)/3)`. Dodge `AGI×2%` cap 30%. Crit `AGI×1.5%` cap 20%. |
| WITS | Ranged/spell dmg bonus `floor((WITS-5)/2)` |
| SP | Regens 1 per 4 turns |

---

## PASSIVE HP REGEN

Every 4 turns, if 4+ turns have passed since last damage taken: restore `ceil(maxHP × 10%)`. Silent. Mutant Health doubles this to 20% per 4 turns.

---

## MADNESS & RUIN

Passive +1 Madness every 4 turns in any dungeon. Stairs/exit resets to 0.

| Ruin | Resets to | Effects |
|------|-----------|---------|
| 1 | 20 | Phantoms appear |
| 2 | 40 | Phantoms 2× frequent |
| 3 | 60 | HP display scrambled ±25%. Hysteria at Madness 50. |
| 4 | 80 | Phantoms deal 1 HP. Random enemy +5 HP/8 turns (cap +10 per enemy). |
| 5 | — | Death |

**Madness tiers:** Uneasy 0-40 / Fractured 41-74 / Hysteria 75-99 (adjusted thresholds at Ruin 3+).

---

## WEAPONS

### Melee

| Weapon | Base | +1 | +2 | Notes |
|--------|------|----|----|-------|
| Fists | 2 | — | — | No affix |
| Claws | 5 | — | — | Starved only. No affix. |
| Dagger | 3 | 5 | 7 | Sound: Slash |
| Whip | 4 | 6 | 8 | 2-tile reach. Ascetic only. Sound: Slash |
| Pickaxe | 5 | 7 | 9 | 30% Fracture (Grave Digger). Sound: Chunk |
| Bone Club | 6 | 8 | 11 | 25% knockback on hit (STR-based distance). Wall stun 1t. Sound: Clang |
| Morningstar | 5 | 7 | 9 | 20% Stagger (−2 STR −1 AGI, 2t). Sound: Clang |
| Shortsword | 4 | 6 | 8 | High affix chance (2× base). Sound: Slash |
| Spear | 5 | 7 | 9 | 2-tile reach, cardinal directions only. Sound: Chunk |
| Annihilator | 50 | — | — | 1% from any chest. No affix. |
| Warden's Sword | 8 | — | — | Unique. Spectral Edge + Undead Bane + 8% follower proc. 1 affix slot. |

### Ranged

| Weapon | Base | +1 | +2 | Ammo | Notes |
|--------|------|----|----|------|-------|
| Pistol | 4 | 6 | 8 | 6 | Sound: Pop |
| Revolver | 5 | 7 | 9 | 6 | Sound: Pop |
| Rifle | 7 | 9 | 12 | 3 | Sound: Bang |
| Shotgun | 10 | 13 | 16 | 2 | Sound: Blast |
| Infiltrator | 20 | — | — | 4 | 100% accuracy. Range 20. Bypasses dodge, ghost reduction, flat dmg reduction. |

### Bone Club Knockback
25% chance per hit. Distance: 1 tile (STR < 9), 2 tiles (STR 9+). Enemy knocked into impassable tile is Stunned for 1 turn.

### Spear Reach
Attacking in a cardinal direction toward an enemy 2 tiles away triggers a thrust attack without moving. Diagonal attacks still work at range 1.

### Weapon Tiers & Affix Slots

| Tier | Affix Slots |
|------|-------------|
| Base weapon | 1 slot |
| +1 weapon | 1-2 slots |
| +2 weapon | 2-3 slots |
| Shortsword (any tier) | Double base affix chance |

### Melee Affixes

| Affix | Short | Effect |
|-------|-------|--------|
| Sharp | Shp | 25% Bleed on hit (not vs skeletons) |
| Weighted | Wgt | +2 flat damage always |
| Vampiric | Vmp | Life drain 3 turns on non-undead |
| Venomous | Vnm | Poison 4 turns on non-undead |
| Cruel | Crl | +50% dmg on first hit (full HP target) |

### Ranged Affixes

| Affix | Short | Effect |
|-------|-------|--------|
| Penetrating | Pnt | 50% chance: passes through, hits next enemy for 50% dmg |
| Explosive | Exp | 20% chance: 1-tile blast, 3 dmg to nearby enemies |
| Silenced | Sln | Resets enemy last-known-position on hit |
| Hollow Point | HP | +2 flat damage always |
| Vampiric | Vmp | Life drain 3 turns on non-undead |
| Venomous | Vnm | Poison 4 turns on non-undead |

Affix rates: 15% starting weapon, 22% enemy drop, 35% iron chest, 50% gilded chest.

### Spells

| Spell | Cost | Effect |
|-------|------|--------|
| Forbidden Tome | 3 SP | 4+WITS dmg all visible. 1.5× vs ghosts. |
| Holy Symbol | 60% maxSP (min 4) | Banishes all undead. −40% Madness. |
| Hymn of Unmaking | 5 SP | floor(Mad/10) dmg all visible. 1.5× vs ghosts. |

### Throwables (3-turn CD)

| Item | Effect | Source |
|------|--------|--------|
| Dynamite | 12 dmg, 5×5 blast | Footlocker (fl4+), Barracks, Armoury |
| Molotov | 5 dmg + Burn, 3×3. 2× vs plants | Same as Dynamite |
| Slime Vial | 8 dmg + Acid, 3×3 | Floor loot pool |

---

## MUTATIONS

### Standard Pool (from Mutagen)

| Sym | Mutation | Effect |
|-----|----------|--------|
| ♥ | Mutant Health | 2× max HP. 20% passive HP regen per 4 turns (doubled). |
| ⚡ | Quickened Neurons | Attack twice. Dodge AGI×3% cap 40%. Counter-ready on dodge (+2 next melee). |
| ◆ | Chitinous Hide | Flat dmg reduction `floor(lvl/2)` max 5. Doubled by Iron Vessel. |
| ○ | Void-Touched | −2 light radius. See enemies ≤4 tiles through walls. |
| ● | Crimson Tide | +2 HP per kill. |
| ☠ | Compounding Muscles | Crits deal 3×. Bleed on crit. +1 STR per level-up. |

### Transformation Tier (Primers)

| Sym | Transformation | Primer | Effects | Downside |
|-----|---------------|--------|---------|----------|
| ⚙ | Iron Vessel | Rat King's Ichor | Madness immune. No biological healing (Grease Tube only). Chitin reduction doubled. | — |
| ≈ | Amalgamation | Warden's Marrow | Acid immune. 18% slow on hit. | 1.5× fire damage taken |
| ◎ | Void Walker | Pale Draught | Phase Step. 20% Unreadable. 30% Terrifying. +1 SP/kill. | −25% max HP |

### Upgrade Mutations (unobtainable without paired Transformation)

| Sym | Upgrade | Requires | Replaces | Effect |
|-----|---------|----------|----------|--------|
| ◎ | Void Sight | Void Walker | Void-Touched | Enhanced wall-sight. Stronger perception. |
| ⚙ | Clockwork Body | Iron Vessel | Chitinous Hide | Keeps flat dmg reduction. Immune to ALL status effects. Moving alerts enemies ≤9 tiles. Waiting makes no noise. |
| ☠ | Consuming Mass | Amalgamation | Crimson Tide | +2 HP/kill. 20% chance permanent +1 maxHP per kill (cap +10/run). Enemies that see you may flee (15%) or rage (+2 dmg, 85%). |

**Upgrade trigger:** Works both directions — get the transformation after the mutation, or the mutation after the transformation. Both upgrade immediately on obtain.

---

## STATUS EFFECTS

| Effect | Source | Mechanical Effect | Cure | Immune (Clockwork/Iron) |
|--------|--------|-------------------|------|------------------------|
| Bleed | Sharp affix, Thorn Bush, Compounding crit | 1 dmg/turn. Blood splatters nearby tiles. | Serum, time | ✓ |
| Poison | Venomous affix, Fly Trap | 1-2 dmg/turn | Serum | ✓ |
| Burning | Molotov, fire | 1-3 dmg/turn 3-5 turns | Time | ✓ |
| Infected | Zombie, Fly Swarm | Ongoing dmg | Serum | ✓ |
| Confused | Vine, Blueberry | 50% random movement | Time | ✓ |
| Rooted | Giant Fly Trap | No move/ranged/spell | Kill Fly Trap | ✓ |
| Sedated | Happy Pill | −1 Madness/turn, −1 AGI | Time | ✓ |
| Slowed | Cobweb | +1 turn cost per tile | Move away | ✗ |
| Fractured | Pickaxe (GD) | −2 STR −1 AGI per stack (max 2) | Time | ✗ |
| Staggered | Morningstar | −2 STR −1 AGI for 2 turns | Time | ✗ |
| Stunned | Bone Club wall knock | Skip 1 turn | — | ✗ |

---

## CONSUMABLES

| Item | Effect | Stack |
|------|--------|-------|
| Bandage | +4 HP | 5 |
| Medical Kit | +8 HP | 5 |
| Grease Tube | +6 HP (Iron Vessel only) | 5 |
| Whiskey Flask | +6 SP | 5 |
| Bourbon | +10 SP | 5 |
| Strange Elixir | Random | 5 |
| Hearty Meal | +10 HP, −10 Madness | 5 |
| Serum | Purge status, −20 Madness, 18% −1 Ruin | 5 |
| Mutagen | 33% standard mutation + 12 Madness | 5 |
| Blueberry | +1-4 HP, −1-4 Madness, Confused | 25 |
| Foxglove | Melee poison until floor change | 5 |
| Trapezohedron Shard | −25 Madness + teleport | — |
| Starblood Vial | +4 WITS 8 turns, +1 Madness/turn | — |
| Happy Pill | −5 Madness + Sedated 12-15 turns. 20% addiction. Ruin 1+. | 15 |
| Hedgeapple | Random: +6 HP / +6 SP / +1 STR / +8 Madness | — |

---

## LOOT CONTAINERS

| Container | Empty? | Contents | Notes |
|-----------|--------|----------|-------|
| Standard Chest □ | 35% | Consumables | |
| Iron Chest ▣ | Never | Weapons (base or +1), 35% quality bonus, 50% bonus item, 5% Primer | Key check for Warden's chest |
| Gilded Chest ◈ | Never | +2 weapons, quality items, 10% Primer | |
| Footlocker ▤ | 35% | Ranged weapon + drinks. Throwable (fl4+, 40%) | |
| Crate ■ | 35% | 1-3 healing items | Breaks on open |
| Ossuary Chest ◆ | Never | Relic of Interred / Shroud Fragment / Mutagen (1-in-3 each) | Catacomb only |
| Warden's Chest ▣ | Never | Warden's Sword + Bone Caller's Staff | Requires Catacomb Key |

### Primer Drop Rates

| Source | Chance | Notes |
|--------|--------|-------|
| Iron Chest | 5% | One-time per run per primer |
| Gilded Chest | 10% | One-time per run per primer |
| Rat King (death) | Guaranteed | Rat King's Ichor |
| Deathless Warden (true death) | Guaranteed | Warden's Marrow |
| Pale Draught | Gilded Chest | |

---

## ENEMY REFERENCE

### Scaling Formula
`HP + floor((level-1) × 1.4)` | `DMG + floor((level-1) × 0.3)` | `STR + floor((level-1) × 0.2)` | XP unchanged.

### Main Dungeon Pool

| Name | HP | DMG | STR | AGI | XP | Floors | Flags |
|------|----|-----|-----|-----|----|--------|-------|
| Mass of Worms | 3 | 1 | 1 | 2 | 3 | 1-4 | tiny, splits |
| Rat | 4 | 1 | 2 | 6 | 4 | 1-4 | |
| Sewer Rat | 3 | 1 | 2 | 5 | 8 | 1-3 | tiny, alerts nest |
| Skeleton | 5 | 2 | 2 | 2 | 6 | 1-4 | immune status |
| Small Slime | 3 | 1 | 2 | 3 | 4 | 1-4 | tiny |
| Cultist | 6 | 2 | 3 | 3 | 10 | 1-5 | summons Worms at Fractured+ |
| Zombie | 12 | 2 | 4 | 1 | 15 | 1-6 | isZombie, reanimates kills (cap 3) |
| Wolf | 8 | 3 | 4 | 6 | 12 | 2-6 | high AGI |
| Deep One | 12 | 5 | 6 | 4 | 22 | 3-7 | |
| Dark One | 14 | 4 | 5 | 3 | 25 | 4-8 | ranged, double shot at Hysteria |
| Floating Horror | 10 | 3 | 3 | 7 | 20 | 4-8 | ranged, high AGI |
| Armored Skeleton | 14 | 3 | 4 | 1 | 22 | 5-7 | flat 2 dmg reduction, immune status |
| Wraith | 13 | 5 | 6 | 5 | 30 | 5-9 | ghost |
| Mi-Go | 18 | 7 | 8 | 6 | 50 | 6-10 | ranged |
| Ghoul | 16 | 7 | 8 | 5 | 35 | 7-10 | |
| Elder Thing | 22 | 9 | 10 | 4 | 60 | 8-10 | ranged |

### Room-Specific Enemies

| Name | HP | DMG | Room | Flags |
|------|----|-----|------|-------|
| Cultist Gunman | 8 | 3 | Chapel | ranged, no summon |
| Feral Fighter | 12 | 4 | Barracks | melee |
| Feral Gunner | 8 | 4 | Barracks | ranged |
| Rifle Turret | 10 | 4 | Barracks fl3+ | stationary, ranged |
| Ravenous Zombie | 18 | 5 | Feeding Ground | isZombie, heals 1/hit, fl2+ |
| Infested Zombie | 14 | 3 | Fungal Hollow | isPlant, reanimates kills as Infested (cap 3) |
| Infested Cultist | 8 | 2 | Fungal Hollow | isPlant |
| Infested Fighter | 14 | 4 | Fungal Hollow fl4+ | isPlant, grenade/5t (5 dmg 3×3) |

### Plant Enemies (isPlant — fireVuln 2×, immune Poison/Infect, no plant-on-plant combat)

| Name | HP | DMG | Behaviour |
|------|----|-----|-----------|
| Seed Spitter | 8 | 2 | Stationary. Ranged range 2-9. |
| Giant Fly Trap | 14 | 3 | Stationary. Roots + poisons adjacent non-plants. |
| Corpse Lily | 20 | 0 | Stationary. Summons 1 Fly Swarm/2t (cap 5). Spreads moss on death. |
| Fly Swarm | 4 | 1 | tiny, summoned only, 10% infect |

### Catacomb Enemies

| Name | HP | DMG | Notes |
|------|----|-----|-------|
| Skeleton | 5 | 2 | Immune status |
| Burial Shade | 8 | 3 | Ghost |
| Bone Archer | 7 | 4 | Ranged |
| Mummified Knight | 20 | 7 | Heavy |
| Deathless Warden | 40 | 9 | Boss. Resurrects once at 20 HP. Drops Catacomb Key + Warden's Marrow on true death. Boss music. |

### Obelisk Guardians (noMainDungeon, XP=0 on respawn)

| Name | HP | DMG | Special |
|------|----|-----|---------|
| The Watcher in Yellow | 18 | 4 | +2 Madness/turn ≤6 tiles. Ranged. |
| Spawn of the Deep Tongue | 12 | 3 | Blood pool on death. |
| The Frayed | 22 | 6 | 20% deflect. Immune physical status. |
| The Eldest Hunger | 35 | 10 | +2 Madness/hit. −20% Madness on death. |

---

## ZOMBIE & PLANT FACTION BEHAVIOUR

**Zombies (isZombie):** Attack non-zombie, non-plant enemies on sight when player first enters a breached room. Do not fight each other. Kills reanimate as Reanimated Zombie (cap 3 chain).

**Infested Zombies (isPlant, isInfested):** Plant faction — fight regular zombies. Kills reanimate as Infested Zombie (cap 3 chain).

**Followers (Warden's Sword):** isFollower entities follow the player, attack any enemy in player FOV. Player bumping a follower swaps positions. Full XP on follower kills. Silently dismissed on any floor transition.

**Pulped Corpses:** Cannot reanimate, cannot be consumed by The Starved.

---

## WEIGHTED ROOM SYSTEM

| Room | Weight | Min Size | Fallback | Once | Floor |
|------|--------|----------|----------|------|-------|
| Living Space | 8 | 6×5 | 5×4 | No | 2+ |
| Guard Post | 7 | 8×6 | 6×5 | No | 2+ |
| Jail Room | 6 | 8×8 | 7×6 | No | 2+ |
| Barracks | 5 | 8×6 | 6×5 | Yes | 2+ |
| Armoury | 5 | 7×6 | 6×5 | Yes | 2+ |
| Crypt Room | 4 | 8×6 | 6×5 | Yes | 2+ |
| Feeding Ground | 4 | 8×6 | 6×5 | Yes | 2+ |
| Chapel | 4 | 7×6 | 6×5 | Yes | 2+ |
| Overgrown Room | 4 | 6×5 | 5×4 | Yes | 3+ |
| Slime Chamber | 3 | 8×6 | 6×5 | Yes | 3+ |
| Torture Chamber | 3 | 6×5 | 5×4 | Yes | 2+ (needs Jail) |
| Fungal Hollow | 3 | 6×5 | 5×4 | Yes | 3+ |
| Ritual Room | 2 | 10×8 | 9×7 | Yes | 3+ |

Target: leave ~3 generic rooms per floor. Start room always generic.

### Passes (after room assignment)

**Decoration Pass:** All rooms get blood/gore scatter (4-6%), 0-2 bone piles near walls, 30% candelabra near center, 20% broken door, 40% random furniture, cobwebs in neglected rooms (2×2 clusters cap 3), 25% crates (1-2).

**Collapse Pass (fl3+):** 1-2 rooms per floor get ~20% floor tiles converted to Rubble. Furniture can be crushed. Ritual Room exempt.

**Breach Pass (fl3+):** 25% chance. One room gets blood walls, blood/gore floor, 2-4 Breach Zombies. Zombies activate (attack all room enemies) when player first sees them.

---

## ROOM TYPES

| Room | Walls | Floor | Contents | Enemies |
|------|-------|-------|----------|---------|
| Living Space | Stone | Carpet | Bed, table, chair, footlocker, mirror | — |
| Guard Post | Stone | Stone | Chokepoint gap | 1-2 generic |
| Jail Room | Iron | Jail | Iron grates, chains | Jail enemies |
| Barracks | Stone | Stone | Multi-bed, footlockers, broken door, 1-2 throwables | Feral Fighter×2, Feral Gunner, Rifle Turret (fl3+) |
| Armoury | Stone | Stone | Iron chest, braziers, 1 throwable | 1-2 melee |
| Chapel | Marble | Flagstone | Altar, candelabras, bone piles, confessional | Cultist×2, Cultist Gunman |
| Crypt Room | Stone | Crypt | Bone piles, catacomb entry | Burial Shade×2 |
| Slime Chamber | Slime | Mold | Acid/slime patches, slime pit entry | Slimes |
| Torture Chamber | Iron | Jail | Chains, gore, blood | Sadists/Masochists |
| Overgrown Room | Stone | Moss | Vines, mushrooms, thorn bushes, trees | Seed Spitter, Fly Trap, Corpse Lily |
| Fungal Hollow | Fungal | Fungal | Mushrooms, vines | Infested Zombie, Cultist, Fighter |
| Feeding Ground | Stone | Stone | Blood/gore, 3-6 pulped corpses | Ravenous Zombie×2, Zombie×2 |
| Ritual Room | Stone | Stone | Obelisk + 4 symbols | Cultist×2 |

### Chapel Confessional
One-use. Removes 50% current Madness. 40% chance to purge 1 Ruin if player has Ruin.

### Mirror (Living Space)
One interact per mirror per run. Floor 2+ only. Ruin-scaled Dark Reflection spawn: 0%/10%/25%/40%/50%/100% at Ruin 0-4. On miss: *"Gaze into the abyss, and it might stare back..."*

---

## OBELISK SEQUENCE

1. Claim spell → Obelisk flashes red/orange (5-turn countdown)
2. Turn 3: symbols glow green (3-turn countdown)
3. Turn 5 of symbol countdown: 4 guardians respawn aware
4. Turn 5 of obelisk countdown: 7×7 diamond blast (60% maxHP + 15 Madness if in radius). Guardians take 100% HP.

---

## CATACOMBS

Access via grave to the right of the church (CATACOMB_ENTRY tile). −2 light radius on entry.

**Layout:** Three horizontal spines (y=6, y=20, y=33) connected by vertical corridors. North alcoves off spine1 with pocket rooms. Approach corridor leads to ossuary at bottom-center. Warden room east of ossuary.

**Ossuary:** 3 skeletal guards (random from Skeleton/Armored Skeleton/Bone Archer/Mummified Knight pool). Ossuary chest (south wall center) — rewards: Relic of the Interred / Shroud Fragment / Mutagen.

**Warden Room:** Accessible via open passage from ossuary east wall. Deathless Warden + Warden's iron chest (requires Catacomb Key from Warden's true death drop).

**Warden Loot (true death):** Catacomb Key (guaranteed), Warden's Marrow Primer (guaranteed), Hymn of Unmaking (25%).

**Catacomb Key unlocks:** Warden's iron chest on spine1 → Warden's Sword + Bone Caller's Staff.

**Tile colors:** Walls bone-white `#e8e0d0`, floor `#c8bfb0`, stone floor `#bdb5a5`.

**Bone piles:** Bump to interact. 40% crumbles to dust. Otherwise: loot roll + 30% ambush regardless of loot. Ambush draws from skeletal pool, spawns adjacent, immediately aware.

---

## LIGHTING SYSTEM

Built once on floor entry (`buildLightMap()`). Applied to visible tiles only during render.

| Source | Radius | Tint |
|--------|--------|------|
| Candelabra | Full room | Yellow `#ffcc44` at 40% |
| Brazier (vestry) | Full room | Orange `#ff8800` at 40% |
| Brazier (dungeon) | Radius 3 BFS | Orange `#ff8800` at 40% |
| Mushroom | Radius 3 BFS | Blue `#6090ff` at 40% |

---

## SETTINGS

| Setting | Options |
|---------|---------|
| Music / SFX | On/Off |
| Colorblind Mode | None / Deuteranopia / Protanopia / Tritanopia / Achromatopsia |
| Brightness | 50-150% slider |
| HUD Text Size | S / M / L / XL |
| Tile Symbol Size | S / M / L |
| Mobile Viewport | Cols 14/20/26/32, Rows 8/12/16/20 |
| Desktop Viewport | Cols 28/42/56/70, Rows 14/20/26/32 |

---

## TILE CONSTANTS (T.*)

```
WALL:0  FLOOR:1  DOOR:2  SD:3  SU:4  CHEST:5  CHEST_OPEN:6  DOOR_OPEN:7
GRASS:8  TREE:9  BUSH:10  FENCE:11  FENCE_GATE:12  TOMB:13  STONE_FLOOR:14  PATH:15
OFFERING:16  OFFERING_OPEN:17  ALTAR:18  BUSH_BERRY:19  BURROW:20  CATACOMB_ENTRY:21
OSSUARY_CHEST:22  OSSUARY_OPEN:23  ALCOVE:24  SEALED_DOOR:25  SLIME_FLOOR:26
ACID_POOL:27  SLIME_PIT_ENTRY:28  TELEPORT:29  FOXGLOVE:30  WELL:31  CONFESSIONAL:32
CANDELABRA:33  LOCKED_DOOR:34  IRON_CHEST:35  IRON_CHEST_OPEN:36  GILDED_CHEST:37
GILDED_CHEST_OPEN:38  BLOOD:39  CATACOMB_ALTAR:40  BONE_PILE:41  OBELISK:42
OBELISK_ACTIVE:43  SYMBOL_INACTIVE:44  SYMBOL_ACTIVE:45  IRON_WALL:46  IRON_GRATE:47
JAIL_FLOOR:48  CHAINS:49  GORE:50  CRYPT_FLOOR:51  MOLD_FLOOR:52  SLIME_WALL:53
CARPET_RED:54  CARPET_BLUE:55  CARPET_GREEN:56  CARPET_PURPLE:57  CARPET_BEIGE:58
CARPET_BLACK:59  CARPET_WHITE:60  BED_HEAD:61  BED_BODY:62  BED_FOOT:63
CHAIR:64  TABLE:65  FOOTLOCKER:66  FOOTLOCKER_OPEN:67  BROKEN_DOOR:68
MOSS_FLOOR:69  VINE:70  MOSSY_STONE:71  MUSHROOM:72  THORN_BUSH:73
OVERGROWN_TREE:74  HOLLOW_TREE:75  HEDGEAPPLE:76  BRAZIER:77  MARBLE_WALL:78
FLAGSTONE:79  FUNGAL_WALL:80  FUNGAL_FLOOR:81  BLOOD_WALL:82  RUBBLE:83
PULPED_CORPSE:84  MIRROR:85  COBWEB:86  CRATE:87
```

---

## KNOWN NOTES / DEFERRED

- Test dungeon entrances (Rat's Nest, Slime Pit on surface) — remove before release
- Vestry item TBD
- Spider system planned (deferred)
- Right half of graveyard — purpose TBD
- `getRuin()` kept for future spell system
- Dark Reflection encounter removed — replaced by mirror interaction
- Inventory UI optimisation planned
- Warden's Sword symbol `†` same as Bone Caller's Staff — consider differentiating
