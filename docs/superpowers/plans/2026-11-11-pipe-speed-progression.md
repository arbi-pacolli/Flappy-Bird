# Pipe Speed Progression Implementation Plan

> **For agentic workers:** REQUIRED SUB-SKILL: Use superpowers:subagent-driven-development (recommended) or superpowers:executing-plans to implement this plan task-by-task. Steps use checkbox (`- [ ]`) syntax for tracking.

**Goal:** Start pipe movement more slowly and increase it more noticeably as the player scores.

**Architecture:** Keep the existing inline JavaScript and score-based speed calculation in `index.html`. Adjust only the base speed and score multiplier.

**Tech Stack:** HTML, inline JavaScript, browser.

**Spec:** [2026-11-11-pipe-speed-progression-design.md](../specs/2026-11-11-pipe-speed-progression-design.md)

## Global Constraints

- Set `pipes.baseSpeed` to `2.2`.
- Calculate current pipe speed as `pipes.baseSpeed + (score * 0.08)`.
- Keep progression uncapped and tied to score.
- Do not change bird movement or pipe spacing.

## Review Focus

- Score zero starts pipe movement at 2.2 pixels per frame — check the base speed and verify a fresh run starts at this speed.
- Each scored pipe adds 0.08 pixels per frame — inspect the calculation and verify speed increases as pipes are passed.
- Restarting resets the progression — verify a new run starts at score zero and the base speed.
- Pausing does not alter score-based progression — pause and resume a run and confirm speed continues from the same score.
- Bird physics and pipe spacing remain unchanged — compare those existing values before and after the edit.

---

### Task 1: Tune pipe speed progression

**Files:**
- Modify: `index.html` (inline game script: `pipes.baseSpeed` and `currentPipeSpeed`)

**Interfaces:**
- Consumes: Existing `pipes.baseSpeed` and `score` values.
- Produces: Current pipe speed of `2.2 + (score * 0.08)`.

- [ ] **Step 1: Confirm the existing values**

Read `pipes.baseSpeed`, the `currentPipeSpeed` calculation, and the bird physics and pipe spacing values in `index.html`.

- [ ] **Step 2: Update the two pipe speed values**

Set `pipes.baseSpeed` to `2.2` and change the score multiplier in `currentPipeSpeed` from `0.05` to `0.08`. Leave all other values unchanged.

- [ ] **Step 3: Verify the script values and behavior**

Confirm the calculation is `pipes.baseSpeed + (score * 0.08)` and the existing bird physics and pipe spacing values are unchanged. Open `index.html` in a browser, start and score during a run, then restart and confirm the pace returns to the slower opening speed.
