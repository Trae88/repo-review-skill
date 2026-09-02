---
name: repo-review
version: 1.0.0
description: >
  Analyze and review any GitHub repository URL the user shares. Produces a
  structured scorecard: repo name + one-line description, 3 UP (genuine
  strengths with evidence), 3 DOWN (real weaknesses or risks), GO/NO-GO
  verdict with one-sentence rationale, and an offer to help test, install,
  or practice with the repo for "my first big moment." Use this skill any
  time the user drops a GitHub URL and asks to review, analyze, check out,
  or evaluate it — or says "new repo," "repo review," or similar.
  Also maps the repo to FLF use cases when relevant.
---

# Repo Review Skill

## Output Format — always use exactly this structure

```
**Repo Name** — one-line description

**⬆️ 3 UP**
1. [specific strength with evidence]
2. [specific strength with evidence]
3. [specific strength with evidence]

**⬇️ 3 DOWN**
1. [real weakness or risk — honest, no sugarcoating]
2. [real weakness or risk]
3. [real weakness or risk]

**🟢 GO / 🔴 NO-GO / 🟡 CONDITIONAL GO** — one sentence why.

---

**🛠️ Want help with this for your first big moment?**
[Offer 2-3 specific actions: testing, installing, practicing, or wiring into FLF]
```

## Research process

Before scoring, gather:
- Stars, forks, contributor count (signal of community health)
- Commit recency and frequency (signal of active maintenance)
- README depth — does it explain setup, use cases, limitations honestly?
- Issue count open vs closed (high close rate = responsive maintainer)
- Dependency on proprietary backends, paywalls, or auth walls
- Whether it's a fork — check upstream if so, note what the fork adds (often nothing)

Use `browser_use` to read the repo README, commits page, and issues page.
Use `shell_execute` to fetch raw README via curl when the browser is slow.

## Scoring rubrics

**UP criteria (pick the three strongest):**
- Active commits (daily/weekly = strong; last commit > 6 months = flag)
- Multi-contributor with merged PRs (not a solo project)
- Real test coverage or CI/CD
- Production-grade safety engineering (auth, rate limiting, error handling)
- Genuine feature differentiation vs existing tools
- Cross-platform or cross-agent compatibility
- Directly installable into the current stack (Claude Code, iSH, Minis)
- Proven outputs (demo videos, live hosted version, real examples)

**DOWN criteria (flag the three most significant):**
- Fork with zero additions (always flag — point to upstream)
- Solo contributor / bus-factor risk
- Unsigned binaries or unresolved CVEs
- Hard dependency on proprietary service (auth wall, paid API required)
- Platform-locked (Windows-only, GPU-required, no iOS/mobile path)
- No license or restrictive license for commercial/nonprofit use
- Stale (no commits > 3 months on an active-seeming project)
- Known security issues in issue tracker

## FLF relevance mapping

After the scorecard, if any of the following apply, add a **FLF Relevance** section:

- Image/video generation → map to FLF Video Studio (flf_studio/)
- Agent skills / MCP → map to skill installation or FLF workflow hub
- API / backend tools → map to FLF API (flf_api/)
- Design / UI tools → map to Stitch + Lovable pipeline (flf_lovable/)
- Document / form tools → map to Document Studio or Workflow Hub
- Security / compliance tools → map to 501(c)(3) compliance workflows

Format:
```
**🗺️ FLF Relevance**
| What | How it fits | Priority |
|---|---|---|
| [feature] | [specific FLF use] | Now/Soon/Later |
```

## Installation offer

Always close with a concrete offer tied to the user's "first big moment" context.
Options to offer (pick 2-3 most relevant):
- Install and test locally on iSH right now
- Wire into FLF Studio, FLF API, or FLF-HUB as a new capability
- Run a live demo generation (image, video, narration)
- Clone and read the AGENT_GUIDE / SKILL.md files
- Map all features to FLF workflows and build a use-case plan

## Context to keep in mind

- User is running Minis on iOS (iSH/Alpine Linux aarch64) — no GPU, no Docker
- Active stack: FLF Video Studio (port 8742), FLF API (FastAPI), FLF-HUB (Lovable/React)
- API keys live: ElevenLabs (`ELEVEN`), Pexels (`PEXELS_API_KEY`), Pixabay (`PIXABAY_API_KEY`), MuAPI (`MUAPI_API_KEY`), GitHub (`NEW_MINI`)
- Windows-only or GPU-only tools are always a DOWN — note the constraint clearly
- Forks without additions are always flagged — GO verdict applies to upstream, not the fork
- FLF is a 501(c)(3) pending nonprofit — compliance and cost matter
