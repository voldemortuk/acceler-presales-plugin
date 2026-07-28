# Changelog

This repo hosts two plugins in one marketplace: `acceler-presales` (root) and `acceler-post-sales` (`post-sales/`). Each versions independently in its own `.claude-plugin/plugin.json`; entries below are labeled by plugin. Bump the relevant plugin's `version` on every release so teammates' `/plugin marketplace update` picks up the change.

## acceler-post-sales [0.1.0] — 2026-07-28

### New plugin — split out of acceler-presales
- **`acceler-post-sales`** (NEW plugin, same repo, `post-sales/` subdirectory) — delivery/post-sales artifacts now version and install separately from the pre-sales pipeline: `session-deck` (live delivery deck), `session-recap` (post-delivery learner recap), `session-recap-report` (post-delivery stakeholder analytics).
- **Why:** `acceler-presales` is a pre-sales orchestrator by name and by the majority of its commands (discovery, similar, proposal, proposal-deck, pricing, instructors). Session deck and session recap are delivery-phase artifacts generated after the deal closes, for a different moment in the lifecycle and often a different user. Splitting them into their own plugin makes both names accurate and lets either be installed/updated independently.
- Moved from `acceler-presales` into `acceler-post-sales`: `commands/session-deck.md`, `commands/session-recap.md`, `commands/session-recap-report.md`, `skills/live-session-deck/`, `skills/session-recap/`.
- Both plugins are listed in the same `.claude-plugin/marketplace.json` (`acceler-local`), so one `/plugin marketplace add` still gets you both; install either or both with a separate `/plugin install`.
- `commands/setup.md` (in `acceler-presales`) updated: the `live-session-deck` workspace symlink now targets `acceler-post-sales`'s own plugin cache path, and the "what to run next" listing is split by plugin.

## acceler-presales [0.6.0] — 2026-07-28

### Delivery/post-sales commands moved out (see acceler-post-sales above)
- `session-deck`, `session-recap`, `session-recap-report` and their skills (`live-session-deck/`, `session-recap/`) moved to the new `acceler-post-sales` plugin. `acceler-presales`'s own description and pipeline now cover only discovery → similar → proposal → proposal-deck → pricing → instructors.
- `commands/full-cycle.md`'s post-sales cross-reference updated to point at `/acceler-post-sales:session-deck` in the companion plugin instead of a command that no longer lives in this plugin.

## acceler-presales [0.5.0] — 2026-07-28

### Session recap skill (NEW) — post-delivery, learner + stakeholder artifacts
- **`skills/session-recap/SKILL.md`** (NEW) — how to build the two artifacts that follow every live delivery day: the learner-facing `dayN-learner-recap` (topics covered, quiz/discussion, cheat sheet, hands-on labs, parked Q&A, resources) and the stakeholder-facing `dayN-recap-report` (KPIs, per-learner tier categorisation with evidence, engagement by topic, feedback breakdown, action items). Documents the exact data pointers to ask for upfront for each, the design system/component library for each (they're deliberately distinct palettes), section-by-section structure with last-day branching logic (Wrapping Up + assessment link vs Before Day N+1), and the tier-assignment rule (behavior that day, never tenure).
- **`/acceler-presales:session-recap`** (NEW) — learner recap command.
- **`/acceler-presales:session-recap-report`** (NEW) — stakeholder report command.
- Both reference the standing prose style rules (no em-dashes, no 3-item rhetorical cadence, no antithesis-as-crutch constructions) so recap prose stays consistent with house style.
- Extracted from the LVT AI-Assisted Development Program's 4-day recap series (learner recaps + Day 3 stakeholder report) as the worked reference.

## acceler-presales [0.4.0] — 2026-07-01

### HTML→PPTX converter is now bundled + deck sources locked to canonical folders
- **`skills/pptx-deck/build_pptx.py`** (NEW) — the image-fidelity assembler (per-slide 2× PNGs → 16:9 PPTX) is now bundled next to its SKILL.md, so the converter is self-contained. Previously the SKILL referenced `build_pptx.py` "in this folder" but the script was missing — teammates couldn't run the export.
- **Deck sources verified byte-identical** to the canonical Pre-Sales Product builder folders:
  - `skills/proposal-deck/` ← `Deck Proposal Builder /HTML-Deck-Builder/` (KASE engine + Images + e& reference)
  - `skills/pptx-deck/` ← `Deck Proposal Builder /HTML-to-PPTX Converter/` (HTML→PPTX)
  - `skills/live-session-deck/` ← `Live Session-Deck-Builder/`
- README: corrected the `pptx-deck/` description (it is the HTML→PPTX image-fidelity converter that exports the **proposal-deck**, not a match-a-reference rebuild).

## acceler-presales [0.3.0] — 2026-07-01

### Deck commands split — pre-sales vs post-sales
- **`/acceler-presales:proposal-deck`** (NEW) — the pre-sales PITCH deck. Client-facing proposal/solution deck on the KASE engine (data-driven `SLIDE_DATA` slides, ~80 templates). The engine (`Slide_Library_v2/kase-render.js` + `kase-styles.css`), brand images, and the e& AI Builder reference deck are bundled in `skills/proposal-deck/` so the command is self-contained.
- **`/acceler-presales:session-deck`** (RENAMED from `/deck`) — the post-sales DELIVERY deck. The live hands-on session deck an instructor presents during delivery (VM setup, screenshots, build steps).
- **Why:** the old single `/deck` command only built the delivery deck and was mislabeled "live session deck," while the KASE proposal deck — the thing you actually pitch with — had no command at all.
- `full-cycle` Stage 4 now generates the pre-sales proposal deck; the session deck is documented as a later post-sales artifact.
- README, setup listing, plugin + marketplace descriptions updated.

## acceler-presales [0.2.0] — 2026-06-26

### Pricing skill
- **Excel (.xlsx) is now the default deliverable** for `/acceler-presales:pricing` (markdown demoted to an in-chat summary). Sheet is formula-driven off yellow assumption cells.
- **FX rule:** always pull the live USD→INR rate before quoting — never hardcode (≈ ₹94.5/$ as of Jun 2026; old ₹84 retired for new quotes).
- **Infra = VMs + Tools** combined into one line. Client-provisioned tools (e.g. Claude Code seats) excluded.
- **VM-days window:** cost VMs over program-days + 3–4 buffer days (not just in-session days); call the buffer out to the user.
- **Assignment Review** is now explicitly optional (drop when not grading / releasing solutions).

### Instructors command
- Fixed query to read instructor nodes from `graph.json` (not the file-index `instr_candidates.json`).
- Added **pre-sales vs post-sales (deliverable)** split with a deliverability heuristic.
- New `knowledge/instructor_delivery_flags.json` — human override of who is actually staffable; current `pre_sales_only`: Cassie Kozyrkov, Michelle K. Lee, Eric Siegel.
- Caveat header added to `skills/mini-ut-context/Instructor_Pool.md`.

## acceler-presales [0.1.0]
- Initial plugin: discovery → similar → proposal → deck → pricing → instructors, powered by the Acceler Knowledge Graph.
