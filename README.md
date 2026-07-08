# Stellar Scout

> Know what's been built. Find your gap.

An AI skill that turns your coding agent into a Stellar ecosystem analyst — for builders entering hackathons, applying for SCF grants, or shipping independently.

Installable into any agent that loads `SKILL.md` files: Claude Code, Codex, Cursor, OpenClaw, Amp, Antigravity, Cline, and 50+ others via the [`skills`](https://github.com/vercel-labs/skills) CLI.

## Install

```bash
# Claude Code (default)
npx skills add Stellar-Light/stellar-scout

# Codex
npx skills add Stellar-Light/stellar-scout -a codex

# OpenClaw
npx skills add Stellar-Light/stellar-scout -a openclaw
```

Or paste [the raw `SKILL.md`](https://stellarlight.xyz/skills/stellar-scout.md) into your agent's skill directory.

## What you ask it

**Pre-hackathon idea vetting**

> *"I want to build a Stellar wallet for college students with instant USDC off-ramps in Latin America. Should I?"*

Scout pulls existing wallet projects, checks SCF funding patterns, surfaces past hackathon winners in adjacent space, classifies the gap (full / partial / false), and tells you honestly whether to differentiate or pick a different lane.

**SCF grant prep**

> *"Is there an open Stellar RFP that matches my idea — a real-time price API for Soroban tokens?"*

Scout queries the live RFP feed, finds the matching brief, surfaces author + tech requirements + funding terms, and warns when an RFP is closed for the current quarter.

**Security-conscious design**

> *"I'm designing a Soroban lending market. What audit findings should I worry about?"*

Scout searches a corpus of every published Soroban audit (Certora, OtterSec, Halborn, OpenZeppelin, Code4rena, Veridise, Cantina, Runtime Verification, etc.) and surfaces real findings with auditor + severity + protocol inline — *"per the Certora audit of Blend Protocol V2 (HIGH severity), the oracle can be manipulated when …"*

**Ecosystem questions**

> *"How has Stellar's developer count changed since 2019?"*

Scout pulls from the Electric Capital Developer Reports (2019–2023) for hard macro data, not vibes.

## How it works

Scout teaches your AI agent how to query 14 public read-only APIs on [stellarlight.xyz](https://stellarlight.xyz) — no auth, edge-cached, rate-limited.

| Endpoint | Purpose |
| --- | --- |
| `/api/research` | Vector search over a ~4,500-chunk research corpus (Voyage AI `voyage-3` + Atlas Vector Search). Sources: SDF blog, SCF Handbook, SEPs, dev docs, foundational papers, lumenloop community playbooks, Soroban audits, Electric Capital Developer Reports. |
| `/api/hackathons` & `/api/hackathons/{slug}` | Curated hackathons + live DoraHacks feed. Empty-state surfaces fallback channels (BuildOnStellar / stellarlight / DoraHacks org). |
| `/api/hackathons/compare` | Side-by-side compare of 2–5 hackathons with delta notes. |
| `/api/analyze` | Cross-event analytics: prize totals, dev funnel, top categories, SCF distribution. |
| `/api/clusters` | Topic clusters across the projects directory with crowdedness scores (log-scaled, 1–10). |
| `/api/projects/search` | Prior-art + competitor lookup with tiered match-mode (strict → loose → majority). |
| `/api/rfps` | Open + closed RFPs with quarter awareness. |
| `/api/builders` | Builder directory (sourced from Stellar Passport). |
| `/api/skills` & `/api/skills/{name}` | Catalog of [skills.stellar.org](https://skills.stellar.org)'s 7 official skills. |
| `/api/leaderboard` | Ecosystem dev activity, peer L1 comparison. |
| `/api/feedback` | Submit feedback when the skill gets something wrong. Closes the loop. |
| `/api/status` | Health check + endpoint enumeration. |

## Composable with skills.stellar.org

This skill covers **strategy**: *what's already been built, what should you build, who should you talk to*.

For **execution** (writing Rust on Soroban, building a dApp frontend, integrating SEP standards), install the official [skills.stellar.org](https://skills.stellar.org) skills alongside this one. The skills compose:

- `stellar-scout` ← what to build, what's been built, where's the gap, what's open RFP
- `skills.stellar.org/soroban` ← how to write the contract
- `skills.stellar.org/dapp` ← how to wire the frontend
- `skills.stellar.org/anchors` ← how to integrate with Stellar anchors

## Sources

Scout grounds answers in primary sources, with inline citations:

- **Stellar Development Foundation** — blog posts, SCF Handbook, developers.stellar.org
- **SEPs** — all 57 protocol standards from [stellar/stellar-protocol](https://github.com/stellar/stellar-protocol)
- **Foundational papers** — Mazières et al. on the Stellar Consensus Protocol
- **[lumenloop](https://lumenloop.com)** community — SCF playbooks + ecosystem research
- **[sorobansecurity.com](https://sorobansecurity.com)** — every published Soroban audit (13 firms, 56 reports)
- **[Electric Capital Developer Reports](https://www.developerreport.com)** — annual + geographic ecosystem analyses

## Updates

Run periodically to pull the latest version:

```bash
npx skills update stellar-scout
```

The corpus auto-refreshes daily at 06:00 UTC. New audit reports, RFPs, and ecosystem posts appear in `/api/research` within a day of publication.

## Issues, tracking & contributing

**This repo is the official public tracker for the Stellar Scout data layer** — the live API, this skill, and the MCP server. If Scout returns something wrong, stale, or missing:

- **Open an issue here** → [Stellar-Light/stellar-scout/issues](https://github.com/Stellar-Light/stellar-scout/issues). Include the exact request URL + what you expected; a reproducible `curl` makes fixes same-day.
- Agents can also file directly via `POST https://stellarlight.xyz/api/feedback`.
- Suggest a missing project/source via [stellarlight.xyz/submit](https://stellarlight.xyz/submit).

Fixes are announced on the machine-readable changelog — `GET https://stellarlight.xyz/api/changelog` — so automated consumers can detect resolved findings without polling issues.

**The whole surface, for reference:**

| Surface | Where |
|---|---|
| Live API | `https://stellarlight.xyz/api/*` |
| OpenAPI 3.1 spec | `https://stellarlight.xyz/api/openapi.json` |
| Changelog (machine-readable) | `https://stellarlight.xyz/api/changelog` |
| Health / freshness | `https://stellarlight.xyz/api/status` |
| TS client | [`@stellar-light/api-client`](https://www.npmjs.com/package/@stellar-light/api-client) |
| MCP server | [`@stellar-light/scout-mcp`](https://www.npmjs.com/package/@stellar-light/scout-mcp) · [repo](https://github.com/Stellar-Light/scout-mcp) |

The application codebase is being migrated into this org; until that lands, this repo is the tracking home and the live API + spec + changelog are the source of truth for behavior.

## Source of truth

This `SKILL.md` mirrors [`stellarlight/public/skills/stellar-scout.md`](https://stellarlight.xyz/skills/stellar-scout.md). The skill content lives next to the APIs it documents.

## License

MIT. Built as a public good for Stellar builders by [stellarlight.xyz](https://stellarlight.xyz).
