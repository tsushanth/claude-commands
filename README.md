# Custom commands — what's here and when to use it

**Not sure where to start? Run `/sush-runbook`** — it maps "what are you here to do" (new feature,
fix bugs, just orienting yourself, checking ship-readiness, etc.) directly to the right command
sequence, and can run that sequence for you. Everything below is the reference for when you already
know which command you want.

Two separate flows, plus a couple of standalone tools. Descriptions below are pulled from each
command's own frontmatter — if they ever drift, the `.md` file is the source of truth, not this file.

## Flow 1 — auditing and fixing an *existing* codebase

Use this when the goal is "find out what's wrong with this system and fix it," not building
something new.

| Command | When to use |
|---|---|
| `/sush-system-overview` | First look at any codebase — publishes an architecture diagram and a request/call-journey diagram. Add `simple` for a plain-language version. |
| `/sush-intent` | After the overview — derives what the system is *actually trying to do*, from the code itself (not docs), and flags where docs and code disagree. |
| `/sush-blindspots` | After intent is established — checks whether the system actually achieves it. Top-down: whole-system flows first, then each component in isolation. Diagnostic only, finds but doesn't fix. |
| `/sush-harden` | After blind spots are found — fixes each one with a regression test, re-running `/sush-blindspots` each round, until 2 consecutive rounds turn up nothing new. Then codifies guardrails and does final verification. This is the only one of the four that changes code. |
| `/sush-fix-repo-cycle` | Runs all four of the above in sequence, in one command — still pauses for your review after each step. Use this instead of invoking the four manually if you just want the whole flow to happen with less typing. |

## Flow 2 — building a *new* feature

Use this when you have a specific requirement to implement.

| Command | When to use |
|---|---|
| `/sush-system-overview` | Same as above — orient yourself before proposing a change. |
| `/sush-explain-approach` | Turn a requirement into a reviewable proposal (current state, approach, file-by-file plan) *before* any code is written. Hard approval gate — nothing gets built until you say go. |
| *(implementation happens here, following the approved plan)* | |
| `/sush-code-review` | After implementing — runs a code review, then sanity-checks the change against this repo's real test/build/run setup. Includes a TODO/FIXME scan. |
| `/sush-coverage` | Critically checks test coverage for the change (default) or the whole system (`full` argument) — not a percentage, an assessment of what's actually exercised vs. just imported. |
| `/sush-guardrails` | Checks the change against `GUARDRAILS.md`'s accumulated rules, and proposes new rules for risks it covers that aren't codified yet. |
| `/sush-new-feature-cycle` | Runs overview → explain-approach → implementation → code-review/coverage/guardrails in one command, pausing between each. Use instead of invoking them manually. |

## Other wrapped intents (no multi-step flow of their own, but still one-command)

| Command | When to use |
|---|---|
| `/sush-orient` | `/sush-system-overview` → `/sush-intent`, then stops. Use when you need to understand a codebase before touching anything — no changes intended yet. |
| `/sush-catch-up` | `/sush-explain-code-change`, plus `/sush-code-review` if you ask for it. Use when returning after time away, or reviewing someone else's work, and want to know what changed. |
| `/sush-ship-check` | `/sush-coverage full` → `/sush-guardrails` → `/sush-code-review`, ending with a go/no-go verdict. Use for a pre-release gate — "can this go out the door?" |
| `/sush-refactor-cycle` | Same shape as `/sush-new-feature-cycle`, but the requirement is framed as a refactor and `/sush-coverage` afterward is treated as proof nothing behaviorally changed. Use for restructuring without adding a capability. |

## Standalone utilities (either flow, or on their own)

| Command | When to use |
|---|---|
| `/sush-explain-code-change` | Plain-language recap of whatever's currently uncommitted in the repo — faster than reading a raw diff. No argument for the whole working tree; add a file for a deep dive on just that file. |
| `/sush-change-full-review` | A single heavier one-shot: code review + unit tests + sanity/integration tests + a security review, all published as one combined artifact. Use when you want the full picture in one pass rather than running the individual pieces separately. |
| `/sush-runbook` | The entry point when you're not sure which command fits — maps an intent (new feature, fix bugs, orient, ship-readiness, refactor, catch up) to the right command, and can run it for you after confirming. |

## Quick decision

- **"What is this codebase and is it healthy?"** → `/sush-fix-repo-cycle`
- **"I need to build X."** → `/sush-new-feature-cycle "X"`
- **"What changed just now?"** → `/sush-explain-code-change`
- **"Give me everything on this diff, once."** → `/sush-change-full-review`
- **"I just want to understand this codebase, not change it."** → `/sush-orient`
- **"Can this ship?"** → `/sush-ship-check`
- **"I'm restructuring this without changing behavior."** → `/sush-refactor-cycle "X"`
- **"Not sure which of the above I want."** → `/sush-runbook`
