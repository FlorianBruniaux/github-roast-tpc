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
| 3 | score-profile (/100 grade + table + fixes + score card) | done |
| 4a | suggest-keywords (findability, SEO, cross-linking) | done |
| 4b | analyze-linkedin-profile (multimodal input) | done |
| 5 | route-next-steps (TPC offers / Reddit post draft) | done |
| 6 | community packaging | upcoming |

## Components

- `commands/` : six slash commands (`gh-readme`, `gh-profile`, `gh-score`, `gh-keywords`, `gh-linkedin`, `gh-next`)
- `skills/` : six skills holding the procedures and grids
- `agents/` : four agents (`readme-critic`, `github-profile-analyst`, `keyword-strategist`, `linkedin-analyst`)
- `signals.md` : shared signal grid, the single source of truth

## Requirements

- Claude Code (the plugin runs inside it, there is nothing to deploy or host).
- The GitHub CLI `gh`, authenticated (`gh auth login`). It is the core data source: profile, repos, pinned items, contribution timeline, commits, issues and PRs.
- Optional: a StarMapper instance for organic score and stargazer geography. The skills call its public HTTP API when available and fall back to raw `gh` signals otherwise. Nothing to run locally.

## Install

This repo is a single-plugin Claude Code marketplace. From inside Claude Code:

```
/plugin marketplace add FlorianBruniaux/github-roast-tpc
/plugin install github-roast-tpc@github-roast-tpc
```

The first command registers the marketplace (it reads `.claude-plugin/marketplace.json`), the second installs the plugin. Restart Claude Code if the slash commands do not show up immediately.

Local development, to test changes before pushing:

```
/plugin marketplace add /absolute/path/to/github-roast-tpc
/plugin install github-roast-tpc@github-roast-tpc
```

Verify it loaded with `/help`: the `gh-readme`, `gh-profile`, `gh-score`, `gh-keywords`, `gh-linkedin` and `gh-next` commands should be listed.

## Using the skills

Two ways to trigger the plugin once installed.

Slash commands drive a specific stage on demand, for example `/gh-profile USERNAME` or `/gh-readme owner/repo` (full list below). Each command loads its skill and the matching agent.

Skills also fire automatically from natural language. Asking "audit my github" or "is this README AI-generated?" loads the relevant skill without typing a command. For a full end-to-end run, use the one-shot meta-prompt in the section further down.

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

Score a profile out of 100 with fixes and a shareable card:

```
/gh-score username
```

Chains profile analysis, flagship README review, then a /100 grade weighted by the detected objective, a signal table, fixes sorted by return on effort, and a score card ready to screenshot or download as PNG.

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

## Full audit (one prompt)

Run the whole pipeline in a single shot. Paste this into a Claude Code session that has the plugin loaded, replacing `USERNAME` and `OWNER/REPO`:

```
Run a complete github-roast-tpc audit for USERNAME (flagship: OWNER/REPO).

Go through every stage in order and produce one consolidated report:
1. analyze-github-profile: detect the main objective, then run the interactive
   intake: reflect your hypothesis back to me and ask up to 3 targeted questions.
   Wait for my answer before continuing.
2. eval-readme on the flagship: human vs AI verdict, fixes.
3. score-profile: /100 grade weighted by the confirmed objective, signal table, and score card generated locally.
4. suggest-keywords: keyword-rich descriptions, topics, bio, llms.txt / SEO gaps.
5. route-next-steps: next actions for the confirmed objective.

Use the gh CLI as the data source. Cite the gh command behind every number.
Stop at stage 1 for my confirmation, then run 2 to 5 without further questions.
```

The intake pause in stage 1 is intentional: the objective drives the weighting of every later stage. Everything after the confirmation runs end to end.

## Setup

Once the plugin is installed, three things to configure. Only the first is mandatory.

1. GitHub CLI. Authenticate `gh` so the skills can read profiles, repos, timelines, commits, issues and PRs:

```
gh auth login
gh auth status   # confirm you are logged in
```

A token with the default scopes is enough for public data. Without auth, the GitHub API caps at 60 requests/hour and most calls fail.

2. TPC job offers link, used by `/gh-next` on the job track. It is not hardcoded. Provide it at runtime when the command asks, or set it here once:

```
TPC_OFFERS_URL = (to fill in)
```

3. StarMapper integration (optional). It powers the organic score and stargazer geography. No environment variable and no local server are needed: the skills call the public StarMapper HTTP API (`https://starmapper.bruniaux.com/api/mcp/...`) when a repo has been scanned, and fall back to raw `gh` signals (forks, watchers, releases) otherwise. To use a self-hosted StarMapper, give its base URL to the skill when prompted. Reminder: the organic score is gated at 500 stars, below which it returns `insufficient`.
