# Case (negative): a deliberately short deck must NOT be failed for slides outside its stated scope

**Reviewer:** `acceler-post-sales:deck-reviewer` / `deck-review/SKILL.md` §1.1 (boilerplate slides) and §1.2 (objective coverage), read with `agent-loops/SKILL.md` §4 (review against the scope that was actually asked for)
**Source:** real, the Ferguson-DEMO adapt run of 2026-09-29 (`Outputs/Ferguson-DEMO/session-deck/day-1/index.html`). A human asked for a short 7-slide deck adapted from the e& Day 1 deck. The review raised "missing Instructor, house rules, Quiz and Thank You slides" and "two objectives have no teaching slide" as HIGH failures, none of which had been asked for. That run had no light brief, so the reviewer was checking the deck against a one-line chat message. The handoff below is what the same run carries under the 2026-10-01 rules.

## Input

An adapt-mode handoff, per `content-generation/SKILL.md` §6c:

```
Mode: Adapt. The e& Day 1 deck is the starting point, rebuilt for Ferguson.
Scope: 7 slides only: cover, agenda, timing, prompt engineering section, demo hand-off
Light brief: Outputs/[Client]/light-brief.md
```

The light brief's objectives outline holds three objectives, all inside that scope (few-shot prompting as a schema contract, guarding against invented values, handing off to the hands-on demo).

The deck has exactly these 7 slides: cover, agenda, timing table, one phase divider, two prompt engineering teaching slides, one demo slide. It has no instructor slide, no house rules, no quiz and no thank you slide. Its agenda slide names two later blocks of the day that have no teaching slides in this deck.

## Expected

- Verdict: no FAIL for the missing instructor slide, house rules, quiz or thank you slide. They sit outside the stated scope.
- No FAIL for objectives outside the light brief's outline, including the later blocks the agenda names. The reviewer says in one line that the light brief's objectives outline is the stated objectives for this review.
- Everything outside the scope appears once, as a single informational line ("out of scope for this run, needed before delivery: instructor slide, house rules, quiz, thank you, teaching slides for the later blocks"), never as separate findings and never counted toward the verdict.
- Inside the scope every rule still applies in full. In the real run the demo slide's "Click Here" button pointed at a demo guide that had not been built yet, and that is still a FAIL under §1.1's "all links resolve".
- A FAIL on any out-of-scope item is itself the defect this case exists to catch, a false positive, not a true one.

## Why this case exists

This false failure really happened, and it made a deck that did exactly what was asked look broken. Without this negative case, a later rubric edit to the boilerplate or objective-coverage rules could quietly bring the same false failure back, since every other deck case in this suite is a true-positive check.
