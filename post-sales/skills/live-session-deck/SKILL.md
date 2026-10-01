---
name: live-session-deck-skills-acceler-html-live-session-deck-builder
description: "How to build a client-facing live session HTML deck — full-screen slides with keyboard navigation, speaker notes, progress bar, touch swipe, and print fallback. Default reference and design tokens updated 2026-09-18 to the e& Low-Code Day 3 deck (indigo/purple palette, Lexend font), per Utkarsh's own instruction; Nucleus and Hungary remain available as named alternates. Covers the slide-type library (cover · instructor intro · phase divider · pipeline · checklist · screenshot · ecosystem · chapter rail · pick cards · statement · handoff), the JS controller (40 lines), and accessibility. This is the SOURCE that PPTX_Deck_Skills.md converts from — keep the HTML and PPTX in sync via shared data. Companion to [[pptx-deck-skills]], [[doc-proposal-skills]], [[pricing-skills]], [[session-mapping-skills]]."
metadata:
  node_type: memory
  type: reference
  originSessionId: 6d83a6a7-37dc-44fa-b73c-61e1418d2a82
---

# Acceler Live Session Deck Builder — SKILL.md (v1)

**The HTML deck is the source of truth.** A PPTX or PDF copy is downstream, produced only when someone actually needs one, per `content-generation/SKILL.md` §1b's export note: default to `pptx-deck/SKILL.md`'s screenshot-per-slide method (fast, pixel-perfect, never drifts, PDF comes free from the same rendering step), and only fall back to `PPTX_Deck_Skills.md`'s native, editable rebuild when a human explicitly asks to be able to type-edit the PPTX afterward, per `content-generation/SKILL.md` §2a. Keep this HTML deck's content data in a single block at the top either way, so whichever conversion path runs can reuse it without drift.

**Default reference, updated 2026-09-18:** `eand-aibuilder-lowcode-day3-replica` (e& AI Builder Low Code, Day 3, real delivered deck, 86 slides), per Utkarsh's own direct instruction to build new content in this deck's style. Nucleus (Apr 2026) is kept as a named alternate, per §2.1a below, same as Hungary already was. Fixed, used every time by default, not inferred or auto-matched to a client per engagement, same reasoning as before: a real run once tried to auto-detect a same-client match, picked a wrong one (a different company sharing only a name prefix), and a second run skipped using a real reference file entirely rather than risk the same mistake. **Reusable for:** any live session. The engine is reference-agnostic, a human can point it at Nucleus, Hungary, or any other reference deck for a specific engagement, but that's an explicit human choice each time, never something this skill infers on its own.

**What "follow this reference" actually means, clarified 2026-09-18.** It's a template, not a file-copying instruction: each slide *type* (cover, instructor intro, warm-up, quiz, teaching content, closing) has its own consistent look in the reference, follow that same look per type, the flow, the visual language, the story arc. It does not mean literally rebuilding every slide as a separate static file pixel-traced from a source deck, that's simply how this particular reference happened to be produced, not a requirement for how this skill generates new ones. Keep building through this skill's existing single-page, component-based approach (§3.17), just make each slide type's actual look match this reference's real slide types (§2.1a).

---

## 1. The deck shape (4 movements)

**Rewritten 2026-09-18, against the real, complete e& Low-Code Day 3 deck (85 slides), checked slide by slide, not sampled.** The old 15-25 slide estimate and movement list below were closer to a short pre-sales taster than an actual delivered class day. Every live session deck now follows this real arc:

1. **Open** — cover (program, day number, instructor name) · **Instructor Detail** (§3.2, a real two-column bio, not a simple intro card) · "pop into the chat" warm-up (role/location/name) · today's timing table (Time | Activity, sessions and breaks) · **the 4-Day Mindmap** (§3.18, all days shown, today's card visually highlighted) · "optimise your experience" / house-rules cards
2. **Set up** — Virtual Lab / VM readiness checklist (a short checklist slide, not the old 5-numbered-screenshot pattern, keep this lean per the earlier real fix) · **Today's Agenda** (§3.19, a numbered list of today's own topics, e.g. #1/#2/#3)
3. **Build** — the actual concept-teaching content per §3.16 (this is the large majority of the slide count, confirmed again here: roughly 65 of 85 real slides), organized into topic blocks matching Today's Agenda's own numbered items. **Re-show Today's Agenda between each topic block** (§3.19), same slide, so learners always know where they are, confirmed a real recurring pattern (shown 3 times across this deck, not once). **Demo** (§3.20, a plain, minimal link-out slide, title + "Link for Demo" + a click-through button, not an embedded walkthrough) sits at the point in Build where that topic's hands-on demo actually happens.
4. **Close** — **Quiz** (§3.21, a real in-session knowledge check, question + options + the correct one revealed) then a simple **Thank You** closing slide (§3.22). No dark statement/CTA slide in the real reference, don't force one in if the engagement doesn't call for it.

**Corrected 2026-09-18: slide count is not a fixed number, don't target one.** This real reference happened to run 85 slides for its own content, that's an example of what a real day can look like, not a target to hit. What actually decides slide count is the real content this specific day needs and the real time available (commonly around 6 hours, but confirm per engagement, never assumed). A day with heavier hands-on building time might genuinely land around 50 slides, lighter teaching, more doing. A day dense with new concepts might run past 85. Build to what the Lesson Plan and Deep Research actually call for, then check it against the time available, don't build toward a slide-count number either way. The old 15-25 estimate was still wrong, too thin for a real delivered day, but replacing it with a new fixed number would be the same mistake in the other direction. Cover, Instructor Detail, the Mindmap, and Today's Agenda are framing, built once and reused, only their data changes per client/day.

---

## 2. Design tokens (lock these)

**Correction, 2026-09-14, superseded 2026-09-18.** §2.1a (Nucleus) used to be the default palette. Per §1's update, the default reference is now the e& Low-Code Day 3 deck, §2.0 below is its real, confirmed palette, checked directly against the real files (`01-slide.html`, `03-slide.html`, `06-slide.html`, `82-slide.html`, `85-slide.html`). **Use §2.0 by default.** Use §2.1a (Nucleus) or §2.1 (Hungary) only on the specific occasions a human explicitly points this run at one of them instead.

### 2.0 Palette (e& Low-Code Day 3, the actual default reference)

Text colors, confirmed consistent across every slide type checked, cover through closing, this is one real, unified system, not different colors per slide type despite the backgrounds looking different:

```
--text-heading:  #2D2D5C   /* headings, labels, most body text */
--text-body:     #393E59   /* house-rules / instructional body copy */
--text-black:    #030303   /* a few slides use near-black instead */
--text-muted:    #595969   /* secondary/muted text */
--accent:        #6568F5   /* kickers, small highlight labels */
--accent-2:      #6E69EB   /* close relative of accent, used interchangeably */
--link-accent:   #0097A7   /* clickable "Click Here" text specifically, e.g. §3.20's demo link-out button */
--page-bg:       #e8e6f4   /* fallback background behind the slide canvas */
```

**Added 2026-09-25, real gap found on a live test run.** `--link-accent` was missing from this list entirely, confirmed by checking the real reference deck's actual slide 81 pixels directly (`#0097A7`), and independently cross-confirmed against a second real deck (the actual e& Low-Code Day 1 delivery, same exact value). Any clickable "Click Here" text uses this color, not `--text-heading`, that was the real cause of a demo link-out button rendering with no accent color at all.

