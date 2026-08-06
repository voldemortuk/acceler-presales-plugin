---
name: build-lab-picker
description: "POST-SALES / delivery micro-tool. Generate a standalone 'pick your use case' web page for a leadership Build Lab — the moment where a cohort self-selects which Copilot Studio agent/use case their team builds. Pulled out of live-session-deck's inline pick-cards component (§3.11) into its own shareable URL. Worked example: copilot-leadership-lab.vercel.app/pick-your-use-case (CETIN × Yettel × Acceler)."
---

## What this is

Most session decks fold the "pick your use case" moment into one slide inside the full deck (`live-session-deck` §3.11, the `pick` / `pcard` component). Some engagements need that moment to stand on its own instead — a page a cohort opens on their own devices at the start of a **Build Lab** (3 leaders + 1 Build Coach, or any small-team build format), independent of whether they're following the full slide deck at the front of the room.

Use this skill when:
- The delivery format is a hands-on **Build Lab**, not a slide-driven walkthrough — people need the use-case picker on their own screen, not just projected once.
- The same picker needs to be reused across multiple sessions/cohorts without regenerating the whole session deck each time.
- A client wants a link they can send to a cohort ahead of time ("here's what you'll be building").

If the engagement is a single instructor-led session with one deck for everyone, just use the inline `pick` component in `live-session-deck` instead — don't stand up a separate page for it.

## Structure

A single-page, no-build-step HTML file (same self-contained-artifact constraints as the rest of this plugin family: inline CSS, no external fonts/CDN dependencies beyond what the client's network allows).

1. **Brand row** — logos of every party in the room (client + any delivery partners, e.g. the CETIN / Yettel / Acceler three-logo row in the reference). Keep it minimal — this isn't the cover slide, it's a utility page.
2. **Pick cards** — reuse `live-session-deck` §3.11's `pick` / `pcard` spec exactly: 2–3 column grid, each card = function eyebrow (the *job to be done*, e.g. "Staying current") + agent/use-case name (e.g. "Yettel Market Pulse") + 1–2 line description + 2–3 cyan-soft `ptag` chips (the skills/connectors that use case touches). Same tokens as the parent deck (`--navy`, `--cyan`, cream `--bg`) so it reads as the same family, not a different tool.
3. **Selection state** — clicking/tapping a card should visibly commit to it (border/fill change), since teams reference "which card did we pick" for the rest of the lab. No backend needed — client-side state is enough; this is a workshop prop, not a tracked funnel.
4. **Handoff line** — one line under the grid telling the team what happens next ("Tell your Build Coach which one you picked, then head to Copilot Studio").

## Content sourcing

Pull the actual use cases from the same place `live-session-deck` does for that engagement — the proposal's Day-by-Day content or the client's brief. Don't invent generic use cases; each card should map to a real agent/extension in that engagement's build plan (see the HR Agent Build Lab pattern: 1 base agent + N Copilot Studio extensions — each extension is a candidate card).

## Deployment

Same pattern as other decks in this family — ship it as its own static page (e.g. a dedicated Vercel deployment per engagement, `<engagement>-leadership-lab.vercel.app/pick-your-use-case`), separate from the session-deck deployment so it can be linked to a cohort independently and revisited after the session.

## Worked reference

`copilot-leadership-lab.vercel.app/pick-your-use-case` — the CETIN × Yettel × Acceler "Build your AI team" Build Lab (e& PPF Hungary engagement). Same design tokens as `live-session-deck`'s e& PPF reference deck.
