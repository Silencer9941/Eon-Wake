# Eon Wake — Complete Reference Sheet v3.5.1

---

## VERSION HISTORY

| Version | Summary |
|---------|---------|
| 3.0.0 | BSP generation, weighted room system, room types, plant faction, lighting, settings |
| 3.1.0 | Catacomb rework, Warden's Sword + follower system, mutation upgrades, new enemies |
| 3.2.0 | Spear + Shortsword, multi-affix system, Bone Club knockback, Infiltrator rework |
| 3.3.0 | Inscription system, 15 new tile constants, tabbed inventory overhaul, iron grate consolidation |
| 3.4.0 | Enemy AI overhaul (worm split, zombie scent, cultist patrol, wolf packs), floor events, Dire/Savage Wolf, Penitent blood trail, slime chamber update, enemy density increase |
| 3.5.0 | The Wasting system (replaces Infect), Purifier item, floor event weighted system (10 events), Gods' Quarrel event |
| 3.5.1 | Stability pass: stale infected refs purged, gods quarrel double-process fix, catacomb floor level restore bug fix, post-pass connectivity check, serrated remnants removed, wasting char screen fixed |

---

## ARCHITECTURE

Single-file HTML5 roguelike. Lovecraftian horror. Mobile-first D-pad + keyboard. Turn-based ASCII canvas. Save/load 3 slots (localStorage). Web Audio API. Map `MW=80 MH=50`. `VW=42 VH=20` desktop, `VW=20 VH=12` mobile (overridable via settings).

---

## CLASSES

| Class | Sym | HP | SP | STR | AGI | WITS | Unique |
|-------|-----|----|----|-----|-----|------|--------|
| Grave Digger | G | 16 | 2 | 8 | 4 | 1 | 25% less Madness. Pickaxe 30% Fracture. |
| Rogue | R | 10 | 8 | 4 | 8 | 4 | High dodge/crit. |
| Apostate | A | 10 | 12 | 3 | 4 | 9 | Casting spells restores 1 HP. |
| Inquisitor | I | 10 | 10 | 7 | 4 | 5 | Undead kills restore 2 SP. |
| The Starved | S | 18 | 4 | 9 | 5 | 2 | +1 HP/kill. Corpse consume (50% Wasting risk if corpse was infected). |
| Ascetic | A | 12 | 6 | 6 | 5 | 4 | 1.5× dmg below 50% HP. Whip 2-tile reach. |

---

## STATS

| Stat | Effect |
|------|--------|
| STR | Melee dmg bonus `floor((STR-5)/2)` |
| AGI | Dmg reduction `floor((AGI-4)/3)`. Dodge `AGI×2%` cap 30%. Crit `AGI×1.5%` cap 20%. |
| WITS | Ranged/spell dmg bonus `floor((WITS-5)/2)` |
| SP | Regens 1 per 4 turns |

**Passive HP regen:** `ceil(maxHP × 10%)` every 4 turns if 4+ turns since last damage. Silent. Mutant Health → 20%.

---

## MADNESS & RUIN

Passive +1 Madness every 4 turns in dungeon. Stairs resets to 0.

| Ruin | Resets to | Effects |
|------|-----------|---------|
| 1 | 20 | Phantoms appear |
| 2 | 40 | Phantoms 2× frequent |
| 3 | 60 | HP display scrambled ±25%. Hysteria at Madness 50. |
| 4 | 80 | Phantoms deal 1 HP. Random enemy +5 HP/8 turns (cap +10). |
| 5 | — | Death |

---

## THE WASTING (replaces Infect)

Severity-based, lasts until cured. No stacking from multiple sources.

| Sev | Name | DMG/turn | Stat drain | Madness | Escalates |
|-----|------|----------|-----------|---------|-----------|
| 1 | Wasting | 1 | — | — | 20 turns |
| 2 | Festering | 2 | −1 AGI | +1/8t ×Ruin | 15 turns |
| 3 | Rotting | 3 | −2 AGI −1 STR | +1/4t ×Ruin | 10 turns |
| 4 | Consumed | 4 | −2 AGI −2 STR −1 WITS | +Ruin/2t | Never |

- Stat drains restored on cure
- HUD border tints per severity: green→yellow-green→amber→dark bile
- Player symbol tints to severity color
- Sources: Zombie (20%), Ravenous Zombie (35%), Rats (40%), Plague Rat (100%), Sewer Rat (40%), Fly Swarm (10%), Savage Wolf (35%), Infested variants
- **Serum:** full cure Sev 1-2; Sev 3+: 60% full cure, 40% drops by 1 with log
- **Purifier:** always full cure. Stackable ×5. Floor item + 5% drop from Wasting enemies + standard chests
- Clockwork Body / Iron Vessel: status immune (blocks Wasting)

