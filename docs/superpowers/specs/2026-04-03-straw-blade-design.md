# Straw Blade -- Game Design Spec

## Overview

Straw Blade is a real-time gesture-controlled combat game built as a single-file web application (HTML/JS/Canvas). The player uses physical gestures captured by a webcam and classified by a Teachable Machine ML model to fight enemies across 3 stages of escalating difficulty.

The player holds a straw as a sword and performs gestures (block, attack, dodge) to battle on-screen enemies in real time. Both player and enemy have health bars; first to 0 HP loses.

## Assignment Context

- ML model trained with Teachable Machine (image classification)
- Game built as a web app (alternative to Raise Playground .sb3)
- Deliverable: single HTML file + Teachable Machine model URL
- Requires usage instructions and a 2-3 minute presentation video

## Teachable Machine Model

**Type:** Image classification (pose/gesture via webcam)

**Classes (5):**
- `idle` -- player standing ready, no action
- `block_left` -- player holds straw to block from the left
- `block_right` -- player holds straw to block from the right
- `attack` -- player swings the straw blade forward
- `dodge` -- player ducks or sidesteps

**Model delivery:** Exported from Teachable Machine as a shareable URL, loaded at runtime via TensorFlow.js.

## Game Flow

1. **Title Screen** -- "STRAW BLADE" logo (Orbitron font), animated background, "START" button
2. **Webcam Permission** -- prompt camera access, quick gesture calibration check
3. **Level Intro** -- "STAGE I / II / III" splash with enemy name, 3-2-1-FIGHT countdown
4. **Combat** -- main gameplay screen
5. **Level Clear** -- score breakdown, "NEXT STAGE" button
6. **Game Over** -- player HP reaches 0, final score, "RETRY" button
7. **Victory** -- beat all 3 stages, celebration screen with total score

## Game Screen Layout

- **Top HUD:** Player HP bar (left), Stage/Score (center), Enemy HP bar (right)
- **Arena:** Player character (left) vs Enemy character (right), impact effects in center
- **Bottom bar:** Small webcam preview (left), detected gesture + confidence % (center), reaction time (right)

## Visual Style

AAA cinematic dark theme:
- Dark background with color grading (deep navy/purple)
- Orbitron font (headings, game text) + Rajdhani font (secondary text), weights 700-900
- Characters drawn as humanoid silhouettes using canvas 2D paths:
  - Player: blue-themed armored warrior with straw blade and shield
  - Enemy: red-themed horned demon with sword
- Glowing HP bars with gradient fills and border glow
- Character aura glow (blue for player, red for enemy)
- Floor glow beneath characters

## Combat Mechanics

### Real-time Gesture Combat

The game runs in real time. The enemy cycles through states: **guard** (default, can't be damaged), **telegraph** (shows upcoming attack direction), **attack** (strikes the player), and **recovery** (vulnerable to player attacks for ~0.5s after attacking). The player responds with physical gestures detected by the webcam. The player should attack during the enemy's recovery window to deal damage.

### Gesture-to-Action Mapping

| Gesture | Action |
|---------|--------|
| `idle` | Character stands ready, no action |
| `block_left` | Blocks enemy attacks from the left |
| `block_right` | Blocks enemy attacks from the right |
| `attack` | Swings straw blade, deals damage if enemy is in recovery |
| `dodge` | Ducks/sidesteps, avoids unblockable thrust attacks |

### Enemy AI Per Stage

**Stage I (Easy):**
- Attacks every 2-3 seconds
- Telegraphs attack direction 1 second before striking
- Limited to left/right strikes only

**Stage II (Medium):**
- Attacks every 1.5-2 seconds
- Shorter telegraph window (0.7 seconds)
- Adds thrust attacks (requires dodge)
- Occasional 2-hit combos

**Stage III (Hard):**
- Attacks every 1-1.5 seconds
- Very short telegraph (0.4 seconds)
- Full moveset including unblockable thrusts
- 3-hit combos
- Faster recovery between attacks

### Damage Rules

| Situation | Result |
|-----------|--------|
| Correct block | 0 damage, +score, combo continues |
| Wrong block / no block | Player takes 10-20 HP damage (scales with stage) |
| Attack during enemy recovery | Enemy takes 15 HP damage |
| Attack during enemy block | No damage, brief player stagger |
| Dodge unblockable attack | 0 damage, +score |

### Combo System

Consecutive successful blocks/dodges build a combo multiplier (x2, x3, x4...) that increases score gained per action. Getting hit resets the combo to x1.

### Health

- Player: 100 HP
- Enemy: 100 HP per stage
- No health regeneration between stages

## Technical Architecture

### Single HTML File

The entire game is contained in one HTML file with embedded JavaScript. No build tools, no external dependencies beyond the CDN-loaded TensorFlow.js and Teachable Machine libraries.

### Modules

- **Model Loader** -- loads Teachable Machine model via TensorFlow.js from URL, runs webcam prediction every ~200ms
- **Game Loop** -- `requestAnimationFrame` at 60fps, handles update + render cycle
- **State Machine** -- manages screen transitions (title, intro, combat, game over, victory)
- **Combat Engine** -- enemy AI attack timer, pre-scripted attack queue per stage, damage calculation, combo tracking
- **Renderer** -- draws all visuals on a single canvas: backgrounds, characters (canvas 2D paths), HP bars, particles, text (Orbitron/Rajdhani via Google Fonts)
- **Audio Manager** -- Web Audio API for all sounds
- **Particle System** -- impact sparks, floating dust, glow pulses

### Character Rendering

Characters drawn with canvas 2D path operations (moveTo, lineTo, arc, bezierCurveTo) matching the humanoid silhouette style from the mockups. Each character has pose frames for: idle, block_left, block_right, attack, dodge, hit_stagger. Smooth interpolation between poses.

### Model Integration

```
Webcam -> TensorFlow.js prediction (every ~200ms) -> highest confidence class -> game input
```

The prediction runs on a separate timing loop from the game render loop to avoid frame drops.

## Visual Effects

- Floating dust particles in arena background
- Character glow aura pulses (blue player, red enemy)
- Impact spark particle burst on hit/block
- Screen shake on heavy damage
- Combo counter bounce scale animation
- Floating damage numbers (rise and fade)
- HP bar white flash on damage taken
- Stage transition: fade to black with "STAGE CLEAR" text

## Audio

All audio generated with Web Audio API (oscillators + noise) to keep the game self-contained in a single file.

**Background music:** Looping beat pattern per stage, tempo increases each level.

**Sound effects:**
- Block: short metallic clang
- Hit: low thud impact
- Attack swing: whoosh
- Combo milestone (x5, x10): rising chime
- Level clear: victory fanfare
- Game over: low drone

## External Dependencies (CDN)

- TensorFlow.js
- Teachable Machine Image library
- Google Fonts (Orbitron, Rajdhani)
