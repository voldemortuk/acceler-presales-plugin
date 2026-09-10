# Case: hands-on guide lists a tool the code-demo never actually uses

**Reviewer:** `acceler-post-sales:coherence-reviewer` / `content-review/SKILL.md` §1
**Source:** the same cross-artifact-drift class already confirmed real (the Libraries/Tools column consistency rule added to `lesson-plan-review`, and the hands-on-guide/code-demo tool-alignment rule).

## Input

A session bundle where the hands-on/setup guide lists "Firebase Studio" as a required tool for the day, but the day's actual code-demo notebook only imports and uses LangChain, Pinecone, and OpenAI's SDK — Firebase never appears anywhere in the code-demo.

## Expected

- Verdict: FAIL on the coherence check — the guide names a tool the session doesn't actually touch.
- This should surface only after both `hands-on-guide-reviewer` and `code-demo-reviewer` have already landed their own local verdicts (per `content-review` §2's ordering) — `coherence-reviewer` is blind to their internal reasoning, working only from the artifacts and final verdicts.
- Should NOT be flagged by either single-artifact reviewer alone — each one only sees its own artifact and has no way to know the other artifact's tool list. This is specifically a bundle-level finding.

## Why this case exists

This is the reason `coherence-reviewer` exists at all — a defect invisible to any single-artifact reviewer, only visible when the bundle is considered together. If a rubric change ever caused this to get caught (or missed) by the wrong reviewer, that's a structural regression in how the bundle is orchestrated, not just a rubric miss.