---

## WEAPONS

### Melee

| Weapon | Base | +1 | +2 | Notes |
|--------|------|----|----|-------|
| Fists | 2 | — | — | |
| Claws | 5 | — | — | Starved only |
| Dagger | 3 | 5 | 7 | |
| Whip | 4 | 6 | 8 | 2-tile reach, Ascetic only |
| Pickaxe | 5 | 7 | 9 | 30% Fracture (GD) |
| Bone Club | 6 | 8 | 11 | 25% knockback, wall stun 1t |
| Morningstar | 5 | 7 | 9 | 20% Stagger |
| Shortsword | 4 | 6 | 8 | High affix chance (2×) |
| Spear | 5 | 7 | 9 | 2-tile reach, cardinal only |
| Annihilator | 50 | — | — | 1% from any chest |
| Warden's Sword | 8 | — | — | Unique. Spectral Edge + Undead Bane + 8% follower proc |

### Ranged

| Weapon | Base | +1 | +2 | Notes |
|--------|------|----|----|-------|
| Pistol | 4 | 6 | 8 | |
| Revolver | 5 | 7 | 9 | |
| Rifle | 7 | 9 | 12 | |
| Shotgun | 10 | 13 | 16 | |
| Infiltrator | 20 | — | — | Range 20, 100% accuracy, bypasses dodge+DR+ghost reduction |

### Multi-Affix System

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
| Weighted | Wgt | +2 flat damage |
| Vampiric | Vmp | Life drain 3t on non-undead |
| Venomous | Vnm | Poison 4t on non-undead |
| Cruel | Crl | +50% dmg on first hit vs full HP target |

### Ranged Affixes

| Affix | Short | Effect |
|-------|-------|--------|
| Penetrating | Pnt | 50% chain to next enemy at 50% dmg |
| Explosive | Exp | 20% 1-tile blast 3 dmg |
| Silenced | Sln | Resets enemy last-known-position |
| Hollow Point | HP | +2 flat damage |
| Vampiric | Vmp | Life drain 3t |
| Venomous | Vnm | Poison 4t |

---

## MUTATIONS

### Standard Pool

| Sym | Mutation | Effect |
|-----|----------|--------|
| ♥ | Mutant Health | 2× max HP. 20% passive regen. |
| ⚡ | Quickened Neurons | Attack twice. Dodge AGI×3% cap 40%. Counter on dodge. |
| ◆ | Chitinous Hide | Flat dmg reduction `floor(lvl/2)` max 5. |
| ○ | Void-Touched | −2 light radius. See enemies ≤4t through walls. |
| ● | Crimson Tide | +2 HP per kill. |
| ☠ | Compounding Muscles | Crits 3×. Bleed on crit. +1 STR/level. |

### Transformation Tier (Primers)

| Sym | Transformation | Primer | Effect | Downside |
|-----|---------------|--------|--------|----------|
| ⚙ | Iron Vessel | Rat King's Ichor | Madness immune. Grease Tube only. Chitin ×2. | — |
| ≈ | Amalgamation | Warden's Marrow | Acid immune. 18% slow on hit. | 1.5× fire dmg |
| ◎ | Void Walker | Pale Draught | Phase Step. 20% Unreadable. 30% Terrifying. +1 SP/kill. | −25% max HP |

### Upgrade Mutations (unobtainable without paired Transformation)

| Sym | Upgrade | Requires | Replaces | Effect |
|-----|---------|----------|----------|--------|
| ◎ | Void Sight | Void Walker | Void-Touched | Enhanced wall-sight |
| ⚙ | Clockwork Body | Iron Vessel | Chitinous Hide | Keeps DR. Immune all statuses. Moving alerts enemies ≤9t. |
| ☠ | Consuming Mass | Amalgamation | Crimson Tide | +2 HP/kill. 20% +1 maxHP/kill (cap +10). Enemies flee (15%) or rage (+2 dmg). |

---

## FLOOR EVENTS (weighted system)

10% no event · 70% one event · 20% two events. Events never repeat on same floor.

