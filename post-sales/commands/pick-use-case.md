---
description: "POST-SALES / delivery micro-tool. Generate a standalone 'pick your use case' page for a leadership Build Lab — the moment a cohort self-selects which agent/use case their team builds. Pulled out of live-session-deck's inline pick-cards slide into its own shareable URL. For the full session deck, use /acceler-post-sales:session-deck instead."
argument-hint: "<client name + list of use cases/agents to choose from, or 'use last' to pull from the latest session deck's build-phase content>"
---

Generate a standalone **pick your use case** page for this brief — the page a Build Lab cohort opens on their own devices to self-select which agent/use case their team builds. (For the full slide-driven session deck, use `/acceler-post-sales:session-deck`; this command is for when that moment needs to stand alone.)

## Input
```
$ARGUMENTS
```

## How to build

1. Read `skills/build-lab-picker/SKILL.md` for the page structure, the `pick`/`pcard` component spec (shared with `live-session-deck` §3.11), content-sourcing rules, and deployment pattern.
2. If a session deck was generated earlier in this session (`/acceler-post-sales:session-deck`), reuse its build-phase use cases and design tokens directly — same brand row, same `--navy`/`--cyan`/cream `--bg` tokens, so the picker reads as the same family as the deck.
3. Pull use cases from the actual engagement brief (Day-by-Day content, or the HR Agent Build Lab pattern of 1 base agent + N extensions) — never generic placeholders.
4. Build: brand row → 2–3 column `pick` grid of `pcard`s (function eyebrow + agent name + description + `ptag` chips) → visible selection state on click → one handoff line telling the team what to do next.

## Quality checklist (apply before saving)

- [ ] Brand row lists every party in the room, kept minimal (not a full cover slide)
- [ ] Each `pcard` maps to a real use case/agent from this engagement's build plan, not a generic example
- [ ] Selecting a card visibly commits to it (border/fill change) with no backend required
- [ ] One handoff line under the grid states the next physical step
- [ ] Same design tokens as the paired session deck, if one exists for this engagement
- [ ] Self-contained HTML — inline CSS, no external font/CDN dependency