**Every slide's actual background is a custom-designed image, not a flat token, and this is where the deck's real look actually comes from.**

**Corrected 2026-09-27, real root cause of a live test run's "colors don't match at all" complaint.** This section used to say not to use the real background images and to use a flat color instead. That was wrong: the text tokens above were correct in a real generated deck, and it still looked nothing like the real one, because the real deck's look lives almost entirely in its backgrounds. Checked all 85 real Day 3 slides: they use only a handful of real background images, and cross-checked against the real Day 1 deck (104 slides), same family, same mapping. The real ones are now saved permanently in `post-sales/knowledge/deck-reference/eand-lowcode-day3-replica/assets/backgrounds/`. **Use them, copied into the deck's own `assets/`, as `background: url(...) center/cover`, never a flat fill.**

| Background file | Look | Used for (real slide types, confirmed in both real decks) |
|---|---|---|
| `cover.png` | warm blush, three stacked pastel circles on the right | Cover only |
| `framed.png` | peach-to-lavender with a large rounded frosted card | One-idea "statement" slides: Pop Into The Chat, Virtual Lab Access intro, Let's Get Started, every section divider ("Part 1 / How AI is reshaping the Tech World"), Break Time, Thank You |
| `content-lavender.png` | cool lavender, one soft large circle | The default content background, most teaching slides, Instructor Detail, Mindmap, Agenda, house rules |
| `content-blush.png` | soft warm peach/cream wash | Alternate content background: timing table, VM checklist, and teaching slides where the real deck alternates for variety |
| `demo-laptop.png` | lavender with a laptop mockup bottom-right, gradient screen | Demo slides only, §3.20 |

**`content-lavender.png` is the base background for every slide, set on `.slide` itself; the others are overrides for specific slide types.** Found on a real generated deck: only the special backgrounds (cover, framed, blush, demo) got applied, and the default one was never set at all, so every ordinary teaching slide (most of the deck) fell back to a flat color, with plain strips showing at the sides. Write it in this order:

```css
.slide { background:url(assets/backgrounds/content-lavender.png) center/cover; }   /* default, every slide */
.slide.cover { background-image:url(assets/backgrounds/cover.png); }
.slide.statement, .slide.divider, .slide.thankyou { background-image:url(assets/backgrounds/framed.png); }
.slide.timing, .slide.vmcheck { background-image:url(assets/backgrounds/content-blush.png); }
.slide.demo { background-image:url(assets/backgrounds/demo-laptop.png); }
```

No inner element inside a slide ever gets its own background image or full-width background color.

**Never reuse a slide-type class name for an inner element's style.** Found on a real generated deck: slides were marked `class="slide build concept-intro"`, and a separate rule `.concept-intro{max-width:900px}` written for the text box inside, so every slide of that type got squeezed to 900px wide, with plain strips on both sides. Slide-type classes (on `.slide`) and inner element classes must have different names, and nothing that sits on `.slide` ever gets a `max-width`.

A visual reference of these real slide types side by side lives at `post-sales/knowledge/deck-reference/eand-lowcode-day1-real/slide-types-contact-sheet.png`, open it before building if unsure which background a slide type takes.

**Two more real constants on every non-cover slide:** the logo sits small in the top-right corner (per `content-generation/SKILL.md` §1d, Acceler by default), and the title sits top-left in `--text-heading`, often with one key word in `--accent` (e.g. "**Optimise** Your Experience", "Agenda **For Today**").

Font, confirmed real from the actual embedded files (`assets/fonts/Lexend-*.ttf`):

```
--font-display: 'Lexend SemiBold', 'Lexend', sans-serif;   /* headings */
--font-body:    'Lexend', sans-serif;                       /* body text */
--font-medium:  'Lexend Medium', 'Lexend', sans-serif;       /* emphasis, labels */
--font-mono:    'JetBrains Mono', monospace;                 /* code blocks, §3.17, unconfirmed against this reference, kept from the prior default */
```

### 2.1a Palette (Nucleus, named alternate only)

```
--bg-primary:    #f8fafc   /* page background, cool slate, not cream */
--bg-secondary:  #f1f5f9
--bg-tertiary:   #e2e8f0
--bg-card:       #ffffff
--bg-dark:       #0f172a   /* dark slides */
--bg-dark-card:  rgba(255,255,255,0.04)
--text-primary:  #0f172a
--text-secondary:#475569
--text-muted:    #94a3b8
--accent:        #2563eb   /* blue, not navy/cyan */
--accent-hover:  #1d4ed8
--accent-light:  #eff6ff
--accent-glow:   rgba(37,99,235,0.15)
--green:         #16a34a
--green-light:   #f0fdf4
--green-border:  #bbf7d0
--orange:        #ea580c
--orange-light:  #fff7ed
--red:           #dc2626
--red-light:     #fef2f2
--border:        #e2e8f0
--border-light:  #f1f5f9
```

Fonts, also confirmed real, and notably different from §2.2 below, Nucleus uses a serif display face, not a system sans:

```
--font-display: 'DM Serif Display', Georgia, serif;   /* headings */
--font-body:    'DM Sans', -apple-system, sans-serif;  /* body text */
--font-mono:    'JetBrains Mono', monospace;            /* code blocks, §3.17 */
```

### 2.1 Palette (Hungary, named alternate only)

```
--bg:            #FAF7F1   /* cream/beige page background */
--bg2:           #EEF0FB   /* secondary surface (chat warm-up) */
--card:          #FFFFFF   /* card surfaces */
--navy:          #2C3F8E   /* primary navy (banners, accents) */
--navy-deep:     #0F1632   /* dark slides + body fallback */
--cyan:          #5BC4D2   /* progress · accents · CTAs */
--cyan-dk:       #2C3F8E   /* cyan dark (headings) — alias of navy */
--cyan-soft:     #E8F7FA   /* cyan tint (callouts, urlcard) */
--ink:           #1A2240   /* body text */
--text2:         #4B5563   /* secondary text */
--muted:         #6B7280   /* tertiary / labels */
--border:        #E5E7EB   /* hairlines */
--green:         #10B981   /* success */
--green-l:       #ECFDF5   /* success tint (allgreen check) */
--amber:         #B45309   /* warn (locked steps) */
--amber-l:       #FFF8ED   /* warn tint */
--red:           #DC2626   /* error */
--red-l:         #FEF2F2   /* error tint */
```

Dark slides use a navy gradient: `linear-gradient(135deg,#0F1632 0%,#2D3666 100%)`. Cover uses radial: `radial-gradient(ellipse at 30% 25%,#2D3666 0%,#0A0A1A 100%)`.

### 2.2 Typography (Hungary, only when a human explicitly picks this reference)

```
--font: -apple-system, BlinkMacSystemFont, "Segoe UI", "Helvetica Neue", Arial, sans-serif;
--mono: "SF Mono", Monaco, "Cascadia Code", monospace;
```

Type scale (use `clamp()` for responsiveness):

