---
name: verify-against-intent
description: After a build session, check what was built against .lonewolf/intent.md and flag drift. Use when the user wants to verify their build matches intent.
---

## What this skill does

Closes the loop on a build session with disk-grounded checks, not session-memory claims. Walks the plan's data model, auth and permission boundaries, integration surfaces, and deploy posture against actual source, config, and the test suite. Names drift honestly: decorative integration claimed as load-bearing, permissions enforced nowhere, core flows without tests, deploy state that does not match the planned rollout.

Without this gate, agents say "done" against a target that was never written. With it, every criterion gets a pass / fail / partial verdict tied to a concrete artifact.

## When to use it

- A build skill (`build-mvp`, `scaffold-project`, `build-ai-agent`, etc.) just reported done.
- The user says "did we get there", "is this what I asked for", or "review what we built".
- A session is about to be wrapped, before `learn` captures it.
- The user is preparing to ship: run this before `launch-checklist`.

## When NOT to use it

- `.lonewolf/intent.md` does not exist. There is nothing to verify against. Use `clarify-intent` to backfill, then re-run.
- The user wants a security audit, use `security-review` (different gate, P0 to P3 findings).
- The user wants UX critique, use `product-review` or `roast-my-product`.
- The user wants a code-style review. Not this skill.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- `.lonewolf/intent.md` (required).
- `.lonewolf/build-plan.md` if `plan-before-code` ran.
- `.lonewolf/build-context.md`, `.lonewolf/deploy-context.md` if present.
- The current project tree: sources, config, lockfiles, `tests/`, frontend if any.

If `intent.md` is missing, stop. Tell the user verification needs a recorded intent. Offer `clarify-intent` to backfill.

## Outputs

A printed report and an append to `.lonewolf/build-context.md`:

```markdown
## verify-against-intent, <timestamp>

### Success criteria check
1. <criterion>: pass | fail | partial, evidence: <file:line | passing test | deployed URL | tx hash>
2. <criterion>: pass | fail | partial, evidence: <...>
...

### Data model check
- <Entity>: planned <shape, who can touch it> | actual <schema/model in code>: match | mismatch
- ...

### Auth and permission check
- <boundary>: planned <who is allowed> | actually enforced at <file:line, or "nowhere">: match | mismatch

### Integration check (load-bearing test)
- <LLM | chain | storage | data API>: planned surface <...> | actual: <imported only | called but result unused | load-bearing in user flow with evidence <...>>

### Test coverage check
- Core-flow paths with at least one test: <count> / <total>
- Permission-gated paths with an expected-failure test for the unauthorized caller: <count> / <total>
- Paths missing tests: <list>

### Build check
- Project build command (`npm run build` or equivalent): pass | fail (<error summary if fail>)
- Dependencies pinned (lockfile committed, no floating majors): yes | no

### Deploy posture check
- Planned rollout: <local -> staging -> production, or chain testnet -> mainnet>
- Actual deploy state: <per-environment URL / package id from deploy-context.md, or "not deployed">
- Credential and key holders: planned <X> | actual <Y>: match | mismatch

### Drift summary
<one paragraph, honest>

### Follow-ups
- <thing to fix, with the skill that fixes it>
- <thing to document as accepted scope cut>
```

The skill does not fix the drift. It names it, points to the right skill, hands off.

## Workflow

1. **Read inputs**
 - `.lonewolf/intent.md`. Missing? Stop.
 - `.lonewolf/build-plan.md` if present.
 - `.lonewolf/build-context.md`, `.lonewolf/deploy-context.md`.
 - List source directories, schema/model files, tests, config, frontend entry points if applicable.

2. **Map criteria to concrete evidence**
 For each success criterion in `intent.md`, decide what would prove it:
 - A function or endpoint existing? Check for it in source.
 - A test passing? Run the named test.
 - A deploy? Check `deploy-context.md` for a URL or package id in the right environment, then hit it.
 - Frontend behavior? Check that the route calls the named endpoint or action.
 - An integration in the user flow? Check that the call sits inside a real user path, not a top-of-file import only.

 Do not trust session memory. Read disk.

3. **Data model check**
 For each entity in `build-plan.md`:
 - Find the schema, model, or struct in source.
 - Compare to plan. Note mismatch on ownership and access (e.g. plan said user-owned and private, code exposes it in a public list endpoint).

4. **Auth and permission check**
 For each permission boundary in the plan:
 - Locate where it is enforced (middleware, guard, on-chain check, row-level policy).
 - "Enforced in the UI only" is a fail. Compare to plan. Surface mismatch.

5. **Integration load-bearing check**
 For each integration in `build-plan.md` (LLM, chain, storage, data API):
 - Confirm the call exists in source.
 - Confirm the call is reachable from a user-triggered path.
 - Confirm the result is used (the model output changes what the user sees, the stored file is read back, the tx result updates state, etc.).
 - If only imported, or the result is unused: mark decorative. This is the most common drift; do not soften it.

6. **Test coverage check**
 - Count core-flow paths vs paths covered by at least one test.
 - For each permission-gated path, confirm there is an expected-failure test for the unauthorized caller.
 - List the gaps.

7. **Build check**
 - Run the project's build command. Record pass or fail.
 - Confirm dependencies are pinned: lockfile committed, no floating major versions on load-bearing deps.

8. **Deploy posture check**
 - Compare planned rollout to `deploy-context.md`. Per environment: deployed or not, URL or package id captured or not.
 - Compare planned credential/key holders to what the deploy actually uses.

9. **Call drift honestly**
 - For each mismatch above: pass, fail, or partial. Evidence tied to a file path, a test name, a URL, or a tx hash.
 - Anything in the plan but not built: was it intentionally cut or forgotten? Ask the user.
 - Anything built but not in the plan: scope creep, accident, or load-bearing? Ask.
 - Anything in intent but not in plan or code: worst kind of drift, surface first.

10. **Report and writeback**
 - Print the report shape above.
 - Append the same text to `.lonewolf/build-context.md` under a new `## verify-against-intent` section.

11. **Hand off**
 - Clean report: recommend `launch-checklist` or `learn`.
 - Drift exists: recommend the right skill for the largest gap via `skills/SKILL_ROUTER.md`.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself:

- Did I read actual source and config, not recall from session memory?
- Did every success criterion get pass / fail / partial with a concrete artifact reference (file:line, test name, URL, tx hash)?
- Did I run the integration load-bearing test, not accept "the import is there"?
- Did I check where permissions are enforced, not just that a role field exists?
- Did I name drift honestly when the build skill called itself done?
- Did I avoid fixing the drift in this skill (the job here is calling, not fixing)?
- Did I append the verdict to `.lonewolf/build-context.md` so the next session sees it?

If any answer is no, the skill keeps working before reporting done.

## Use in your agent

- Claude Code: `claude "/launchkit:verify-against-intent <your message>"`
- Codex: `codex "/verify-against-intent <your message>"`
- Grok Build: run `grok`, then `/verify-against-intent <your message>` in the session
- Cursor: paste a message like "did we build what I asked for", or reference `~/.cursor/rules/verify-against-intent.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
