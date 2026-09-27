---
name: post-sales-engagement-catalog
description: "Plain list of past post-sales engagements and what real content exists for each (lesson plan, demo, assignment, assessment, orientation deck, closing ceremony deck, hands-on/VM setup, project). Same shape and purpose as pre-sales knowledge/INDEX.md, pointed at delivered content instead of proposals. A human reads this and picks a precedent, generation skills do not search it automatically yet, see §Status."
metadata:
  type: reference
---

# Post-Sales Engagement Catalog (v1)

**What this is.** A plain, tagged list of real past engagements and the real content that already exists for each one, so a new engagement with 50-70% overlap doesn't start from a blank page. Same idea as pre-sales `knowledge/INDEX.md`, pointed at delivered content (lesson plans, demos, assessments, decks) instead of proposals.

**Status: manual lookup only.** No generation skill queries this automatically yet. When a new engagement starts, a human reads this list, picks the closest precedent, and tells the relevant generation skill ("use LVT's demo as the reference") explicitly. See [[content-generation]] for how generation skills consume a named precedent once pointed at one. An automatic matcher (same shape as `acceler-presales:similar`) is a deliberate later step, not built now, once engagement volume makes eyeballing this list slow.

**Source of the underlying files.** Everything below is real client-delivered content, not templates, shared by Utkarsh (Sep 2026). It lives outside this repo, at `postsales skills/B2B AI Programs/` in the shared Drive folder, because the raw files are large binaries (decks, videos) that don't belong in git. This catalog is the lightweight, committed pointer into that folder, the folder itself is the actual library.

**Read discipline.** "Confirmed" below means a file was actually opened and its structure checked against the matching reviewer's `SKILL.md`. Everything else is listed from the folder/file names only, not yet opened, per the same discipline used for every other skill in this repo, real files get read before being relied on, not assumed from a name.

---

## e& (multiple programs, several cohorts, richest precedent set)

**Audience:** ranges from pure engineers (Pro Code, Tech Teams) to non-technical business/leadership (AI for Leaders, AI Enabler). Calibrate per cohort, don't treat "e&" as one audience.

- **E& Pilot AI Programs / AI Builder Program for Tech Teams** (earliest e& cohort, mid-2025)
  - Content available: Lesson Plan (xlsx), Demo Notebooks (Day 1-3, Python/RAG/Agents), MCQ set (Day 3), Pre/Post Course Assessments, VM & Tools Setup, Course Impact Report
  - **Confirmed:** Lesson Plan (`Lesson Plans for Tech _ e&.xlsx`) and the Day 3 MCQ set both opened and checked, match `lesson-plan-review` and `mcq-review` structure respectively
  - Source: `E& Pilot AI Programs/E& _ AI Builder Program for Tech Teams/`

- **E& Pilot AI Programs / AI Enabler Program for Business Team** (same cohort era, non-technical)
  - Content available: Day-by-day demo files (Perplexity, Gamma, Relevance AI), problem-statement docs, post-class exercises
  - Source: `E& Pilot AI Programs/E& _ AI Enabler Program for Business Team/`

- **e& AI for Leaders** (multiple versions V1-V4, Jan-Feb 2026)
  - Content available: Orientation deck, 4-day live slides, Closing Ceremony deck + post-class test, VM/Tools Setup, AI Canvas / Blueprint worksheets
  - Source: `B2B e& AI for Leaders/`

- **e& AI Builder, No Code / Low Code / Pro Code variants** (Nov-Dec 2025 and Q1 2026 cohorts)
  - Content available: Day 1-4 live slides, coding/no-code demos, capstone assignment, per-learner post-program assessment gradesheets, VM setup, Orientation, Closing Ceremony
  - **Confirmed 2026-09-24, Low Code specifically:** both Orientation and Closing decks opened directly and read in full, real precedent for `orientation-generation`/`closing-ceremony-generation`'s three-tier copy-and-swap approach. Real, confirmed pre-test shape (embedded in Orientation): 10 single-correct + 2 subjective, harder/diagnostic. Real, confirmed post-test shape (embedded in Closing): 10 single-correct + 2 subjective, easier/foundational. Both PowerUp-branded in the source file, swap to Acceler per `content-generation/SKILL.md` §1d unless a human asks to keep PowerUp.
    - Orientation: `PowerUp | AI Builder Program (Low Code) for e& - Orientation`, https://docs.google.com/presentation/d/1fJtl_u745stcoGtIrR60JdgDAW3I_kkgbP_AT9tct3Y/edit
    - Closing: `Closing notes | e& - AI Builder Accelerator (Low Code)`, https://docs.google.com/presentation/d/1gDgg4wpGunamAaKoPKeO44k9LNdrpI-ci4ecLu3ibOg/edit
  - **Confirmed 2026-09-25, Day 1's real live-class deck, saved permanently at `post-sales/knowledge/deck-reference/eand-lowcode-day1-real/`** (104 real slides, read in full, not the Curriculum Graph, which doesn't have this file, see that folder's own README for why). Real 4-part structure: (1) "How AI is reshaping the Tech World" (Codex, Karpathy's Software 1.0/2.0/3.0, Map of GitHub, HF Model Atlas), (2) Prompt Engineering taught through **Expertex**, not generic ChatGPT, includes the real "Maya" persona demo, (3) the n8n build (Intelligent Client Inquiry Response System), (4) Responsible AI (hallucination, prompt injection incl. a real GitHub MCP vulnerability, privacy/PII). Real instructor shown: Anshaj Khare. **e&-TESTRUN's own Day 1 (built before this was found) only covers parts 2 (differently, via generic ChatGPT) and 3, parts 1 and 4's real depth are a confirmed gap, not yet reconciled**, see that folder's README for the fuller comparison.
  - Source: `B2B e& AI Builder (No Code|Low Code|Pro Code) - Nov-Dec_25/` and `- Q1_26/`

