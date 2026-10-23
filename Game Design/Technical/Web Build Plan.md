---
tags:
  - gdd
  - technical
  - plan
---

# Web Build Plan

**Decision (2026-10-23):** Stormbound Online is built **entirely on the web, in TypeScript end-to-end**. That covers the game client, server, Build Lab, public database and the Tauri desktop wrapper.

**Why one language:** the [[Technical Architecture#Shared Rules Package|Shared Rules Package]] must run identically on the server, in the client, in the Build Lab and in the public database. Only a single-language stack does that without a hand-ported second copy of the formulas, which is the drift problem PKO's simulator has ([[PKO - Tech & Infrastructure]]).

**Rejected alternatives:**

| Option | Why not |
|---|---|
| Go server | Fine server language, but the rules can't be shared with the browser cleanly, so formulas would drift |
| Rust server + WASM rules | Strong option, but slower iteration and two languages anyway (the UI stays TS). Kept as an escape hatch: `packages/rules` can be rewritten in Rust and compiled to WASM later without touching anything else |

**Performance check:** 1,000 CCU with 8-10s commit rounds is at most about 100-200 round resolutions per second of integer math. That's trivial for Node. *This is an estimate from the round timer, not a benchmark.* The real limits are design iteration speed and Postgres transaction throughput.

---

## Stack

| Layer | Choice | Notes |
|---|---|---|
| Language | TypeScript (strict) | Shared across every package and app |
| Repo | pnpm workspaces monorepo (Turborepo optional) | Rules, protocol, content, apps versioned together |
| Game client | SvelteKit | Resolves the Svelte/Solid choice in [[Platform & Requirements#Frontend]] |
| Build Lab + public database | Same SvelteKit app, prerendered static routes | Generated from `packages/rules` + `packages/content` at build time ([[Creator Ecosystem#Build Lab & Public Database]]) |
| Game server | Node.js, `uWebSockets.js` (or `ws`) + Fastify for REST | One process, modular monolith |
| Message validation | zod schemas in `packages/protocol` | One definition, used by both client and server |
| Database | PostgreSQL | Marketplace SERIALIZABLE, append-only provenance ([[Technical Architecture#Constraints (Must Not Violate)]]) |
| Live state | Redis | Combat sessions, presence, matchmaking, rate limits |
| Auth | Argon2id + TOTP 2FA, server sessions | Per [[Account & Security System]] |
| Edge | Cloudflare | DDoS, CDN for static routes, WebSocket proxy |
| Hosting (MVP) | Fly.io or Railway (Hetzner VPS if cost-sensitive) | Docker images, so the host is portable |
| Desktop (post-MVP) | Tauri wrapping the same web bundle | Per [[Platform & Requirements#Desktop Wrapper]] |

### Numeric rule for TypeScript

JavaScript numbers are floats, so the [[Gem System Math#Integer Per-Mille Representation|per-mille integer rule]] needs enforcing in code:

- All rules-package values are integers. Use `Math.floor` (or a `pm()` helper) at layer boundaries only.
- Lint rule: no `/` outside helper functions in `packages/rules`.
- Golden tests assert exact integers, never approximate equality.
- Values stay well below `Number.MAX_SAFE_INTEGER`. Use `bigint` only for gold ledgers, if totals can ever exceed about 9×10¹⁵.

---

## Repo Layout

```text
stormbound/
├── packages/
│   ├── rules/        # formulas, coefficient tables, stack order, golden tests. Pure: no I/O, RNG seed injected
│   ├── protocol/     # zod schemas for every WS/REST message
│   └── content/      # gems, modifiers, origins, dungeons, bosses as data (JSON/YAML)
├── apps/
│   ├── server/       # modular monolith: combat/ economy/ identity/ hub/ matchmaking/ instances/
│   ├── web/          # SvelteKit: game client + Build Lab + public database
│   └── desktop/      # Tauri wrapper (post-MVP)
└── infra/            # Dockerfiles, SQL migrations, deploy config
```

Each server folder maps to a service boundary in [[Technical Architecture#Service Boundaries]], and the folders talk only through interfaces. Extracting one into a service later is a deploy change, not a rewrite.

---

## MVP Infrastructure Profile

[[Technical Architecture]] describes the **scale** target. At MVP (1,000 CCU) these simplifications apply:

| Scale-target component | MVP replacement | Reintroduce when |
|---|---|---|
| NATS message bus | In-process typed event emitter | The first module is extracted into a service |
| TimescaleDB | Postgres tables + nightly rollups | Behavioral telemetry writes degrade Postgres |
| AWS EKS | 2-3 containers on Fly.io/Railway | Any extraction trigger in [[Technical Architecture#Architectural Decision: Service-Oriented Monolith to Microservices]] |
| RDS / ElastiCache | The host's managed Postgres/Redis | Multi-AZ or regional deployment is required |

Rough MVP hosting cost: about $50-150/month. *Back-of-envelope, not priced.*

---

## Build Order

| Step | Deliverable | Needs backend? | Done when |
|---|---|---|---|
| 1 | `packages/rules` from [[Gem System Math]] + [[Stats System]] | No | Golden tests pass for the 3 example builds (Stormkinetic, Resonant Bastion, Chaosweaver) |
| 2 | Build Lab page | No | Paste a build code and get a stat sheet, power budget and breakpoint markers |
| 3 | Local combat loop vs bot | No | Draw → Commit → Clash → Aftermath playable in the browser using `packages/rules` |
| 4 | Authoritative server | Yes | Combat resolves server-side, client sends commits only, reconnection works ([[Combat System]]) |
| 5 | Persistence | Yes | Accounts, gem provenance (append-only), inventory, NPC marketplace with SERIALIZABLE transactions |
| 6 | Social layer | Yes | Hubs, chat, parties, guilds over the same WebSocket |
| 7 | PWA, then Tauri wrapper | — | Per [[Platform & Requirements]] |

**Steps 1-3 prove the core loop with zero infrastructure.** Don't start step 4 until step 3 is fun.

---

## Related Pages

- [[Technical Architecture]] — Scale-target architecture, constraints, services
- [[Platform & Requirements]] — Browser targets, PWA, desktop wrapper
- [[Gem System Math]] — The formulas `packages/rules` implements
- [[MVP Design]] — Scope and success metrics
