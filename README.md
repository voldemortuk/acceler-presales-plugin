# Acceler Atlas — Claude Code Plugins (Pre-Sales + Post-Sales)

This repo hosts **two** Claude Code plugins in one Git marketplace, together branded **Acceler Atlas**:
**`acceler-presales`** — displayed as **Acceler Atlas** (repo root — discovery through proposal/pricing) — and
**`acceler-post-sales`** — displayed as **Acceler Atlas · Delivery** (`post-sales/` subdirectory: the full delivery
pipeline for a closed deal, from discovery facts and the lesson plan through decks, demos, quizzes, assignments and
post-delivery recaps, with a reviewer agent on every stage). `displayName` is a UI label only —
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

Once the deal closes, **`acceler-post-sales`** (separate plugin, same repo) takes the engagement from discovery
to delivery and wrap-up:

```
Deal closed
   ↓
/acceler-post-sales:discovery-checklist          →  score the 33 questions again, save the Discovery Facts Sheet
/acceler-post-sales:onboarding-form-generation   →  Learner Onboarding Form (lead-only section inline when needed)
/acceler-post-sales:deep-research                →  one research brief from everything already known (no web research)
/acceler-post-sales:lesson-plan-generation       →  internal minute-level Lesson Plan, one tab per day
/acceler-post-sales:instructor-finalization      →  day-by-day instructor roster, matched to the Lesson Plan
   ↓
/acceler-post-sales:mcq-generation               →  one MCQ set: pre-test, post-test, or a day's in-session quiz
/acceler-post-sales:demo-generation              →  in-class hands-on build: notebook, code file, or no-code build guide
   ↓
/acceler-post-sales:slide-content-planning       →  slide-by-slide content plan for one day, before the deck is built
/acceler-post-sales:session-deck                 →  live DELIVERY deck (HTML), rendered from that plan on the e& template
   ↓
/acceler-post-sales:orientation-generation       →  program Orientation deck (PPTX, edited from a real past deck)
/acceler-post-sales:closing-ceremony-generation  →  program Closing Ceremony deck (PPTX, edited from a real past deck)
   ↓
/acceler-post-sales:assignment-generation        →  one rubric-graded assignment for a session
/acceler-post-sales:project-generation           →  one capstone / multi-milestone project
/acceler-post-sales:hands-on-guide-generation    →  tool-access / setup guide covering every tool in the lesson plan
   ↓
/acceler-post-sales:session-recap                →  post-delivery LEARNER recap (per session day)
/acceler-post-sales:session-impact-report        →  post-delivery STAKEHOLDER impact report (per session day)

Alongside the pipeline:
/acceler-post-sales:dry-run-feedback             →  capture dry-run notes, route content findings through the fix loop
/acceler-post-sales:pick-use-case                →  standalone "pick your use case" Build Lab chooser
```

An engagement only runs the stages it needs. Each generation command also works on its own, in one of three
ways: a **fresh build** from this engagement's own upstream artifacts, an **adaptation** of a real past artifact,
or a **tweak** to one that already exists. In adapt or standalone runs it writes a short light brief first, so the
build and the reviewer both have stated facts to work from (`post-sales/skills/content-generation/SKILL.md` §6b, §6c).

Each generated artifact is handed to its own reviewer agent, with a human-approved fix loop: reviewers never
edit, and `content-fixer` applies only the fixes a human approved. (Deep research has no reviewer agent, a human
read is its gate.) Outputs are saved under `post-sales/Outputs/[Client]/`, which is local only and never
committed. In v1, outputs go to Google Drive only when a human uploads them or asks the plugin to; automatic upload with a service
account is planned for v2.

Before a full session's content ships to a client or cohort, the **content-review gate** runs the session-level
bundle review. The plugin ships 15 reviewer agents plus one fixer:

```
/acceler-post-sales:content-review   →  runs every applicable single-artifact reviewer first (deck, code-demo,
                                         mcq, assignment, hands-on-guide, project), then two bundle-level
                                         checks, cross-artifact coherence and audience-fit. Produces a
                                         ✅ Approve / 💬 Comment / 🔴 Request Changes tier (a recommendation
                                         for human sign-off, never an auto-ship)
```

