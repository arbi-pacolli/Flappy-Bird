# Speed-Linked Flight Design

## Intent

Make the bird's vertical movement match the pace of the game: slower at the start and faster as pipes accelerate.

## Goals and non-goals

- Scale both falling acceleration and flap impulse with the current pipe speed.
- Preserve the current bird physics when a run starts at base pipe speed.
- Do not change pipe spacing, spawning, or collision rules.

## Approaches

- **Chosen:** Scale gravity and flap strength together in proportion to pipe speed. This keeps both parts of the bird's vertical movement responsive as difficulty rises.
- **Not chosen:** Scale gravity alone. This could make flaps feel weaker against the faster fall.
- **Not chosen:** Keep bird physics fixed. This would not make the fall track the game's speed.

## Constraints and implementation

Keep the change in the inline game script in `index.html`. Derive a multiplier from the current score-based pipe speed divided by `pipes.baseSpeed`. Apply it to the base gravity (`0.5`) while updating the bird and to the base jump strength (`-8`) whenever a flap is triggered. Calculate from the existing base values each time; do not mutate them or compound the multiplier. At score zero the multiplier is `1`. Keep the existing uncapped score-based pipe progression.

## Builder decisions

No new dependencies or game systems are needed. Verify that gravity and flap strength both use the same score-derived multiplier, score-zero values remain unchanged, and the multiplier increases as pipes speed up.
