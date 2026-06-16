# github-roast-tpc

A Claude Code plugin that audits a GitHub profile through its author's intent. It reviews READMEs, flags AI-generated markers, reads recruiter signals, and suggests keywords for findability. Born from [The Product Crew (TPC) GitHub Roast live, ep. 2](https://www.youtube.com/watch?v=gfDBEUImB-A).

## Philosophy

Each repo is read against its objective, not against code quality in the abstract. Four objectives form the grid: build a flagship, raise funds, get hired, run a business. Few stars does not mean low value.

Technical principle: orchestrate, do not rewrite. Existing building blocks (organic score and geo on the StarMapper side, anti-AI detection on the personal skills side) are called, not duplicated.

## Status

| Phase | Content | Status |
|-------|---------|--------|
| 0 | Plugin scaffold | done |
| 1 | eval-readme (skill + readme-critic agent) | done |
| 2 | analyze-github-profile (objective, pins, timeline, commits) | done |
| 3 | score-profile (/5 grade + table + fixes) | done |
| 4a | suggest-keywords (findability, SEO, cross-linking) | done |
| 4b | analyze-linkedin-profile (multimodal input) | done |
| 5 | route-next-steps (TPC offers / Reddit post draft) | done |
| 6 | community packaging | upcoming |

## Components

- `commands/` : six slash commands (`gh-readme`, `gh-profile`, `gh-score`, `gh-keywords`, `gh-linkedin`, `gh-next`)
- `skills/` : six skills holding the procedures and grids
- `agents/` : four agents (`readme-critic`, `github-profile-analyst`, `keyword-strategist`, `linkedin-analyst`)
- `signals.md` : shared signal grid, the single source of truth

## Usage

Review a single README:

```
/gh-readme owner/repo
```

The agent fetches the README via the GitHub API, applies the `eval-readme` grid, and returns a human/AI verdict, strengths, and ready-to-apply fixes.

Analyze a full profile:

```
/gh-profile username
```

The agent fetches repos, pinned items, timeline and commits, detects the author's main objective, and returns a profile sheet with signals by category and points of attention.

Score a profile out of 5 with fixes:

```
/gh-score username
```

Chains profile analysis, flagship README review, then a /5 grade weighted by the detected objective, a signal table, and fixes sorted by return on effort.

Optimize findability:

```
/gh-keywords username
```

Suggests keyword-rich descriptions and topics per repo, an optimized bio, and the missing cross-platform links. Everything is copy-paste ready into GitHub.

Cross-check your LinkedIn against your GitHub:

```
/gh-linkedin username
```

You provide your own profile (screenshots or pasted text), the agent checks the alignment with your GitHub and returns fixes. No scraping, no storage.

Plan next steps:

```
/gh-next username
```

Branches on your objective. Job track: a fix checklist then a bridge to TPC offers. Outreach track: a Reddit or LinkedIn post draft ready to publish.

## Configuration

The TPC job offers link (used by `/gh-next`, job track) is not hardcoded. Set it here or provide it at runtime:

```
TPC_OFFERS_URL = (to fill in)
```