Each single-artifact reviewer also runs standalone (`/acceler-post-sales:deck-review`,
`/acceler-post-sales:mcq-review`, etc. — full list under "Agents," below) for fast feedback right after one
artifact is generated, without waiting on the rest of the bundle.

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
- **Skills** (`post-sales/skills/`), 34 in all
  - `content-generation/`: the shared pattern every generation skill follows: pipeline order, where `Outputs/` lives, mandatory versus best-effort inputs, self-checks before handoff, the three build modes (§6b) and the start-up check each generation command runs first (§6c)
  - **Pipeline and generation skills**, one per stage: `discovery-checklist/`, `onboarding-form-generation/`, `deep-research/`, `lesson-plan-generation/`, `instructor-finalization/`, `mcq-generation/`, `demo-generation/`, `slide-content-planning/`, `orientation-generation/`, `closing-ceremony-generation/`, `assignment-generation/`, `project-generation/`, `hands-on-guide-generation/`, `dry-run-feedback/`
  - `live-session-deck/` — the live delivery deck engine (HTML). The default template is the real e& Low-Code Day 3 deck (fixed 1280×720 canvas, Lexend font, indigo/purple palette), with Nucleus and Hungary kept as named alternates a human can pick. Open / Set up / Build / Close arc, slide types §3.1 to §3.23
  - `build-lab-picker/` — standalone "pick your use case" page for a Build Lab, pulled out of `live-session-deck`'s inline pick-cards slide (worked reference: `copilot-leadership-lab.vercel.app`)
  - `session-recap/` — post-delivery pair: the learner-facing `dayN-learner-recap` and the stakeholder-facing impact report (built by `session-impact-report`, with a summary copy saved to `Outputs/[Client]/impact-report.md`), including the data pointers to ask for upfront and both artifacts' design systems
  - **Content-review skills** (`deck-review/`, `code-demo-review/`, `mcq-review/`, `assignment-review/`, `project-review/`, `hands-on-guide-review/`, `lesson-plan-review/`, `discovery-fit-review/`, `onboarding-form-review/`, `orientation-review/`, `closing-ceremony-review/`, `session-recap-review/`, `impact-report-review/`, `audience-fit-review/`, `content-review/`) — one rubric per artifact type, every rule grounded in a real Acceler program document or a real cited finding, not assumed. Shared mechanics (bounded human-gated fix loop, a baseline quality bar, a prose-quality/anti-AI-tell check verified against external research, universal discovery-fidelity checking) all live in `agent-loops/`, not duplicated per skill.
