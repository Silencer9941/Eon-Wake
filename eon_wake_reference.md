# Eon Wake — Complete Reference Sheet v4.0.0

---

## VERSION HISTORY

| Version | Summary |
|---------|---------|
| 3.0.0 | BSP generation, weighted room system, room types, plant faction, lighting, settings |
| 3.1.0 | Catacomb rework, Warden's Sword + follower system, mutation upgrades, new enemies |
| 3.2.0 | Spear + Shortsword, multi-affix system, Bone Club knockback, Infiltrator rework |
| 3.3.0 | Inscription system, 15 new tile constants, tabbed inventory overhaul, iron grate consolidation |
| 3.4.0 | Enemy AI overhaul, floor events, Dire/Savage Wolf, Penitent blood trail, encounter system |
| 3.5.0 | The Wasting system (replaces Infect), Purifier item, Gods' Quarrel event |
| 3.5.1 | Stability pass: stale infected refs purged, gods quarrel double-process fix, connectivity check |
| 3.5.2 | Vestry relics (8), Vestry Chest, Large Jail room, Eldritch Warden, Tentacle Cluster, charge-based throwables, Throwing Knife Belt, Blackout Flask, Armor system (6 pieces, 6 affixes) |
| 3.6.0 | Static lighting system (baked per floor), room light profiles, light fixtures, FOV scaling, lantern item, lighting-based hit/detection modifiers, armor tier badges, new armor symbols (⬡/⛨), candelabra now static-only |
| **4.0.0** | **Directional vision + patrol AI, state machine (patrol→investigate→alerted→combat), hearing/noise system, sneak attacks (1.2-1.5×), blind/omniscient enemy flags, 4-state render tints, worm split-only (no autonomous dupe), church catacomb entry via guardian giant skeleton, graveyard overhaul (+6 skeletons), sub-room door fix, wardrobe spawn fix, graveyard cache randomized, large jail disabled** |

---

## ARCHITECTURE

Single-file HTML5 roguelike. Lovecraftian horror. Mobile-first D-pad + keyboard. Turn-based ASCII canvas. Save/load 3 slots (localStorage). Web Audio API. Map `MW=80 MH=50`. `VW=42 VH=20` desktop, `VW=20 VH=12` mobile. `IS_DESKTOP = window.innerWidth>=768 && navigator.maxTouchPoints===0`.

---

## CLASSES

All classes start with: starting weapon, Oil Lantern (180 charges, full), 2 Bandages, 1 Whiskey, class-specific extras.

| Class | Sym | HP | SP | STR | AGI | WITS | Unique |
|-------|-----|----|----|-----|-----|------|--------|
| Grave Digger | G | 16 | 2 | 8 | 4 | 1 | 25% less Madness. Pickaxe 30% Fracture. |
| Rogue | R | 10 | 8 | 4 | 8 | 4 | High dodge/crit. Starts with 3 Blackout Flasks. Sneak attacks hit upper range (+15%). |
| Apostate | A | 10 | 12 | 3 | 4 | 9 | Casting spells restores 1 HP. |
| Inquisitor | I | 10 | 10 | 7 | 4 | 5 | Undead kills restore 2 SP. |
| The Starved | S | 18 | 4 | 9 | 5 | 2 | +1 HP/kill. Corpse consume. 20% chance worm follower on kill. |
| Ascetic | A | 12 | 6 | 6 | 5 | 4 | 1.5× dmg below 50% HP. Whip 2-tile. Starts with Revolver. |

---

## STATS

| Stat | Effect |
|------|--------|
| STR | Melee dmg bonus `floor((STR-5)/2)` |
| AGI | Dmg reduction `floor((AGI-4)/3)`. Dodge `AGI×2%` cap 30%. Crit `AGI×1.5%` cap 20%. |
| WITS | Ranged/spell dmg bonus `floor((WITS-5)/2)` |
| SP | Regens 1 per 4 turns |
| DR | Flat damage reduction: AGI-based + armor + Chitinous Hide stacked |

**Passive HP regen:** `ceil(maxHP×10%)` every 4 turns. Mutant Health → 20%.

---

## LIGHTING SYSTEM

### Architecture

