# Acceler Atlas — Claude Code Plugins (Pre-Sales + Post-Sales)

This repo hosts **two** Claude Code plugins in one Git marketplace, together branded **Acceler Atlas**:
**`acceler-presales`** — displayed as **Acceler Atlas** (repo root — discovery through proposal/pricing) — and
**`acceler-post-sales`** — displayed as **Acceler Atlas · Delivery** (`post-sales/` subdirectory — delivery deck
through post-delivery recaps and the standalone Build Lab use-case picker). `displayName` is a UI label only —
the stable ids (`acceler-presales`, `acceler-post-sales`) used for install and command namespacing never change.
Install one or both from the same `git clone`/marketplace add; fork or update either from this single repo.
Uses your existing Claude Max / Pro / API subscription — no separate API key needed.

## What it does

When a client requirement lands, `acceler-presales` orchestrates the pre-sales pipeline:

```
Requirement (notes / RFP)
   ↓
/acceler-presales:discovery   →  score against 33 questions
   ↓
/acceler-presales:similar     →  find closest precedents in the Knowledge Graph
   ↓
/acceler-presales:proposal      →  draft program document (IK-Acceler house style)
   ↓
/acceler-presales:proposal-deck →  pre-sales PITCH deck (KASE HTML · client-facing)
   ↓
/acceler-presales:pricing     →  bottom-up cost stack (INR India · USD US strict)
   ↓
/acceler-presales:instructors →  rank SMEs from the indexed pool (773 profiles)
```

Or run the whole pre-sales pipeline at once: **`/acceler-presales:full-cycle`**.

Once the deal closes, **`acceler-post-sales`** (separate plugin, same repo) picks up delivery and wrap-up:

```
Deal closed → program delivery begins
   ↓
/acceler-post-sales:session-deck         →  live DELIVERY deck an instructor presents (per session day)
   ↓
/acceler-post-sales:pick-use-case        →  standalone "pick your use case" Build Lab chooser
   ↓
/acceler-post-sales:session-recap        →  post-delivery LEARNER recap (per session day)
/acceler-post-sales:session-recap-report →  post-delivery STAKEHOLDER report (per session day)
```

> Slash commands are namespaced by plugin: `/acceler-presales:<command>` or `/acceler-post-sales:<command>`.

## What's inside

### `acceler-presales` (repo root) — "Acceler Atlas"
- **Skills** (`skills/`) — every Acceler pre-sales skill markdown, auto-loaded by Claude Code when relevant
  - `discovery-checklist/` — the 33-question pre-sales discovery scorer (6 sections, 14 must-asks)
  - `doc-proposal/` — IK-Acceler proposal house style v2
  - `pricing/` — bottom-up cost stack, INR/USD strict, first-time vs repeat
  - `requirement-mapping/` — 3-col coverage matrix with gap flagging
  - `proposal-deck/` — pre-sales pitch deck on the KASE engine (data-driven `SLIDE_DATA` slides · ~80 templates · engine + e& reference bundled)
  - `pptx-deck/` — HTML→PPTX image-fidelity converter for the proposal deck (`build_pptx.py` bundled · per-slide 2× PNG → 16:9 PPTX)
  - `mini-ut-context/` — Utkarsh's operating profile + Mini-UT principles
- **Commands** (`commands/`) — slash commands for each pre-sales pipeline step
- **Knowledge** (`knowledge/`) — the Acceler Knowledge Graph
  - `graph.json` (2.6 MB) · `files.json` (761 KB) · `instr_candidates.json` (14 KB)
  - `INDEX.md` — one-page digest of all 61 clients with pricing bands
  - `USING_THE_KG.md` — how the agent should query the graph
- **Samples** (`samples/`) — reference proposals to lift patterns from

### `acceler-post-sales` (`post-sales/` subdirectory) — "Acceler Atlas · Delivery"
- **Skills** (`post-sales/skills/`)
  - `live-session-deck/` — post-sales live delivery deck (4-movement arc · 15 slide types)
  - `build-lab-picker/` — standalone "pick your use case" page for a Build Lab, pulled out of `live-session-deck`'s inline pick-cards slide (worked reference: `copilot-leadership-lab.vercel.app`)
  - `session-recap/` — post-delivery recap pair: learner-facing `dayN-learner-recap` + stakeholder-facing `dayN-recap-report`, including the data pointers to ask for upfront and both artifacts' design systems
- **Commands** (`post-sales/commands/`) — `session-deck`, `pick-use-case`, `session-recap`, `session-recap-report`

## Install (from the public GitHub marketplace)

This repo **is** the marketplace, and it's **public** — anyone can install directly from it. No invite, no
collaborator access, no local download required. (Collaborator/write access is only needed if you want to
*push changes* yourself — see [CONTRIBUTING.md](CONTRIBUTING.md).)

### Pre-reqs
1. **Claude Code** installed (`brew install claude`, or from claude.com/code) and signed in (`claude /login`).
2. **git can reach GitHub** — `gh auth login` (GitHub CLI), or have SSH keys already set up. Nothing else.

Runs the same way in any terminal Claude Code supports — Warp, iTerm, the system terminal, VS Code/JetBrains'
integrated terminal, or Claude Desktop's built-in Claude Code integration.

