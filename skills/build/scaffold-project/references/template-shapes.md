# Project templates

Baseline shapes. Mix and match per project intent.

## API-only service

```
my-project/
├── package.json
├── src/
│ ├── index.ts
│ └── routes/
├── tests/
├── .lonewolf/
│ └── build-context.md
├── .gitignore
└── README.md
```

Use when the product is an API, a bot, or a worker with no user-facing frontend yet.

## Frontend app

```
my-project/
├── web/
│ ├── package.json
│ ├── next.config.js
│ ├── app/
│ │ ├── page.tsx
│ │ └── layout.tsx
│ ├── components/
│ ├── lib/
│ │ └── api.ts
│ ├── public/
│ └── tsconfig.json
├── .lonewolf/
│ └── build-context.md
├── .gitignore
├── pnpm-workspace.yaml
└── README.md
```

Use for a typical user-facing app. `web/lib/api.ts` holds the typed client for whatever backend or chain the app talks to.

## Full-stack with backend

```
my-project/
├── web/
├── server/
│ ├── package.json
│ ├── src/
│ │ ├── index.ts
│ │ └── routes/
│ └── tsconfig.json
├── .lonewolf/
├── pnpm-workspace.yaml
└── README.md
```

Use when there is a backend (payments, webhooks, indexing, anything holding secrets). The backend holds all private keys and API secrets. Treat `server/.env` as security-critical.

## Agent service

```
my-project/
├── agent/
│ ├── package.json
│ ├── src/
│ │ ├── index.ts      # runtime loop
│ │ ├── tools/        # tool definitions, least-privilege
│ │ └── prompts/
│ ├── evals/
│ │ └── golden-tasks.json
│ └── tsconfig.json
├── .lonewolf/
└── README.md
```

Use when the product is an agent. The `evals/` directory is not optional; a prompt change without a golden-task replay is a regression waiting to ship.

## Contracts add-on (crypto products)

```
├── contracts/
│ └── my_pkg/
│ ├── <manifest>       # Move.toml, foundry.toml, Anchor.toml — per chain
│ ├── sources/ or src/
│ └── tests/
```

Add to any shape above when on-chain logic is load-bearing. Pin the framework dependency to a specific rev or tag. Wire to the app through one typed client module (`web/lib/chain.ts`), never scattered RPC calls.

## Integration add-ons

Only when declared load-bearing at scaffold time:

- **LLM**: install the vendor SDK, add `server/src/llm.ts` with one wrapper (model name, retries, cost logging in one place).
- **Payments**: provider SDK in `server/`, webhook route with signature verification from day one.
- **Storage**: one `lib/storage.ts` helper; bucket or endpoint config in env, never inline.

## Workspace setup

Multi-package layouts use `pnpm-workspace.yaml`:

```yaml
packages:
 - "web"
 - "server"
 - "agent"
```

A root `package.json` with shared scripts:

```json
{
 "name": "my-project",
 "private": true,
 "scripts": {
 "build": "pnpm -r build",
 "dev": "pnpm -r --parallel dev",
 "test": "pnpm -r test"
 }
}
```

## .gitignore essentials

```
node_modules/
dist/
.next/
.lonewolf/.update-check
.env*.local
build/
*.log
```

`.lonewolf/build-context.md` is committed; it is the project's memory.
