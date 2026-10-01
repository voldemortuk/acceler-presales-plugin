---
name: hands-on-guide-generation-skills-acceler-tool-access-setup
description: "Generates one standalone tool-access/setup guide per engagement, the leave-behind doc a learner uses to get VM and AI tool access working, distinct from the deck's in-deck setup movement and from the demo notebook. Cross-checks its own tool list against that day's demo before handoff, and never bakes in a real credential value. Routes to the existing hands-on-guide-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Hands-On Guide Generation · Post-Sales · SKILL.md (v1)

**How this skill is started (added 2026-10-01):** every run begins with the start-up check in `content-generation/SKILL.md` §6c (name the mode per §6b, check what already exists for this client, ask at most 3 to 5 questions, write a light brief in adapt or standalone runs). Where this file says an input is mandatory or says to stop when one is missing, that describes a fresh build inside the full pipeline; in adapt or standalone runs the gap is handled the §6c way instead. Before building, also read `agent-loops/SKILL.md` (§2a, §2a-1, §2b) and this artifact type's `generation-learnings/` file where one exists, they are not loaded automatically.

**What this produces.** One standalone setup guide, the same artifact `hands-on-guide-review/SKILL.md` already reviews, a leave-behind reference a learner returns to independently, distinct from both the session deck's own setup movement and the demo notebook itself.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved. The Libraries/Tools column, across all days, is the required-tools list this guide has to cover completely.
- `Outputs/[Client]/deep-research.md`, for whether this client uses personal-account or pooled/shared-training-account access, corporate network constraints included. **Added 2026-09-18, per `content-generation/SKILL.md` §2: this account-scheme signal is the single most load-bearing granular fact here.** §2's own rule already says don't default to one, but if deep research doesn't actually state it clearly, don't stop, flag it plainly and state which scheme this run assumed rather than presenting a guess as the client's real requirement.

**Best-effort:**
- `Outputs/[Client]/demo/`, if already generated, for the tool-alignment cross-check.
- An existing guide as a formatting and branding reference.

---

## 2. Rules, reused from hands-on-guide-review, not reinvented

- Every tool named as required for any day gets a complete step-by-step section, tool name, a one-line description of what it's for, numbered and concrete steps (click X, go to Y, never "set up your account"), and the access URL.
- A support channel is included for when setup fails live, a guide with no escalation path is not acceptable.
- **No real credential value, ever.** Reference credentials as shared through a separate secure channel, or show an obvious placeholder (e.g. `windowsjuneaiuserXX` / `XXXXXXXX`), never a real password, API key, or token in the text. If a screenshot is included, it must not visibly contain a real credential either.
- Personal-account and pooled-account schemes are both legitimate, use whichever deep research indicates this client needs, don't default to one.
- Where example prompts are included per tool, they're realistic for that tool and this audience's actual use case, not generic.
- Client and Acceler branding present, a sign-off line included, matching house format, not a generic unbranded doc.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3: cross-check the tool list against `Outputs/[Client]/demo/`'s actual tools where it already exists, a guide listing a tool the session never touches, or missing one the demo depends on, is a defect to catch here, not leave for review. Also scan the full text and any referenced screenshots for anything credential-shaped before saving, this is the one check that must never be skipped regardless of how the guide otherwise looks.

---

## 4. Where it gets saved

`Outputs/[Client]/hands-on-guide.docx`, same per-engagement folder as everything else.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:hands-on-guide-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/hands-on-guide.md` and `agent-loops` by reading it at the start of the run (skills have no `skills:` frontmatter field, only agents do). No candidates are seeded there yet. Per §6a, once real fix-loop history accumulates and the promotion rule is met, this skill is expected to actually change how it writes the next guide, not just fix the one in front of you.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the gap handled per `content-generation/SKILL.md` §6c
- [ ] Every required tool across every day has a complete step-by-step section
- [ ] No real credential value anywhere in text or screenshots, checked explicitly, not assumed clean
- [ ] Account scheme (personal vs. pooled) matches what deep research indicates, not defaulted
- [ ] Tool list cross-checked against the generated demo where it exists
- [ ] Branding and sign-off present, matching house format
- [ ] Saved to `Outputs/[Client]/hands-on-guide.docx`
- [ ] Handed to the existing `hands-on-guide-reviewer`, no bespoke review invented