| Event | Weight | Floor | Effect |
|-------|--------|-------|--------|
| The Hungry Dark | 8 | any | +1 Madness every 2 turns |
| Weeping Walls | 7 | any | 17% walls bleed crimson, spread every 12t, +1 Mad/6t adjacent |
| The Charnel Wind | 6 | any | All enemies spawn at half HP |
| Feast of Fools | 4 | any | All XP doubled (G._xpMult=2) |
| Thin Pickings | 5 | any | All XP halved (G._xpMult=0.5) |
| The Pack Runs | 5 | 2+ | 4-6 wolves + Dire Wolf (alpha) + Savage Wolf, all in hunt mode |
| A Generous Dead | 4 | any | Gilded chest spawns in random room |
| Wrong Kind of Quiet | 6 | any | No effect. Just dread. |
| Gods' Quarrel | 3 | 4+ | All enemies ignore player, fight each other |
| Dark Floor | 6 | any | Light radius penalty |

---

## ENEMY REFERENCE

### Scaling
`HP + floor((level-1)×1.4)` | `DMG + floor((level-1)×0.3)` | `STR + floor((level-1)×0.2)`
Enemy count: `fl1=6, fl2+=6+level×2`

### Main Dungeon Pool

| Name | HP | DMG | Floors | AI |
|------|----|-----|--------|----|
| Mass of Worms | 3 | 1 | 1-4 | Dormant wander ≤5t spawn. Provoke-only. 25% split on hit (cap 3, half HP). |
| Rat | 4 | 1 | 1-4 | Skittish (flees ≤4t). Aggressive if Rat King within 8t. Reverts 3t after king dies. |
| Sewer Rat | 3 | 1 | 1-3 | Tiny, skittish, alerts nest |
| Skeleton | 5 | 2 | 1-4 | Immune status |
| Small Slime | 3 | 1 | 1-4 | Dormant ≤5t spawn until attacked. Aggressive in Slime Pits. |
| Cultist | 6 | 2 | 1-5 | Patrols 2 rooms, 2-4t pause. Summons Worms at Fractured+. |
| Zombie | 12 | 2 | 1-6 | Wary: pursues within 3-tile scent. Swarm alerts within 5t (3+ zombies). |
| Wolf | 8 | 3 | 2-6 | Alpha/flanker pack tactics. Alpha howls once → summons wolf. |
| Deep One | 12 | 5 | 3-7 | |
| Dark One | 14 | 4 | 4-8 | Ranged, double shot at Hysteria |
| Floating Horror | 10 | 3 | 4-8 | Ranged, high AGI |
| Armored Skeleton | 14 | 3 | 5-7 | Flat 2 DR, immune status |
| Wraith | 13 | 5 | 5-9 | Ghost |
| Mi-Go | 18 | 7 | 6-10 | Ranged |
| Ghoul | 16 | 7 | 7-10 | |
| Elder Thing | 22 | 9 | 8-10 | Ranged |

### Special / Event Enemies

| Name | HP | DMG | Source |
|------|----|-----|--------|
| Dire Wolf | 10 | 5 | Pack Runs event. Always alpha, +2 all stats vs Wolf. |
| Savage Wolf | 8 | 3 | Pack Runs event. 35% Wasting on hit. |
| Ravenous Zombie | 18 | 5 | Feeding Ground, fl2+. Heals 1/hit. |
| Infested Zombie/Cultist/Fighter | varies | varies | Fungal Hollow. isPlant. |
| Deathless Warden | 40 | 9 | Catacombs boss. Resurrects once. Drops Key + Marrow on true death. |

### Catacomb Ossuary Guards (random from pool)

Skeleton · Armored Skeleton · Bone Archer · Mummified Knight

---

## CATACOMB FLOW

1. Surface grave → `T.CATACOMB_ENTRY` → catacombs (dl preserved, restored on exit)
2. Three spines (y=6, 20, 33). North alcoves, south altars, ossuary at bottom.
3. Ossuary: 3 skeletal guards + ossuary chest (Relic / Shroud Fragment / Mutagen).
4. Warden room east of ossuary: Deathless Warden + locked iron chest.
5. True death → Catacomb Key + Warden's Marrow + 25% Hymn of Unmaking.
6. Warden's Chest (Catacomb Key required) → Warden's Sword + Bone Caller's Staff.

---

## WARDEN'S SWORD FOLLOWER SYSTEM

- 8% chance on undead kill → slain enemy rises as follower (green tint, `isFollower:true`)
- Follower stats: 50% HP, 75% DMG/STR/AGI rounded down
- Follows player, attacks nearest enemy in player FOV
- Player bumping follower → swap positions (no friendly fire)
- Full XP on follower kills
- Dismissed silently on any floor transition

