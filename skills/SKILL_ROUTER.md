# Launch Kit skill router

The canonical routing table. 36 skills across six phases. Route by intent, not by keyword match alone; when two rows fit, `references/router-precedence.md` in `navigate-skills` breaks the tie.

## The journey

Learn -> Idea -> Build -> Ship -> Grow -> Earn. Phases hand off through `.lonewolf/` context files; any skill can also run standalone.

```
find-next-idea        writes ->  .lonewolf/idea-context.md
validate-idea         appends ->
clarify-intent        writes ->  .lonewolf/intent.md
plan-before-code      writes ->  .lonewolf/build-plan.md
scaffold-project      writes ->  .lonewolf/build-context.md
build-mvp             appends ->
verify-against-intent appends ->
launch-checklist      reads  ->  all of the above
```

## Learn

| Skill | Route here when |
|---|---|
| `founder-101` | New to building products or to the agent economy; wants the 0-to-1 vocabulary (MVP, PMF, distribution, unit economics). |
| `learn` | Wrap up a session; save decisions and learnings to `.lonewolf/learnings.md`. |

## Idea

| Skill | Route here when |
|---|---|
| `find-next-idea` | No idea picked; wants brainstorm, suggestions, "what should I build". |
| `validate-idea` | Has an idea; wants go/no-go on demand, competition, feasibility, distribution fit. |
| `competitive-landscape` | Wants a competitor map or "what's my edge". |
| `defi-market-research` | Wants DeFi TVL/protocol/yield data for a chain via DefiLlama. |

## Build

| Skill | Route here when |
|---|---|
| `clarify-intent` | About to build anything and `.lonewolf/intent.md` does not exist. Trigger eagerly. |
| `plan-before-code` | Intent exists, build is non-trivial, no approved plan yet. |
| `verify-against-intent` | A build session claims done; check drift against intent before shipping. |
| `navigate-skills` | Vague request; user does not know which skill applies. |
| `scaffold-project` | Start, init, bootstrap a new project or workspace. |
| `build-mvp` | Guided slice-by-slice MVP building loop. |
| `build-ai-agent` | The product is or contains an AI agent (tools, memory, payments, marketplace listing). |
| `brand-design` | Pick a name, palette, or typography. |
| `design-taste` | UI looks generic or AI-generated; wants a taste diagnosis. |
| `frontend-design-guidelines` | Wants tasteful frontend defaults or a UI review. |
| `number-formatting` | Token amounts, USD, gas, or big numbers render badly. |
| `page-load-animations` | Janky loading, layout shift, skeleton work. |
| `product-review` | Balanced UX review from a first-time user's POV. |
| `roast-my-product` | Brutal, investor-grade critique. Explicitly requested heat. |
| `validate-business-model` | Who pays, how much, why they keep paying. |
| `retention-loop` | Day 1 / 7 / 30 loop definition; stickiness. |
| `will-real-users-pay` | Pricing experiments before launch. |
| `security-review` | Threat model, STRIDE, OWASP, supply chain, pre-launch hardening. |

## Ship

| Skill | Route here when |
|---|---|
| `launch-checklist` | Ready to go live; run the gates (domain, analytics, error tracking, rollback, on-chain deploy checks). |
| `create-pitch-deck` | 10-slide deck for judges, investors, or a grant. |
| `marketing-video` | Plan a 30-60s distribution video. |
| `video-craft` | Polish an existing video draft (pacing, captions, audio). |
| `apply-grant` | Draft an ecosystem grant application. |

## Grow

| Skill | Route here when |
|---|---|
| `positioning` | One-sentence positioning; who it's for, what it beats, why now. |
| `landing-copy` | Landing page copy that converts; hero, proof, CTA. |
| `distribution-plan` | 30-day channel-by-channel plan to reach the first users. |
| `first-100-users` | Manual, unscalable tactics to get and keep the first 100. |

## Earn

| Skill | Route here when |
|---|---|
| `pricing` | Set the actual price and packaging (tiers, metering, anchors). |
| `revenue-review` | Money is (or isn't) coming in; diagnose and fix the revenue engine. |
| `get-paid` | Wire up payments: fiat rails, crypto rails, or agent-economy earnings. |

## Common wrong picks

| User says | Wrong pick | Right pick |
|---|---|---|
| "review my product" (wants heat) | `product-review` | `roast-my-product` |
| "review my product" (wants balance) | `roast-my-product` | `product-review` |
| "validate my idea" (has no idea yet) | `validate-idea` | `find-next-idea` |
| "plan my launch" (means marketing) | `launch-checklist` | `distribution-plan` |
| "plan my launch" (means go-live gates) | `distribution-plan` | `launch-checklist` |
| "pricing" (pre-launch, untested) | `pricing` | `will-real-users-pay` |
| "pricing" (validated, needs the number) | `will-real-users-pay` | `pricing` |
| "make it look better" (no brand yet) | `frontend-design-guidelines` | `brand-design` |
| "security" (means code review) | `product-review` | `security-review` |
| "build an agent" (one LLM call behind a button) | `build-ai-agent` | `build-mvp` |

## Phase entry points

- Nothing exists yet -> `find-next-idea`
- Idea exists, no code -> `validate-idea` then `clarify-intent`
- Code exists, no users -> `verify-against-intent` then `launch-checklist`
- Users exist, no revenue -> `retention-loop`, then Earn phase
- Revenue exists -> `revenue-review`