### A · First-time install (direct from GitHub — recommended)
```bash
claude
/plugin marketplace add voldemortuk/acceler-presales-plugin
/plugin install acceler-presales@acceler-local      # pre-sales — "Acceler Atlas"
/plugin install acceler-post-sales@acceler-local    # delivery / post-sales — "Acceler Atlas · Delivery" (install if you need it too)
```
Both plugins are listed in the same marketplace (`acceler-local`) from the one `marketplace add` — install just `acceler-presales` if you only work pre-sales, or both if you also deliver/run recaps.

### B · Prefer a local folder? You can install that way too
Cloning directly from GitHub (path A) is the recommended default, but if you'd rather work from a local copy —
for example if you're also pulling the source [1. PowerUp Drive folder](https://drive.google.com/drive/u/1/folders/1FQnoa5PbzM7JosfW8aEfRgiqYLWrXGbM)
for KG/skill work — you can point `/plugin install` at a local path instead:
```bash
claude
/plugin install "/path/to/your/local/acceler-presales-plugin"
```
Already installed via the *old* local-file method and want to switch to Git instead? Don't uninstall or remove
the old marketplace first (that auto-uninstalls the plugin) — re-add under the same name to silently swap the
source:
```bash
claude
/plugin marketplace add voldemortuk/acceler-presales-plugin   # replaces the local 'acceler-local' source
/plugin marketplace update acceler-local                        # pull latest from Git
```
The plugin stays installed as `acceler-presales@acceler-local`, now sourced from GitHub.
*(Optional, to force a clean re-download: `/plugin uninstall acceler-presales@acceler-local` then `/plugin install acceler-presales@acceler-local`.)*

### Verify
```bash
/plugin list                # should show acceler-presales and/or acceler-post-sales, each with its own version
/plugin marketplace list     # 'acceler-local' should point to voldemortuk/acceler-presales-plugin (not a local path)
```
Then type `/acceler-presales:` in a Claude Code session — the command menu should list `discovery`, `similar`,
`proposal`, `proposal-deck`, `pricing`, `instructors`, `coverage`, `full-cycle`, `setup`. Run
`/acceler-presales:setup` once to confirm the Knowledge Graph is reachable.

## Updating

Whenever a new version is published (either plugin):
```bash
/plugin marketplace update acceler-local
```
(Updates ship only when the maintainer bumps `version` in that plugin's own `.claude-plugin/plugin.json` — see [CONTRIBUTING.md](CONTRIBUTING.md). The two plugins version independently.)

## Use it (in any terminal)

Open Claude Code in your terminal (Warp / iTerm / VS Code / etc):
```bash
claude
```

Then run any of:

```
/acceler-presales:full-cycle      Drive the entire pre-sales pipeline end-to-end from meeting notes
/acceler-presales:discovery       Score a brief against the 33-question checklist
/acceler-presales:similar         Find the closest past Acceler precedents (KG-powered)
/acceler-presales:proposal        Draft the program document (IK-Acceler house style)
/acceler-presales:proposal-deck   Generate the pre-sales pitch deck (KASE HTML)
/acceler-presales:pricing         Compute the cost stack (INR India · USD US strict)
/acceler-presales:instructors     Rank SMEs from the indexed pool

/acceler-post-sales:session-deck         Generate the live delivery deck (HTML)
/acceler-post-sales:pick-use-case        Generate the standalone "pick your use case" Build Lab page
/acceler-post-sales:session-recap        Generate the post-delivery learner recap (HTML)
/acceler-post-sales:session-recap-report Generate the post-delivery stakeholder report (HTML)
```

Each command takes a brief / notes / topic as input. The orchestrator (`full-cycle`) chains them with appropriate handoffs and review gates.

## Refreshing the Knowledge Graph

The KG is built from the historical Drive folder. To refresh after new proposals land, then publish via this repo:

```bash
cd "/Users/voldemort/Downloads/1. PowerUp/APR - Pre-Sales Product/Knowledge Graph/_kg"
python3 build_corpus.py       # only when new files added
python3 build_graph.py         # classify + extract tools/topics/pricing
python3 merge_instructors.py   # merge + classify instructors
python3 build_html.py          # render graph.html
# copy the regenerated files into this repo's knowledge/ (the source of truth):
cp graph.json files.json instr_candidates.json INDEX.md USING_THE_KG.md \
   "$HOME/acceler-presales-plugin/knowledge/"
# then publish: bump version in .claude-plugin/plugin.json, commit & push
cd "$HOME/acceler-presales-plugin" && git add -A && git commit -m "KG refresh" && git push
```

## Architecture notes

- **No backend.** Everything runs in your Claude Code session. The plugin's skills + commands give Claude the instructions; the KG gives it precedent data.
- **No API key juggling.** Uses whatever Claude Code is authenticated against (Claude Max / Pro / API).
- **Top-tier models.** Opus 4.x via Claude Code by default — 200K-1M context, best instruction following.
- **Reusable.** Anyone with Claude Code installed can install this plugin and get the same pre-sales workflow.

## Companion tools

- **Acceler OS** (`acceler-os.html`) — the visual workspace that runs the same logic in a browser (Cowork artifact)
- **KT Deck** (`acceler-presales-kt.html`) — the handover document for someone taking over the role
- **Discovery Checklist** (`Acceler-PreSales-Discovery-Checklist.html`) — standalone 33-question scorer

---

*acceler-presales (Acceler Atlas) v0.6.1 · acceler-post-sales (Acceler Atlas · Delivery) v0.2.0 · Aug 2026 · Utkarsh Raj · Acceler / Interview Kickstart B2B · [CHANGELOG](CHANGELOG.md) · [CONTRIBUTING](CONTRIBUTING.md)*
