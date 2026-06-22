# 🐱 GATEKEEPER GUS — A Cat-and-Mouse Caper

A funny, fully-playable **3D browser game** built from your idea:

> A funny-looking cat must catch a funny-looking mouse — but the mouse lives behind a
> gate guarded by **Gus**, a self-important, snack-obsessed hippo. Gus faces the yard.
> Creep too close and he whirls around and **tail-splashes the whole screen with swamp
> muck**. Your trick: when Gus gets **HANGRY**, fetch food and lob it to him. He eats for
> **exactly 2 seconds** with the gate wide open — *dash through in time* — then he splashes again.

Built with **Three.js (WebGL)** in a single self-contained HTML file. No build step, no assets —
every character and prop is modeled from primitive geometry with procedural animation, soft
shadows, swamp fog, a pooled muck-particle splash, and procedural WebAudio sound.

---

## ▶️ How to run

The game loads Three.js from a CDN, so it needs to be opened over **http** (not just double-clicked
as a `file://` in every browser). Easiest ways:

**Option A — Python (already on most machines):**
```bash
cd hippo-gate-game
python -m http.server 8123
```
Then open <http://localhost:8123> in Chrome/Edge/Firefox.

**Option B — VS Code:** right-click `index.html` → *Open with Live Server*.

**Option C — just double-click `index.html`** — works in most Chromium browsers since the
Three.js import is an absolute `https://` URL (needs an internet connection for the CDN).

---

## 🎮 Controls

| Key | Action |
|-----|--------|
| **W A S D** / **Arrows** | Move the cat |
| **SHIFT** or **SPACE** | Dash (your move through the open gate) |
| **E** | Pick up / throw food (auto-picks up at the glowing bowl; press near Gus to feed) |
| **Q** | Drop a cardboard decoy (Level 2+) — Gus splashes it instead of you |
| **Right-mouse drag** | Orbit the camera |
| **ESC** | Pause · **R** | Retry level |

## 🧠 How to win

1. **Wait for HANGRY.** Gus's hunger drains over time. At the green "HANGRY!" tell he'll accept food.
2. **Fetch the fish** from the glowing bowl (walk over it).
3. **Creep close, don't sprint.** Moving fast inside his vision cone makes him notice and splash.
   Move slowly (or stay wide) to sneak into feeding range.
4. **Press E to lob the food.** Gus chomps for **exactly 2.0 seconds** — the gate swings open and a
   countdown ring appears.
5. **DASH through the gate** and reach the mouse before the timer hits 0. If you're still in the
   yard when he finishes, you eat muck and lose a life.

The gate is **solid stone unless Gus is eating** — there is no shortcut. Feeding is the only way in.

## 🏞️ Three escalating yards

- **L1 — The Sleepy Yard** (sunny tutorial): generous telegraph, frequent hunger. Learn the loop.
- **L2 — Muck O'Clock** (golden hour): twitchier Gus, random unprovoked splashes, decoy unlocked.
- **L3 — The Night Shift** (moonlit): sweeping spotlight, rarely hungry, razor-thin timing.

The **2.0-second eat window never changes** — difficulty comes from positioning and timing, so every
level stays fair and winnable (verified: ~1.2–1.6 s of dash margin on each).

---

## 🛠️ Design & tech notes

- **Single file**, ~1k lines. Three.js r160 ES module from `unpkg`.
- **Hippo FSM:** `WATCHING → TURNING (3-phase fair telegraph) → SPLASH → RECOVER`, with
  `EATING` (the 2.0 s contract) entered on a successful feed.
- **Detection** = inside the vision cone **and** the proximity ring **and** moving faster than a creep —
  so stealth is a real option. A short suspicion debounce keeps it from feeling like a cheap shot.
- **Juice:** squash-and-stretch, camera trauma shake, FOV dash-punch, pooled 320-particle muck burst,
  full-screen muck overlay that clears center-out, projected DOM speech bubbles, and fully procedural
  WebAudio (footsteps, whoosh, splash, munch, countdown beeps, win arpeggio).
- Frame-rate independent (clamped `dt`, exponential smoothing) so the 2.0 s window is exact.

### Debug mode
Append `?debug` to the URL to expose `window.__g` with helpers (`state()`, `sim()`, `tp()`,
`setHunger()`, `feed()`) used to playtest and balance the game. It is inert without `?debug`.

---

*Made with Claude Code. Muck stays tasteful — it's swamp/pond sludge (lily pads, rubber-duck energy),
never the literal thing. Gus has filed a complaint.*