| Element                    | Size                          | Weight | Notes                            |
|----------------------------|-------------------------------|--------|----------------------------------|
| Cover h1                   | `clamp(40px,5vw,68px)`        | 800    | letter-spacing -0.025em          |
| Section h2                 | `clamp(34px,4.4vw,56px)`      | 800    | letter-spacing -0.02em           |
| Statement big              | `clamp(34px,4.6vw,60px)`      | 800    | letter-spacing -0.025em          |
| Slide h (.h)               | `clamp(24px,2.9vw,35px)`      | 800    | letter-spacing -0.015em          |
| Lead body (.lead)          | 18px                          | 400    | line-height 1.6                  |
| Eyebrow                    | 13.5px                        | 700    | letter-spacing 0.16em · UPPERCASE|
| Card title (.card .t)      | 20px                          | 800    | letter-spacing -0.01em           |
| Card desc (.card .d)       | 15px                          | 400    | line-height 1.55                 |
| Banner                     | 15px                          | 400    | line-height 1.5                  |
| Mono (URL block)           | 13px                          | 400    | --mono family                    |

### 2.3 Page size & layout

**Corrected 2026-09-27, the real root cause of slides looking misaligned on a live test run.** This used to say each slide stretches to fill the whole browser window. That's why a real generated demo slide broke: the background picture (with the laptop drawn into it) stretched one way, and the text and button placed on top moved a different way, so text ran into the laptop and the button landed on its edge. The real reference is a **fixed 1280×720 canvas**, everything placed on it, and the whole canvas scaled up or down to fit the window. Background and text then always stay locked together, on any screen.

```css
body { margin:0; overflow:hidden; background:#e8e6f4; }
.slide {
  position:absolute; left:50%; top:50%;
  width:1280px; height:720px;
  transform:translate(-50%,-50%) scale(var(--fit,1));
  background-size:cover; background-position:center;   /* the real background, on the whole canvas */
  padding:48px 64px; box-sizing:border-box;
}
```
```javascript
function fit(){ document.documentElement.style.setProperty('--fit',
  Math.min(innerWidth/1280, innerHeight/720)); }
addEventListener('resize',fit); fit();
```

- **The background image goes on `.slide` itself**, the full canvas, never on an inner box. A background on an inner box leaves plain strips of color on the left and right, confirmed on a real generated deck.
- **Layout inside the canvas is in canvas pixels**, not viewport units (`vw`, `clamp()` with `vw`), those break the lock between background and content.
- Progress bar, nav, and notes stay outside the canvas, pinned to the window as before.
- **Slide counter goes bottom-left, not top-right.** Top-right is the logo's spot on every real slide, a counter there sits right on top of it.
- **Print:** `@page { size: 1280px 720px; margin: 0; }`, each `.slide` becomes `page-break-after:always`, forced `opacity:1`, and `transform:none`. Hides progress, brand, fs, counter, zone, notes, nav.

**Real type scale on the canvas, measured from all 85 real slides**, use these, don't shrink them. The real deck's text is big and fills the slide, a real generated deck using smaller sizes came out mostly empty space. **Text size is part of the template, not the content**: any "update to the latest template" or "layout only" pass applies this table too. A real retrofit skipped it as "not layout" and left body text at 13-16px, don't repeat that. Nothing on a slide goes below 17px except a footnote or source line (never below 14px):

| Element | Real size | Notes |
|---|---|---|
| Body text, bullets | 19-21px | the most common size in the whole real deck |
| Small labels, captions | 17px | |
| Slide title | 25-36px | 25px on busy slides, up to 36px on lighter ones |
| Cover title | 49px | |
| Statement / divider title | 57px | Let's Get Started, Part N, Break Time, Thank You |
| Statement emoji | 85px | above the statement title |

---

## 3. Slide-type library (every shape you need)

### 3.1 Cover (slide 0)

**Rewritten 2026-09-27 to the real cover, checked in both real e& decks (Day 1 and Day 3).** The old spec here was a centered, chip-heavy cover from the Hungary/Nucleus decks, use that only if a human explicitly picks one of those as the reference. The real default is simple and **left-aligned**, on `cover.png` (the three pastel circles sit on the right, the text on the left):

| Element | Real position on the 1280×720 canvas | Style |
|---|---|---|
| Logo | top-left, ~left 42px, top 40px | per `content-generation/SKILL.md` §1d, Acceler by default |
| "Day N" pill | left 42px, top ~220px | light lavender pill, text `--accent`, ~24px |
| Program title | left 42px, top ~269px, up to 2 lines | `--text-heading`, ~49px, `--font-display` |
| "By [instructor name]" | bottom-left, left 42px, top ~608px | white pill, "By" in ~21px light weight, name in `--accent` |

Nothing else, no chips, no subtitle paragraph, no centered logo plate. **The title is just the program name** (real: "AI Builder Program (Low Code)"), in the bold display font, never cohort numbers, internal labels like "TESTRUN", or a tagline squeezed in with separators. The instructor name follows the same Proposed/Confirmed rule as the instructor slide, don't present a Proposed name as final.

### 3.2 Instructor Detail (real slide 2 of the Day 3 deck, checked directly, replaces the older description below)

Two-column layout, `360px` fixed left, flexible right, matching the real deck exactly:

- **Left** (white background): circular photo (220px, gradient placeholder `linear-gradient(135deg,#6568F5,#6E69EB)` with initials if no real photo yet), "Technical Specializations" label + bullet list, then a row of **employer logo badges** (pill-shaped, e.g. Apple / Google / Adobe / CMU), pinned to the bottom of the column.
- **Right** (soft gradient background `linear-gradient(120deg,#EDEBFB 0%,#E3E1F7 60%,#D9D7F2 100%)`): name (large, bold), one tagline line (current role + employer, bolded, plus past employers/degree inline), then two labeled sections with an emoji icon each: **"🔷 Career Highlights"** (2-3 bullets, bold the employer names within each) and **"🎓 Academic & Teaching"** (degree, research affiliations, notable recognitions, and instructor track record, e.g. "Instructor @ Acceler — trained 3000+ working professionals"). Acceler logo bottom-right of the whole slide.

Text colors: headings/name `#1a1a2e`, body/bullets `#2D2D5C`, section labels `#1a1a2e` semibold. Real example content confirmed from the reference (Shivam Patel, Sr. ML Engineer @ Apple): don't copy this content, this is the shape to fill with the real instructor's real bio.

Rules:
- Speaker notes: ~60 seconds. Establish credibility, then move.
- Two section blocks (Career Highlights, Academic & Teaching) is the confirmed real pattern, don't add more and dilute it.
- Employer badges are a simple, real, low-effort credibility signal, use them whenever the instructor's real background includes recognizable names, don't fabricate ones that aren't real.

### 3.3 Pipeline (3 steps)

`pipe` = flex row of 3 `pstep` cards separated by `parrow` arrows. Each step = icon + title + 1-line description.

```html
<div class="pipe">
  <div class="pstep"><div class="pi">🧰</div><div class="pt">Set up</div><div class="pd">Get everyone into the virtual lab and working.</div></div>
  <div class="parrow">→</div>
  <div class="pstep"><div class="pi">🤖</div><div class="pt">Build your agent</div><div class="pd">One agent, end-to-end, live on your own account.</div></div>
  <div class="parrow">→</div>
  <div class="pstep"><div class="pi">🌙</div><div class="pt">See one run</div><div class="pd">An agent that works while you sleep.</div></div>
</div>
```

Followed by a `banner` with a conversation-mode reminder (`💬 This works best as a conversation. Ask any time…`).

### 3.4 Chat warm-up (engagement)

Background: `linear-gradient(135deg,#F4F6FC 0%,#D8DCEF 100%)`. Centered. Big "Pop into the chat:" line followed by 3 white pills (Name · Role · Location). Used right before the first phase to break the ice.

