# VAROOM (Retro Drive)

## What this is
A single-file, dependency-free browser game. Open `index.html` in any browser to play.
Everything lives in one `<script>` block: no build step, no libraries, plain HTML5 Canvas.

Title screen name: **"Retro Drive - Levi .D inc & Fox.co"** (v1.24).

## How to run
- `open index.html` (or just double-click it in Finder).

## Controls
- **LEFT / RIGHT arrows** — steer
- **DOWN arrow** — slow down (hold). Double-tap DOWN = full stop (toggles, persists until toggled again; brake lights show while stopped)
- **Double-press P** — pause / open the Garage / resume (guarded against key-repeat)
- **SPACE / ENTER** — start game; **SPACE** also restarts after game over
- **Mouse click** — start game; in the Garage, pick paint job and game mode

## Game rules
- Dodge the oncoming cars (obstacles). A collision ends the game (crash explosion + debris + crash sound).
- Every obstacle that passes the bottom of the screen = **+10 score**.
- Speed increases with score (`blockIndex` every 500 pts; night blocks are faster).
- The game alternates **DAY / NIGHT** every 500 pts (night = dark colors + headlight cone + "NIGHT MODE" label).
- High score is persisted in `localStorage` (`varoomHighScore`).
- **Semi-trucks** (wide, tall) spawn in ALL game modes (~10%; 35% during a Traffic jam).
- **Skid marks**: when the car drifts hard (|vx| > 1.5) the rear tires leave fading skid marks on the road that scroll with the world.

## Game modes (selectable in the Garage/pause menu)
- **Regular** — standard obstacles (incl. occasional semis).
- **Traffic** — road alternates between "ROAD CLEAR" (break) and "TRAFFIC JAM" phases every ~10s; during a jam, cars spawn far faster and semis appear more often.
- **Getaway** — cop cars (max 2) spawn ahead and home in on your X position while chasing you down the road; hitting one is game over.
- **Survival** — manage **gas** and **tiredness** (meters bottom-left). Gas drains continuously; tiredness rises. Every 500 score a **gas station** tile scrolls down the road — drive over it to refuel (+toast "REFUELED!"). Running out of gas = pull over + **10s auto-fix** (toast "AUTO-FIXED! RESERVE GAS", refills to 25). At tiredness 100 you fall asleep at the wheel and crash (special "YOU FELL ASLEEP" game over). Press **Z** to pull over and sleep (20s, toast "PULLED OVER - SLEEPING..."); while parked you're safe from collisions and traffic passes by, then you wake rested. Zzz floats above the car while sleeping. When below 50 tiredness the label "GETTING SLEEPY!" warns you.

All modes share the same seasons/physics below.

## Seasons (score-based, apply to ALL game modes)
- **0–999: normal** — green day / dark green night, and **slippery physics**: steering eases into lateral velocity (grip = 0.15) so the car drifts/slides after releasing the keys. Sliding stops at the road edge.
- **1000–1999: winter** — snowy palette, snow-capped trees, falling snow particles, "WINTER MODE" label, and instant steering (grip = 1.0). (No summer mode — after 1999 it returns to regular.)

## Paint jobs (Garage)
- Four skins; the 3rd ("Purple & Blue") unlocks at a score/high-score of 1000; the 4th ("Red Challenger" — the Challenger design from `VAROOM .html`, red body + 2px yellow center stripe) is a **secret** unlock: it requires crashing into semi-trucks 3 times total (persisted in `varoomSemiHits`). The unlock method is NOT shown in-game (locked box just says "????").
- Selection persisted in `localStorage` (`varoomSkin`); game mode in `varoomMode`. Song unlocks (VHS tapes) are **session-only** (reset each page load).

## Dev / secret things
- **`ld` cheat**: type `l` then `d` while playing to get **+100 score**. Works even while paused (Garage). Not documented in-game.
- **Red Challenger skin**: hidden unlock (see Paint jobs). Also not documented in-game.

## Technical conventions
- `update(delta)` / `draw()` split; `delta` is seconds, converted to frame units via `dt = delta * 60` so all physics are frame-rate independent (60fps-reference values).
- Key handlers guard against `e.repeat` to avoid double-press/pause/full-stop flicker.
- Hardcoded Garage click regions (x/y) must stay in sync with the drawn boxes.
- Canvas is 450x500, road at `x:120` width `240`.

## Git history
- Initial commit: imported the original export as `gemini-code-1786761189156.html`.
- `972acdf`: renamed to `index.html`; fixed key-repeat + frame-rate bugs; added winter mode.
- Later: seasonal grip physics, speed bump (3.2→3.5 base), `ld` cheat works while paused, summer reverted to regular after 1999.
