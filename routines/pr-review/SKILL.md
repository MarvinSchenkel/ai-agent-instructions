---
name: pr-review
description: Auto review PRs
---

I want you to go through every PR that has changed since the last time this routine ran. 'Changed' means either a new commit has been pushed to the PR, a user has submitted a comment, or anything else that has been modified. Don't focus on newly created PRs only. Ignore PRs that are still in draft.

## Review workflow
Parallelize the reviews using subagents when applicable. Per PR:
- Trigger a 'balanced' copilot review if no review has taken place yet or if changes have been pushed to the PR since the last Copilot review
- Do a review yourself
- Categorize it as S (< 50 lines) M (5-299 lines) L (300-999 lines) or XL (1000+ lines)

Judge if it is either:
- ready to merge' (both you and copilot have found no criticals / problems)
- ‘almost ready' (e.g. missing test, or something we can easily fix with a single commit), 
- 'not ready yet' (the code really needs attention

Request Copilot with gh api --method POST repos/music-assistant/server/pulls/N/requested_reviewers -f 'reviewers[]=copilot-pull-request-reviewer[bot]' and confirm the request on the PR timeline. Never use gh pr edit --add-reviewer @copilot, it silently does nothing.
Besides changed PRs, review and add every open non-draft PR that isn't in the report yet.

## Auto approval
You are allowed to auto-approve and merge a PR if:
- The PR is deemed ‘ready to merge’ (both you and copilot have found 0 issues)
- The PR is S, M or L
- It does not touch any core code, meaning the diff is scoped to only providers/* and the related tests

The following important providers are EXCLUDED from auto approve behavior: 
- Sonos
- Airplay
- Spotify
- Sendspin
- Squeezelite

When you auto approve the PR:
- Approve the PR with a message, thanking the original author by tagging their Github handle and a fun emoji, e.g. 'Thanks @OzGav, looks good to me :raised-hands:. Make sure to include the note that the comment was posted on my behalf by you and that it was auto approved because 2 reviews were green
- Set Auto merge on
- The CI will now be re-triggered and the PR should be auto-merged if all is green

## Output report
As output report, I want a matrix with the status (Y axis, can be 'waiting for user' or 'needs my attention') + ready state (x axis, 'ready to merge' etc). The X-axis should also contain 1 extra column which is 'auto approved' so I can keep track of the auto approved PRs. They should only show auto approved PRs up till 1 week ago. I have a personal SLA for 1 week, if a PR has been waiting for more than 1 week for my response, colour it appropriately so that I can react on it.

Whenever I click a PR, I want to see the following:
- Always: The review. The review pane should contain a button per finding with 'post to PR', so that I can quickly post your findings to the PR if I deem that worth fixing.
- 'almost ready':  A suggestion from you to fix the small outstanding thing + a button that will copy a prompt with the necessary context and instructions, so I can kick it off in a new chip session. The diff should be coloured so that I can easily see what gets added/removed

The following badges need to be added to t he matrix (only in the report not as github labels):
- Size (S, M, L etc)
- Draft when the PR is in draft
- 'backport recommended'. PRs that are 'auto approved' or 'ready to merge' should contain a label that indicates whether the PR should be backported to stable. We backport bugfixes that can be (almost) cleanly cherrypicked to stable.

The output report must always be up to date and combined with the previous report. For example, this means that if the previous run tagged an 'almost ready', I still want to see it in the report of the current run if I have not acted on it yet. A Pr will only leave the report when it gets merged (unless auto approved, which will be shown for 1 week)

The idea behind the exercise is that I can quickly stay on top of the PRs