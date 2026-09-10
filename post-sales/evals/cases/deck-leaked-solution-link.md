# Case: leaked instructor-only link on a student-facing slide

**Reviewer:** `acceler-post-sales:deck-reviewer` / `deck-review/SKILL.md` §1.1
**Source:** real, from Acceler's own module-creation review checklist: "Do none of the slides contain links or viewable copies of slides/notebooks hyperlinked or accessible by learners?"

## Input

A deck slide (hands-on/coding-demo section) whose speaker-notes-equivalent content has leaked onto the visible slide as a clickable link, e.g.:

```html
<div class="slide">
  <h2>Hands-On: Build the RAG Pipeline</h2>
  <p>Follow along, then check your work against the
     <a href="https://drive.google.com/instructor-solutions/day3-rag-complete.ipynb">
     complete solution notebook</a>.
  </p>
</div>
```

— where that solution-notebook link is visible/clickable on the learner-facing slide itself, not confined to speaker notes.

## Expected

- Verdict: FAIL, treated at security-finding severity per the skill's explicit instruction — not bundled in with routine formatting findings.
- Evidence should name the exact slide and the leaked link.
- Suggested fix: remove the link from the visible slide body; if a pointer to the solution is needed post-class, it belongs in speaker notes or a post-session resource, not on the slide during the hands-on portion.

## Why this case exists

This is a real house-checklist item, not a hypothetical — and it's the one rule in `deck-review` explicitly given security-severity treatment. If this ever silently degrades to a routine "Comment"-tier finding, that's a meaningful regression in the reviewer's judgment, not just a missed nitpick.