---

## WEIGHTED ROOM SYSTEM

| Room | Weight | Floor | Once |
|------|--------|-------|------|
| Living Space | 8 | 2+ | No |
| Guard Post | 7 | 2+ | No |
| Jail Room | 6 | 2+ | No |
| Barracks | 5 | 2+ | Yes |
| Armoury | 5 | 2+ | Yes |
| Crypt Room | 4 | 2+ | Yes |
| Feeding Ground | 4 | 2+ | Yes |
| Chapel | 4 | 2+ | Yes |
| Overgrown Room | 4 | 3+ | Yes |
| Slime Chamber | 3 | 3+ | Yes |
| Torture Chamber | 3 | 2+ | Yes |
| Fungal Hollow | 3 | 3+ | Yes |
| Ritual Room | 2 | 3+ | Yes |

---

## SLIME CHAMBER

Green slime walls + mold floor + acid pools + slime floor patches. Slime Pit entry at center.
- Base: 2 slimes
- Floor 4+: Corrosive Horror
- Floor 6+: Pit Lurker (hidden on acid pool, reveals within 2t)

**Acid Pools (traversable):** +1 turn cost + `max(1, 3 − floor((AGI-4)/3))` damage on entry. Adjacent fumes: 1 dmg/2t. Enemies take 2 dmg on entry (slimes immune). Amalgamation fully immune.

**Vial of Slime:** places acid pools on passable tiles in 3×3 blast. Slime Pits exclusive drop.

---

## INSCRIPTION SYSTEM

- Pass runs after all generation. 5% density fl1 → 20% fl10. Tiles must be adjacent to floor (diagonals OK). Min 3-tile spacing.
- Church: up to 6 inscriptions.
- **Eligible tiles:** WALL, IRON_WALL, MARBLE_WALL, BLOOD_WALL, FUNGAL_WALL, SLIME_WALL, MOSSY_STONE, IRON_GRATE, CRACKED_WALL, STATUE, PILLAR, VASE, FOUNTAIN, BARREL, BOOKCASE, LECTERN, STONE_SLAB
- Unread inscriptions render in **cyan `#40c8c0`**. Revert to normal color on read.
- Log hint `☣ Something is written nearby...` when inscription enters FOV (once per inscription).
- Bump to read → popup modal.
- **XP:** 5-7% of `p.xpNext`. **Madness:** +floor level. One read per inscription per run.
- **Ruin 2:** cryptic footnote. **Ruin 3:** fragmented text. **Ruin 4:** near-incomprehensible.

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
CARPET_RED:54-CARPET_WHITE:60  BED_HEAD:61  BED_BODY:62  BED_FOOT:63
CHAIR:64  TABLE:65  FOOTLOCKER:66  FOOTLOCKER_OPEN:67  BROKEN_DOOR:68
MOSS_FLOOR:69  VINE:70  MOSSY_STONE:71  MUSHROOM:72  THORN_BUSH:73
OVERGROWN_TREE:74  HOLLOW_TREE:75  HEDGEAPPLE:76  BRAZIER:77  MARBLE_WALL:78
FLAGSTONE:79  FUNGAL_WALL:80  FUNGAL_FLOOR:81  BLOOD_WALL:82  RUBBLE:83
PULPED_CORPSE:84  MIRROR:85  COBWEB:86  CRATE:87
STATUE:88  CRACKED_STATUE:89  PILLAR:90  CRACKED_PILLAR:91  VASE:92  FOUNTAIN:93
BARREL:94  BOOKCASE:95  LECTERN:96  PEW:97  CLOSET:98  STONE_SLAB:99
CRACKED_WALL:100  ROCKS:102
NOTE: IRON_GRATE:47 used for jail bars AND wall-replacement grates (consolidated)
```

---

## KNOWN NOTES / DEFERRED

- Test dungeon entrances (Rat's Nest, Slime Pit on surface) — remove before release
- Vestry item TBD
- Spider system planned (deferred)
- Right half of graveyard — purpose TBD
- Library room type — planned
- Furniture placement for new tiles (statue, pillar, vase, fountain, barrel, bookcase, lectern, pew, closet, stone slab) — constants and render added, room placement pending
- Cracked statue/pillar crush mechanic — deferred
- Bookcase spell interaction — deferred
- Fountain heal/poison mechanic — deferred
- Closet contents — flavor text only until armor system
