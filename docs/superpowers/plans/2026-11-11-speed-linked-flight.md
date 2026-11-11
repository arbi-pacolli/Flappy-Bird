# Speed-Linked Flight Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Scale the bird's fall and flap strength with the score-driven pipe speed.

**Architecture:** Keep the existing inline game script in `index.html`. Derive the pipe speed and a shared speed multiplier from the current score, then use that multiplier for bird gravity and both keyboard and click flaps without mutating base physics values.

**Tech Stack:** HTML, inline JavaScript, browser.

**Spec:** [2026-11-11-speed-linked-flight-design.md](../specs/2026-11-11-speed-linked-flight-design.md)

## Global Constraints

- Derive the multiplier from the current score-based pipe speed divided by `pipes.baseSpeed`.
- Apply it to the base gravity (`0.5`) while updating the bird and to the base jump strength (`-8`) whenever a flap is triggered.
- Calculate from the existing base values each time; do not mutate them or compound the multiplier.
- At score zero the multiplier is `1`.
- Keep the existing uncapped score-based pipe progression.
- Do not change pipe spacing, spawning, or collision rules.

## Review Focus

- At score zero the bird's current physics remain gravity `0.5` and jump strength `-8` — verify the multiplier is exactly `1`.
- Scoring increases both falling acceleration and flap impulse — verify both use the same multiplier.
- Repeated flaps do not compound speed — verify each impulse is computed from base jump strength.
- Keyboard and click controls remain consistent — test both flap paths with a non-default score.
- Pause, resume, and restart retain their existing behavior — inspect the base physics assignments and verify a restarted score-zero run returns to multiplier `1`.

---

### Task 1: Link bird physics to game speed

**Files:**
- Modify: `index.html` (inline game script: pipe-speed calculation, `update()`, and flap handlers)

**Interfaces:**
- Consumes: Existing `pipes.baseSpeed`, `score`, `bird.gravity`, `bird.jumpStrength`, `update()`, `handleKeyDown()`, and `handleClick()`.
- Produces: `getCurrentPipeSpeed() -> number` and `getSpeedMultiplier() -> number`, used for pipe movement, gravity, and flap impulses.

- [ ] **Step 1: Confirm the current physics and control paths**

Locate the existing score-based `currentPipeSpeed` calculation, gravity application, and jump assignments in `update()`, `handleKeyDown()`, and `handleClick()`.

- [ ] **Step 2: Share the current speed and multiplier calculations**

Add `getCurrentPipeSpeed()` using the existing formula `pipes.baseSpeed + (score * 0.08)`. Add `getSpeedMultiplier()` returning `getCurrentPipeSpeed() / pipes.baseSpeed`. Use `getCurrentPipeSpeed()` for pipe movement instead of duplicating the formula.

- [ ] **Step 3: Apply the multiplier to bird movement**

Update gravity by adding `bird.gravity * getSpeedMultiplier()` each frame. In both keyboard and click flap paths, set velocity to `bird.jumpStrength * getSpeedMultiplier()`. Do not change the stored base gravity or jump strength.

- [ ] **Step 4: Verify formulas and script syntax**

Confirm score `0` yields speed `2.2`, multiplier `1`, gravity `0.5`, and flap impulse `-8`; confirm score `10` yields speed `3.0` and multiplier `3.0 / 2.2` for both gravity and flap. Extract the inline script and run `node --check` on it. Confirm the diff leaves pipe spacing, spawning, and collision checks unchanged.

- [ ] **Step 5: Verify the browser behavior**

Open `index.html`; check that the initial score-zero flight retains the current feel, and that after passing pipes both falling and flaps become faster. Verify mouse/click and Spacebar flaps are consistent, and restart returns the multiplier to `1`.
