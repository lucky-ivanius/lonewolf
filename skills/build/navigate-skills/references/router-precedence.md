# Router precedence

When two or more skills could apply to a user's request, use these tie-break rules. They mirror what `skills/SKILL_ROUTER.md` codifies but condensed for in-the-moment decisions.

## Rule 1, the failing-build rule

If the user's last message contains a literal compile error, abort code, or stack trace, fix the build first. Do not route to feature-building or review skills until the build is green.

## Rule 2, the no-context rule

If neither `.lonewolf/idea-context.md` nor `.lonewolf/build-context.md` exist, and the user mentions "build", "scaffold", or "start", route to `clarify-intent` (then `scaffold-project`). Skills that need build context to function should not be the first stop.

## Rule 3, the launch-blockers rule

If the user wants to launch but the prerequisites are missing (no `security-review` output, no `validate-business-model`, no `retention-loop`), route to the missing prerequisite first. `launch-checklist` itself enforces this, but `navigate-skills` should preempt the friction.

## Rule 4, the integration rule

If the user names a specific load-bearing integration (payments, an LLM pipeline, an agent marketplace), prefer the skill that owns that decision (`get-paid`, `build-ai-agent`) over a generic build skill. Generic coding help can come from `build-mvp` after the integration decisions are made.

## Rule 5, the meta rule

If the user is asking about Launch Kit itself ("what can you do", "list skills", "which skill"), this is `navigate-skills`. Do not recursively re-invoke.

## Rule 6, the anti-slop rule

If the user shows symptoms of slop (vague claims about their product, no validation evidence, fuzzy retention story), but is asking about launch or marketing, route to the relevant anti-slop skill (`validate-business-model`, `retention-loop`, `roast-my-product`) before the launch or marketing skill.

## Rule 7, the explicit override rule

If the user explicitly names a skill ("run roast-my-product", "use pricing"), respect it. Do not second-guess unless the prerequisites are missing (then fall through to Rule 3).

## Tie-break order

When multiple rules apply, apply in the order above. Rule 1 wins if the build is broken, regardless of any other signal.

## What to do when no skill fits

If after applying these rules nothing in the catalog matches, say so plainly:

> No installed skill is a clean fit for this. You can file a skill request at <github issues URL>, or describe the goal differently and we can try again.

Do not pick the closest skill out of obligation. Slop in equals slop out.
