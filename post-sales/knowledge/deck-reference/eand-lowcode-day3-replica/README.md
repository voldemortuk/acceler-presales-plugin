# e& Low-Code Day 3, reference deck (structural copy)

The real, actually-delivered e& AI Builder Program (Low Code) Day 3 session deck, 85 slides, pixel-traced from the original PPTX. This is `live-session-deck/SKILL.md`'s current default reference for palette, fonts, and component structure (§1, §2.0, §3.x).

**What's kept here:** the 85 real slide HTML files (`01-slide.html` ... `85-slide.html`), `deck.json`, `index.html`, `slide_summary.txt`, the real fonts (`assets/fonts/`), and the five real background images (`assets/backgrounds/`: cover, framed, content-lavender, content-blush, demo-laptop) that `live-session-deck/SKILL.md` §2.0 depends on. Moved here from a session's temp scratchpad, 2026-09-24, so every future session/terminal can reach it, not just the one that originally cloned it.

**What's deliberately not kept here:** the real per-slide content screenshots (~25MB, `assets/bg_*.png`, `assets/img_*.png`). Those are that specific class's actual delivered screenshots, not structurally reusable across other clients, keeping them here would just bloat the repo. Because of that, the slide files still point at `assets/bg_*.png`, `assets/img_*.png` and an instructor photo that are not in this folder, expect broken images if a slide file is opened on its own. The logo (`acceler-logo-dark.svg`) lives separately in `post-sales/knowledge/brand-assets/`, since that one *is* reusable across every deck, not specific to this one.

When building a new client's session deck, copy structure/CSS/components from these slide files per `live-session-deck/SKILL.md`, pull the logo from `brand-assets/`, and use this client's own real screenshots, not these.