### 3.5 House rules (3 cards)

Background: same light gradient. 3 numbered cards with coloured corner dots (amber, cyan, navy). Rules: **Be Vocal** · **Be Confident** · **Be Active**. Each card has a `01/02/03` numerator (small, muted).

### 3.6 Section divider and other one-idea "statement" slides

**Rewritten 2026-09-27 to the real layout, checked in both real e& decks.** The old dark-navy divider belongs to the Hungary deck, only use it if a human explicitly picks that reference. The real default: background `framed.png` (the big rounded frosted card), and everything **centered in the middle of the card**, never tucked into a corner:

- Optional emoji on top, ~85px (🚀, 🤖, ⏰, 🙌).
- Title, centered, `--text-heading`, ~57px. For a section divider, the real pattern is **"Part N"** in `--accent`, with the section name as the line below it (e.g. "Part 1" / "How AI is reshaping the Tech World").
- Optional one-line subtitle, centered, ~21px.

Same layout for every one-idea slide in the real deck: section dividers, Let's Get Started, Virtual Lab Access intro, Pop Into The Chat, Break Time ("See you in 10 Mins"), Thank You. Use a divider at the start of each real Part of the day, per the Lesson Plan's own structure.

### 3.7 Setup split (cards + URL/checklist)

Two-column grid `1fr 1fr`. Left = 2 numbered cards (`.card.top`). Right = `urlcard` (cyan-soft pill with URL + button) + readiness checklist (`✓` items). Concludes with the green "All green? Let's move forward. ✅" pill.

### 3.8 Screenshot step (VM walkthrough)

`shotwrap` = column of: framed browser mockup (`shot__bar` with 3 colored dots + `<img>`) + banner with the 1-line instruction. Used in numbered series ("Sign in · 1 of 5", "Open lab · 2 of 5", etc.). Eyebrow indicates progress.

### 3.9 Ecosystem (5-card grid)

5-column grid of `ecard` cards. Each = icon + name + 1-line description. Cyan top border (`border-top:4px solid var(--cyan)`). Used to introduce a stack ("The building blocks you'll use today").

### 3.10 Chapter rail (3-stage)

3 `chnode` cards separated by `charrow` arrows. Each node = numbered tag + icon + name + description. Use for "Basic → Intermediate → Advanced" or any 3-stage progression. The narrative banner below ties it together.

### 3.11 Pick cards (2-option chooser)

`pick` = 2-column grid of `pcard` cards. Each = function eyebrow (e.g. "Staying current") + agent name (e.g. "Yettel Market Pulse") + description + 3 cyan-soft `ptag` chips. Use for "pick your use case" decision moments.

### 3.12 Locked / pre-build steps

`lstep` rows with numbered tag + title + tagline + blurred mono code block (`filter:blur(4.5px)`). The pseudo-element absolute-positions a `LOCKED` pill in the corner. Use sparingly — a teaser of what's about to come.

### 3.13 Outbox (live result)

Green-tinted outcome row showing what arrives after the agent runs. Small uppercase "EMAIL" label + the actual subject/body excerpt.

### 3.14 Statement / handoff

```html
<div class="slide dark stmt">
  <div class="inner">
    <div class="kick">Your build guide</div>
    <div class="big">The next hour is <span class="accent">yours.</span></div>
    <div class="sub">Each of you builds your own agent.</div>
    <div style="margin-top:32px;"><a class="cta" href="…">Open the build lab →</a></div>
  </div>
</div>
```

Dark navy gradient. Centered. Kicker + big line + sub + CTA. Use as a phase handoff or session close. Always 1 CTA max.

### 3.15 Speaker notes

Every slide MUST have a `data-notes="…"` attribute. Notes are toggled by `.` key. Format: 1–3 sentences, written as direct address to the instructor (not the audience). Include cues like "Don't dwell — name them, then move." or "Keep this to ~60 seconds."

### 3.16 Concept-teaching content, the substance of the Build movement, not just its wayfinding

**Added 2026-09-16: this content now gets decided before deck generation runs, not during it.** `skills/slide-content-planning/SKILL.md` produces a slide-by-slide plan (which real §3.17 component each concept becomes, and its actual written content) as its own separate stage. Deck generation reads that plan and renders it, it doesn't make the content-and-visual-component decision in the same pass as HTML assembly anymore, that's what produced a thin, plain-text-heavy deck across three real attempts even with explicit instructions. What follows in §3.16-3.17 is still the real reference for what a good plan and a well-rendered slide look like, planning reads it too, just applied one stage earlier now.

**Found missing entirely, confirmed against a real test run (e& AI Builder Low Code, 2026-09-14).** A generated deck had 17 slides and zero actual teaching content, cover through handoff, nothing that explains a concept. Checked seven real, actually-delivered decks (e& Low Code Days 2/3/4, LVT Days 2/3/4) to see what was missing: every one runs 37-85 slides, and nearly all of that is teaching content, not framing. §3.10 (chapter rail, Basic→Intermediate→Advanced) and §3.11 (pick cards) are real and still correct, but they are wayfinding inside this content, not the content itself. A real deck's "Build" movement is dominated by a repeating block:

```
[mini-agenda: numbered topics, e.g. "#1 RAG Fundamentals · #2 Vector DBs · #3 Case Study"]
  → concept-intro (a relatable question or everyday example, no jargon yet)
  → definition slide(s)
  → comparison/tradeoff slide, where a real alternative exists
  → component-breakdown (one part of a system per slide or per bullet block)
  → worked example, often reused and progressively revealed across 2-5 slides
     (real example, e& Day 3: the same arithmetic problem shown first as plain
     few-shot, then re-shown with reasoning spelled out as Chain-of-Thought,
     then "Even fewer examples work!" as the payoff line)
  → [optional] quiz + quiz-solution pair, 4-option — **added 2026-09-18: only if `slide-content-planning` placed one here.** That stage reads the real, already-reviewed questions from `mcq-generation`'s in-session file and decides placement, per its own §3a. Deck generation renders whatever the plan says, it does not write or fetch quiz questions itself, same discipline as every other Build-movement slide per §3.16 above.
  → live demo slide (kicker "LIVE DEMO" + what it does + link out)
[repeat for the next concept]
```

**This is a menu, not a checklist.** Not every concept needs all of it, a simple concept might be one definition slide, a genuinely load-bearing one (RAG, in the real e& example) might walk concept-intro through worked-example across six or seven slides. Judge weight by how central the concept is to the day's actual build, not by mechanically running every concept through every slide type.

**Expand the Lesson Plan's topic, don't just relabel it.** The Lesson Plan's Subtopic and Flow of Examples columns correctly name the right concepts (confirmed: e& Day 1's real Subtopic column already says "Instructional / Role-Based / Few-Shot / Chain-of-Thought prompts", matching exactly what real decks teach), but naming a concept and teaching it are different jobs, the Lesson Plan is deliberately scoped as a facilitator schedule, not a content-authoring document, and shouldn't be asked to carry full slide text. That expansion, topic name to real definition, real comparison, real worked example grounded in this cohort's actual tools and audience, happens here, in deck generation, nowhere else in the pipeline does it.

