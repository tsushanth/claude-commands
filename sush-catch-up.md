---
description: Catch up on recent changes in the git working tree — plain-language explanation, optionally plus a quality review
argument-hint: [optional file path for a diff-level deep dive, and/or "review" to also run /sush-code-review]
---

Runs the catch-up flow — `/sush-explain-code-change` on its own by default, or followed by `/sush-code-review` if you also want a quality judgment on top of the description.

This is the **catch up on recent changes** pass — use it when you're returning after time away, or reviewing someone else's work, and want to know what changed before forming an opinion on whether it's good.

Parse `$ARGUMENTS`: if it contains "review" (in any phrasing — "and review it", "with review", "review too"), strip that phrase out and treat it as a request to also run `/sush-code-review` after the explanation; whatever remains (if anything) is passed through to `/sush-explain-code-change` as its file argument for a diff-level deep dive.

## Steps

1. **Run `/sush-explain-code-change [remaining arguments]`.** Report its output — the plain-language explanation of what changed in the git working tree (uncommitted, vs HEAD).

2. **If "review" was requested: run `/sush-code-review`** (current diff, at the level last used or `medium` if none specified) against the same working-tree changes. Report findings.

3. **If "review" was not requested:** stop after step 1. Don't run a review the user didn't ask for — ask them to `/sush-catch-up review` (or `/sush-code-review` directly) if they decide they want one after reading the explanation.

## After the flow completes

State clearly which step(s) ran. This command doesn't fix anything — if the explanation or review surfaces something that needs action, point at the fix/harden or new-feature flows via `/sush-runbook` rather than starting an implementation here.

## When to use this

When you want to know what changed in the working tree without re-typing `/sush-explain-code-change` yourself, and optionally want a quality judgment layered on top without a second manual command.
