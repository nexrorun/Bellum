# BELLUM — Arena of Legends

A dark-fantasy stick figure boss rush game built entirely in a single HTML5 Canvas file. Fight through waves of enemies, buy upgrades, and battle 6 unique bosses in an endless arena.

**Play now:** [nexrorun.github.io/Bellum](https://nexrorun.github.io/Bellum/)

---

## Controls

| Key | Action |
|-----|--------|
| **A / D** | Move left / right |
| **SPACE** | Jump (double jump by default, triple with upgrade) |
| **J** | Light Attack (3-hit combo chain) |
| **K** | Heavy Slam (AoE) |
| **L** | Dash (i-frames, attack after for dodge counter) |
| **F** | Parry (attack after for riposte) |
| **Jump + J** | Air Dive (slams down on landing) |

---

## Combat System

### Light Attack Chain (J)
A 3-hit combo that increases in power with each swing:

| Hit | Name | Multiplier | Notes |
|-----|------|-----------|-------|
| 1st | Light | 1.0x | Fast, bread and butter |
| 2nd | Light 2 | 1.3x | Slight ramp |
| 3rd | Combo Finisher | 2.5x | Wide hitbox, breaks enemy guard |

- Base damage: **14** (+ bonus damage from upgrades)
- Combo timer: **25 frames** to land the next hit before the chain resets (40 with upgrade)
- The 3rd hit (combo finisher) **breaks enemy parry stance** for 1.5x bonus damage and a 40-frame stagger

### Heavy Slam (K)
- Multiplier: **1.2x** (+ heavy bonus from upgrades)
- Cooldown: **55 frames**
- Wide AoE hitbox (100x80 pixels)
- With **EARTHQUAKE** upgrade: spawns a shockwave that hits all enemies within 200px for 40% damage

### Parry (F)
- Active window: **14 frames** (extendable with upgrades, up to 25)
- Cooldown: **16 frames** (reducible with upgrades)
- On successful parry:
  - **Stuns nearby enemies** within 150px
  - **Heals 3 HP** (8 HP with Perfect Guard upgrade)
  - Opens a **60-frame riposte window** (100 frames with upgrade)
  - Grants **20 i-frames**
  - Triggers dramatic slow-motion (12 frames at 8% speed)
  - Bosses get knocked out of their current attack

### Riposte (J after successful parry)
- Multiplier: **5.0x** — the highest damage attack in the game
- Requires landing an attack during the riposte window after a parry
- Glowing red aura indicates the window is active

### Dodge Counter (J after dash)
- Multiplier: **2.0x** (3.0x with Shadow Strike upgrade)
- Must attack within **18 frames** of dash ending
- Orange aura indicates the counter window is active

### Air Dive (J while airborne)
- Multiplier: **1.5x** (2.25x with Aerial Ace upgrade)
- Player slams downward on landing with screen shake
- With **METEOR STRIKE** upgrade: creates an explosion dealing 60% damage to all enemies within 180px

### Dash (L)
- Duration: **10 frames**
- Cooldown: **28 frames** (reducible to 14 with upgrades)
- Grants **12 i-frames** (up to 18 with Phantom Step)
- Leaves cyan afterimage trail

### Critical Hits
- Default: 0% chance
- **PRECISION** upgrade: 15% chance for 2.0x damage
- **ASSASSIN** upgrade: 25% chance for 2.5x damage
- Crits display golden particle burst

### Combo Counter
- Tracks consecutive hits without pause
- Displayed at 3+ hits
- With **FURY** upgrade: each combo hit adds +2% damage

---

## Player Stats

| Stat | Base Value |
|------|-----------|
| HP | 100 |
| Movement Speed | 5.5 |
| Jump Force | -14 |
| Max Jumps | 2 |
| Gravity | 0.65 |
| Base Damage | 14 |
| Parry Window | 14 frames |
| Parry Cooldown | 16 frames |
| Dash Cooldown | 28 frames |
| Dash i-frames | 12 frames |
| Heavy Cooldown | 55 frames |

---

## Enemy Types

All enemy stats scale with progression using the formula: `enemyScale = 1 + bossIdx × 3.8`

| Type | Color | Base HP | Speed | Base Damage | Behavior |
|------|-------|---------|-------|-------------|----------|
| **Grunt** | Pink (#ff4488) | 42 × scale | 1.8 | 8 + bossIdx×2 | Standard melee rusher |
| **Fast** | Green (#44ff88) | 30 × scale | 3.0 | 6 + bossIdx×1.5 | Quick, darting attacks |
| **Heavy** | Orange (#ff8844) | 65 × scale | 1.0 | 14 + bossIdx×3 | Slow but powerful |
| **Archer** | Yellow (#ffcc44) | 35 × scale | 1.2 | 7 + bossIdx×2 | Ranged projectiles |

### Enemy Parry
- All enemies except archers can enter a **guard stance** (cooldown: 140-280 frames)
- Guard blocks light attacks (hits 1 and 2) — player gets knocked back
- **Guard breakers**: Combo finisher (3rd hit), riposte, and dodge counter shatter the guard for 1.5x damage + 40-frame stagger
- Guard stance lasts 25 frames with a visible golden "GUARD" indicator

---

## Wave System

Before each boss, you fight through **3 waves** of enemies:

| Wave | Enemy Count | Composition |
|------|------------|-------------|
| Wave 1 | 4 | Grunt, Grunt, Grunt, Fast |
| Wave 2 | 6 | Grunt, Fast, Fast, Heavy, Grunt, Archer |
| Wave 3 | 8 | Heavy, Fast, Grunt, Archer, Heavy, Fast, Grunt, Heavy |

- Enemies spawn staggered from the right side of the arena
- Spawn interval: 60 frames for bosses 1-2, 30 frames for bosses 3-6
- After each wave, the **shop** opens for upgrades
- After wave 3, the boss fight begins

---

## Bosses

### Boss 1: THE SENTINEL — Guardian of the Gate
- **HP:** 5,000 | **Color:** Blue (#3399ff) | **Arena:** Sentinel Fortress
- **Phase 2** at 50% HP (faster attacks, projectile spread)

| Attack | Description | Damage |
|--------|-------------|--------|
| Sweep | Close-range horizontal slash | 14 |
| Slam | Dramatic jump → ground pound (P2: spawns 2 projectiles) | 20 |
| Shoot | Aimed projectile (P2: fires 3-shot spread) | 12 |

---

### Boss 2: THE TWINS — Dance of Destruction
- **HP:** 9,000 (shared) | **Colors:** Pink (#ff3366) + Orange (#ffaa00) | **Arena:** Twin Sanctum
- **Phase 2** at 40% HP (faster, more projectiles)
- Two independent stick figures with a shared health bar

| Attack | Description | Damage |
|--------|-------------|--------|
| Rush | One twin charges at player | 12 |
| Crossfire | Both twins fire projectiles (P2: 3 each) | 10 |
| Pincer | Both twins converge from sides | 14 |

---

### Boss 3: THE COLOSSUS — Mountain That Walks
- **HP:** 14,000 | **Color:** Green (#44dd44) | **Arena:** Colossus Cavern
- **Phase 2** at 50% HP (more boulders, wider stomp)
- Largest boss (65px scale), causes ground shake while idle

| Attack | Description | Damage |
|--------|-------------|--------|
| Stomp | Jump slam + boulder scatter | 25 + 10/boulder |
| Boulder | Throws 3-4 boulders (gravity-affected) | 14 each |
| Rampage | Charges across the arena | 18 |

---

### Boss 4: THE PHANTOM — Whisper in the Dark
- **HP:** 12,000 | **Color:** Purple (#cc44ff) | **Arena:** Phantom Void
- **Phase 2** at 50% HP (5 clones instead of 3, faster rain)
- Floats and oscillates, leaves ghost afterimages

| Attack | Description | Damage |
|--------|-------------|--------|
| Blink | Teleports behind player, slashes | 18 |
| Rain | Projectiles fall from sky in columns | 10 each |
| Clones | Summons 3-5 spectral copies that shoot inward | 12 each |

---

### Boss 5: THE CROWN — Lord of Legends
- **HP:** 20,000 | **Arena:** Crown Throne
- **3 Phases**: Gold (#ffcc00) at 100% → Orange (#ff8800) at 40% → Red (#ff4400) at 15%
- Spawns orbiting orbs in P2/P3

| Attack | Description | Damage |
|--------|-------------|--------|
| Blast | 8-11 projectiles in a circle | 10 each |
| Beam | Sweeping laser (tracks player) | 2/frame |
| Slam | Jump slam + 8 projectile spread | 12 each |
| Meteor | (P2+) Projectiles rain from sky | 15 each |

---

### Boss 6: VOID MONARCH — Sovereign of the Abyss
Two-phase boss with a dramatic transformation.

**Phase 1 (P1):** Gold (#c8aa40) | **HP:** 20,000
- Can be stunned by parry (50-frame stun, takes 1.5x damage)
- Has a guard stance that reflects damage if you attack during it

| Attack | Description | Damage |
|--------|-------------|--------|
| Double Slash | Two quick cuts + spike hazards | 14 + 16 |
| Spikes | Line of ground spikes | 12 each |
| Guard | Parry stance (attack = reflected damage) | 18 |
| Lunge | Forward charge + spike on landing | 18 |

**Phase 2 (Demon Form):** Red (#cc2233) | **HP:** 22,000
- Triggered by defeating P1 — tornado transformation cutscene
- Larger model with demonic wings and giant sword

| Attack | Description | Damage |
|--------|-------------|--------|
| Claymore | Wide sword swing + 6 spike line | 22 |
| Spike Line | Ground spikes on both sides | 12 each |
| Teleport | Blinks to player location | 22 |
| Thrust | Forward charge + spike finisher | 18 |

---

## Shop Items (37 total)

All prices scale by `base × 1.5^bossIdx` (more expensive as you progress).

### Healing
| Item | Cost | Effect |
|------|------|--------|
| HEAL | 5 | Restore 30 HP |
| BIG HEAL | 10 | Restore 60 HP |
| FULL RESTORE | 25 | Fully heal HP |

### Max HP (one-time)
| Item | Cost | Effect |
|------|------|--------|
| VITALITY | 15 | +25 Max HP |
| FORTIFY | 35 | +40 Max HP |
| TITAN HEART | 60 | +80 Max HP |

### Damage (one-time)
| Item | Cost | Effect |
|------|------|--------|
| SHARPEN | 20 | +3 sword damage |
| WHETSTONE | 40 | +5 sword damage |
| DEMON EDGE | 75 | +10 sword damage |

### Movement (one-time)
| Item | Cost | Effect |
|------|------|--------|
| SWIFT | 12 | Faster dash cooldown |
| PHANTOM STEP | 30 | Even faster dash + longer i-frames |
| WIND WALKER | 18 | +1.5 movement speed |
| GALE FORCE | 35 | +2 movement speed |
| SKYBOUND | 28 | Triple jump |

### Parry (one-time)
| Item | Cost | Effect |
|------|------|--------|
| IRON GUARD | 18 | +5 frame parry window |
| AEGIS | 40 | +6 frame window + faster cooldown |
| PERFECT GUARD | 50 | Parry heals 8 HP instead of 3 |
| RIPOSTE MASTER | 35 | Riposte window lasts +40 frames longer |

### Combo (one-time)
| Item | Cost | Effect |
|------|------|--------|
| CHAIN LINK | 15 | Longer combo timer (25 → 40 frames) |
| RELENTLESS | 30 | Combo finisher hits 2x wider |
| FURY | 45 | +2% damage per combo hit |

### Heavy Attack (one-time)
| Item | Cost | Effect |
|------|------|--------|
| POWER SLAM | 25 | Heavy attack +50% damage |
| EARTHQUAKE | 50 | Heavy slam spawns shockwave |

### Aerial (one-time)
| Item | Cost | Effect |
|------|------|--------|
| AERIAL ACE | 22 | Air attacks +75% damage |
| METEOR STRIKE | 45 | Air dive creates explosion on landing |

### Passive (one-time unless noted)
| Item | Cost | Effect |
|------|------|--------|
| VAMPIRIC BLADE | 55 | Heal 2 HP per hit |
| BLOOD DRINKER | 80 | Heal 5 HP per hit |
| THORNS | 30 | Enemies take 8 damage when hitting you |
| RETRIBUTION | 55 | Enemies take 20 damage when hitting you |
| COIN MAGNET | 10 | Coins attracted from further away |
| LUCKY CHARM | 20 | Enemies drop +1 extra coin |
| FORTUNE | 45 | Enemies drop +2 extra coins |
| BARRIER | 35 | Block the next hit completely *(repeatable)* |
| BERSERKER | 30 | +30% damage when below 25% HP |
| SECOND WIND | 65 | Revive once with 30% HP |
| PRECISION | 40 | 15% chance for 2.0x critical hits |
| ASSASSIN | 70 | 25% chance for 2.5x critical hits |
| SHADOW STRIKE | 35 | Dodge counter deals +100% damage |

---

## Endless Mode

After defeating all 6 bosses, the game loops with increasing difficulty:

| Loop | Boss HP Scale | Boss Damage Scale | Boss Order |
|------|--------------|-------------------|------------|
| 0 (first run) | 1.0x | 1.0x | Fixed: Sentinel → Twins → Colossus → Phantom → Crown → Void Monarch |
| 1 | 1.2x | 1.1x | Randomized |
| 2 | 1.4x | 1.2x | Randomized |
| 3 | 1.6x | 1.3x | Randomized |
| n | 1 + n×0.2 | 1 + n×0.1 | Randomized |

Enemy stats also scale with boss index: `enemyScale = 1 + bossIdx × 3.8`

---

## Coin System

- **Enemies** drop `1 + floor(bossIdx × 0.5)` coins on death (+ bonus coins from upgrades)
- **Bosses** drop `5 + (bossIdx × 2) + (loopCount × 3)` coins (+ double bonus coins)
- Coins have physics (gravity, bounce) and a **180-frame lifetime**
- Coins are magnetically attracted when within **80px** (160px with Coin Magnet)
- Auto-collected at **25px** distance

---

## Arena Themes

Each boss has a unique arena with its own color palette:

| Arena | Boss | Glow Color | Ground |
|-------|------|-----------|--------|
| Sentinel Fortress | Sentinel | Blue (#3399ff) | #081428 |
| Twin Sanctum | Twins | Pink (#ff3366) | #1a0810 |
| Colossus Cavern | Colossus | Green (#44dd44) | #081a0d |
| Phantom Void | Phantom | Purple (#cc44ff) | #100820 |
| Crown Throne | Crown | Gold (#ffcc00) | #1a1608 |
| Abyssal Gate | Void Monarch | Magenta (#cc2277) | #0d0818 |

---

## Music System

| Context | Track | Volume | Playback Speed |
|---------|-------|--------|----------------|
| Title Screen | `make_me.mp3` | 1.0 | 1.0x |
| Battle (low tension) | `Epic 1/2/3.mp3` | 0.7 | 1.0x |
| Battle (high tension) | `Epic 1/2/3.mp3` | 1.0 | 1.0x |
| Shop | Current battle track | 0.6 | 0.5x |

- Battle music **continues through death and retry** — no reset to menu music
- Battle always starts with **Epic 1**, then randomly cycles through all 3 tracks
- **Tension** is calculated from alive enemies (+0.12 each) and boss state (+0.5, +0.3 if low HP)
- Volume smoothly lerps between values for seamless transitions
- Audio unlocks on first user interaction (browser autoplay policy)

---

## Visual Effects

- **Particles**: Lightweight double-circle glow system (no canvas shadowBlur for performance). Capped at 400 particles.
- **Screen Shake**: Intensity-based camera offset on hits, boss slams, and phase transitions
- **Slow Motion**: Brief hitstop on heavy hits, parries, and boss kills
- **Color Flash**: Screen-wide tint on damage (red), parry (cyan), boss phase transitions
- **Vignette**: Radial darkening at screen edges; pulses red when below 25% HP
- **Scanlines**: Horizontal line overlay, more visible during boss fights
- **Chromatic Aberration**: Color separation at screen edges during high tension
- **Lightning**: Random lightning bolts in the background during intense boss fights
- **Slash Effects**: Curved arc trails on sword swings, color-coded by attack type
- **Afterimages**: Cyan stick figure ghosts during dash
- **Boss Warnings**: Pulsing circles with "!" markers telegraph attacks before they land
- **Ground Spikes**: Black crystalline spikes with pink shards, ground cracks, and debris particles

---

## Tutorial

On the first game (Loop 0, Boss 1), an 8-step interactive tutorial appears:

1. MOVE with A / D
2. JUMP with SPACE
3. LIGHT ATTACK: press J (3-hit combo!)
4. HEAVY SLAM: press K (AoE!)
5. DASH with L (i-frames!)
6. PARRY with F — then attack for RIPOSTE!
7. DASH then ATTACK for DODGE COUNTER!
8. Jump + ATTACK for AIR DIVE!

Each step advances when you press the required key or after a timeout.

---

## Tech

- **Single file**: Everything is in one `index.html` (~1400 lines)
- **No dependencies**: Pure HTML5 Canvas + vanilla JavaScript
- **Fonts**: Orbitron (headings) + Rajdhani (body) via Google Fonts
- **Audio**: 4 MP3 files (`make_me.mp3`, `Epic 1.mp3`, `Epic 2.mp3`, `Epic 3.mp3`)
- **Hosted on**: GitHub Pages

---

## License

Apache-2.0