**Mine Deep Research and the Lesson Plan for their actual depth, don't write a thinner summary of them.** Found on a real, measured comparison (e& AI Builder Low Code, Day 1, 2026-09-14): a generated deck averaged 104 words per slide against Nucleus's real 169, thinner on every slide, not just shorter overall, and used a real §3.17 component only 14 times across 48 slides, plain text was the default instead of the exception. This happened even though the inputs weren't thin, `deep-research.md`'s precedent notes and concrete use cases, and the Lesson Plan's Flow of Examples & Topics column, both already carry real, specific detail. The gap wasn't missing information, it was generation writing its own shorter version instead of using what was already gathered. Before writing a concept slide's text, reread that concept's actual entries in both source documents and use their real specifics, the real example, the real audience concern, the real tool quirk, don't paraphrase down to a generic one-liner. As a rough, checkable target: a Build-movement content slide should land nearer Nucleus's real 150-170 words than a bare label, and should default to a real §3.17 component (diagram, comparison, code, breakdown) over plain paragraph text whenever the content fits one, plain text should be the exception on a load-bearing concept, not the default.

**Tone and register come from the same reference deck already chosen for tokens, never a third, invented voice.** Confirmed across all seven real decks: e& (Low Code, business/ops audience) teaches plainly, everyday relatable examples ("help me write an email"), few typographic flourishes. LVT (Pro Code, engineering audience) uses a heavier kicker-label system (ALL-CAPS eyebrow + bold headline, e.g. "THE METHOD", "DEFINITION") and cites real outside research with real statistics. Both are correct for their audience, neither is the "right" default. Nucleus is the fixed default per §1 and §5, always used unless a human explicitly points this run at a different reference for a specific engagement, never inferred automatically. Pull the teaching voice from whichever reference deck is actually in use, Nucleus's own voice by default, don't default to a generic tone independent of it.

### 3.17 Nucleus's real component library, select and fill, never invent new HTML

**Why this section exists.** §3.16 described the concept-teaching pattern in prose. That wasn't enough, a real test run followed the prose correctly but had no actual markup to build from, so it fell back to the generic `card`/`banner` shapes from §3.1-3.15 (themselves extracted from Hungary, not Nucleus) for everything, comparisons, breakdowns, worked examples alike. The result read as flat and repetitive even though the words were accurate. Below is Nucleus's own real component markup, pulled directly from its file. **Pick the component that matches what you're teaching and copy its real structure, changing only the text/values inside it. Never write new slide HTML from a text description when one of these already fits.**

