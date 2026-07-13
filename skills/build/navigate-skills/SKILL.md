---
name: navigate-skills
description: Launch Kit skill catalog and router. Use when the user's request is vague, asks "which skill", "what's available", "how do I X", or does not clearly map to one specific launchkit skill. Lists relevant skills and hands off.
---

## What this skill does

A meta skill. It lists what skills are installed, groups them by phase (learn / idea / build / ship / grow / earn), and helps the user pick the right one for their current goal. It reads the routing table in `skills/SKILL_ROUTER.md`. It is the answer to the question "which launchkit skill should I use right now".

It does not run other skills. It hands the user a name and the trigger phrase to use. The user activates the chosen skill.

## When to use it

- The user says they do not know which skill applies.
- The user wants a tour of what is available before committing to a workflow.
- An agent (Claude Code, Codex, Cursor) routes a vague request here for triage.

## When NOT to use it

- The user has a clear goal that maps to one skill, hand off directly to that skill.
- The user is asking about one specific skill's behavior, read its `SKILL.md` instead.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The user's stated goal, even if vague ("I want to launch something", "I am stuck").
- Optional: `.lonewolf/idea-context.md` or `.lonewolf/build-context.md` to constrain suggestions to the user's project shape.

If unclear, ask:

- Are you exploring (no project yet), building (have code), shipping (ready to launch), or growing/earning (launched, need users or revenue)?
- What is the friction right now: idea, code, design, security, launch, growth, revenue?

## Outputs

A short response of three parts:

1. **Best fit**: one skill name, its trigger phrase, one sentence on what it will do.
2. **Adjacent options**: up to two alternatives with one-sentence reasons to pick each instead.
3. **If none fit**: a one-liner saying so, with the repo's GitHub issues URL for a skill request.

The skill never invents skill names. If the catalog does not contain a skill matching the user's goal, it says so.

## Workflow

1. **Load the catalog**
 - Read `skills/SKILL_ROUTER.md` — the canonical routing table with every skill, its phase, and its triggers.

2. **Classify the goal**
 - Map to a phase: learn, idea, build, ship, grow, earn.
 - Map to a category: intent/planning, coding, design, anti-slop review, security, launch, distribution, revenue.

3. **Match against the catalog**
 - For each skill, score the description and the trigger phrase list against the user's goal text.
 - Pick the top scorer as best fit.
 - Pick up to two adjacent skills, prefer ones in the same phase.

4. **Apply router rules**
 - If `skills/SKILL_ROUTER.md` declares a common-wrong-pick route for the user's phrasing, use that route's recommendation.

5. **Format the response**
 - Three parts above.
 - Always include the trigger phrase verbatim (so the user can copy-paste).
 - Always include the phase the chosen skill belongs to.

6. **Hand off**
 - The skill ends without invoking the recommended skill. The user activates it explicitly.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself:

- Did I actually read the catalog, or did I recall skill names from memory?
- Is the recommended skill name an exact match in the catalog (not a hallucination, not a near-miss)?
- Did I include the trigger phrase verbatim from the skill's description?
- Did I include adjacent options only when they are real alternatives, not filler?
- If no skill fit, did I say so plainly instead of forcing a bad match?

If any answer is no, the skill keeps working before recommending.

## References

On-demand references (load when relevant to the user's question):

- `references/phase-glossary.md`: One-paragraph definitions of the six phases.
- `references/router-precedence.md`: Tie-break rules when two skills could apply.

Knowledge docs (load when scope expands beyond what is in references):

- `skills/SKILL_ROUTER.md`: The canonical per-trigger routing table.

## Use in your agent

- Claude Code: `claude "/launchkit:navigate-skills <your message>"`
- Codex: `codex "/navigate-skills <your message>"`
- Grok Build: run `grok`, then `/navigate-skills <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "which launchkit skill should I use", or load `~/.cursor/rules/navigate-skills.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
