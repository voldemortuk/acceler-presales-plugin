---
name: demo-generation-skills-acceler-hands-on-build
description: "Generates one in-class hands-on build artifact per session, a notebook, a code file, or a no-code/low-code build guide (Copilot Studio, Figma Make), matching whichever this day's lesson plan row actually calls for. Runs its own code before handoff to capture real evidence, not claimed evidence. Routes to the existing code-demo-reviewer for the actual review pass."
metadata:
  type: reference
---

# Acceler Demo Generation · Post-Sales · SKILL.md (v1)

**What this produces, and in which of three shapes.** One in-class, instructor-led build artifact, the same thing `code-demo-review/SKILL.md` already reviews: a Jupyter notebook, a standalone code file, or a no-code/low-code build guide (a step-by-step doc for building something in Copilot Studio, Figma Make, or similar, a confirmed real content type, not a lesser substitute for code). Which of the three depends on that day's Lesson Plan row, its Libraries/Tools column tells you which shape applies, don't default to notebook when the row calls for a no-code build.

---

## 1. Inputs

**Mandatory:**
- `Outputs/[Client]/lesson-plan.xlsx`, approved. The relevant day's Topic, Learning Objective, and Live Demo/Coding Demo columns drive what gets built, and its Libraries/Tools column decides the shape (notebook/code vs. no-code guide). **Added 2026-09-18, per `content-generation/SKILL.md` §2: the single most load-bearing granular fact is whether this column actually names a specific tool**, not a vague category like "automation platform." That's what §2's shape decision depends on entirely, defaulting to notebook when the row is vague is exactly the mistake §1's own header warns against. If it's genuinely vague, don't stop, but flag it plainly and state which shape this run assumed and why.
- `Outputs/[Client]/deep-research.md`, for the client's own use cases and audience technical level.

**Best-effort:**
- An existing notebook, code file, or build guide as a structural reference, where deep research points to a similar precedent.

**Mandatory when the demo's result is visual, not just code** — real evidence (an actual e& Low-Code demo doc) confirms this applies broadly, not just to no-code guides, a before/after chat comparison in a low-code demo needed screenshots just as much as a Power Automate click-path did:
- **Real screenshots of the actual demo being run**, provided by the person who ran it, in order. This skill curates and writes around real screenshots, it does not generate, fake, or invent them, it has no way to actually drive the tool's UI itself. **How they arrive is flexible**, pasted directly into the session, or handed over already organized in a folder or a doc, either is fine, what matters is that every real one provided actually gets placed, per §2/§3 below, not where it came from.
- **A plain description of what happened at each step**, from whoever ran it, enough to write real step text around each screenshot, not so polished it reads like a finished doc already.

---

## 2. Rules, reused from code-demo-review, not reinvented

**If notebook or code file:**
- Runs top to bottom without error in a clean environment, with output cells actually present, not just claimed to work.
- No leftover debug prints, dead cells, or commented-out blocks.
- Dependencies importable, pinned where house convention expects it.
- Environment-setup instructions stated explicitly, dependencies to install and dataset/link sources, not assumed known.
- Large code blocks are commented at each meaningful step, a one-line header on a 20-line block doesn't count as explained.
- Formatting (heading weight/size for sections vs. subsections vs. body) stays internally consistent.

**If a no-code/low-code build guide:**
- Every step concrete and mechanically followable, exact field names, exact text to paste, exact click targets. Paraphrased or vague steps ("configure the connector appropriately") are not acceptable.

**Screenshots, whenever the demo's result is genuinely easier to see than to describe (any shape, not just no-code):**
- Every real screenshot provided (§1) gets placed next to the specific step or moment it shows, in the order it actually happened, not clustered at the end.
- Don't caption a screenshot with something it doesn't show, describe what's actually visible in it.
- **No real credential, ever, visible in a screenshot**, same rule `hands-on-guide-generation/SKILL.md` §2 already enforces for its own screenshots. Scan every one before saving, this is never assumed clean just because the text around it looks fine.
- Never invent a screenshot or describe one that wasn't provided. If a step genuinely needed one and none was given, say so plainly rather than writing around the gap silently.

**Applies to all three shapes:**
- A fully worked example precedes the first task the learner completes independently, never open cold with unscaffolded independent practice.
- Scaffolding fades across the demo, fully guided, then partially completed, then independent, not flat the whole way through. The confirmed real pattern: a 3-stage build going manual, then automated-with-a-manual-trigger, then fully autonomous, each stage adding real independence.
- The hands-on work demands the objective's stated Bloom's level, not lower.
- No real API keys, credentials, or secret-shaped strings. No real customer or PII data, sample data must read as obviously synthetic. No license-incompatible copied code.

---

## 3. Self-verify before handoff

Per `content-generation/SKILL.md` §3, the check differs by shape, they need genuinely different kinds of proof:

- **Notebook or code file**: actually run it before saving, capture the real executed output, don't hand off something that only claims to run. This is exactly what `code-demo-review` §1.1 checks for as run evidence, catch it here first rather than let review discover it doesn't run.
- **Screenshot-based demo** (any shape): this skill can't run anything itself here, so instead confirm every screenshot provided in §1 actually got placed, in the right order, next to the right step, and that none show a real credential. A demo with steps but no screenshot where one was clearly provided, or a credential visible in one, is caught here, not left for review.

---

## 4. Where it gets saved

`Outputs/[Client]/demo/`, a subfolder rather than a single file, since a demo can be a notebook plus data files, or a build guide plus screenshots. Same per-engagement parent folder as everything else.

---

## 5. Handoff, and this loops, not a one-time pass

Hand off to the existing `acceler-post-sales:code-demo-reviewer` for the actual review pass, per `content-generation/SKILL.md` §6. Reference `generation-learnings/code-demo.md` and `agent-loops` via the `skills:` frontmatter field. No candidates are seeded there yet. Per §6a, once real fix-loop history accumulates and the promotion rule is met, this skill is expected to actually change how it builds the next demo, not just fix the one in front of you.

**Runs before `slide-content-planning`, not after (`content-generation/SKILL.md` §1).** Once reviewed, this is a mandatory input to that day's Content Plan, per `slide-content-planning/SKILL.md` §1, which builds that day's real narrative around what this demo actually does, not a guess at what it might do. This is why review has to actually happen here first, Content Planning reads the reviewed version, not a draft.

---

## 6. Checklist
- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] Correct shape chosen (notebook/code vs. no-code guide) from that day's Libraries/Tools column, not defaulted
- [ ] Notebook/code actually executed before handoff, real output captured, not claimed
- [ ] No-code guide steps are concrete and mechanically followable, not paraphrased
- [ ] Every provided screenshot placed next to its real step, in order, none invented or skipped
- [ ] No real credential visible in any screenshot, checked explicitly, not assumed clean
- [ ] A worked example precedes the first independent task, scaffolding fades across the demo
- [ ] Security/data-boundary checks run regardless of how clean the artifact looks
- [ ] Saved to `Outputs/[Client]/demo/`
- [ ] Handed to the existing `code-demo-reviewer`, no bespoke review invented
