# Straw Blade - Gesture Combat Game

**Straw Blade** is a real-time gesture-controlled combat game that uses machine learning to recognize player gestures via webcam. The player holds a straw as a sword and performs physical gestures (block, attack, dodge) to battle enemies across 3 stages.

**Play now:** [https://your-project.vercel.app](https://your-project.vercel.app)

---

## How to Play

### Requirements
- A computer with a **webcam**
- **Google Chrome** browser (recommended)
- A **straw** (used as your sword)

### Getting Started
1. Open the game in your browser
2. Click **"CLICK TO START"** on the title screen
3. **Allow camera access** when prompted
4. The webcam setup screen will show your detected gesture -- test your poses before proceeding
5. Click to begin combat

### Controls (Gestures)

| Gesture | How to Perform | When to Use |
|---------|---------------|-------------|
| **Idle** | Stand still, straw at your side | Default stance |
| **Block Left** | Hold straw to your left side to block | When you see **"BLOCK LEFT!"** prompt |
| **Block Right** | Hold straw to your right side to block | When you see **"BLOCK RIGHT!"** prompt |
| **Attack** | Swing the straw forward | When you see **"ATTACK NOW!"** (enemy is stunned) |
| **Dodge** | Duck or sidestep | When you see **"DODGE!"** (red warning) |

### Gameplay Tips
- Watch the **center of the screen** for action prompts telling you what to do
- The enemy **telegraphs** attacks before striking -- you have time to react
- After the enemy attacks, they enter a **recovery** state (green "ATTACK NOW!") -- swing your straw to deal damage!
- **Consecutive blocks** build a combo multiplier for higher scores
- Your **HP carries over** between stages -- be careful!
- Press **M** to mute/unmute sound

### Stages

| Stage | Enemy | Difficulty |
|-------|-------|-----------|
| I | SHADOW | Slow attacks, long telegraph, low damage |
| II | WRAITH | Faster attacks, adds thrust (dodge required) |
| III | DEMON LORD | Fast combos, short telegraph, high damage |

---

## Technical Details

### Machine Learning Model
- Trained with [Teachable Machine](https://teachablemachine.withgoogle.com/) (Image Classification)
- 5 gesture classes: `idle`, `block_left`, `block_right`, `attack`, `dodge`
- Model runs predictions every 200ms via TensorFlow.js

### Tech Stack
- **Single HTML file** -- no build tools, no installation required
- **Canvas 2D** for all game rendering (characters, effects, HUD)
- **TensorFlow.js** + Teachable Machine Image Library (CDN)
- **Web Audio API** for procedurally generated music and sound effects
- **Google Fonts** (Orbitron, Rajdhani)

### Features
- AAA cinematic dark visual style
- Canvas-drawn humanoid character silhouettes with multiple pose frames
- Real-time enemy AI with pre-scripted attack patterns
- Particle effects, screen shake, floating damage numbers
- Combo system with score multiplier
- Procedural background music (tempo increases per stage)
- Mute toggle (M key)

---

## Project Structure

```
straw-blade/
  index.html              # The entire game (single file)
  tm-my-image-model.zip   # Teachable Machine model export (backup)
  README.md               # This file
```

---

## How to Run Locally

1. Clone or download this repository
2. Open `index.html` in Google Chrome
3. Allow camera access
4. Play!

> Note: The game loads the ML model from a Teachable Machine URL, so an internet connection is required.

---

## Credits

- Game developed as a class project
- ML model trained with [Google Teachable Machine](https://teachablemachine.withgoogle.com/)
- Built with vanilla HTML/JS/Canvas, TensorFlow.js, and Web Audio API
