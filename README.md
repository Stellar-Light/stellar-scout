# Stellar Scout

An AI skill for scouting the [Stellar](https://stellar.org) ecosystem before you build. Validate ideas, surface prior art across Stellar hackathons + SCF rounds + the curated project directory, recommend SDK tracks, find Stellar builders, and ground answers in primary sources (SEPs, papers, audit reports, the SCF Handbook).

Installable into any AI agent that loads `SKILL.md` files — Claude Code, Codex, Cursor, OpenClaw, and 50+ others via the [`skills`](https://github.com/vercel-labs/skills) CLI.

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
- **Deep Dive** — triggered by *"vet this idea"* / *"should I build X"*. Runs an 8-step research workflow: prior-art search → gap classification + crowdedness score → competitor list → SDK recommendation → teammate candidates → funding signal → suggested next steps.

Gap classification: **Full gap** / **Partial gap** / **False gap**. Refuses to speculate when the data doesn't support a claim (evidence floor).

The skill routes its answers based on user type — hackathon entrant, SCF grant applicant, or independent builder/team. Each gets a different lead endpoint and example flow.

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

The skill teaches your AI agent how to query public read-only APIs on [stellarlight.xyz](https://stellarlight.xyz) — no auth, edge-cached, rate-limited.

| Endpoint | Purpose |
| --- | --- |
| `/api/hackathons` | Curated hackathons + live DoraHacks events. Auto-includes `fallbackChannels` (BuildOnStellar / stellarlight / DoraHacks) when status-scoped queries return 0. |
| `/api/hackathons/{slug}` | Submissions, winners, tracks, outcome funnel. Dual-shape: curated (full detail) or DoraHacks-only (metadata + prize pool). |
| `/api/builders` | Stellar Passport builder directory (currently empty pending sync — treat as future capability). |
| `/api/projects/search` | Prior-art / competitor lookup. Tiered matching: strict AND → loose-1 → majority fallback, surfaced via `.meta.matchMode`. |
| `/api/rfps` | Open + closed RFPs (sponsor briefs that get SCF-funded). Quarter-aware with `activeQuarter` and `counts.open`. |
| `/api/research` | Semantic search over a ~4,500-chunk corpus (Voyage AI `voyage-3` + MongoDB Atlas Vector Search). Sources: SDF blog, SCF Handbook, SEPs, dev-docs, foundational papers, lumenloop playbooks + research, Soroban protocol audits (Certora / OtterSec / Halborn / OpenZeppelin / Code4rena via sorobansecurity.com), Electric Capital Developer Reports. |
| `/api/skills` | Catalog of SDF skills from skills.stellar.org. |
| `/api/skills/{name}` | Full content of one SDF skill. |
| `/api/leaderboard` | Ecosystem developer activity (active devs, commits, peer L1 comparison). |
| `/api/status` | Self-check + data freshness per source. |

## What's grounded in primary sources

When the user asks a *thesis / design-tradeoff / security* question, the skill calls `/api/research` and cites primary sources inline:

- *"What audit findings have been reported for Blend's oracle?"* → surfaces real Certora / OtterSec / Code4rena findings with auditor, protocol, and severity metadata
- *"How does SCP federated consensus actually work?"* → cites Mazières et al.
- *"What does the SCF awards committee look for?"* → cites the SCF Handbook + lumenloop voter playbooks
- *"How has Stellar's developer count changed over years?"* → cites Electric Capital Developer Reports

## Companion skills

For *how to build* (Rust on Soroban, dapp frontends, SEP standards, etc.), install the official [skills.stellar.org](https://skills.stellar.org) skills as well. Scout (this skill) covers strategy and prior-art; the SDF skills cover execution. They compose.

## Source of truth

This `SKILL.md` mirrors [`stellarlight/public/skills/stellar-scout.md`](https://github.com/theboycoder/stellarlight/blob/main/public/skills/stellar-scout.md) and is auto-synced from there. The skill content lives next to the APIs it documents.

## License

MIT
