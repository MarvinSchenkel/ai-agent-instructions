---
name: issue-triage
description: Auto triage new issues
---

The purpose of this routine is to act as my first line of support / secretary. Act on any issues that have changed since the last run. 'Changed' means either a new comment has been added to the issue or the issue has been newly posted. Also use the MA issues that are reported in HA as a source: https://github.com/home-assistant/core/labels/integration%3A%20music_assistant. Also monitor the Discussions section in the support repository where @OzGav has tagged me in.

Filter out any issues that are:
- Closed
- Are assigned to someone else

## New issues
Then, per new issue, run a triage. Only start working on a fix when the issue can be reproduced locally. If reproduced locally,  spin up a subagent to fix the issue using the gogogo skill. Make sure to AB test the fix locally by removing the fix and confirming the issue comes back. Then submit a draft PR with the fix, trigger a copilot review and babysit the PR until Copilot has no more comments.

## Existing issues that have been updated
Do not run a fill triage on issues that have been triaged before. Only run the triage on existing issues if an issues flips from ‘not enough info’ to ‘enough info to reproduce’. Just take new comments into account when suggested a reply to the user

## Possible issues states
Per issue, the possible outcomes are:
- Not enough info -> See 'stop early' 
- Cannot reproduce
- Reproduced, cannot fix
- Reproduced + fixed + draft PR
- Already fixed on dev
- User / config error
- Fix merged

## Reproduction rules
You are allowed to use the following speakers for reproduction. ALL AT MAX 5% volume!

At nighttime (7pm -> 8am)
- Local web player (this device)
- Woonkamer NAD
- WiiM Keuken
- WiiM Mini

During daytime (8am -> 7pm)
- All players available to you
- Note, my production runs on WiiM Mancave, WiiM Keuken, WiiM mini and Woonkamer NAD, so only use those when you really need to

When you need a streaming provider or music from my NAS, copy the data dir from ~/.musicassistant into your worktree. It has fully configured music sources for YT Music, Apple music, Spotify and Tidal

Stop early when:
- An issue does not have enough info to triage / reproduce. In that case the outcome is a suggested draft reply to the user
- An issue is a user-error or configuration error (ie no code error). In that case also suggest a draft reply to the user

## Output report
An output report with a column per issue state. I would like to see every issue as a separate card in the report

I want to see at least the following badges on the cards:
- 'Waiting for user' -> Either me or a colleague replied and we are waiting for the user to respond
- 'Needs attention' -> The issue requires an action from me

No need to put badges like 'Draft reply' etc. in there, I expect them to be there when I open the issue.

Sort the issue cards in each column based on what needs my attention first

When I click the issue card I want to see:
- In case the badge/status is 'needs my attention', a highlighted pane at the top with a 1-2 sentence summary what action is needed from me
- A description / summary of what was done / the status
- An optional link to a draft PR
- When applicable, a suggested draft comment to ask for more info + a button to copy the contents so that I can quickly post it to github.

## Issue bookkeeping
- Whenever one of our draft PRs is fully green according to copilot, I want the draft reply to reflect a question to ask the user to verify our fix. You can use the Github saved reply ‘devaddon’ for it.
- Whenever a PR with a related issue gets merged, we need to update the issue accordingly
    - Set the ‘Fix to be confirmed’ label on the issue
    - When the ‘backport to stable’ label has been added, set the milestone to the next patch version (e.g. 2.10.5)
    - When no backport label exist:
        - If the PR can be easily backported, let me know. I expect this to show up as a badge on the card of the Issue in the ‘Fix merged’ column
        - If it cannot be easily backported, set the milestone on the issue to the next minor version (eg 2.11.0)

Make sure to merge the content from previous runs in the report, include the reports from the past days.  For example, this means that if the previous run worked on an issue that has not changed since, I still want to see it in the report of the current run if I have not acted on it yet. An issue will only leave the report when it gets closed