---
name: local-coder
description: Use when you need the local-only OpenCode helper for an Ollama-backed coding loop.
---

# Local Coder

Use the local helper for a bounded local coding loop with no cloud fallback.

## Server

```bash
/Users/marvin/.local/bin/local-coder serve
```

Starts Ollama in the foreground with the local-only environment. It should be attached to a terminal session and will not daemonize.

## Delegated run

```bash
/Users/marvin/.local/bin/local-coder run \
  --worktree /path/to/local-coder-worktree \
  --plan /path/to/plan.txt \
  --allow-edit 'src/**/*.ts' \
  --allow-command 'git status --short'
```

Use quoted glob patterns so the shell does not expand them first.

## Correction loop

```bash
/Users/marvin/.local/bin/local-coder run \
  --worktree /path/to/local-coder-worktree \
  --plan /path/to/local-coder-correction-plan.txt \
  --session 'existing-session-id' \
  --allow-edit 'src/**/*.ts' \
  --allow-command 'git diff --stat'
```

Use `--session` to continue a local correction loop when you need another bounded pass on the same work.

## Guardrails

- Use the exact Ollama model `qwen3-coder:30b-a3b-q4_K_M`.
- Keep the session local only; do not rely on cloud providers or login-based services.
- Review the actual diff and callers before accepting a change.
- Do not treat the model’s self-report as acceptance.
- Only approve edits for the repo-relative paths you want changed.
- Only approve bash commands that are safe, exact checks.
- The command allowlist is only a guardrail; it is not an OS sandbox.
- Do not expect this helper to commit, push, install packages, or run unapproved tests.
- No test files should be edited unless the user explicitly allowed it.
- Logs and run metadata are stored under `~/.local/share/local-coder/runs/`.

## Notes

- The helper uses dedicated XDG config and data directories under `~/.config/local-coder`, `~/.local/share/local-coder`, `~/.local/state/local-coder`, and `~/.cache/local-coder`.
- Isolation is enforced with `--pure`, `share=disabled`, `autoupdate=false`, `subagent_depth=0`, `lsp=false`, a local-only provider list, and disabled default plugins/model fetches.
- If the local Ollama server or model is missing, start the foreground server first or wait for the parent-controlled model download to finish.
- Idle 5-minute unload is handled by the server-side Ollama environment.

## Workflow

1. Cloud and user approve a detailed plan before local work starts. It includes interfaces,
   edge cases, non-obvious code samples, and agreed executable acceptance checks.
2. The local worker implements the approved plan and runs those checks without modifying them.
3. Cloud reviews the actual diff, callers, contracts, error behavior, and coverage; it does
   not accept the model's self-report.
4. Local corrections use `--session 'existing-session-id'` and remain within the approved scope.
5. Cloud performs final acceptance against the diff and the agreed checks.

The helper is an on-demand foreground tool. An asynchronously launched run or attached `serve`
process must remain attached to its controlling session; detached services and login daemons are
not supported. Do not auto-edit tests, commit, push, install dependencies, or change acceptance
checks unless the user explicitly authorizes it.
