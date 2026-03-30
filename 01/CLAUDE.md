# CLAUDE.md

This file provides guidance to Claude Code (claude.ai/code) when working with code in this repository.

## Files

- `hello.py` — Python script that prints "Hello, World!"
- `push-notification.html` — Standalone HTML page with a 3D panda character built with Three.js (r128 via CDN)

## Running

```bash
python hello.py
```

`push-notification.html` requires no build step — open directly in a browser.

## push-notification.html Architecture

Single-file app with no dependencies except Three.js loaded from CDN.

**3D rendering** — `buildPanda(scene)` constructs the panda from `THREE.Mesh` primitives (spheres, boxes, torus). Each limb is a `THREE.Group` with its pivot at the joint, enabling independent rotation:
- `root` — entire panda body
- `headG` — head group (neck pivot)
- `lArmG` / `rArmG` — arm groups (shoulder pivot); right arm carries the shopping bag
- `lLegG` / `rLegG` — leg groups (hip pivot)

**Animation state machine** — `state` variable drives which animation function runs each frame:
- `'idle'` → `animateIdle()`: wandering system (`wander` object) alternates between `'walk'` and `'pause'` phases. While walking, legs/arms swing in opposite phase. While paused, `doPausedBehavior()` picks randomly from `BEHAVIORS` array.
- `'dance'` → `animateDance()`: panda moves toward camera (Z+) and dances
- `'sad'` → `animateSad()`: panda retreats from camera (Z−) and turns away
- `'comeback'` → `animateComeback()`: returns panda to center; transitions to `'idle'` on completion

**Key globals:**
- `wander` — `{ tx, tz, phase, pauseTimer, pauseDuration }` controls XZ wandering target
- `walkCycle` — accumulated phase for walk animation
- `state` — current animation state, set via `window.setPandaState(s)`

**Browser history** — `history.pushState()` is called on allow, and a `popstate` listener restores the main screen when the back button is pressed.

**Success screen** — a second Three.js scene (`success-canvas`) is lazily initialized on first allow click via `initSuccessPanda()`.
