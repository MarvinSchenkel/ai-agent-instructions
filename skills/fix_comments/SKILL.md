---
name: fix_comments
description: Use when asked to fix, trim, or clean up code comments that are too verbose, or when the user says /fix_comments
---

# Fix Comments

Trim overly verbose comments in the code we just changed.

## Rules

- Leave existing comments alone when they still apply. Do not rewrite an author's comment unless it is now wrong.
- Only keep or add a comment where the code is not self-explanatory.
- Prefer short one-liner comments. Drop comments that just restate what the code obviously does.
- Comments describe the current code, not what changed. No "previously" or "was X now Y" notes.
- Docstrings explain intent for the caller, not the internal workings.

## Scope

Default to the files changed in the current task or diff. Do not touch unrelated files.
