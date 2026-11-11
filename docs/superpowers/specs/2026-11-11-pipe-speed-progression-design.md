# Pipe Speed Progression Design

## Intent

Make the opening of each run easier to react to, then raise the challenge as the player scores.

## Goals and non-goals

- Start pipe movement slower and make its score-based increase noticeable.
- Keep speed progression tied to score, as the game already describes.
- Do not change bird movement, pipe spacing, or other game behavior.

## Approaches

- **Chosen:** Reduce starting pipe speed and increase the per-score increment. This preserves the existing score-driven design.
- **Not chosen:** Increase speed over elapsed time. This would change progression independently of scoring.
- **Not chosen:** Only reduce starting speed. This would not make later play harder.

## Constraints and implementation

Keep the change in the inline game script in `index.html`. Set `pipes.baseSpeed` to `2.2` and calculate current pipe speed as `pipes.baseSpeed + (score * 0.08)`. Keep the progression uncapped, matching the existing behavior.

## Builder decisions

No additional dependencies or game systems are needed. Verify the starting speed and score increment in the script and confirm through browser play that the opening is slower and pipes get faster as score grows.