**Pipeline** (a sequential process, steps in order):
```html
<div class="pipeline">
  <div class="pipeline__stage">
    <div class="pipeline__label">Step 1</div>
    <div class="pipeline__title">Website</div>
    <div class="pipeline__desc">Live financial data sources</div>
  </div>
  <div class="pipeline__arrow">→</div>
  <div class="pipeline__stage">
    <div class="pipeline__label">Step 2</div>
    <div class="pipeline__title">BeautifulSoup</div>
    <div class="pipeline__desc">Scrape &amp; clean HTML</div>
  </div>
  <!-- repeat pipeline__arrow + pipeline__stage per real step, don't pad to a fixed count -->
</div>
```
Use for: a build sequence (e.g. e& Day 1's Web form → ChatGPT classification → n8n routing → Power Apps display), an architecture flow, any "this happens, then this happens" concept.

**Pitfall card** (a wrong-vs-right comparison, numbered, grid of them):
```html
<div class="pitfall-grid">
  <div class="pitfall-card">
    <div class="pitfall-card__num">#1</div>
    <div class="pitfall-card__title">Chunk Size</div>
    <div class="pitfall-card__wrong">Wrong: chunk_size=500 everywhere</div>
    <div class="pitfall-card__right">Right: 200-300 for Q&amp;A, 800-1000 for summaries. Wrong size = retrieval that looks right but misses context.</div>
  </div>
  <!-- repeat per real pitfall, only as many as are genuinely load-bearing that day -->
</div>
```
Use for: a common mistake worth naming explicitly (the RAG vs fine-tuning framing pattern found in real e& decks fits here too, off-the-shelf vs fine-tuned as a pitfall-shaped tradeoff, not just a plain list).

**Code block** (real syntax-highlighted code, paired with explanatory text, 2-column):
```html
<div style="display:grid;grid-template-columns:1fr 1fr;gap:40px;align-items:center;">
  <div>
    <div class="text-md">An embedding is a <strong>high-dimensional numerical representation</strong> of text.</div>
  </div>
  <div class="code-block" style="position:relative;">
    <button class="copy-btn" onclick="navigator.clipboard.writeText(this.parentElement.innerText.replace('Copy',''))">Copy</button>
<span class="cm"># What an embedding looks like</span>
<span class="kw">from</span> langchain_community.embeddings <span class="kw">import</span> <span class="fn">OpenAIEmbeddings</span>

embed = <span class="fn">OpenAIEmbeddings</span>()
vector = embed.<span class="fn">embed_query</span>(<span class="str">"home loan rate"</span>)
  </div>
</div>
```
Use for: any concept with a real, runnable snippet, n8n JSON config, a Python call, a prompt template. `cm`/`kw`/`fn`/`str` are the token classes, comment/keyword/function/string. Write real, correct code for this cohort's actual tools, never a fake illustrative snippet.

**Added 2026-09-24: `.copy-btn` on every `.code-block`, real and confirmed missing before this.** Small button, top-right of the block (`position:absolute;top:8px;right:8px;` in the real CSS), `navigator.clipboard.writeText(...)` on click, no new dependency. Same component, same rule, reused by `demo-generation/SKILL.md`'s no-code guides for any exact-paste text, not just code.

**Metrics** (stat callouts, 3-4 in a row):
```html
<div class="metrics">
  <div class="metric">
    <div class="metric__num">70.6%</div>
    <div class="metric__label">Attendance</div>
    <div class="metric__desc">12 of 17 invited learners joined the live session</div>
  </div>
  <!-- repeat per real metric -->
</div>
```
Use for: a real number worth landing (a benchmark, a measured improvement), not for decorative stats. Every number here must trace to something real, never invented to fill the shape.

**Query cards** (example questions/prompts to test or explore a concept):
```html
<div class="query-cards">
  <div class="query-card">
    <div class="query-card__icon">?</div>
    <div>
      <div class="query-card__label">Concept</div>
      <div class="query-card__text">"What is an index fund and how does it differ from an actively managed fund?"</div>
    </div>
  </div>
  <!-- repeat, vary the label per card (Concept / Mechanics / Definition / Comparison, etc.) -->
</div>
```
Use for: worked-example prompts a learner should actually try, grounded in this cohort's real tools and domain.

**Stack items** (icon + name + description, tech list):
```html
<div class="stack-grid">
  <div class="stack-item">
    <div class="stack-item__icon">🕸️</div>
    <div class="stack-item__name">BeautifulSoup</div>
    <div class="stack-item__desc">Web scraping &amp; HTML parsing</div>
  </div>
  <!-- repeat per real tool this cohort actually uses -->
</div>
```
This is Nucleus's richer version of §3.9's ecosystem grid, prefer this one over §3.9 when building against Nucleus (the default), reserve §3.9 for when a human has explicitly picked a different reference.

---

### 3.18 The 4-Day Mindmap (real slide 5, all-program agenda highlighting today)

Shown once, in Open. One card per day, laid out in a row: day number + day theme (bold), then "Participants complete" + a numbered list of that day's 3-4 real subtopics, pulled from the Lesson Plan, not invented. **Today's card is visually distinct**: light purple fill (`#E7E8FF`) with a colored border (`#6568F5`), every other day's card is plain white. Above the row, three grouping labels span the relevant cards with a bracket mark: "Covered in last two sessions" (over the already-completed days), "Today's Agenda" (over today's card specifically), "Upcoming" (over the days still ahead). Day 1 gets no "covered" label since nothing precedes it; the last day gets no "upcoming" label since nothing follows it, adjust the grouping to whichever day this actually is, don't always show all three.

### 3.19 Today's Agenda (real slides 10/24/54, shown repeatedly, not once)

A simple numbered list of today's own topic blocks, e.g. "#1 Conventional Bots & RAG Recap, #2 CoT Prompting & ReAct, #3 Agents", under an "Agenda For Today" heading. **Confirmed real pattern: this exact slide is shown again at the start of each new topic block**, not just once at the start of the day, three times across an 85-slide real deck. Re-show it as Build's own internal wayfinding, learners always know which of today's numbered topics they're currently in.

### 3.20 Demo (real slide 81, a plain link-out, not an embedded walkthrough)

**Rebuilt 2026-09-27 from the real e& Low-Code Day 1 demo slide (page 73), the richer real version, not the bare one.** The old spec here was only "title + Link for Demo + button." The real Day 1 demo slide also carries the demo's context on the left, so learners know what they're about to build and what "done" looks like, before they click out.

Background: `demo-laptop.png` (§2.0 table), always.

**Corrected 2026-09-27: every element on this slide is absolutely positioned in canvas pixels, never a grid or flex column layout.** The laptop is drawn into the background image, so it sits at one fixed spot. Measured directly from `demo-laptop.png`: **the laptop's dark frame runs from x 641 to 1189, y 288 to 663** on the 1280×720 canvas, screen center at about x 915, y 475. A real generated deck laid this slide out as two flexible columns, so its "Link for..." text landed above the screen and its button on the laptop's top edge, and its bullets ran under the laptop. Pin everything:

```css
.slide.demo .demo-title { position:absolute; left:48px;  top:34px;  width:1100px; }
.slide.demo .demo-left  { position:absolute; left:48px;  top:120px; width:560px; }  /* must end before x 620, the laptop starts at 641 */
.slide.demo .demo-link  { position:absolute; left:693px; top:385px; width:445px; text-align:center; }
.slide.demo .demo-btn   { position:absolute; left:778px; top:504px; width:278px; height:71px; }
```

Everything in the left column (description and both bullet lists) wraps inside its 560px, nothing crosses x 620. If the content doesn't fit, shorten it, don't widen the column or shrink text below the real type scale.

**The left column also has to end above y 630**, the bottom of the canvas is where the nav bar sits. The real slide fits because its bullets are short labels, 2 to 5 words each ("Zero-Latency Engagement", "Hyper-Personalization via AI", "Workflow triggers when new data enters the Google Sheet"), and its description is one or two sentences. A real generated deck wrote full-sentence bullets and ran off the bottom of the slide. Write short labels here, the full detail already lives in the demo guide this slide links to.

Layout on the 1280×720 canvas, real positions from the reference files:

- **Title, top-left** (~left 30px, top 35px): `Live Demo: [real demo name]`, `--text-heading`, ~25px, `--font-display`.
- **Left column** (~left 30px, top ~160px, width ~560px), `--text-body`, ~16px, three real blocks in this order:
  1. One or two sentences: what this demo builds and why.
  2. **Key Objectives:** 3-4 short bullets.
  3. **Technical Success Criteria:** 3 short, checkable bullets (what has to actually work for the build to count as done).
- **On the laptop screen, right side** (text box ~left 695px, top 385px, width 445px, centered): `Link for Hands-on workshop` (or `Link for Demo`), white, ~43px.
- **Button below it** (~left 778px, top 504px, 278×71px): white pill, radius ~36px, text `Click Here` in `--link-accent` (`#0097A7`), ~25px, links out to the real demo guide from `demo-generation`.

All the left-column content comes from that demo's own real guide (its "What We're Solving" section and its steps' real success conditions, per `demo-generation/SKILL.md`), never invented here. This slide still doesn't contain the demo steps themselves, it's a handoff point with context, the real walkthrough stays in the separate guide.

**Added 2026-10-01: the demo link goes in two places, each with its own job.** The "Click Here" button on the laptop links to the **learner's** demo guide, the version learners follow. The slide's speaker notes (`data-notes`) carry that same link again, plus the **SME's own direct link** to the instructor version (solution, answer key, a notebook with outputs) so the instructor can open it without hunting. The instructor version never goes on the visible slide. If the demo guide hasn't been built yet, don't ship a button that leads nowhere: say so in the notes and flag it in the run's summary as outstanding, `deck-review/SKILL.md` §1.1 checks both the button and the notes.

### 3.21 Quiz (real slides 82-84, question then reveal)

Two-part pattern: an intro slide ("🙋‍♀️ Quiz Time! Answer in the Chat Box"), then the actual question slide, the question stem, 4 options, the correct one marked (✅) once revealed, the rest left plain or marked wrong (⬜️). Pulls its real, already-reviewed content from `mcq-generation`'s in-session file, per `slide-content-planning/SKILL.md` §3a, never authored fresh here.

### 3.22 Thank You (real slide 85, the actual close)

Simple: "Thank You!" plus a celebratory emoji, centered. The real reference has no dark statement/CTA slide, don't force one in unless a specific engagement actually calls for a distinct closing message beyond this.

### 3.23 Timing table (real e& Day 1 slide 5, checked directly)

**Added 2026-09-27**, this slide had no spec before, and a real generated deck's version lost all its table styling during an update. The real one, on `content-blush.png`:

- Title top-left, "Tentative Schedule" (or "Today's Timing"), `--text-heading`, ~30px.
- A full-width white table, thin row borders `#FAD8D2`, every cell centered, ~20px text.
- Header row filled `#E3BEB4` (dusty rose), bold dark text.
- **Columns: the client's own local time first, then IST, then Activity** (real: "Time (Dubai Time) | Time (IST) | Activity"). Acceler runs across time zones, the room needs its own clock, the instructor needs IST. Take the client's time zone from the Facts Sheet, if it isn't stated, ask rather than guess. Only drop to one time column when client and instructor are in the same zone.
- Rows stay at session level, not pod level ("Session 1", "Break", "Lunch Break + Prayer Break", "Session 4 (Final Session)"), the detailed breakdown already lives in the Lesson Plan.

---

## 3a. Updating an existing deck to a newer template, check nothing got deleted

**Added 2026-09-27, real pattern found across several update rounds on one deck in a single day.** Each round edited some style rules and quietly deleted neighboring ones by accident: once the copy button's style lost its selector (so the buttons showed unstyled), later the timing table lost its header and row styles (so it showed as bare text). Nothing flagged either one, both were only caught by looking at the rendered slide.

So whenever an existing deck is updated to a newer template (§6b "tweak" mode in `content-generation/SKILL.md`):
1. Before editing, save the list of every CSS selector in the file.
2. After editing, list them again and compare. Every selector that disappeared must be one the update meant to remove, anything else is a bug, restore it.
3. Render and look at one slide of every slide type (cover, instructor, timing, mindmap, agenda, statement/divider, each content component, demo, quiz, thank you), not just the slides the update was aimed at. A lost style shows up on a slide nobody meant to touch.

---

## 4. The JS controller (40 lines, copy verbatim)

```javascript
const slides=[...document.querySelectorAll('.slide')], total=slides.length;
let cur=0; const $=id=>document.getElementById(id);
const speaker=slides.map(s=>s.dataset.notes||''); let notesOn=false, ctO;
function updNotes(){ $('notesC').textContent=speaker[cur]||''; }
function progress(){
  $('progress').style.width=((cur+1)/total*100)+'%';
  const c=$('counter'); c.textContent=(cur+1)+' / '+total;
  c.classList.add('on'); clearTimeout(ctO); ctO=setTimeout(()=>c.classList.remove('on'),2000);
  updNotes(); syncNav();
}
function goTo(n){ if(n<0||n>=total) return; slides[cur].classList.remove('active'); cur=n; slides[cur].classList.add('active'); progress(); }
document.addEventListener('keydown',e=>{
  if(e.key==='ArrowRight'||e.key==='ArrowDown'||e.key===' '){e.preventDefault();goTo(cur+1);}
  else if(e.key==='ArrowLeft'||e.key==='ArrowUp'){e.preventDefault();goTo(cur-1);}
  else if(e.key==='Home')goTo(0); else if(e.key==='End')goTo(total-1);
  else if(e.key==='.'){ notesOn=!notesOn; $('notes').classList.toggle('on',notesOn); }
  else if(e.key==='f'||e.key==='F'){ /* fullscreen toggle */ }
});
// fixed 1280x720 canvas, scaled to fit the window (per §2.3), required, not optional
function fit(){ document.documentElement.style.setProperty('--fit', Math.min(innerWidth/1280, innerHeight/720)); }
addEventListener('resize',fit); fit();
// deep link, required: ?s=N or #N opens slide N (1-based), used for review and for sharing one slide
{ const q=new URLSearchParams(location.search).get('s')||location.hash.slice(1);
  const n=parseInt(q,10); if(n>=1&&n<=total) goTo(n-1); }
// click zones + bottom nav + touch swipe
```

**Added 2026-09-27:** the `fit()` and deep-link lines above were missing from a real generated deck. Without `fit()` the slides stretch and misalign (§2.3). Without the deep link, nobody can jump straight to "slide 24" to review or share it, a real review of a generated deck needed exactly that.

Features (all free with the same script):
- **Keyboard:** `←/→/Space` navigate · `Home/End` jump · `.` toggle notes · `F` fullscreen.
- **Touch:** swipe left/right (50px threshold).
- **Click zones:** left 11% / right 11% of the viewport.
- **Bottom nav:** dot indicator + prev/next + count, auto-scrolls active dot into view.
- **Progress bar:** cyan, top-of-screen, animates on slide change.
- **Counter:** appears **bottom-left** for 2s after each change, then fades (not top-right, that's the logo's spot, per §2.3).
- **Deep link:** `?s=N` or `#N` opens to slide N (1-based) on page load.
- **Fit to window:** the fixed 1280×720 canvas scales to fit any screen, background and content stay locked together (§2.3).
- **Speaker notes:** absolute-positioned tray that slides up from bottom; toggled with `.`.

---

## 5. Per-client retargeting (the generalisation)

**Corrected 2026-09-24, real stale reference found:** this section used to say to copy `acceler-nucleus-session-deck/index.html`, the deck §1/§2.0 stopped using as the default reference back on 2026-09-18. That correction updated the design spec (palette, fonts, components) but never this actual build workflow, so old content kept leaking through even after the reference changed. Fixed here.

To produce a deck for a new client:

1. **Copy** the current default reference deck's structure from `post-sales/knowledge/deck-reference/eand-lowcode-day3-replica/` (the real e& Low-Code Day 3 deck, per §1/§2.0) to a new folder named `acceler-[client]-session-deck/`. Only fall back to a different reference (Nucleus, Hungary) when a human explicitly points this run at one of them instead, per §2.0's own note.
2. **Swap framing tokens** in slide 0 (cover): client name in badge, partner logo if applicable (see below), big-line phrasing, chips (tools · output · duration). Logo comes from `post-sales/knowledge/brand-assets/`, Acceler by default, PowerUp only if a human explicitly asks, per `content-generation/SKILL.md` §1d, never guessed or left as a placeholder.
3. **Swap instructor** in slide 1: photo path, name, role line, specializations, experience tiles (feature the most relevant brand for the audience — telco uses Airtel, banking uses HDFC, etc.), `bsec` content.
4. **Swap content** in build slides (use cases, screenshots, ecosystem). Keep slide IDs (data-step ordering) stable so the PPTX twin can re-use the same data block.
5. **Update CTAs** to point at the next-deck folder (`pick-your-use-case/index.html`).
6. **Update speaker notes** to client context.
7. **Validate**: open in browser, walk every slide via keyboard, check screenshots load, check notes appear, check print preview.

The 4 movements stay the same. Tokens, screenshots, copy change.

---

## 6. Content data block (sync with PPTX twin)

To keep the HTML and the PPTX in sync, the content data lives in a single top-of-file object that both decks consume:

```javascript
const DECK = {
  client: "e& PPF Hungary",
  partner: "CETIN × Yettel",
  programme: "Build your AI team",
  duration: "2.5 hours",
  output: "1 agent · built end-to-end",
  instructor: {
    name: "Madan Rawtani",
    role: "AI + Growth + Engineering · ex-Bharti Airtel · Growth @ Acceler",
    photo: "assets/slide02_2.png",
    specializations: ["AI-Powered Growth Systems", "Multi-Agent Systems & Agentic Workflows", "…"],
    featuredExperience: { brand: "Airtel", color: "#E40000", caption: "Telecom" },
    experience: [
      { brand: "Paytm",         color: "#0B2A6B" },
      { brand: "OYO",           color: "#EE2E24" },
      { brand: "PhysicsWallah", color: "#6C4FB6" },
      { brand: "Housing.com",   color: "#EF1C74" }
    ],
    blocks: [
      { heading: "Career Highlights", items: ["…"] },
      { heading: "📡 Airtel — Telecom", items: ["…"] },
      …
    ]
  },
  useCases: [
    { fn: "Staying current", agent: "Yettel Market Pulse", desc: "…", tags: ["🌐 Live web","📧 Work IQ","🕘 9 AM daily"] },
    { fn: "Walking in cold", agent: "Yettel Meeting Prep", desc: "…", tags: ["🗓️ Calendar","📧 Email + Teams","🕘 9 AM daily"] }
  ],
  ecosystem: [
    { i: "🛠️", n: "Copilot Studio", d: "Where you build, test & publish your agent." },
    …
  ]
};
```

The PPTX builder (`PPTX_Deck_Skills.md`) consumes the same `DECK` object, so when you update the HTML, the PPTX rebuild gets the same data automatically.

---

## 7. Accessibility & polish

- **Contrast:** all text passes WCAG AA on cream bg. Dark slides use white at `≥0.7` opacity for non-headings.
- **Touch targets:** click zones are 11% of viewport on each side. Bottom nav buttons are 30×30px (above the 24px AA minimum).
- **Keyboard-only:** every interaction has a key binding. No mouse-only paths.
- **Screen readers:** `<img alt>` on every screenshot. Logos have `alt` set. `aria-label` on prev/next buttons.
- **Reduced motion:** the `.slide.active` opacity transition is `.4s ease` — gentle enough for vestibular sensitivity. Skip the SVG animations (`.anim-svg`) on `prefers-reduced-motion`.

---

## 8. Worked example — e& PPF Hungary (historical, predates the 2026-09-18 reference change)

**Kept as a real historical example of the retargeting mechanics, not as the current reference.** This deck was built by copying the old default (Nucleus), before §1/§2.0 switched the default to the e& Low-Code Day 3 deck. The 4-movement arc and swap mechanics below are still accurate, but don't copy this deck's own palette, fonts, or component choices, those follow §2.0 now, not this example's.

**File:** `e&-pp-hungary-session-deck/index.html`  
**Format:** 17 slides, 2.5 hours.  
**Arc:** Cover · Madan intro · How 2.5 hrs run · Pop into chat · Optimise · **Phase 1 Set up** · VM access split · 5 VM steps · Browser tabs · Find Copilot Studio · Copilot ecosystem · Basic→Intermediate→Advanced · **Phase 2 Build** · Pick use case · Handoff to lab.  
**Output:** Each participant builds their own Copilot agent (Yettel Market Pulse OR Yettel Meeting Prep) end-to-end on their VM.  
**PPTX twin:** Built by copying the 46-slide reference deck and rebuilding only Days 2–4 content per `PPTX_Deck_Skills.md` §5 (reorder via `sldIdLst` element refs). Output: `Acceler_Solution_Deck-e&-PP-Hungary.pptx`.

**No worked example against the current e& Low-Code Day 3 default exists yet**, since no session deck has actually been built against it in a real engagement so far. Add one here once the first real client deck is built against §2.0's palette, rather than leaving this Hungary/Nucleus-era example as the only illustration.

---

## 9. Build script pattern

Recommended file layout:

```
[client]-session-deck/
├── index.html              ← the deck (this skill)
├── pick-your-use-case/
│   └── index.html          ← per-use-case lab (linked from handoff CTA)
├── assets/
│   ├── slide02_2.png       ← instructor photo
│   ├── slide07_1.png       ← VM login screenshot
│   ├── slide08_1.png       ← My Labs screenshot
│   └── …
└── acceler_logo.png        ← brand mark, copied from post-sales/knowledge/brand-assets/ (Acceler by default, PowerUp only on explicit request, per content-generation/SKILL.md §1d)
```

Open `index.html` in any modern browser. No build step. No npm. No bundle. Hosts on any static server (S3, GitHub Pages, Vercel static, Cloudfront).

---

## 10. Quality checklist

- [ ] Cover has client + partner branding · badge · 1 h1 with cyan-accent phrase · 3 chips · keyboard hint footer
- [ ] Instructor intro: photo · 4–5 specializations · featured experience tile (`--feat`) · 3–4 `bsec` blocks · name + role + eyebrow
- [ ] Every slide has `data-notes` (1–3 sentences, instructor-facing)
- [ ] Phase divider before each major arc transition (Set up → Build → Close)
- [ ] All 3 pipeline arrows are `→` (`parrow` class)
- [ ] Setup screenshots all use `shot` frame with browser-bar dots
- [ ] Ecosystem grid is exactly 5 cards (rebuild as `c3` if 3 or `c4` if 4)
- [ ] Chapter rail has 3 nodes with numbered tags
- [ ] Pick cards have 3 `ptag` chips each (max)
- [ ] Statement / handoff slides use `.dark.stmt` class with kicker + big + sub + 1 CTA
- [ ] Print stylesheet hides progress, brand, fs, counter, zone, notes, nav
- [ ] Deep link `?s=N` opens correct slide
- [ ] Keyboard nav works: arrows · space · home/end · `.` notes · `F` fullscreen
- [ ] Touch swipe works (50px threshold)
- [ ] Deck plays through end-to-end without errors in console
- [ ] Every Lesson Plan concept that's load-bearing for the day's build is actually taught per §3.16 (definition, comparison, or worked example as weight warrants), not just named as a pattern-stage label
- [ ] Teaching content uses §3.17's real components (pipeline, pitfall-card, code-block, metrics, query-cards, stack-items), grounded in the current e& Low-Code Day 3 default reference (`post-sales/knowledge/deck-reference/eand-lowcode-day3-replica/`), not the generic `card`/`banner` shapes reused for everything
- [ ] Concept-slide word density checked against the ~150-170 words/slide benchmark. **Still open, per `slide-content-planning/SKILL.md` §3:** this number was measured against the old Nucleus reference and never re-verified against the current e& Low-Code Day 3 default, use it as a working estimate, not a confirmed number, until that re-measurement actually happens
- [ ] Palette and fonts actually match §2.0 (e& Low-Code Day 3, the default), not §2.1a (Nucleus) or §2.1-2.2 (Hungary), unless a human explicitly chose one of those as the reference this run
- [ ] Every slide uses its matching real background image from §2.0's table (cover / framed / content-lavender / content-blush / demo-laptop), copied into the deck's own `assets/`, never a flat color fill
- [ ] Demo slides follow §3.20's real layout: title, left-column context (description, Key Objectives, Technical Success Criteria), link + teal "Click Here" on the laptop screen
- [ ] Every slide is a fixed 1280×720 canvas scaled to fit with `fit()`, background on the whole canvas, no plain strips at the sides, checked at two different window sizes, not just one
- [ ] Text uses the real type scale in §2.3 (body 19-21px, titles 25-36px), not shrunk, nothing under 17px except footnotes
- [ ] `content-lavender.png` set as the base background on `.slide` itself, every ordinary slide actually shows it
- [ ] Demo slide elements absolutely positioned to the measured laptop (§3.20), "Link for..." and the button sit on the laptop screen, nothing in the left column crosses x 620 or runs below y 630, bullets are short labels not sentences
- [ ] No slide-type class shares a name with an inner element's class, nothing on `.slide` has a `max-width`
- [ ] Timing table matches §3.23: rose header row, client local time + IST + Activity, centered cells
- [ ] If this was an update to an existing deck: CSS selector list compared before and after, nothing lost by accident, one slide of every type rendered and looked at (§3a)
- [ ] Cover is the real left-aligned layout (§3.1), section dividers and statement slides are centered on the framed card (§3.6)
- [ ] Slide counter sits bottom-left, never over the logo; `?s=N` / `#N` deep link actually works
- [ ] PPTX twin (`PPTX_Deck_Skills.md`) consumes the same `DECK` data object

---

## 11. File naming convention

```
[Client]-session-deck/index.html
eand-lowcode-day3-replica/                      ← current default reference, see post-sales/knowledge/deck-reference/
acceler-nucleus-session-deck/index.html         ← Apr 2026, superseded 2026-09-18, historical
e&-pp-hungary-session-deck/index.html           ← Jun 2026, historical
bosch-india-masterclass-deck/index.html
csod-copilot-agent-masterclass-deck/index.html
edelweiss-day-1-deck/index.html
```

Per-day decks for multi-day programs go in subfolders: `edelweiss-day-1-deck/index.html`, `edelweiss-day-2-deck/index.html`, etc.

---

*Built from: e& Low-Code Day 3 deck (current default reference, Sep 2026, `post-sales/knowledge/deck-reference/eand-lowcode-day3-replica/`); e& PPF Hungary live session deck (`Build your AI team`, Jun 2026, historical); acceler-nucleus-session-deck reference (Apr 2026, historical). Pairs with [[pptx-deck-skills]] (the PPTX twin), [[doc-proposal-skills]] (the proposal that anchors the deck), [[pricing-skills]] (the commercials behind the deck), [[session-mapping-skills]] (the requirement coverage that justifies the deck).*
