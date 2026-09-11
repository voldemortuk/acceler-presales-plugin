---
description: "Generates one program Closing Ceremony deck, backward-looking wrap-up content following the confirmed recurring 5-program structure. Any embedded assessment questions are pulled from the already-generated and reviewed MCQ set, not written fresh. Hands off to the existing acceler-post-sales:closing-ceremony-reviewer for the actual review pass."
argument-hint: "<client name>"
---

Generate the Closing Ceremony deck for this engagement.

## Input
```
$ARGUMENTS
```

## How to run

1. Read `skills/closing-ceremony-generation/SKILL.md` in full first.
2. Load the mandatory inputs per §1: `Outputs/[Client]/lesson-plan.xlsx` (approved) and `Outputs/[Client]/deep-research.md`.
3. Build all six structural sections per §2: reflection prompt, Key Takeaways naming real topics from the Lesson Plan, next-steps, feedback link, assessment pointer, sign-off.
4. If embedding assessment questions as slides, pull them from `Outputs/[Client]/mcq.docx` if it exists, never write fresh questions here.
5. Generate the live/external version only, a Dry Run is the same deck rehearsed internally, not a separate artifact.
6. Build through the existing deck engine/tokens (`live-session-deck`), not a new rendering mechanism.
7. Self-verify per §3: Key Takeaways names real topics, embedded assessment content (if any) came from the reviewed MCQ set.
8. Save to `Outputs/[Client]/closing-ceremony-deck/`.
9. Hand off to `acceler-post-sales:closing-ceremony-reviewer` for the actual review pass, this command doesn't review its own output.

## Quality checklist (apply before presenting results)

- [ ] Both mandatory inputs loaded, or the run stopped and asked
- [ ] All six structural sections present, Key Takeaways specific to this program's real curriculum
- [ ] Embedded assessment content, if any, pulled from the reviewed `mcq.docx`, not authored fresh
- [ ] Live/external deck generated, not a separate Dry Run artifact
- [ ] Saved to `Outputs/[Client]/closing-ceremony-deck/`
- [ ] Handed to the existing `closing-ceremony-reviewer`, not reviewed inline here
