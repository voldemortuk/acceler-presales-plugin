# Changelog

All notable changes to the `acceler-presales` plugin. Bump `.claude-plugin/plugin.json` `version` on every release so teammates' `/plugin marketplace update` picks up the change.

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