- `G.map.lightLevel` — `Uint8Array(MW×MH)`, baked once per floor generation via `bakeLightLevels(map)`
- Re-baked at: every floor transition (genMap, slime pit, catacombs, rat's nest, surface, load game)
- Light levels: **0=Pitch Black, 1=Dim, 2=Lit, 3=Bright**
- Corridors default to Dim (1). Rooms get a rolled profile. Static fixtures boost local radius via BFS.

### Room Light Profiles

Rolled once at generation via `rollRoomLightProfile(roomType)`:

| Level | Name | Base Weight | Room Overrides |
|-------|------|------------|----------------|
| 0 | Pitch Black | 35% | Crypt +25, Fungal +25, Feeding Ground +15 |
| 1 | Dim | 40% | (default) |
| 2 | Lit | 20% | Chapel +20, Armoury +20 |
| 3 | Bright | 5% | Chapel +10, Armoury +10 |

### Static Fixtures

| Fixture | Radius | Intensity |
|---------|--------|-----------|
| Brazier | 3 | 3 |
| Candelabra | 2 | 2 (static only — cannot be picked up) |
| Oil Lamp | 2 | 3 |
| Fire Tile | 2 | 2 |
| Mushroom | 2 | 1 |

BFS stops at walls — light does not bend around corners.

### Player FOV by Light Level

`getPlayerFOVRadius()` returns `baseRadius(5) + mod`:

| Ambient Level | FOV Mod | Without Lantern |
|---------------|---------|-----------------|
| 0 Pitch | −2 | 3 tiles |
| 1 Dim | 0 | 5 tiles |
| 2 Lit | +1 | 6 tiles |
| 3 Bright | +2 | 7 tiles |

With active Lantern (radius 4): `baseRadius + lanternRadius = 9 tiles`.

### Combat Hit Modifiers

`applyLightToHit(baseChance, targetX, targetY, isRanged)` — checks light at **target's** tile:

| Level | Melee Mod | Ranged Mod |
|-------|-----------|------------|
| 0 Pitch | −30% | −45% |
| 1 Dim | −15% | −22.5% |
| 2 Lit | 0% | 0% |
| 3 Bright | 0% | 0% |

Applied in `doSingleAtk()` (melee), `doFire()` (ranged), and enemy attack block.

---

## OIL LANTERN

```
{id:'lantern', type:'light_source', charges:180, maxCharges:180, radius:4, active:false}
```

- All classes start with one at full charge.
- Equip from inventory (Equipment tab). Button shows **EQUIP → LIGHT → SNUFF**.
- Charges drain **only while active** — 1 per turn. Off = no drain.
- HUD weapon line shows: `♦ON[charges]` or `♦off[charges]`
- Equipment bar shows LIGHT slot with current charge.
- Snuffed automatically when charges reach 0: *"The lantern sputters out."*

---

## DIRECTIONAL VISION + PATROL AI SYSTEM

### Enemy Facing & Vision Cones

Every enemy has:
- `facing` — integer 0-7 (N=0, NE=1, E=2, SE=3, S=4, SW=5, W=6, NW=7)
- `visionCone` — `'narrow'` (1 dir) / `'medium'` (3 dirs) / `'wide'` (5 dirs)
- `visionRange` — base detection range in tiles
- `darkSight` — if true, lighting does not reduce detection range

### Vision in Darkness

`getEffectiveConeRange(e, p)` uses `min(lightAtEnemy, lightAtPlayer)`:

| Combined Light | Range Multiplier |
|---------------|-----------------|
| 0 Pitch | 0.15× (~1 tile) |
| 1 Dim | 0.40× (~3-4 tiles) |
| 2 Lit | 1.00× (full) |
| 3 Bright | 1.00× (full) |

**Pitch black adjacency:** Even when beyond visual range, enemies within 2 tiles have a 15% chance per turn to sense the player anyway (breath, heat, disturbance).

### Enemy Vision Config

| Enemy | Cone | Range | Notes |
|-------|------|-------|-------|
| Skeleton / Armored Skeleton | medium | 8 / 7 | |
| Zombie / Ravenous Zombie | wide | 5 | Poor focus |
| Wolf / Savage Wolf | wide | 10 | Hearing dominant |
| Rat / Giant Rat / Plague Rat | wide | 4-5 | Mostly hearing |
| Cultist / Cultist Gunman | medium | 9 | |
| Feral Fighter / Feral Gunner | medium | 8-9 | |
| Ghoul | wide | 6 | |
| Giant Skeleton | medium | 8 | |
| Bone Archer | narrow | 14 | Sniper focus |
| Mummified Knight | medium | 7 | Stand-and-scan patrol |
| Burial Shade | medium | 10 | |
| Wraith | medium | 10 | darkSight:true |
| Mi-Go | narrow | 12 | darkSight:true |
| Eldritch Warden | medium | 10 | darkSight:true |
| Deathless Warden | medium | 12 | darkSight:true |
| Pale Watcher | narrow | 14 | |

### Blind Enemies

`blind:true` — skip all vision cone processing. Hearing range multiplier ×3.0.

**Blind:** Mass of Worms, Flesh Scrap, Crawling Choir, Infested Zombie, Reanimated Corpse/Zombie, Fly Swarm, Slime variants, Seed Spitter, Giant Fly Trap, Corpse Lily, Spore Puffer.

### Omniscient Enemies

`omniscient:true` — always in combat state, always know player position regardless of distance, walls, or light.

**Omniscient:** Dark Player, The Unravelling.

### State Machine

```
PATROL ──[sees/hears]──► INVESTIGATE ──[8 turns no confirm]──► PATROL
                               │
                          [confirms]
                               ▼
                           ALERTED ──[5 turns no sight]──► INVESTIGATE
                               │
                          [adjacent]
                               ▼
                            COMBAT ──[6 turns no sight]──► INVESTIGATE
```

**State render tints:**
- Patrol: violet `rgba(80,0,120,0.35)` — unaware
- Investigate: orange `rgba(100,50,0,0.45)` — heard something
- Alerted: yellow `rgba(100,90,0,0.45)` — spotted you
- Combat: no tint

### Patrol Behavior

Assigned once at spawn via `assignPatrol(e, room)`. Three patterns:

- **Room Circuit** — 4 waypoints at room corners, cycled clockwise. 0-2 turn pause at each waypoint. Random start offset so guards orbit differently.
- **Stand-and-Scan** — Bone Archer, Rifle Turret, Mummified Knight. Stays in place, rotates through 4 facing directions with pauses. Never moves.
- **Linear** — 2-point back-and-forth along a short path. Used when no room available.

If stuck at a waypoint for 4+ turns, skips to next waypoint automatically.

### Hearing System

`G.p._noiseThisTurn` set by `emitNoise(type)` at each player action:

| Action | Noise Score | Hearing Range |
|--------|-------------|---------------|
| Move (light armor) | 1 | 1.5 tiles |
| Move (no armor) | 2 | 3 tiles |
| Move (heavy armor) | 4 | 6 tiles |
| Melee hit | 3 | 4.5 tiles |
| Ranged silenced | 1 | 1.5 tiles |
| Ranged pistol/revolver | 6 | 9 tiles |
| Ranged shotgun | 8 | 12 tiles |
| Ranged rifle | 10 | 15 tiles |
| Open door | 2 | 3 tiles |
| Break barrel | 4 | 6 tiles |

Hearing range = `noise × 1.5`. Blind enemies use `noise × 3.0`.

### Enemy Detection Range vs Darkness

`LIGHT_DETECT_PENALTY` applied to detection range at **player's tile**:

| Player's Ambient Level | Detection Range Penalty |
|------------------------|------------------------|
| 0 Pitch | −4 tiles |
| 1 Dim | −2 tiles |
| 2-3 Lit/Bright | 0 |

Stacks with light armor `detectReduce`. Bypassed by `darkSight:true` enemies.

---

## SNEAK ATTACKS

Triggered when player melee-attacks an enemy that:
1. Is in state `patrol` or `investigate` (not `alerted` or `combat`)
2. Player is **outside** the enemy's vision cone

**Damage multiplier:** 1.2× to 1.5× (random range). Rogue adds +15%, reliably hitting the upper range (1.35-1.65×).

**On sneak attack:** Enemy immediately enters `combat` state and knows player position.

**Not possible against:** Omniscient enemies, enemies that are already alerted/in combat, blind enemies (they have no cone to be outside of).

---

## MADNESS & RUIN

| Ruin | Resets to | Effects |
|------|-----------|---------|
| 0 | — | Nothing extra. Passive +1 Madness every 4 turns in dungeon. |
| 1 | 20 | Phantoms spawn. Fake atmospheric log messages begin. |
| 2 | 40 | Phantoms 2× frequent. Fake enemy sightings begin. |
| 3 | 60 | Hysteria: all enemies permanently aware. |
| 4 | 80 | Phantoms deal 1 HP. HP display scrambles ±25%. |
| 5 | — | Death |

- Tongue of Ul-Nareth relic: blocks one Ruin increment per floor (silent)
- Warding armor affix: −1 Madness per 4 turns passively
- Descending stairs: resets Madness to 0

---

## THE WASTING

Severity-based. Lasts until cured. No stacking.

| Sev | Name | DMG/t | Stat Drain | Madness | Escalates |
|-----|------|-------|-----------|---------|-----------|
| 1 | Wasting | 1 | — | — | 20t |
| 2 | Festering | 2 | −1 AGI | +1/8t | 15t |
| 3 | Rotting | 3 | −2 AGI −1 STR | +1/4t | 10t |
| 4 | Consumed | 4 | −2 AGI −2 STR −1 WITS | +Ruin/2t | Never |

- **Hit chance to apply Wasting:** 5% per hit for most enemies. Plague Rat: 10%.
- **Serum:** clears Wasting, −20 Madness, 18% chance −1 Ruin.
- **Purifier:** always full cure. Stack ×5.
- **Marrow of Soth-Uvael:** pauses all status ticks 8 turns on new floor.

---

## WEAPONS

### Melee

| Weapon | Base | +1 | +2 | Notes |
|--------|------|----|----|-------|
| Fists | 2 | — | — | |
| Claws | 5 | — | — | Starved only |
| Dagger | 3 | 5 | 7 | |
| Whip | 4 | 6 | 8 | 2-tile reach, Ascetic only |
| Pickaxe | 5 | 7 | 9 | 30% Fracture (Grave Digger) |
| Bone Club | 6 | 8 | 11 | 25% knockback |
| Morningstar | 5 | 7 | 9 | 20% Stagger |
| Shortsword | 4 | 6 | 8 | 2× affix chance |
| Spear | 5 | 7 | 9 | 2-tile reach, cardinal only |
| Annihilator | 50 | — | — | 1% from any chest |
| Warden's Sword | 8 | — | — | Unique. Spectral Edge + Undead Bane + 8% follower |

### Ranged

| Weapon | Base | +1 | +2 | Noise |
|--------|------|----|----|-------|
| Pistol | 4 | 6 | 8 | 6 |
| Revolver | 5 | 7 | 9 | 6 |
| Rifle | 7 | 9 | 12 | 10 (alert radius ~15 tiles) |
| Shotgun | 10 | 13 | 16 | 8 |
| Infiltrator | 20 | — | — | Range 20, 100% accuracy, bypasses all |

Ammo: **Loose Ammunition** only (max 25). Press `[R]` to reload.

### Throwables (charge-based)

| Item | Range | Effect | Max |
|------|-------|--------|-----|
| Stick of Dynamite | 6 | 5×5 blast, 12 dmg | 6 |
| Molotov Cocktail | 5 | 3×3 fire + Burning | 6 |
| Throwing Knife Belt | 8 | Single target, 6+WITS | 6 |
| Vial of Distilled Slime | 4 | 3×3 acid + acid tiles | 6 |

Spawn with 1-5 charges. Destroyed when empty. Emit `throw_item` noise (2 score).

### Weapon Affixes

| Code | Name | Effect |
|------|------|--------|
| Shp | Sharp | 25% Bleed on hit |
| Wgt | Weighted | +2 flat dmg |
| Vmp | Vampiric | Life drain on hit |
| Vnm | Venomous | Poisons on hit |
| Crl | Cruel | +50% dmg on first hit vs full-HP target |
| Pnt | Penetrating | Ranged: chains to next enemy at 50% dmg |
| Exp | Explosive | Ranged: 3×3 splash |
| Sln | Silenced | Ranged: resets enemy awareness on hit, noise score 1 |
| HP | Hollow Point | Ranged: bonus vs unarmored |

---

## ARMOR

### Light Armor (symbol: ⬡)

| Item | Tier | DR | AGI | Stealth |
|------|------|----|-----|---------|
| Leather Coat | Base (green) | +1 | +1 | −1 detect, 15% miss-notice |
| Stalker's Vestment | +1 (blue) | +2 | +2 | −2 detect, 25% miss-notice |
| Grave Cloth | +2 (purple) | +3 | +3 | −3 detect, 35% miss-notice |

### Heavy Armor (symbol: ⛨)

| Item | Tier | DR | AGI | Noise |
|------|------|----|-----|-------|
| Flak Jacket | Base (green) | +3 | −1 | 2-tile noise per action |
| Chain Mail | +1 (blue) | +4 | −2 | 2-tile noise per action |
| Iron Plate | +2 (purple) | +5 | −3 | 2-tile noise per action |

### Armor Affixes

| Code | Name | Effect |
|------|------|--------|
| Rnf | Reinforced | +1 flat DR |
| Shr | Shrouded | Suppresses heavy noise. +10% miss-notice on light. |
| Ins | Insulated | −2 dmg from fire and acid |
| Fit | Well-fitted | +1 AGI on equip |
| Wrd | Warding | −1 Madness per 4 turns |
| Rct | Reactive | 15% reflect 1 dmg on melee hit taken |

**Notes:** AGI modifier applied on equip, reversed on unequip/drop. Light miss-notice stacks with Shroud relic (cap 75%). Source: Closets in Living Spaces (60%) and Armoury rooms (60%).

---

## MUTATIONS

### Standard Pool

| Sym | Mutation | Effect |
|-----|----------|--------|
| ♥ | Mutant Health | 2× max HP. 20% passive regen rate. |
| ⚡ | Quickened Neurons | Attack twice. Dodge AGI×3% cap 40%. Counter on dodge. |
| ◆ | Chitinous Hide | Flat DR `floor(lvl/2)` max 5. |
| ○ | Void-Touched | −2 light radius. See enemies ≤4 tiles through walls. |
| ● | Crimson Tide | +2 HP per kill. |
| ☠ | Compounding Muscles | Crits 3×. Bleed on crit. +1 STR/level. |

### Transformations (Primer items required)

| Sym | Transformation | Effect |
|-----|---------------|--------|
| ⬟ | Iron Vessel | Full immunity to Wasting, poison, fire, acid. −2 AGI. |
| ◈ | Amalgamation | Absorb corpses for HP. Wasting immunity. −2 STR. |
| ◌ | Void Walker | Full invisibility to non-darkSight enemies. Halved HP regen. |

---

## FLOOR EVENTS

10% no event · 70% one · 20% two. Shown in HUD floor-event-row with colors.

| Event | Effect |
|-------|--------|
| Hungry Dark | +1 Madness every 2 turns |
| Weeping Walls | Walls bleed periodically |
| Charnel Wind | Occasional panic flees |
| Feast of Fools | XP doubled |
| Thin Pickings | Reduced item spawns |
| The Pack Runs (fl2+) | Wolf pack spawns |
| A Generous Dead | Extra corpse loot |
| Wrong Kind of Quiet | Extended quiet / unease |
| Gods' Quarrel (fl4+) | Enemies fight each other |
| Dark Floor | −4 to all ambient light levels |

---

## FLOOR WALL COLORS

| Floor | Stone | Color |
|-------|-------|-------|
| 1 | Limestone | `#6a6055` |
| 2 | Sandstone | `#7a6040` |
| 3 | Cobaltite | `#4a5870` |
| 4 | Olivine | `#4a6040` |
| 5 | Basalt | `#3a3a40` |
| 6 | Diorite | `#504858` |
| 7 | Obsidian | `#282030` |
| 8 | Serpentinite | `#304838` |
| 9 | Cinnabar | `#502828` |
| 10 | Voidstone | `#201828` |

Catacombs: bone-white `#e8e0d0`.

---

## ROOM TYPES

| Room | Weight | Floor | Once | Notes |
|------|--------|-------|------|-------|
| Living Space | 8 | 2+ | No | Carpet, bed, table, mirror, candelabra (static), closet → armor |
| Guard Post | 7 | 2+ | No | Partial wall chokepoint |
| Jail Room | 6 | 2+ | No | Iron grate cells. Eldritch Warden 35% at fl5+ |
| ~~Large Jail~~ | — | — | — | **Disabled** — was blocking corridors |
| Barracks | 5 | 2+ | Yes | Bunk beds, footlockers, enemies, throwables |
| Armoury | 5 | 2+ | Yes | Iron chest, braziers, throwable + armor floor item |
| Crypt Room | 4 | 2+ | Yes | Bone piles, undead |
| Feeding Ground | 4 | 2+ | Yes | Ravenous Zombies, gore |
| Chapel | 4 | 2+ | Yes | Pews, altar, candelabra (static), confessional |
| Overgrown Room | 4 | 3+ | Yes | Vines, plant enemies |
| Slime Chamber | 3 | 3+ | Yes | Acid pools, slimes, Vial of Slime |
| Torture Chamber | 3 | 2+ | Yes | Chains, gore, iron chest |
| Fungal Hollow | 3 | 3+ | Yes | Mushrooms, fungal enemies |
| Ritual Room | 2 | 3+ | Yes | Obelisk encounter |

---

## SURFACE — CHURCH LAYOUT (14×14)

Interior tiles cx+0 to cx+13, walls at cx−1 and cx+14.

| Position | Contents |
|----------|----------|
| cy+1, cx+1 | Catacomb Entry (hidden as WALL until Giant Skeleton slain) |
| cy+1, cx+12 | Stairs Down |
| cy+3, aisleX | Altar (bumping spawns Giant Skeleton guardian) |
| cy+3, aisleX±2 | Candelabra (flanking altar, static light source) |
| cy+5, aisleX | Lectern |
| cy+7, cy+9, cy+11 | Pews: left block cx+2→cx+5, right block cx+9→cx+12 |
| cy+8, cy+9, cy+11, cy+12, cx+1 | Offering boxes (west wall) |
| cy+8, cy+10, cy+12, cx+13 | Confessional booths (east wall) |
| cy+14 center | Door (south wall entrance) |

**Catacomb Entry flow:** Player bumps altar → Giant Skeleton spawns (isChurchGuardian:true) → Kill it → WALL at cy+1,cx+1 becomes CATACOMB_ENTRY → Log: *"A hidden passage opens in the corner of the church!"*

---

## GRAVEYARD

- 4 fixed skeleton guards at set positions
- **6 additional regular skeletons** scattered randomly on grass tiles
- Graveyard catacomb entry **removed** — entry now exclusively through the church
- Gate: two FENCE_GATE tiles, shifted right of the church door column
- Tomb cache slots: **2 randomly selected** from all tombstone slots (was always bottom-right corner — fixed)

---

## VESTRY CHEST

One-time per run. Located in church vestry. Grants 2 of 8 relics (no duplicates). `G._vestryOpened` persisted in save.

### Vestry Relics

| Relic | Effect |
|-------|--------|
| Seal of Yath-Goren | On death: 20% chance nearby enemies enter Gods' Quarrel |
| Tongue of Ul-Nareth | Once per floor: silently blocks one Ruin increment |
| Brand of Keth-Sulaan | Enemies in FOV 3+ turns → Marked → guaranteed crit |
| Canticle of Vor-Naaketh | Double SP regen. After cast: next enemy to spot you silenced 3t |
| Eye of Ith-Karaal | Phantom contact drains Madness. Reveals hidden enemies ≤4 tiles |
| Marrow of Soth-Uvael | On stairs: all statuses paused 8 turns on new floor |
| Threshold of Naath-Kerul | Doors you pass through lock behind you 3 turns |
| Shroud of Ul-Shivaar | 40% chance enemies fail to notice you on first FOV entry |

---

## ENCOUNTER SYSTEM

One random encounter per floor (weighted). Occupies one room.

| Encounter | Floor | Notes |
|-----------|-------|-------|
| Ravenous Zombies | 3+ | Pack encounter |
| Bound Cultist | 2-6 | Chained cultist + obelisk |
| Pale Messenger | 4-8 | Elite spectral, flees when seen |
| Grave Robber | 2-5 | Armed enemy + loot |
| Corrupted Offering | 4+ | Cursed chest |
| The Unravelling | 3+ | Omniscient, reality distortion |
| The Penitent | 3+ | Blood trail, high damage |
| Crawling Choir | 4+ | Blind, constant Madness drain |
| Tentacle Cluster | 3+ | 2-4 stationary Tentacles + Oil Lamps |

---

## TENTACLE CLUSTER

- **Tentacle** — `❧` deep purple. 12 HP. Stationary. 2-tile reach. 50% hit chance. 5 DMG. darkSight:true.
- Double damage from fire and acid. No corpse. XP = 8% of `xpNext` per kill.
- One-time 20 Madness on first cluster sight.
- **Oil Lamp** — bump to knock over: 3×3 fire explosion (12 dmg, 24 to tentacles), creates FIRE_TILEs.
- **Fire Tiles** — passable. 2 dmg/turn player (4 to tentacles). Expire after 10 turns.

---

## WORM MASS BEHAVIOR

- **Worms do NOT duplicate autonomously** (removed in v4.0.0).
- **Split on hit only:** 25% chance per player strike to spawn a new worm mass adjacent. Capped at 3 total splits per original worm.
- `blind:true` — relies entirely on hearing (range ×3.0).
- No corpse.

---

## ENEMY AI REFERENCE

### State Transitions

| State | Exit Condition | Next State |
|-------|---------------|------------|
| Patrol | Sees player | Alerted |
| Patrol | Hears player | Investigate |
| Investigate | Sees player | Alerted |
| Investigate | 8 turns no contact | Patrol |
| Alerted | 5 turns no sight/sound | Investigate |
| Alerted | Adjacent to player | Combat |
| Combat | 6 turns no sight/sound | Investigate |

### Patrol Types

| Type | Behavior |
|------|----------|
| Room Circuit | 4 corner waypoints, clockwise, 0-2 turn pause at each |
| Stand-and-Scan | Stays in place, rotates through N/E/S/W facing on timer |
| Linear | 2-point back-and-forth |

### Special Behaviors

- **darkSight:true enemies** (Wraith, Mi-Go, Eldritch Warden, Deathless Warden, Tentacle) — ignore darkness detection penalties. Full vision range regardless of ambient light.
- **blind:true enemies** — skip vision entirely. Hearing range ×3.0.
- **omniscient:true enemies** (Dark Player, Unravelling) — always in combat state.
- **2-tile universal awareness** — any enemy within 2 tiles of player always detects, regardless of cone or light.
- **Blackout Flask** — alerted enemies blind 2t, then search last position 6t.
- **Heavy armor** — emits noise 4, alerts enemies within ~6 tiles per action.

---

## CORPSE SYSTEM

50% drop rate. Color = enemy color darkened (50% R, 30% G/B).

**No corpse:** Fly Swarm, Seed Spitter, Giant Fly Trap, Corpse Lily, Reanimated Corpse, Reanimated Zombie, Infested Zombie, Tentacle, Phantom, Worm Mass, Slime variants, Burial Shade, Bone Archer, Mummified Knight, Deathless Warden, Zombie, Ravenous Zombie, Wraith, Ghoul, Dark Player, Unravelling, Flesh Scrap, Crawling Choir, Eldritch Warden, Armored Skeleton.

---

## XP SOURCES

- Enemy kills (×`G._xpMult` floor event multiplier)
- Tentacle kills: 8% of `xpNext` each
- Item pickups: 1-6% of `xpNext` (excludes corpses, ammo)
- Chest opens: various flat amounts
- Inscription reads: 5-7% of `xpNext`

---

## CATACOMB FLOW

1. Kill Giant Skeleton guardian in church → CATACOMB_ENTRY revealed at church NW corner
2. Enter Catacombs (dungeon level preserved, restored on exit)
3. Three spines. Ossuary chests with relics/items.
4. Deathless Warden room (darkSight:true, 40 HP, omniscient-tier threat) → drops Catacomb Key + Warden's Marrow
5. Warden's Chest (key required) → Warden's Sword + Bone Caller's Staff

---

## SAVE SYSTEM

3 slots, localStorage. Persists: `G._vestryOpened`, `G.returnDl`, `p.wasting`, `p.armor` (armorId), `p.lightSource` (lightSourceId), all equipment slots, mutations, floor state.

`map.lightLevel` (Uint8Array) — **not** persisted (not JSON-serializable). Re-baked from `map.rooms` and `room.lightProfile` on load. `room.lightProfile` (plain number) **is** persisted and reused, so room lighting is consistent across saves.

---

## INVENTORY

5 tabs: ⚔ Melee | ▶ Ranged | ⛨ Equipment (relics + armor + lantern) | ⚗ Consumables | ? Misc

Equipment bar: Melee · Ranged · Spell · Throw · Relic · Armor · **LIGHT**

Tier badges: STD green, +1 blue, +2 purple, UNIQUE orange. Applied to both weapons and armor.

Action button labels by item type:
- Weapons/armor/relics: EQUIP / UNEQUIP
- Light source: EQUIP → LIGHT (on) → SNUFF (off)
- Consumables: USE
- DROP always available

---

## TILE CONSTANTS (T.*)

```
WALL:0  FLOOR:1  DOOR:2  SD:3  SU:4  CHEST:5  CHEST_OPEN:6  DOOR_OPEN:7
GRASS:8  TREE:9  BUSH:10  FENCE:11  FENCE_GATE:12  TOMB:13  STONE_FLOOR:14  PATH:15
OFFERING:16  OFFERING_OPEN:17  ALTAR:18  CATACOMB_ENTRY:21
OSSUARY_CHEST:22  OSSUARY_OPEN:23  SEALED_DOOR:25  SLIME_FLOOR:26
ACID_POOL:27  SLIME_PIT_ENTRY:28  TELEPORT:29  WELL:31  CONFESSIONAL:32
CANDELABRA:33  LOCKED_DOOR:34  IRON_CHEST:35  IRON_CHEST_OPEN:36
GILDED_CHEST:37  GILDED_CHEST_OPEN:38  BLOOD:39  BONE_PILE:41
OBELISK:42  OBELISK_ACTIVE:43  IRON_WALL:46  IRON_GRATE:47
JAIL_FLOOR:48  CHAINS:49  GORE:50  CRYPT_FLOOR:51  MOLD_FLOOR:52
SLIME_WALL:53  CARPET_RED:54–CARPET_WHITE:60  BED_HEAD:61  BED_BODY:62  BED_FOOT:63
CHAIR:64  TABLE:65  FOOTLOCKER:66  FOOTLOCKER_OPEN:67  BROKEN_DOOR:68
MOSS_FLOOR:69  VINE:70  MOSSY_STONE:71  MUSHROOM:72  THORN_BUSH:73
BRAZIER:77  MARBLE_WALL:78  FLAGSTONE:79  FUNGAL_WALL:80  FUNGAL_FLOOR:81
BLOOD_WALL:82  RUBBLE:83  PULPED_CORPSE:84  MIRROR:85  COBWEB:86
CRATE:87  STATUE:88  PILLAR:90  VASE:92  FOUNTAIN:93
BARREL:94  BOOKCASE:95  LECTERN:96  PEW:97  CLOSET:98  STONE_SLAB:99
CRACKED_WALL:100  ROCKS:102  VESTRY_CHEST:103
FIRE_TILE:104  OIL_LAMP:105
```

**Special notes:**
- `FIRE_TILE:104` — passable, 2 dmg/turn, 5×5 reddish-orange glow, expires 10 turns, re-bakes local light on create/destroy
- `OIL_LAMP:105` — impassable, bump to explode in 3×3 fire
- `VESTRY_CHEST:103` — one-time per run
- `CANDELABRA:33` — static light source only, cannot be interacted with or picked up
- Barrel bump: smashes, 25% loot
- Closet bump: 60% armor drop (fixed — was always finding no wall-adjacent candidates)
- Beds, candelabras, pews: **passable** (player and enemies can walk through)

---

## KNOWN NOTES / DEFERRED

- Large Jail disabled — needs corridor-safe rework before re-enabling
- Spider system planned
- Library room type planned
- Furniture interactions (statue, bookcase spell, fountain) deferred
- ES Module conversion discussed but not started
- XP flat-rate system discussed but not yet overhauled
- Enemy directional sight is implemented; **sneak system visual feedback** (indicator when behind enemy) not yet added
- `G._candleUsed` flag now unused — can be cleaned up
