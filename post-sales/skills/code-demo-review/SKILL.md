---
name: code-demo-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler notebook, code lab, OR no-code/low-code build guide (Copilot Studio agent build, Figma Make prototyping task) for technical correctness (where applicable), pedagogical scaffolding, curriculum alignment, and data-boundary safety before it ships — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Companion to deck-review, mcq-review, assignment-review, and the content-review bundle."
metadata:
  type: reference
---

# Acceler Code / Demo Review — SKILL.md (v1)

Reviews **one hands-on build artifact** in isolation — a Jupyter notebook, a standalone code file, or a **no-code/low-code build guide** (a step-by-step doc for building something in Copilot Studio, Figma Make, or similar — confirmed real content type via `B2B Cornerstone Sales Team/.../Cornerstone_PreCall_Agent_Demo_Guide.docx`, a 3-stage Copilot Studio agent build). All three share this skill rather than splitting further: the pedagogical checks (§1.2, §1.3) apply identically regardless of whether there's executable code underneath; only §1.1's "runs without error" half is code-specific.

---

## 1. Rules

### 1.1 Technical Correctness
- Notebook: executes top to bottom without error in a clean environment, **with output cells present as run evidence** — a notebook that only claims to work without showing executed output doesn't pass this check.
- Code file: runs and passes any provided tests/examples, with the run output captured, not just claimed.
- **No-code/low-code build guide** (Copilot Studio, Figma Make, etc.): this half of §1.1 doesn't apply — there's no code to execute. Instead, every step must be concrete and mechanically followable (exact field names, exact text to paste, exact click targets — the reference guide's "paste the following instructions exactly" pattern), not paraphrased or vague.
- No leftover debug prints, dead cells, or commented-out blocks (code/notebook only).
- Dependencies importable; pinned where house convention expects it (code/notebook only).
- **Clear environment-setup instructions present** — dependencies to install and dataset/link sources, stated explicitly, not assumed known (a real recurring review finding: notebooks reviewed without this were flagged for missing it).
- **Large code blocks are commented, explaining what's happening at each meaningful step** — a real recurring finding: dense, uncommented blocks doing complex multi-step work (data preprocessing, multi-call orchestration) were flagged for needing inline explanation, not just a header comment. A one-line header on a 20-line block doesn't satisfy this.
- Descriptive text formatting is internally consistent (heading weight/size for major sections vs. subsections vs. body descriptions) — a real recurring finding was inconsistent bolding/sizing across a notebook's own markdown cells.

### 1.2 Scaffolding & Pedagogy
*Grounded in: the worked-example effect, scaffolding-fade research. Real worked example of correct scaffolding fade found in-house: the Cornerstone Demo Guide's 3-stage build — manual chat mode → same flow with automated email output → fully autonomous scheduled trigger. Each stage adds independence; none of them opens cold.*
- A fully worked example must precede the first task the learner completes independently — flag any lab that opens cold with unscaffolded independent practice.
- Scaffolding fades across the lab (fully-guided → partially-completed → independent), not flat throughout.

### 1.3 Curriculum & Objective Alignment
- Same Bloom's-verb-match rule as `deck-review` §1.2, applied to the lab's tasks: the lab's hands-on work must demand the cognitive level the objective states, not lower.

### 1.4 Security & Data-Boundary
- No real API keys, credentials, or secret-shaped strings.
- No real customer/PII data in sample datasets — must read as obviously synthetic.
- No license-incompatible copied code blocks.

---

## 2. How this runs

- Launched as the `acceler-post-sales:code-demo-reviewer` agent (`agents/code-demo-reviewer.md`) — may run/execute code via Bash to verify §1.1, but is read-only by tool restriction (`disallowedTools: Write, Edit, NotebookEdit`) on the artifact itself. No visibility into the generating agent's reasoning or other hats' findings.
- Given: the notebook/code file(s) and the session's stated learning objectives (Curriculum KG / Day-by-Day doc — ask if missing, never invent).
- Output: `{cell/file/line, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Code-demo-specific only:

- "The artifact" = the notebook/code file. The fixer may touch only the artifact — never the stated objectives or this skill's §1 rubric (don't let it "fix" a scaffolding FAIL by deleting the requirement instead of adding the worked example).
- Dismissed findings logged to §4 Memories below, not re-surfaced.

---

## 4. Memories

```
- [date] Dismissed: <finding> — <human's stated reason>
```

(empty until first use)

---

## 5. Verdict

| Verdict | Condition |
|---|---|
| ✅ Approve | All §1 rules PASS, directly or after an approved fix |
| 💬 Comment | Only dismissed/subjective findings remain |
| 🔴 Request Changes | Any FAIL didn't converge within 2 fix rounds |

## 6. Checklist
- [ ] Objectives sourced from the real Curriculum KG / Day-by-Day doc, not invented
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval
- [ ] No finding exceeded 2 fix rounds before escalating
- [ ] Security/data-boundary checks ran even if nothing else flagged — this rule doesn't get skipped for a clean-looking notebook
