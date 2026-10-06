---
description: "Generates one MCQ set, pre-test, post-test, or one day's in-session quiz, applying NBME item-writing rules and objective alignment at creation time. Starts with two already-strong generation-learnings candidates (explanation completeness, distractor-text independence) from day one. Hands off to the existing acceler-post-sales:mcq-reviewer for the actual review pass."
argument-hint: "<client name + type: pre-test / post-test / in-session day N>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here. End every run by adding an entry to the client's run notes, per §6d of the same file.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way. If no folder containing `post-sales/.claude-plugin/plugin.json` can be found (the plugin was installed from GitHub and there is no local copy of the repo), use `Post-Sales Outputs/[Client]/...` inside the current work folder instead, per §1a, and tell the person the full path.

Generate one MCQ set for this engagement. **State which type before starting**, pre-test, post-test, or a specific day's in-session quiz, per `skills/mcq-generation/SKILL.md` §1, these are three different files, not variants of one shared set.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/mcq-generation/SKILL.md` in full first, especially §2a for the real shape each type follows.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved, that day's rows only for in-session) and `Outputs/[Client]/deep-research.md`. If generating the post-test, also read `Outputs/[Client]/mcq/pre-test.docx` to confirm real calibration, not just asserted difficulty. If anything mandatory is missing, handle it per `content-generation/SKILL.md` §6c (fresh pipeline build: stop and offer the choice of running the missing stage or going standalone; adapt or standalone run: build from the light brief), never guess.
3. Map each item to a stated Learning Objective from the relevant day's rows, confirm single-best-answer versus multi-select from how the objective/stem should be framed before writing distractors.
4. Write items against §2's rules, plausible distractors, no cueing, no compound claims, correct answer-key format.
5. Apply §3 from this first run: every explanation addresses why each wrong option is wrong, and no two options share near-identical phrasing.
6. Save to `Outputs/[Client]/mcq/pre-test.docx`, `mcq/post-test.docx`, or `mcq/day-N-in-session.docx`, matching the type generated, and list it at the end of the run as belonging in `PostSalesPluginOutput` per `content-generation/SKILL.md` §1b (uploaded only when a human asks, in v1).
7. Hand off to `acceler-post-sales:mcq-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.
8. **If this was the in-session type**, it's a mandatory input to that day's `slide-content-planning`, not to deck generation directly, per `slide-content-planning/SKILL.md` §1/§3a. Don't hand it to `session-deck`.

## Quality checklist (apply before presenting results)

- [ ] Type confirmed before starting, not assumed
- [ ] Both mandatory inputs loaded, pre-test also read first if generating post-test, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] Every item traced to a stated objective, format (single-best vs multi-select) confirmed before writing
- [ ] No implausible distractors, cueing, compound claims, or length giveaways
- [ ] Every explanation addresses each wrong option, not just the correct one
- [ ] No two options share near-identical phrasing differing only in a trailing clause
- [ ] Calibration matches `mcq-generation/SKILL.md` §2a: the pre-test is the harder, diagnostic one, the post-test the easier, foundational one, checked against the real pre-test file, not asserted
- [ ] Saved to `Outputs/[Client]/mcq/<type>.docx`, and listed for `PostSalesPluginOutput` per §1b
- [ ] Handed to the existing `mcq-reviewer`, not reviewed inline here