- **Agents** (`post-sales/agents/`) — 16 real, plugin-namespaced subagents (`acceler-post-sales:<name>`): 15 reviewers plus one fixer, not generic agents handed a prompt. Every reviewer ships with `disallowedTools: Write, Edit` — it cannot touch a file even if instructed to. Only `content-fixer` can edit anything, and only with one human-approved finding at a time.
  - Single-artifact: `deck-reviewer`, `code-demo-reviewer`, `mcq-reviewer`, `assignment-reviewer`, `project-reviewer`, `hands-on-guide-reviewer`, `lesson-plan-reviewer`
  - Pipeline-stage gates: `discovery-fit-reviewer`, `onboarding-form-reviewer`, `orientation-reviewer`, `closing-ceremony-reviewer`
  - Post-delivery: `session-recap-reviewer`, `impact-report-reviewer`
  - Bundle-level + fixer: `coherence-reviewer` (cross-artifact consistency), `audience-fit-reviewer` (calibration to the real cohort), `content-fixer` (the only agent with edit access)
  - `post-sales/evals/` — 20 golden test cases covering all 16 agents, run against real content (not just written) — see `evals/README.md` for the manual-run protocol (Claude Code's native `claude plugin eval` is gated to early access on this account)
  - `post-sales/generation-learnings/` — the generation-side half of the improvement loop: one file per content type, read by the matching generation skill. A defect a reviewer keeps catching gets promoted into a directive the generator follows next time. `mcq.md` already holds real promoted entries from the e& test run; the other files are still waiting on their first ones
- **Knowledge** (`post-sales/knowledge/`): what the delivery pipeline reads, separate from the pre-sales Knowledge Graph
  - `engagement-catalog.md`: past engagements and the real content that exists for each, used to find a precedent to adapt
  - `deck-reference/`: the e& Low-Code Day 3 deck template (slides, backgrounds, fonts) plus a written summary and contact sheet of the real e& Day 1 deck
  - `brand-assets/`: Acceler logo (default) and the PowerUp logo (only on explicit request)
  - `curriculum-graph.json` · `testing-rubric.md`: delivered-curriculum data read by Deep Research and Slide Content Planning, and the rubric distilled from the first real pipeline test
- **Outputs** (`post-sales/Outputs/[Client]/`): where every generation command saves, always resolved from the plugin root. Git-ignored: real client output stays on the machine that ran it (and in Drive once a human uploads it)
- **Commands** (`post-sales/commands/`), 33 in all
  - Pipeline and generation (18): `discovery-checklist`, `onboarding-form-generation`, `deep-research`, `lesson-plan-generation`, `instructor-finalization`, `mcq-generation`, `demo-generation`, `slide-content-planning`, `session-deck`, `orientation-generation`, `closing-ceremony-generation`, `assignment-generation`, `project-generation`, `hands-on-guide-generation`, `session-recap`, `session-impact-report`, `dry-run-feedback`, `pick-use-case`
  - Review (15): `content-review`, plus one per reviewer (`deck-review`, `code-demo-review`, `mcq-review`, `assignment-review`, `project-review`, `hands-on-guide-review`, `lesson-plan-review`, `discovery-fit-review`, `onboarding-form-review`, `orientation-review`, `closing-ceremony-review`, `session-recap-review`, `impact-report-review`, `audience-fit-review`)

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

/acceler-post-sales:discovery-checklist          Score the closed deal against the 33 questions, save the Discovery Facts Sheet
/acceler-post-sales:onboarding-form-generation   Generate the Learner Onboarding Form
/acceler-post-sales:deep-research                Pull everything known about the client into one research brief
/acceler-post-sales:lesson-plan-generation       Generate the minute-level Lesson Plan (one tab per day)
/acceler-post-sales:instructor-finalization      Build the day-by-day instructor roster from the Lesson Plan
/acceler-post-sales:mcq-generation               Generate one MCQ set (pre-test, post-test, or a day's in-session quiz)
/acceler-post-sales:demo-generation              Generate the in-class hands-on build (notebook, code file, or build guide)
/acceler-post-sales:slide-content-planning       Plan one day's slides, content and component per slide
/acceler-post-sales:session-deck                 Generate the live delivery deck (HTML, e& template by default)
/acceler-post-sales:orientation-generation       Generate the program Orientation deck (PPTX)
/acceler-post-sales:closing-ceremony-generation  Generate the program Closing Ceremony deck (PPTX)
/acceler-post-sales:assignment-generation        Generate one rubric-graded assignment
/acceler-post-sales:project-generation           Generate one capstone / multi-milestone project
/acceler-post-sales:hands-on-guide-generation    Generate the tool-access / setup guide
/acceler-post-sales:session-recap                Generate the post-delivery learner recap (HTML)
/acceler-post-sales:session-impact-report        Generate the post-delivery stakeholder impact report (HTML)
/acceler-post-sales:dry-run-feedback             Capture dry-run notes and route content findings through the fix loop
/acceler-post-sales:pick-use-case                Generate the standalone "pick your use case" Build Lab page

/acceler-post-sales:content-review        Run the full content-review gate (all applicable reviewers + coherence + audience-fit)
/acceler-post-sales:deck-review            Review one deck standalone
/acceler-post-sales:code-demo-review       Review one notebook/code lab/no-code build guide standalone
/acceler-post-sales:mcq-review             Review one MCQ set standalone
/acceler-post-sales:assignment-review      Review one assignment standalone
/acceler-post-sales:project-review         Review one capstone/multi-milestone project standalone
/acceler-post-sales:hands-on-guide-review  Review one tool-access/setup guide standalone
/acceler-post-sales:lesson-plan-review     Review one Lesson Plan standalone
/acceler-post-sales:discovery-fit-review   Score a discovery brief + extract the Discovery Facts Sheet
/acceler-post-sales:onboarding-form-review Review one Learner Onboarding Form standalone
/acceler-post-sales:orientation-review     Review one program Orientation deck standalone
/acceler-post-sales:closing-ceremony-review Review one program Closing Ceremony deck standalone
/acceler-post-sales:session-recap-review   Review one learner-facing recap standalone
/acceler-post-sales:impact-report-review   Review one stakeholder-facing impact report standalone
/acceler-post-sales:audience-fit-review    Review a bundle's fit to the real cohort standalone
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

*acceler-presales (Acceler Atlas) v0.6.7 · acceler-post-sales (Acceler Atlas · Delivery) v0.4.7 · Oct 2026 · Utkarsh Raj · Acceler / Interview Kickstart B2B · [CHANGELOG](CHANGELOG.md) · [CONTRIBUTING](CONTRIBUTING.md)*
