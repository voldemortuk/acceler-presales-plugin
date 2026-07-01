# Changelog

All notable changes to the `acceler-presales` plugin. Bump `.claude-plugin/plugin.json` `version` on every release so teammates' `/plugin marketplace update` picks up the change.

## [0.4.0] — 2026-07-01

### HTML→PPTX converter is now bundled + deck sources locked to canonical folders
- **`skills/pptx-deck/build_pptx.py`** (NEW) — the image-fidelity assembler (per-slide 2× PNGs → 16:9 PPTX) is now bundled next to its SKILL.md, so the converter is self-contained. Previously the SKILL referenced `build_pptx.py` "in this folder" but the script was missing — teammates couldn't run the export.
- **Deck sources verified byte-identical** to the canonical Pre-Sales Product builder folders:
  - `skills/proposal-deck/` ← `Deck Proposal Builder /HTML-Deck-Builder/` (KASE engine + Images + e& reference)
  - `skills/pptx-deck/` ← `Deck Proposal Builder /HTML-to-PPTX Converter/` (HTML→PPTX)
  - `skills/live-session-deck/` ← `Live Session-Deck-Builder/`
- README: corrected the `pptx-deck/` description (it is the HTML→PPTX image-fidelity converter that exports the **proposal-deck**, not a match-a-reference rebuild).

## [0.3.0] — 2026-07-01

### Deck commands split — pre-sales vs post-sales
- **`/acceler-presales:proposal-deck`** (NEW) — the pre-sales PITCH deck. Client-facing proposal/solution deck on the KASE engine (data-driven `SLIDE_DATA` slides, ~80 templates). The engine (`Slide_Library_v2/kase-render.js` + `kase-styles.css`), brand images, and the e& AI Builder reference deck are bundled in `skills/proposal-deck/` so the command is self-contained.
- **`/acceler-presales:session-deck`** (RENAMED from `/deck`) — the post-sales DELIVERY deck. The live hands-on session deck an instructor presents during delivery (VM setup, screenshots, build steps).
- **Why:** the old single `/deck` command only built the delivery deck and was mislabeled "live session deck," while the KASE proposal deck — the thing you actually pitch with — had no command at all.
- `full-cycle` Stage 4 now generates the pre-sales proposal deck; the session deck is documented as a later post-sales artifact.
- README, setup listing, plugin + marketplace descriptions updated.

## [0.2.0] — 2026-06-26

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

## [0.1.0]
- Initial plugin: discovery → similar → proposal → deck → pricing → instructors, powered by the Acceler Knowledge Graph.
