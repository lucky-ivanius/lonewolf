---
name: build-mvp
description: Pair with a coding agent to build an MVP step by step. Use when the user wants to build the MVP iteratively with an agent.
---

## What this skill does

Runs a structured pair-programming loop for building an MVP. Breaks the work into commit-sized slices, applies a quality gate per slice, hands off to specialist skills (`build-ai-agent`, `security-review`, design skills) at the right moments, and updates `.lonewolf/build-context.md` after each slice.

## When to use it

- Building the first non-trivial slice of a project end to end.
- The user wants a guided loop rather than ad-hoc requests.
- Multi-day or multi-session builds where context can drift between sessions.

## When NOT to use it

- Single, contained tasks; just do the task.
- Pre-scaffold; use `scaffold-project` first.
- Pre-plan; if scope is non-trivial and no `.lonewolf/build-plan.md` exists, use `plan-before-code` first to land a single approved plan. This skill's per-step gates work against that plan, not in place of it.
- Pre-intent; if scope is unclear, use `clarify-intent` to land `.lonewolf/intent.md` before this skill takes over.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- A scaffolded project.
- `.lonewolf/build-context.md` (required for this skill; refuse if absent).
- Optional: `.lonewolf/idea-context.md` for product context.

If unclear, interview the user for:

- What is the next sliced goal? (One sentence.)
- What does "done" look like for this slice? (One sentence.)
- Which specialist skill (if any) does this slice need (`build-ai-agent`, a design skill, `security-review`)?

## Outputs

- Code changes for this slice, committed in a clean diff.
- Updated `.lonewolf/build-context.md` reflecting what shipped.
- A handoff note for the next slice.

## Workflow

1. **Context gathering**
 - Read `.lonewolf/build-context.md`. Refuse to proceed if missing.
 - Confirm the next slice with the user in one sentence.

2. **Slice the work**
 - Break the slice into 3 to 6 small steps, each commit-sized.
 - Walk the breakdown back to the user. Adjust before coding.

3. **Per step**
 - Implement the step.
 - Run the relevant build/test for the affected layer.
 - If a specialist skill is appropriate, hand off to it for that step (e.g. `build-ai-agent` for agent wiring, `number-formatting` for numeric UI, `frontend-design-guidelines` for the design pass).
 - Apply the quality gate (see below) before moving on.

4. **Per-slice quality gate**
 - All tests pass.
 - Build is clean.
 - The slice's success criterion (defined in step 1) is demonstrably met.

5. **Commit**
 - Squash to a single commit per slice with a clear message.
 - Append a `## build-mvp session, <timestamp>` entry to `build-context.md`.

6. **Handoff note**
 - Next slice intent, blockers, open questions.

7. **Closing handoff**
 - If `.lonewolf/intent.md` exists and the slice was non-trivial (new surface, new integration, or material changes to public endpoints or data models), recommend `verify-against-intent` as the next step so drift is caught before the next slice or shipping.
 - If no `intent.md` exists and the slice was non-trivial, surface that gap once: offer `clarify-intent` to backfill, do not force it.

## Quality gate (anti-slop)

Before reporting the slice done, the skill asks itself the following and refuses to declare success if any answer is no:

- Does the relevant build/test command pass cleanly?
- Has the slice's success criterion been demonstrated, not just asserted?
- Are there leftover `TODO:` comments, commented-out code, or stub functions in the diff? If yes, finish or remove them.
- Has `.lonewolf/build-context.md` been updated?
- For integration slices, is the integration load-bearing in a user flow, not just imported?
- For ship-adjacent slices, has the user been pointed at the next anti-slop skill (`validate-business-model`, `retention-loop`, `roast-my-product`)?

If any answer is no, the skill reports the gap and works through it before claiming the slice is complete.

## References

On-demand references (load when relevant to the user's question):

- `references/slice-shapes.md`: Common slice shapes (backend + test, frontend + connect, integration, end-to-end demo).
- `references/handoff-rules.md`: When to hand off to a specialist skill vs continue here.

## Use in your agent

- Claude Code: `claude "/launchkit:build-mvp <your message>"`
- Codex: `codex "/build-mvp <your message>"`
- Grok Build: run `grok`, then `/build-mvp <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "help me build the MVP", or load `~/.cursor/rules/build-mvp.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
