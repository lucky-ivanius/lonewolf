---
name: clarify-intent
description: Pin down what the user is actually building before any code. Use when the user wants to build, create, make, develop, scaffold, implement, or ship anything, in any phrasing, trigger eagerly. Skip if .lonewolf/intent.md already exists for this session.
---

## What this skill does

Stops the agent from jumping into code. Forces a short, product-shaped interview that names the core flow, the stack, the load-bearing integrations (chain, LLM, third-party APIs — load-bearing or skip), the launch posture, and observable success. Writes `.lonewolf/intent.md` so the rest of the build phase reads from one source of truth.

This is the anti-slop entry gate. Skipping it is how launch-week projects end up with three features that share nothing, an AI integration that never affects the user flow, and a data model nobody can explain.

## When to use it

- The user asks for code but has not named the core flow, the audience, or what "working" means.
- The prompt is one line and broad ("build me a marketplace for AI agents"). Broad is fine, AI-assisted pace makes "marketplace in days" plausible, but the agent still needs the product shape pinned before writing code.
- The user explicitly says "step back" or "what am I really building".
- An upstream skill (`find-next-idea`, `validate-idea`) handed off and there is no `.lonewolf/intent.md` yet.
- The user is restarting a stalled session and previous intent is unclear.

## When NOT to use it

- `.lonewolf/intent.md` already exists in the current session, read it and hand off.
- The user is fixing a specific bug in existing code, fix the bug.
- The user is asking a knowledge question, answer the question.
- The user is in the idea phase (no project picked), use `find-next-idea` or `validate-idea` first.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The user's raw build request, however vague.
- Optional: `.lonewolf/idea-context.md` from `validate-idea`.
- Optional: `.lonewolf/build-context.md` from a prior build session.

## Outputs

A single file at `.lonewolf/intent.md` with this schema:

```markdown
# Intent, <timestamp>

## One-sentence summary
<what the user is building, in their voice>

## Problem and audience
- Problem: <real underlying pain, not the surface request>
- Audience: <who uses this, what they do today or elsewhere>
- Core valuable shape: <the one thing the product has to do well, even if everything else ships alongside it>

## Product shape
- Core flow: <the single path a user walks to get value, start to finish>
- Surfaces: <web app | mobile | API | agent | CLI | combination>
- Data the product owns: <the 3-6 entities that matter, who can touch each>

## Stack
- Frontend: <none | Next.js web | mobile | both>
- Auth: <none | OAuth | passkeys | wallet | combination>
- Backend: <none | serverless | dedicated service | on-chain program>
- AI: <none | which model calls, for what user-visible outcome>
- Chain: <none | which chain, what lives on it and why it must>

## Load-bearing integrations only
- <integration>: <what surface, why load-bearing>
- If a decorative integration was proposed, refuse and note here why it was dropped.

## Launch posture
- Target at launch: <private prototype | closed beta | public launch | production with paying users>
- Payments: <none yet | provider, and what is charged for>
- Ops surface: <where it deploys, who holds the keys/credentials>

## Success criteria
1. <observable outcome a stranger could verify, and where it's checked>
2. <observable>
3. <observable>

## Out of scope
- <thing the user might assume but is not in>
- <thing>

## Constraints
- Deadline: <date or event. Note, do not use the deadline to shrink scope by default. AI-assisted builds compress weeks into days.>
- Risk tolerance: <demo | pilot | production>
- Existing assets: <repos, deployed services, domains, audiences>

## Open questions for the user
- <thing the agent could not pin down>
```

The skill stops here. No code. Hand off.

## Workflow

1. **Read prior context**
 - If `.lonewolf/intent.md` exists and is fresh, read it, summarize, ask if it still holds. If yes, skip to step 6.
 - Otherwise read `.lonewolf/idea-context.md` and `.lonewolf/build-context.md` if present.
 - **Fast-path from idea phase**: if `idea-context.md` already has problem, audience, success criteria, pre-fill those and only ask for the product-shape and stack fields below. Do not re-ask what `validate-idea` already answered.

2. **Restate the request**
 - One sentence, in the user's voice. Confirm before continuing. A vague "yeah" is not confirmation.

3. **Walk the interview**
 Group questions so the user is not nickel-and-dimed. Five tracks:

 - **Problem track**: What pain, for whom, what do they do today? What is the one thing the product has to do well? Do not push for a scope cut here, AI-assisted pace means the full product is often in reach, the job is to name the load-bearing surface, not shrink the build.
 - **Product track**: What is the core flow, start to finish? What surfaces exist? What data does the product own, and who can touch each piece?
 - **Stack track**: Frontend or no? If yes, web or mobile? Auth how? Backend where? Does an LLM sit in the user flow, and what does the user see from it? Does a chain sit in the flow, and what must live on it?
 - **Integration track**: For every claimed integration (an LLM, a chain, a storage layer, a data API): is it load-bearing in the user flow, or decoration? Refuse to record decorative integrations as load-bearing.
 - **Ship track**: What is live at launch (prototype, beta, public)? Is anything charged for, through what? Deadline and risk tolerance?

4. **Push back on slop**
 - "Everyone" for audience: push for a specific named wedge.
 - "Good UX" for success: push for observable outcomes ("a new user signs up, completes the core flow, and gets the artifact within 2 minutes").
 - "AI-powered" with no flow: push for the specific model call and what the user sees change. If nothing user-visible changes, drop the integration.
 - "On-chain" with no reason: push for what breaks if the chain is removed. If nothing breaks, drop the chain claim.
 - "Production eventually" with no plan: surface that production means auth, payments, and ops decisions now.
 - One push-back round per topic. If the user insists on vague, record the vagueness honestly and move on.

5. **Write `.lonewolf/intent.md`** using the schema in Outputs. Open questions stay open. Timestamp it.

6. **Hand off**
 - If non-trivial (multiple surfaces, a load-bearing integration, or an unclear data model), recommend `plan-before-code`.
 - If one-step, point at the specific build skill via `skills/SKILL_ROUTER.md`.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself:

- Did I write `.lonewolf/intent.md`, not just talk about it?
- Is the product shape concrete: core flow named, data entities listed, surfaces decided?
- Is every claimed integration load-bearing in a user flow, not a decorative import?
- Is the launch posture named (what is live, what is charged), not deferred?
- Are success criteria observable by a stranger, not aspirational?
- Did I list out-of-scope items, not only in-scope?
- Did I avoid starting any code, even a stub?

If any answer is no, the skill keeps working before handing off.

## Use in your agent

- Claude Code: `claude "/launchkit:clarify-intent <your message>"`
- Codex: `codex "/clarify-intent <your message>"`
- Grok Build: run `grok`, then `/clarify-intent <your message>` in the session
- Cursor: paste a message like "step back, what am I really building", or reference `~/.cursor/rules/clarify-intent.mdc`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
