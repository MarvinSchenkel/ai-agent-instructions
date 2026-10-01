---
name: self_review
description: Use to self-review the work done so far and get a second opinion from other models, or when the user says /self_review
---

# Self Review

Review the work we have done in this session, then get a second opinion from two other models.

## Steps

1. Do your own review of the current diff first. Look for bugs, logic errors, missing edge cases, and deviations from the project standards.
2. In parallel, launch two `code-review` sub-agents on the same diff:
   - one with `model: gpt-5.5`
   - one with `model: gemini-3.1-pro-preview`
   Give each the full context: what we changed and why, and the files in scope.
3. Collect all findings. Merge overlapping points.

## Deciding

You own the end result. You decide which suggestions to accept or reject.

- For each accepted point, apply the fix.
- For each rejected point, say why in one line.
- Do not blindly follow the other models; only act on findings that are correct and matter.

## Output

Short summary grouped by: accepted (fixed), rejected (with reason), and anything still open for the user to decide.
