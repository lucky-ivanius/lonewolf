# Common slice shapes

A "slice" is a unit of work that fits in a single reviewable commit, defined by a clear "done" check, not by a clock. With AI-assisted pacing, a slice may finish in minutes or stretch longer, what matters is that it has one observable outcome and a passing build at the end.

## Backend-only slice

- Implement one endpoint, job, or model (or extend an existing one).
- Write the test alongside it, including the unauthorized-caller case if the surface is permission-gated.
- Run the build and the test suite.

Example slice intent: "implement the deposit and withdraw endpoints on the vault service."

Done means: the code compiles, tests pass, and a caller can hit deposit and withdraw end to end.

## Frontend + connect slice

- One page or component wired to real data (no mocked responses left behind).
- Loading, empty, and error states included — they are part of the slice, not a follow-up.
- Verified in the browser at mobile width.

Example slice intent: "portfolio page renders live positions with skeletons."

## Contract slice (crypto products)

- Author or extend one on-chain module or contract.
- Unit tests including an expected-failure test for the unauthorized caller.
- Deploy to a local node or testnet and record the address in `.lonewolf/deploy-context.md`.

## Integration slice

- One third-party integration (an LLM call, a storage layer, a payment provider, a data API), made load-bearing.
- The integration result must change what the user sees or gets; imported-but-unused fails the slice.
- Include the failure path: what does the user see when the third party is down?

Example slice intent: "add file storage for the user's avatar upload."

## End-to-end demo slice

- Walk the core flow start to finish as a new user and fix everything that breaks the walk.
- Output: a recorded or reproducible demo path, plus the fix list that made it work.

## Pre-ship slice

- Run `security-review`, fix every P0.
- Pin dependencies to release versions.
- Run `verify-against-intent` and resolve or accept each drift item.

## Anti-pattern slices

- "Set up the project structure." That is `scaffold-project`, not a slice.
- "Add three integrations in one commit." Each is its own slice, mainly so the load-bearing test for each is reviewable.
- "Polish everything." Unscoped. Name the surface and the specific fix.
