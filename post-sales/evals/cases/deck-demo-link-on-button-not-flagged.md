# Case (negative): the learner's demo-guide link on the "Click Here" button must NOT be flagged as a leak

**Reviewer:** `acceler-post-sales:deck-reviewer` / `deck-review/SKILL.md` §1.1 (the instructor-only material rule and the demo slide rule)
**Source:** real, the e& Low-Code Day 1 demo slide layout recorded in `live-session-deck/SKILL.md` §3.20, where the learner's demo guide is linked from the "Click Here" button on the laptop. `deck-review` §1.1 and `live-session-deck` §3.20 used to disagree on whether that link belongs on the visible slide. They were reconciled on 2026-10-01.

## Input

A demo slide where both links resolve:

```html
<section class="slide demo"
  data-notes="Learner demo guide: https://drive.example.com/day1/demo-guide.html
              SME link, instructor version with solution: https://drive.example.com/day1/instructor/demo-solution.ipynb">
  <div class="demo-title">Live Demo: Product Spec Automation Agent</div>
  <div class="demo-left">What this demo builds, Key Objectives, Technical Success Criteria</div>
  <div class="demo-link">Link for Hands-on workshop</div>
  <a class="demo-btn" href="https://drive.example.com/day1/demo-guide.html">Click Here</a>
</section>
```

The visible slide carries only the learner's demo-guide link, on the button. The speaker notes carry that same link again plus the SME's direct link to the instructor version.

## Expected

- Verdict: PASS on both rules. The learner's demo-guide link on the visible button is the real, correct layout and must not be flagged as leaked instructor material.
- The instructor-version link sitting in speaker notes is also correct and must not be flagged. Speaker notes are not the learner-facing slide.
- The other side of the rule still holds. If that same instructor-version link (solution notebook, answer key, notebook with outputs) appeared anywhere on the visible slide, as a second button or as a text link in the left column, that is still a FAIL at security-finding severity, exactly as `deck-leaked-solution-link.md` expects.
- A FAIL on the input as given is itself the defect this case exists to catch, a false positive, not a true one.

## Why this case exists

The leak rule is the one `deck-review` rule given security severity, so a reviewer leaning cautious will tend to flag any link on a demo slide. This case pins down the line between the two links, so a later tightening of the leak rule doesn't start failing every correctly built demo slide.
