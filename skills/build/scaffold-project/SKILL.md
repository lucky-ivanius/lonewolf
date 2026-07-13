---
name: scaffold-project
description: Scaffold a new project with the right stack and write build-context.md. Use when the user wants to start, init, bootstrap, set up, or create a new project, workspace, or template, in any phrasing.
---

## What this skill does

Bootstraps a fresh project on disk. Picks a template based on the project intent (API-only service, frontend app, full-stack, agent service, or any of those plus contracts), wires the standard dependencies, generates the manifests, and writes the canonical `.lonewolf/build-context.md` so subsequent skills know where the project lives and what stack it uses.

## When to use it

- Starting a new project from scratch.
- Adding a new surface (contracts package, agent service) to an existing repo that does not have one yet.
- Restarting on a clean scaffold after a prototype outgrew its structure.

## When NOT to use it

- If the user has not picked an idea yet, use `find-next-idea` first.
- If the user already has a working project and wants to extend it, use `build-mvp`.
- For deploys, use `launch-checklist`.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- A target directory (default: current working directory).
- Optional: `.lonewolf/idea-context.md` from `find-next-idea`. Read it if present to derive project shape.
- The intended deployment target (local first, then staging/testnet, then production/mainnet).

If unclear, interview the user for:

- API-only, frontend-only, full-stack, or agent service? Contracts in or out?
- TS frontend (Next.js, Vite) or something else?
- Which integrations are load-bearing from day one (LLM, payments, storage, chain)? Default them in only if so.
- Which package manager (pnpm preferred)?

## Outputs

- A directory with the chosen template's tree (see `references/template-shapes.md`), plus `.lonewolf/build-context.md`, `.gitignore`, `README.md`.
- Initial `.lonewolf/build-context.md` containing:

 ```markdown
 ## scaffold-project session, <timestamp>
 - project name: <name>
 - stack: <api-only | frontend | full-stack | agent-service>
 - frontend: <none | next | vite>
 - contracts: <none | which chain>
 - load-bearing integrations: <llm | payments | storage | chain | none>
 - target environment: <local | staging | testnet>
 - open issues: <list>
 ```

- A working build: `pnpm install + pnpm build` (and the contract build, if contracts are in).

The skill never deletes files outside the scaffolded directory without explicit user confirmation.

## Workflow

1. **Context gathering**
 - Read `.lonewolf/idea-context.md` if present.
 - Confirm project name, stack shape, load-bearing integrations.

2. **Pick the template**
 - API-only: a single service skeleton with tests.
 - Frontend app: Next.js or Vite, typed API client, one starter page.
 - Full-stack: as above plus a backend service shell.
 - Agent service: a runtime loop, tool definitions, an eval harness stub.
 - Contracts add-on: a contracts package with its own build and tests, wired to the app via a typed client.

3. **Generate the directory tree**
 - Create directories.
 - Write manifests with pinned dependency versions.
 - Write a placeholder module so the build succeeds immediately.
 - Write a starter test for the placeholder module.

4. **Frontend setup (if applicable)**
 - Scaffold Next.js or Vite.
 - Install only what the intent requires; check each SDK's current package name against its official docs (SDKs rename; legacy packages stop getting patches).
 - Write a starter page that exercises one real data path (sign-in, or a fetch rendered on screen).

5. **Integration defaults (if applicable)**
 - Wire only the integrations the user declared load-bearing (LLM client, payment provider, storage, chain SDK).
 - Do not add integrations "just in case". Default off.

6. **Quality verification**
 - Run the full build. Confirm zero errors.
 - Run the starter tests. Confirm they pass.

7. **Write build-context.md**
 - Capture stack decisions for downstream skills.

8. **Hand off**
 - Recommend `plan-before-code` next if the data model or integration surface is non-trivial.
 - Recommend `build-mvp` to start the first slice.

9. **Closing handoff**
 - If `.lonewolf/intent.md` exists and the scaffold was non-trivial (integration wired in, multi-package layout), recommend `verify-against-intent` as the next step so the generated tree is checked against recorded intent before code lands on top.
 - If no `intent.md` exists and the scaffold was non-trivial, surface that gap once: offer `clarify-intent` to backfill, do not force it.

## Quality gate (anti-slop)

Before reporting done, the skill asks itself the following and refuses to declare success if any answer is no:

- Does the full build succeed in the scaffolded directory with zero errors?
- Do the starter tests pass?
- Are dependencies pinned (lockfile written, no floating majors on load-bearing SDKs)?
- Is `.lonewolf/build-context.md` written with the stack decisions?
- Are integrations only added when the user explicitly opted in, not "just in case"?

If any answer is no, the skill reports the gap and works through it before declaring the project scaffolded.

## References

On-demand references (load when relevant to the user's question):

- `references/template-shapes.md`: Template shapes with file trees.

## Use in your agent

- Claude Code: `claude "/launchkit:scaffold-project <your message>"`
- Codex: `codex "/scaffold-project <your message>"`
- Grok Build: run `grok`, then `/scaffold-project <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "scaffold my project", or load `~/.cursor/rules/scaffold-project.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
