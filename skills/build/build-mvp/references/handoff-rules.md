# When to hand off

`build-mvp` is the loop. Specialist skills are the work. Handoff at the right moment.

## Hand off to `build-ai-agent` when

- The slice adds an agent, an LLM pipeline, or tool-calling to the product.
- The user is deciding between agent architectures.

Bring back: the working agent path, its eval or smoke test, and the cost-per-run estimate.

## Hand off to `brand-design` / `frontend-design-guidelines` / `design-taste` when

- The slice is the first user-facing surface and no brand tokens exist yet (`brand-design`).
- The surface works but looks like a template (`frontend-design-guidelines`, `design-taste`).

Bring back: applied tokens or the concrete fix list, not a mood board.

## Hand off to `number-formatting` when

- The slice renders money, tokens, or metrics and the formatting is inconsistent.

Bring back: the shared formatter module and updated callsites.

## Hand off to `security-review` when

- The slice touches auth, payments, keys, or anything that auto-spends.
- The pre-ship slice begins.

Bring back: the findings list with every P0 resolved or explicitly accepted.

## Hand off to `verify-against-intent` when

- A non-trivial slice claims done (new surface, new integration, changed public endpoints or data model).
- The session is about to wrap.

Bring back: the drift report appended to `.lonewolf/build-context.md`.

## Stay in the loop when

- The step is ordinary application code the agent can carry (CRUD, wiring, styling, tests).
- The specialist skill would just re-derive context the loop already has.

## Sequencing near ship

- `launch-checklist`: only after `validate-business-model`, `retention-loop`, and `security-review` are clean.
- `positioning` and `landing-copy`: once the core flow demos end to end, in parallel with hardening.
- `distribution-plan`: before launch day, never on it.
