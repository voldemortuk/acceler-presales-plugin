# Case: notebook claims to work with no output cells as evidence

**Reviewer:** `acceler-post-sales:code-demo-reviewer` / `code-demo-review/SKILL.md` §1.1
**Source:** the skill's own explicit rule — "a notebook that only claims to work without showing executed output doesn't pass this check."

## Input

A Jupyter notebook where every code cell has been written and looks syntactically correct, but no cell has been executed — no `Out[]` output, no execution count numbers, no printed results anywhere in the file.

## Expected

- Verdict: FAIL on §1.1 — no run evidence, regardless of how clean the code looks.
- The reviewer should attempt to execute the notebook itself via Bash (it has that access specifically for this) rather than taking the absence of output on faith either way — the check is "does it actually run," not "does it look like it would run."
- Suggested fix: execute top to bottom in a clean environment and save the notebook with real output cells intact.

## Why this case exists

This is the most basic technical-correctness check in the skill, and the easiest one for a reviewer to get lazy about — skimming code for correctness instead of actually running it. This case confirms the reviewer does the latter.
