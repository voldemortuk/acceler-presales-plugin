---
description: "Generates one program Closing Ceremony deck, backward-looking wrap-up content following the confirmed recurring 5-program structure. Any embedded assessment questions are pulled from the already-generated and reviewed MCQ set, not written fresh. Hands off to the existing acceler-post-sales:closing-ceremony-reviewer for the actual review pass."
argument-hint: "<client name>"
---

**Start here, every run (added 2026-10-01):** before building anything, run the start-up check in `skills/content-generation/SKILL.md` §6c. In short: name the mode in one line (fresh build, adapt from a reference, or tweak), check what already exists for this client, ask at most 3 to 5 questions for anything that can't be worked out, and in adapt or standalone runs write and show the light brief first. That section is the single source for this, don't restate or vary it here.

**Where every `Outputs/[Client]/...` path in this command lives (added 2026-09-29):** it always means the full path per `skills/content-generation/SKILL.md` §1a. Before any read or save, find the folder containing `post-sales/.claude-plugin/plugin.json` (search from the current working directory), then use `<that folder>/post-sales/Outputs/[Client]/...`, and read the file back from that exact path after saving. Never let a bare `Outputs/[Client]/` resolve against the current folder: a real run on 2026-09-29 started in the workspace root and saved into an existing client folder (`Ferguson/Outputs/`) that way.

Generate the Closing Ceremony deck for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/closing-ceremony-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`.
3. Build all six structural sections per §2: reflection prompt, Key Takeaways naming real topics from the Lesson Plan, next-steps, feedback link, assessment pointer, sign-off.
4. If embedding assessment questions as slides, pull them from `Outputs/[Client]/mcq/post-test.docx` if it exists, never the pre-test file, never write fresh questions here.
5. Generate the live/external version only, a Dry Run is the same deck rehearsed internally, not a separate artifact.
6. Build through the existing deck engine/tokens (`live-session-deck`), not a new rendering mechanism.
7. Self-verify per §3: Key Takeaways names real topics, embedded assessment content (if any) came from the reviewed MCQ set.
8. Save to `Outputs/[Client]/closing-ceremony-deck/`.
9. Hand off to `acceler-post-sales:closing-ceremony-reviewer` for the actual review pass, this command doesn't review its own output. Per `agent-loops/SKILL.md` §2a-2, this is a hard completion condition, not a step to describe, this run isn't finished until the reviewer has actually been invoked, not just reported as the next step.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the gap handled per §6c (stopped and asked, or light brief written)
- [ ] All six structural sections present, Key Takeaways specific to this program's real curriculum
- [ ] Embedded assessment content, if any, pulled from the reviewed `mcq/post-test.docx` (not the pre-test file), not authored fresh
- [ ] Live/external deck generated, not a separate Dry Run artifact
- [ ] Saved to `Outputs/[Client]/closing-ceremony-deck/`
- [ ] Handed to the existing `closing-ceremony-reviewer`, not reviewed inline here
