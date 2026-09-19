# ⚔️ Stickman Kingdom Wars

A fast-paced real-time strategy battle game where you build an economy, recruit an army, and siege the enemy castle — all in your browser. No downloads, no dependencies, just open the HTML file and play.

![Genre](https://img.shields.io/badge/genre-RTS%20%2F%20Castle%20Defense-blue)
![Platform](https://img.shields.io/badge/platform-Web%20Browser-green)
![Dependencies](https://img.shields.io/badge/dependencies-none-brightgreen)
![License](https://img.shields.io/badge/license-MIT-lightgrey)

---

## 🎮 Overview

**Stickman Kingdom Wars** is a single-file, browser-based real-time strategy game. You control the left castle; a dynamic AI controls the right. Earn gold, recruit workers to boost your income, train a balanced army of four distinct unit types, and destroy the enemy castle before yours falls.

The game emphasizes **counter-play**: unit matchups matter more than raw numbers, and the AI actively reads your composition to build a counter-army. Spamming one unit type will get you punished.

---

## ✨ Features

- **4 Unique Unit Types** — Swordsman, Archer, Spearman, and Knight, each with distinct stats, range, and counters.
- **Rock-Paper-Scissors Combat** — Attack multipliers reward smart composition:
  - 🏹 Archer → ⚔️ Swordsman (1.5×)
  - ⚔️ Swordsman → 🏹 Archer (1.5×)
  - 🛡️ Spearman → 🐎 Knight (1.6×)
  - 🐎 Knight → 🛡️ Spearman (0.75×, resisted)
- **Adaptive Enemy AI** — Reads your army composition and builds counters; difficulty and income ramp up over time.
- **Economy System** — Passive gold income plus workers that scale your income (with escalating cost).
- **Juicy Visual Feedback** — Particle bursts, damage numbers, hit knockback, screen shake, and death animations.
- **Full HUD** — Live gold, income, worker count, both castle HP bars, and battle timer.
- **Keyboard Shortcuts** — Keys `1`–`4` to spawn units, `W` for worker, `Space` to pause.
- **Pause & End Screens** — Detailed post-battle stats (units spawned, enemies defeated, units lost, gold earned, battle time).
- **Single File, Zero Setup** — Pure HTML/CSS/JS with Canvas rendering. No build step, no server, no frameworks.

---

## 🚀 Getting Started

### Play Instantly

1. Download the HTML file (e.g. `stickman-kingdom-wars.html`).
2. Double-click it, or drag it into any modern browser.
3. Click **⚔️ Play Battle**.

That's it — no install, no terminal, no dependencies.

### Supported Browsers

Works in all evergreen browsers: Chrome, Firefox, Edge, Safari, Opera.

---

## 🕹️ How to Play

### Objective

Destroy the **enemy castle (1600 HP)** before it destroys **yours**.

### Core Loop

1. **Earn gold** — You start with 150 gold and a passive income of 8/s.
2. **Recruit workers** — Each worker adds **+4.5 income/s**, but each new worker costs 35% more than the last.
3. **Train units** — Spend gold to spawn units at your castle. They auto-march right and engage whatever they meet.
4. **Adapt** — Watch what the enemy builds and counter it. Don't rely on a single unit type.
5. **Break the castle** — Units that reach the enemy castle deal damage to its HP.

### Controls

| Action | Key / Input |
| --- | --- |
| Spawn Swordsman | `1` or click button |
| Spawn Archer | `2` or click button |
| Spawn Spearman | `3` or click button |
| Spawn Knight | `4` or click button |
| Recruit Worker | `W` or click button |
| Pause / Resume | `Space` or ⏸ button |

---

## 🪖 Unit Reference

| Unit | Icon | Cost | HP | Damage | Range | Attack Speed | Speed | Role |
| --- | --- | --- | --- | --- | --- | --- | --- | --- |
| **Swordsman** | ⚔️ | 60 | 80 | 12 | 22 | 0.8s | 42 | Cheap frontline; counters Archers |
| **Archer** | 🏹 | 90 | 50 | 11 | 130 | 1.1s | 38 | Ranged backline; counters Swordsmen |
| **Spearman** | 🛡️ | 100 | 120 | 14 | 26 | 1.0s | 30 | Tanky anti-Knight specialist |
| **Knight** | 🐎 | 180 | 170 | 24 | 24 | 0.9s | 58 | Fast, high-damage shock unit |

### Counter Chart

```
Archer    ──1.5×──▶  Swordsman
Swordsman ──1.5×──▶  Archer
Spearman  ──1.6×──▶  Knight
Knight    ──0.75×──▶ Spearman  (resisted)
```

---

## 🧠 Strategy Tips

- **Scout the enemy's army** with your eyes — the AI telegraphs its composition. If you see many Archers, build Swordsmen.
- **Workers compound.** Recruiting 3–4 workers early pays off massively by mid-game, but leaves you vulnerable if you over-invest.
- **Don't spam one unit.** The AI reads your most common unit type and builds its counter ~60% of the time.
- **Archers are glass cannons.** They out-range melee but die fast if reached. Screen them with Swordsmen or Spearmen.
- **Knights are expensive but mobile.** They close distance fast and shred Archers and undefended lines.
- **Watch the clock.** The AI's income and spawn rate increase as the battle drags on — end it before it out-scales you.

---

## ⚙️ Technical Details

- **Rendering:** HTML5 Canvas 2D, 960×380 internal resolution, responsive scaling.
- **Architecture:** Single IIFE, no globals leaked. Central `state` object drives simulation.
- **Game Loop:** `requestAnimationFrame` with delta-time integration, capped at 50ms per frame to prevent tunneling.
- **Combat Model:** Continuous target acquisition by nearest enemy; cooldown-gated attacks; projectile simulation for archers.
- **Enemy AI:** Composition analyzer + weighted counter-selection + time-scaled economy.
- **Visual FX:** Particle system, floating damage numbers (DOM-based), screen shake, knockback, and tweened death animations.

### File Structure

Everything lives in one file:

```
stickman-kingdom-wars.html
├── <style>      → UI, HUD, modals, damage-number animation
├── <body>       → Canvas, top/bottom bars, overlays
└── <script>     → Game state, units, AI, combat, rendering, input
```

---

## 🛠️ Customization

Want to tweak the balance? Open the file and edit these sections:

- **`UNIT_DEFS`** — Change cost, HP, damage, range, speed, and icons.
- **`COUNTERS`** — Adjust or add attack multipliers between unit types.
- **`freshState()`** — Set starting gold, income, castle HP (`castleHpMax`), and AI parameters.
- **`updateEnemyAI()`** — Tune AI income growth and spawn frequency (`ai.spawnEvery`, `ai.income`).
- **`pickEnemyUnit()`** — Change how aggressively the AI counters your composition.

Example — make Archers deadlier against Knights:

```js
const COUNTERS = {
  archer:    { swordsman: 1.5, knight: 0.9 },
  swordsman: { archer: 1.5 },
  spearman:  { knight: 1.6 },
  knight:    { spearman: 0.75 },
};
```

---

## 🐛 Known Limitations

- No sound effects or music.
- Single-player only (no multiplayer/networking).
- AI is heuristic-based, not learning-based.
- Damage numbers are DOM elements — extremely high spawn rates could theoretically cause frame drops.

---

## 🗺️ Roadmap Ideas

- 🔊 Sound effects and music
- 🏰 Castle upgrades / defensive towers
- 🌟 Unit veterancy (leveling)
- 🎨 Multiple factions or skins
- 🧠 Difficulty selector (Easy / Normal / Hard)
- 📱 Touch-optimized mobile layout
- 💾 Save/load battle replays

---

## 📄 License

Released under the **MIT License** — free to use, modify, and distribute. Attribution appreciated but not required.

---

## 🙌 Credits

Designed and built as a self-contained browser RTS demo. Built with vanilla HTML, CSS, and JavaScript — no engines, no libraries, no build tools.

**Enjoy the battle — and may your castle stand! 🏰⚔️**
