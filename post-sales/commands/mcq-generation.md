---
description: "Generates one MCQ set, pre-test, post-test, or one day's in-session quiz, applying NBME item-writing rules and objective alignment at creation time. Starts with two already-strong generation-learnings candidates (explanation completeness, distractor-text independence) from day one. Hands off to the existing acceler-post-sales:mcq-reviewer for the actual review pass."
argument-hint: "<client name + type: pre-test / post-test / in-session day N>"
---

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Generate one MCQ set for this engagement. **State which type before starting**, pre-test, post-test, or a specific day's in-session quiz, per `skills/mcq-generation/SKILL.md` §1, these are three different files, not variants of one shared set.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/mcq-generation/SKILL.md` in full first, especially §2a for the real shape each type follows.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved, that day's rows only for in-session) and `Outputs/[Client]/deep-research.md`. If generating the post-test, also read `Outputs/[Client]/mcq/pre-test.docx` to confirm real calibration, not just asserted difficulty. If anything mandatory is missing, stop and ask.
3. Map each item to a stated Learning Objective from the relevant day's rows, confirm single-best-answer versus multi-select from how the objective/stem should be framed before writing distractors.
4. Write items against §2's rules, plausible distractors, no cueing, no compound claims, correct answer-key format.
5. Apply §3 from this first run: every explanation addresses why each wrong option is wrong, and no two options share near-identical phrasing.
6. Save to `Outputs/[Client]/mcq/pre-test.docx`, `mcq/post-test.docx`, or `mcq/day-N-in-session.docx`, matching the type generated, and push the same file to `PostSalesPluginOutput` per `content-generation/SKILL.md` §1b.
7. Hand off to `acceler-post-sales:mcq-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.
8. **If this was the in-session type**, it's a mandatory input to that day's `slide-content-planning`, not to deck generation directly, per `slide-content-planning/SKILL.md` §1/§3a. Don't hand it to `session-deck`.

## Quality checklist (apply before presenting results)

- [ ] Type confirmed before starting, not assumed
- [ ] Both mandatory inputs loaded, pre-test also read first if generating post-test, or the run stopped and asked
- [ ] Every item traced to a stated objective, format (single-best vs multi-select) confirmed before writing
- [ ] No implausible distractors, cueing, compound claims, or length giveaways
- [ ] Every explanation addresses each wrong option, not just the correct one
- [ ] No two options share near-identical phrasing differing only in a trailing clause
- [ ] Post-test genuinely harder than the pre-test, not just longer
- [ ] Saved to `Outputs/[Client]/mcq/<type>.docx` and pushed to `PostSalesPluginOutput`
- [ ] Handed to the existing `mcq-reviewer`, not reviewed inline here