- **e& PPF AI For Leaders, Hungary (CETIN x Yettel)**, this is Ut's own worked example in `live-session-deck/SKILL.md`
  - Content available: Orientation, Closing Ceremony, Virtual Labs Guide, real use-case participant guides (HR benefits, Sales/CCO scenarios)
  - Source: `e& PPF AI For Leaders/Hungary/`, also `Yettel Serbia/` for the Virtual Labs Setup doc

## LVT (B2B-LVT Pro Code Program, Claude-based, engineering-heavy)

Already the primary reference for `onboarding-form-review` and `live-session-deck`. Deepest, most structured precedent for a pure-engineer cohort.

- Content available: Onboarding, Orientation, Day 1-4 live slides, a real Final Project ("Stale Order Alerts", milestone-graded with hooks/spec-driven-dev/MCP code folders), Closing Ceremony, Pre/Post Program Assessment (xlsx with responses), Virtual Labs Setup, per-day real code demo repos (git history, package.json, tests)
- **Not yet opened**, but this is the strongest candidate to validate `project-generation` and `demo-generation` against next, since it has real graded code, not just slides
- Source: `B2B-LVT Pro Code Program - Claude/`

## Deloitte AI For Leaders

- Content available: Proposal + RFP + Sample Proposal Template, Activity Guidebook, Orientation deck, VM/Tools Setup, Masterclass deck (current + archived versions)
- Source: `B2B Deloitte AI For Leaders/`

## Bosch Masterclass

- Content available: Pre/Post Class Assessments, Orientation, Closing Ceremony, one Masterclass deck (MCP/A2A topic)
- Source: `B2B Bosch Masterclass/`

## Nucleus Masterclass

- Also Ut's default reference client for `live-session-deck`, worth noting the deck default and this content catalog point at the same client.
- Content available: Pre/Post Class Assessments, Orientation, Closing Ceremony, two Masterclass decks
- Source: `B2B Nucleus Masterclass/`

## Cornerstone (two distinct programs, don't conflate)

- **Cornerstone AI For PMs Program**: Proposal & Lesson Plan, Live Class Content (Sessions 1-5), Pre/Post Class Assessment, VM Setup
  - Source: `B2B Cornerstone_s AI For PMs Program/`
- **Cornerstone Sales Team**: Class Content/Slide Deck + Demo Guide, Orientation, Closing Ceremony
  - Source: `B2B Cornerstone Sales Team/`

## Lowe's AI Program

- Content available: Proposal & Lesson Plan, Pre/Post Class Assessment, one large Final Slide Deck, Pre-Requisites Setup
- Source: `B2B Lowe_s AI Program/`

## ETS Leadership Program

- Content available: Orientation, Curriculum & Lesson Plans, Live Class Content (Sessions 2-5), Pre/Post Program Test, Login Guide
- Source: `B2B ETS Leadership Program/`

## B2C Agentic AI / GenAI resource library (not a client engagement, a content pool)

Not a real client, this is Interview Kickstart's own B2C curriculum library (Agentic AI Program, SWE and domain modules). Useful as a *content-shape* precedent, real Lesson Plans and Assignments with solutions, even where the audience/pricing logic doesn't apply. `deep-research/SKILL.md` §1 already allows B2C precedent explicitly for this reason.

- Source: `Agentic AI & GenAI Resources ( B2C Programs )/`

---

## How to use this today

1. New engagement closes, you already know roughly who it's most like (per the discussion with the user, this is usually obvious without a matcher).
2. Open this file, find the closest client above, note what content already exists for them.
3. When running `deep-research` or any `*-generation` skill for the new client, tell it explicitly, e.g. "follow LVT's project shape" or "use e&'s Tech Teams lesson plan as the structural reference."
4. The generation skill treats that as a best-effort precedent input (per `content-generation/SKILL.md` §2), never copies verbatim, the new engagement's own Facts Sheet and onboarding answers still govern the actual content.

## Keeping this current

Add a new entry here whenever a new engagement's content gets delivered and shared for reference, same trigger as when a new client folder shows up in the shared Drive folder. This file is not auto-synced (unlike pre-sales `knowledge/INDEX.md`, which pulls from Drive every 12h), it's updated by hand for now since post-sales content volume is still small enough for that to be low-effort.
