# Stellar Scout

An AI skill for scouting the [Stellar](https://stellar.org) ecosystem before you build. Validate ideas, surface prior art across Stellar hackathons + SCF rounds + the curated project directory, recommend SDK tracks, and find Stellar builders.

Installable into any AI agent that loads `SKILL.md` files — Claude Code, Codex, Cursor, OpenClaw, and others.

## Install

```bash
npx skills add Stellar-Light/stellar-scout
```

For Codex:

```bash
npx skills add Stellar-Light/stellar-scout -a codex
```

For OpenClaw:

```bash
npx skills add Stellar-Light/stellar-scout -a openclaw
```

Powered by the [`skills`](https://github.com/vercel-labs/skills) CLI.

## What it does

Two modes:

- **Conversational** — fast, cited answers backed by public stellarlight.xyz endpoints.
- **Deep Dive** — triggered by *"vet this idea"* / *"should I build X"*. Runs an 8-step research workflow: prior-art search → gap classification → competitor list → SDK recommendation → teammate candidates → funding signal → suggested next steps.

Gap classification: **Full gap** / **Partial gap** / **False gap**. Refuses to speculate when the data doesn't support a claim.

## Topic clusters

Aligned to [skills.stellar.org](https://skills.stellar.org)'s taxonomy:

- Soroban smart contracts
- Anchors & off-ramps
- Agentic payments (x402, MPP)
- Asset issuance (SAC, stablecoins, RWA)
- Wallets & dapps
- ZK proofs
- SEP standards
- Data infrastructure

## How it works

The skill teaches your AI agent how to query public read-only APIs on [stellarlight.xyz](https://stellarlight.xyz) — no auth, edge-cached. Endpoints:

| Endpoint | Purpose |
| --- | --- |
| `/api/hackathons` | Curated hackathons + live DoraHacks events |
| `/api/hackathons/{slug}` | Submissions, winners, tracks, outcome funnel |
| `/api/builders` | Stellar Passport builder directory |
| `/api/projects/search` | Prior-art / competitor lookup |
| `/api/skills` | Catalog of SDF skills from skills.stellar.org |
| `/api/skills/{name}` | Full content of one SDF skill |
| `/api/leaderboard` | Ecosystem developer activity |
| `/api/status` | Self-check + data freshness |

## Companion skills

For *how to build* (Rust on Soroban, dapp frontends, SEP standards, etc.), install the official [skills.stellar.org](https://skills.stellar.org) skills as well. Scout (this skill) covers strategy and prior-art; the SDF skills cover execution. They compose.

## Source of truth

This `SKILL.md` is mirrored from [`stellarlight/public/skills/stellar-scout.md`](https://github.com/alexanderkoh/stellarlight/blob/dev/public/skills/stellar-scout.md). The skill content lives next to the APIs it documents.

## License

MIT
