# Generation Learnings

The generation-side half of the improvement loop described in `agent-loops/SKILL.md` §9. One file per content type, parallel to each reviewer's own `Memories` log — but pointed the other direction:

- A reviewer's **Memories** log stops the *reviewer* from re-flagging something a human already ruled out.
- A **Generation Learning** stops the *generator* from making the same mistake in the first place, before a reviewer ever has to catch it.

## Status: infrastructure only, not yet connected

Deliberately empty until real generation agents exist to consume it. Per the user's own stated plan (2026-09-10): "Once Tanmaya has created the generation I will connect it." These files are the ready-to-wire target for that connection — not a guess at what her generation skills will look like.

## The promotion rule (when a fix-loop resolution becomes a Generation Learning)

A single resolved finding is not enough on its own — one bad MCQ explanation might be a one-off authoring slip, not a pattern. Promote a finding into this log only when:

**The same rule fails ≥3 times across different generated artifacts of the same content type**, confirmed via the fix-loop history (see `agent-loops/SKILL.md` §9 for the exact record shape). At that point it's a pattern in how the content gets generated, not a string of unrelated mistakes — worth telling the generator about directly rather than catching it after the fact every single time.

## The connection contract (for whoever wires this up later)

A generation skill for a given content type should reference its matching file here the same way every reviewer agent references `agent-loops` — e.g. via the `skills` frontmatter field pointing at `generation-learnings/deck.md`, so the learnings preload automatically into the generation agent's context rather than needing to be manually pasted into every prompt.

## Files

| File | Content type | Reviewer that feeds it |
|---|---|---|
| `deck.md` | Session decks | `deck-reviewer` |
| `code-demo.md` | Notebooks / code labs / no-code build guides | `code-demo-reviewer` |
| `mcq.md` | MCQ sets | `mcq-reviewer` |
| `assignment.md` | Assignments | `assignment-reviewer` |
| `project.md` | Capstone/multi-milestone projects | `project-reviewer` |
| `hands-on-guide.md` | Tool-access/setup guides | `hands-on-guide-reviewer` |
| `lesson-plan.md` | Lesson plans | `lesson-plan-reviewer` |

Not covered yet: Orientation/Closing Ceremony, session recaps, impact reports — these are lower-iteration-volume artifact types (built once per program, or generated from a single session rather than repeatedly authored), so a recurring-pattern signal is less likely to accumulate the same way. Add a file here if a real repeat pattern shows up for one of them; don't pre-build speculative infrastructure for content types that haven't demonstrated the need yet.
