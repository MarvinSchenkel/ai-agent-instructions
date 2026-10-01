# ai-agent-instructions

The global instructions and skills I use with Claude Code and the GitHub Copilot CLI. Both tools read the same `SKILL.md` format, so every skill here works in either one.

## What's in here

`copilot-instructions.md` holds my global instructions: model choice, comment style, how I want the agent to behave, and my PR workflow. I symlink it to both `~/.claude/CLAUDE.md` and `~/.copilot/copilot-instructions.md`.

`skills/` has one directory per skill:

| Skill | What it does |
|-------|--------------|
| `arena` | Runs N parallel attempts at a task, picks the best as a base and grafts in the strongest parts of the others |
| `babysit` | Watches a PR until CI is green and every Copilot comment is handled |
| `blast-radius` | Finds what a change could break outside the diff, and proves the key safety fact by running code |
| `bro` | Restates the last message in plain language |
| `create-verification-skill` | Generates a project-local skill that drives your app the way a user does |
| `diagnosing-bugs` | Debugging loop for hard bugs: build a red-capable repro first, then hypothesise |
| `fix_comments` | Trims verbose code comments in the current diff |
| `gogogo` | Implements a task following the workflow in `copilot-instructions.md` |
| `grilling` | Interviews you about a plan, round by round, until every decision is settled |
| `how` | Explains how a subsystem works, optionally with an architecture critique from several models |
| `local-coder` | Runs a local Ollama-backed coding loop (needs my `local-coder` helper) |
| `principle-encode-lessons-in-structure` | Turns repeated corrections into lints, checks or scripts instead of more text |
| `self_review` | Reviews the session's diff, then gets a second opinion from two other models |
| `startbefe` | Starts a Music Assistant backend and frontend from the current worktree |
| `unslop` | Strips AI tells from writing |
| `why` | Digs through git, tickets, docs and chat to find out why code is shaped the way it is |

`routines/` has the prompts for my scheduled Claude desktop tasks:

| Routine | Schedule | What it does |
|---------|----------|--------------|
| `issue-triage` | Weekdays, hourly 07:30 to 17:30 | Triages new and updated Music Assistant issues, reproduces and fixes what it can, and keeps a report board |
| `pr-review` | Hourly | Reviews changed PRs with Copilot, auto-approves safe provider-only PRs, and keeps a status matrix |
| `triage-ma-nightly` | Daily at 05:00 | Scans the last 24 hours of logs from the MA nightly add-on for errors and event loop blocks |

Some skills point at my own setup (paths, the Music Assistant repo), so expect to adjust those.

## Using a skill

Copy the skill's directory into `~/.claude/skills/` (Claude Code) or `~/.copilot/skills/` (Copilot CLI):

```bash
cp -r skills/why ~/.claude/skills/
```

Then call it with `/why`, or let the agent pick it up from its description.
