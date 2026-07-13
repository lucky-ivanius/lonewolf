# Profile questions

The fastest way to filter a 100-idea corpus is a 90-second profile pass. Pick from these questions; do not ask all of them.

## Mandatory (always ask if unclear)

1. What domain feels obvious to you (DeFi, consumer, infra, gaming, identity, mobile, AI tooling, agents)?
2. Is there a hard external deadline (hackathon cutoff, grant milestone)? If yes, when. If no, skip, do not invent one.
3. Are you solo or a team? If team, who has shipped what?
4. Have you shipped a product before? If yes, briefly, what happened to it?

## Optional (ask if helpful)

5. What is one product you wish existed?
6. What is one product you find frustrating or incomplete?
7. Where is your distribution (X following, Telegram community, Discord, newsletter, none)?
8. Are you targeting a specific platform or marketplace (an agent marketplace, an app store), or building independent?
9. What is the smallest version of success that would feel worth the time?

## What the answers gate

- **Domain** -> filter the corpus to that domain plus one adjacent.
- **Deadline** -> if a hard external one exists, only use it to flag research-grade unknowns (custom cryptography, training a new model, untested infrastructure) as risks. Do not use it to reject application-layer ideas, AI-assisted builds compress former multi-week scope into days.
- **Solo / team / experience** -> bias toward ideas where the agent can carry the unfamiliar parts. Only reject if the idea needs research-grade work the user must drive personally.
- **Prior shipping experience** -> bias toward ideas where the prior experience translates (a DeFi person should look at DeFi or fintech, not gaming).
- **Wished-for product** -> if it does not exist for a reason (regulatory, technical, market), name the reason. If it does not exist for no reason, that is often the best lead.
- **Distribution** -> ideas that need cold-start distribution should be deprioritized for builders without a community.
- **Platform alignment** -> if a marketplace or hackathon is the target, weight the score toward ideas that platform actually features and pays for.

## Anti-pattern questions

Avoid asking:

- "What is your passion?" (vague)
- "What is your big vision?" (LLM-prompt-shaped)
- "How do you want to change the world?" (over the top)

These produce generic answers. The questions above produce filterable signal.

## Output of the profile pass

A one-paragraph summary the skill writes to itself before reading the corpus:

```
profile: <domain>, <hard deadline or none>, <team shape>, <shipping experience>, <distribution status>
filter: <which corpus categories to include>
reject: <which corpus categories to exclude, e.g. research-grade work the user cannot drive>
```

Then, and only then, query the corpus.
