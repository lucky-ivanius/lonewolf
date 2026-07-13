---
name: brand-design
description: Pick a brand name, color palette, or typography for a product. Use when the user wants to name or brand a project.
---


## What this skill does

Helps the user pick three brand decisions: name, color palette, typography. The output is concrete enough to drop into Tailwind config and a logo brief, not a 30-page brand bible. It actively pushes back on the generic crypto look (purple gradients, neon glow, generic sans-serif) and helps the user develop a brand that does not look AI-generated.

## When to use it

- The product has no name, or a placeholder name the user wants to replace.
- The frontend is being scaffolded and the team needs colors and a typeface.
- An existing brand looks generic and the user wants a critique.

## When NOT to use it

- The user already has a brand they are happy with.
- The user wants a marketing video or pitch deck, route to those skills.
- The user is mid-build on core product logic, finish that first.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.

## Inputs

- The product's positioning in one sentence (what it does, for whom).
- A target tone (serious / playful / technical / consumer).
- Optional: existing logo, existing color usage, existing typography choices.

## Outputs

A brand spec written to `.lonewolf/brand.md`:

```markdown
## brand-design session, <timestamp>
- name: <chosen name>, rationale: <one sentence>
- color palette:
 - primary: <hex>
 - secondary: <hex>
 - background: <hex>
 - text: <hex>
 - accent / warning / success: <hex each>
- typography: <heading family>, <body family>, <mono if used>
- logo brief: <one paragraph for a designer>
- tone references: <2-3 brand examples to follow>
- anti-references: <2-3 brand examples to NOT look like>
```

## Workflow

1. **Pull positioning**
 - Read `.lonewolf/idea-context.md` if present. If not, ask one sentence on what the product does and for whom.

2. **Name**
 - Generate 5 candidates, evaluate each on: pronounceable in 2 seconds,.com or.xyz available (best-effort), not already a project (search the catalog), not a banned word.
 - Pick the top 1, present the user with all 5 to confirm.

3. **Color palette**
 - Pick a primary that is NOT purple, NOT blue, NOT neon green by default. Generic crypto colors signal generic crypto product.
 - Use a 60-30-10 rule: 60% background, 30% surface, 10% accent.
 - Run the palette through a contrast checker (mentally or via reference). All text must hit 4.5:1 against its background.

4. **Typography**
 - Headings: a serif or a distinctive sans (NOT default Inter, NOT default system-ui without intent).
 - Body: a high-readability sans at 16px or larger.
 - Mono: only if the product surfaces code or addresses to users; otherwise skip.

5. **Anti-generic check**
 - Compare the chosen palette and type to `references/anti-generic-checklist.md`. If three or more "looks generic" markers fire, redo.

6. **Writeback**
 - Write `.lonewolf/brand.md`.
 - Note the brand spec in `.lonewolf/build-context.md` so frontend skills pick it up.

## Quality gate (anti-slop)

Before reporting done:

- Does the name pass the "say it on a podcast" test (clear pronunciation, no spelling-out)?
- Does the palette avoid the default crypto-purple-gradient look?
- Does the type pairing have actual contrast in voice (not two near-identical sans-serifs)?
- Does the brand spec name 2-3 anti-references the brand should NOT look like?
- Did the user confirm, or did the skill assume?

If any answer is no, the skill iterates.

## References

On-demand references (load when relevant to the user's question):

- `references/anti-generic-checklist.md`: Markers that signal a generic crypto brand.
- `references/palette-recipes.md`: Starting palettes by tone (serious, playful, technical, consumer).

## Use in your agent

- Claude Code: `claude "/launchkit:brand-design <your message>"`
- Codex: `codex "/brand-design <your message>"`
- Grok Build: run `grok`, then `/brand-design <your message>` in the session
- Cursor: paste a chat message that includes a phrase like "pick brand colors", or load `~/.cursor/rules/brand-design.mdc` and reference it.

If you activated this and the user actually wants something else, consult `skills/SKILL_ROUTER.md` and hand off.
