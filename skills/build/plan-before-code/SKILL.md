---
name: plan-before-code
description: Plan the build before writing code (data model, permissions, public surface, integration commitments). Use when the user wants a build plan before code.
---

## What this skill does

Turns `intent.md` into a file-level plan the user has approved. Forces the decisions that are expensive to reverse: the data model and who can touch each entity, the permission boundaries, the public surface (endpoints, entry points), per-integration verification commitments, and the rollout order. The plan is the contract `verify-against-intent` later checks against.

`clarify-intent` answers "what". This skill answers "how, in just enough detail that yes/no is possible before any code is written".

## When to use it

- `clarify-intent` just ran and the build is non-trivial.
- The user explicitly asks for a plan, an outline, or "walk me through the approach".
- The build touches more than one surface, a shared data store, a load-bearing integration, or a payment flow.
- The user is mid-build and lost about what comes next.

## When NOT to use it

- The build is a single one-function change. Just make the change.
- `.lonewolf/intent.md` does not exist. Use `clarify-intent` first.
- The user is reviewing existing code, use `product-review` or `security-review`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/intent.md` (required, from `clarify-intent`).
- Optional: `.lonewolf/build-context.md` from a prior session.
- Optional: existing project tree if any code already exists.

If `intent.md` is missing, stop and hand off to `clarify-intent`. Do not invent intent.

## Outputs

A single file at `.lonewolf/build-plan.md` with this schema:

```markdown
# Build plan, <timestamp>

## Linked intent
.lonewolf/intent.md, summary: <one sentence from intent>

## Project layout
- Packages/surfaces: <single app | web + server | web + server + contracts | agent service>
- Dependencies: <the load-bearing SDKs, pinned versions>

## Data model (per entity, forced decisions)
- <Entity>:
 - Storage: <db table | on-chain | file | external service>
 - Access: <who can read, who can write, enforced where>
 - Purpose: <one line>
 - Created by: <function/endpoint>
 - Mutated by: <function/endpoint(s)>
 - Deleted by: <function/endpoint, or "never">
- <next entity>: ...

## Permission boundaries
- <boundary, e.g. "admin panel", "paid tier">:
 - Who is allowed: <role/condition>
 - Enforced at: <middleware | guard | on-chain check | row-level policy>
 - Unauthorized caller sees: <error shape>

## Public surface
- `<METHOD /path>` or `<module::fn>`: <effect, caller, auth required>
- ...

## Core flow
- The canonical sequence a user walks from entry to value, step by step, naming the surface each step touches.

## Tests (one per intent.md success criterion)
- <test_name>: covers <surface>, expected <pass | rejection>, ties to intent criterion #<n>
- ...
- Any success criterion without a test: flag here as a risk.

## Integrations (load-bearing, with verification commitment)
- <LLM | payments | storage | chain | data API>:
 - Surface: <which calls, from where>
 - Load-bearing test: <how `verify-against-intent` will prove it is not decorative>
 - Failure path: <what the user sees when it is down>
 - Reference: <docs URL the plan grounded this on>

## Rollout
- Order: local -> staging/testnet -> production/mainnet (or stop earlier)
- Per-environment exit criterion: <what must be true before moving up>

## Ops authority
- Who holds deploy credentials, admin keys, upgrade authority: <named>
- Where secrets live: <env store, never in source>

## Risks and unknowns
- <risk>: severity, how we will resolve (fetch docs, prototype, ask the user)
- ...

## Order of build
1. <step that proves the riskiest unknown first>
2. <step>
3. ...

## What "done" looks like for this plan
- <observable outcome tied to intent.md success criterion #N, in which environment>
```

The skill stops here. No code. Hand off to the right build skill.

## Workflow

1. **Read intent and prior context**
 - Read `.lonewolf/intent.md`. Missing? Stop and hand off to `clarify-intent`.
 - Read `.lonewolf/build-context.md` if present.
 - If a project already exists on disk, list current packages and pinned deps in one short summary.

2. **Project layout first**
 - Single app or multi-package? Multi-package means deployment boundaries and version management; only pick if intent justifies it.
 - Pin the load-bearing dependencies. Floating versions are slop; refuse them.

3. **Data model with forced decisions**
 - For each entity the intent named: decide where it lives and who can touch it.
 - Decide who creates, mutates, deletes. Each verb mapped to a function or endpoint name.

4. **Permission boundaries**
 - Each boundary gets an allowed-set, an enforcement point, and an unauthorized-caller behavior.
 - Refuse "we will figure out permissions later". UI-only enforcement is a fail at plan time, not just review time.

5. **Public surface and core flow**
 - List public endpoints or entry functions. Anything not public is implementation detail; do not enumerate.
 - Draw the canonical core flow from entry to value.

6. **Tests tied to intent criteria**
 - Each success criterion in `intent.md` maps to at least one named test.
 - Permission-gated surfaces get an expected-rejection test for the unauthorized call.
 - Any criterion with no testable path: flag in Risks.

7. **Integrations**
 - For each integration in intent: define the surface, then commit to how `verify-against-intent` will prove it is load-bearing (e.g. "a staging URL where the uploaded file is read back into the UI", not "import path exists").
 - Define the failure path while you are here.
 - If you cannot define a load-bearing test, refuse the integration and write that down.

8. **Rollout and ops authority**
 - Environment order with per-environment exit criteria.
 - Name who holds deploy credentials and admin keys. Each option has consequences; surface them.

9. **Risks, unknowns, build order**
 - Risks named with severity and resolution path.
 - First build step proves the riskiest unknown, not the easiest one.

10. **Walk the plan to the user**
 - Two-sentence summary, then "approve, edit, or scrap?".
 - Do not write the file until the user approves. Acknowledgment is not approval; ask for explicit yes.

11. **Write `.lonewolf/build-plan.md`** and hand off to the right build skill via `skills/SKILL_ROUTER.md`.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself:

- Does every entity have a decided storage location and access rule, not "we'll figure it out"?
- Does every permission boundary have a named enforcement point?
- Does every success criterion in `intent.md` map to at least one named test?
- Does every integration have a load-bearing verification commitment (a real artifact to check), not "import exists"?
- Is the ops authority decision named, with consequence acknowledged?
- Are the load-bearing deps pinned, not floating?
- Did the user explicitly approve the plan, not just nod?
- Did I avoid starting any code, even a stub?

If any answer is no, the skill keeps working before handing off.

## Use in your agent

- Claude Code: `claude "/launchkit:plan-before-code <your message>"`
- Codex: `codex "/plan-before-code <your message>"`
- Grok Build: run `grok`, then `/plan-before-code <your message>` in the session
- Cursor: paste a message like "plan the build before we code", or reference `~/.cursor/rules/plan-before-code.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
