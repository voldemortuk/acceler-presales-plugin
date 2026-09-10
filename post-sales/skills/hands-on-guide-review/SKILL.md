---
name: hands-on-guide-review-skills-acceler-single-artifact-panel
description: "How to review one Acceler hands-on/setup guide (the standalone tool-access doc — e.g. VM & AI Tools SetUp, Pre-Requisites Setup — distinct from both the deck and the notebook) for completeness, credential-safety, and tool/curriculum alignment before it ships — human-gated fix loop, dismissed-finding memory, PR-vocabulary verdicts. Companion to deck-review, code-demo-review, mcq-review, assignment-review, and the content-review bundle."
metadata:
  type: reference
---

# Acceler Hands-On Guide Review — SKILL.md (v1)

Reviews **one hands-on/setup guide** in isolation — the standalone doc a learner uses to get tools/VM access working before or during a session (e.g. `VM & AI Tools SetUp For [Client]/.../Pre-Requisites Setup.docx`). This is a distinct artifact from the session deck's own in-deck setup movement and from the code-demo notebook — it's the leave-behind reference a learner can return to independently. Grounded in a real reference: `B2B e& AI Builder (Low Code) - Nov-Dec_25/VM & AI Tools SetUp For e&/.../Pre-Requisites Setup.docx`.

---

## 1. Rules

### 1.1 Setup & Access Completeness
- Every tool listed as "required for the day" has a complete step-by-step section: tool name, a 1-line description of what it's for, numbered steps, and the access URL. A tool named in the "Tools Required" table with no matching step-by-step section is a FAIL.
- Steps must be sequential and concrete (click X, go to Y) — not vague ("set up your account").
- A support channel (e.g. the reference's WhatsApp group) is present for when setup fails live — a guide with no escalation path is a FAIL.

### 1.2 Credential Safety
*Grounded in the reference doc's own correct practice: it says "the password shared with you," never showing the actual value.*
- No real, live credential value (password, API key, token) is ever baked into the guide text itself — credentials must be referenced as shared through a separate secure channel (or shown as an obvious placeholder, e.g. `windowsjuneaiuserXX` / `XXXXXXXX`, confirmed real house pattern from the Yettel Serbia VM guide), never embedded as a real value.
- Both personal-account and pooled/shared-training-account schemes are legitimate (confirmed: personal email in some e& variants, pooled `aiexplorer_01@uplevel.academy`-style accounts in others, sometimes because corporate networks block personal-email tools — Lowe's guide states this explicitly). Don't flag which scheme is used; only flag a real value appearing in the doc.
- If a screenshot is referenced/included, it must not visibly contain a real credential in the image itself.

### 1.4 Example-Prompt Accuracy *(when the guide includes them)*
- Some guides embed example prompts per tool (e.g. "Summarize this 3-page policy into 5 bullet points" for Word Copilot, confirmed real pattern from an AI Leadership Program Login Guide). Where present, the example must be realistic for that tool and that audience's actual use case — a generic or tool-mismatched example prompt is a FAIL.

### 1.3 Curriculum & Tool Alignment
- The tools listed for a given day must match what that day's actual code-demo/deck use — a guide listing a tool the session never touches, or missing one the code-demo depends on, is a FAIL. Cross-check against `code-demo-review`'s artifact for the same day where available.
- House branding/format consistency — client + Acceler branding present, sign-off line present, matching the house pattern (not a generic unbranded doc).

---

## 2. How this runs

- Launched as the `acceler-post-sales:hands-on-guide-reviewer` agent (`agents/hands-on-guide-reviewer.md`) — read-only by tool restriction (`disallowedTools: Write, Edit`). No visibility into the generating agent's reasoning or other hats' findings.
- Given: the guide file and, where available, that day's code-demo/deck artifacts for the tool-alignment cross-check (§1.3). Ask if the day's other artifacts aren't available yet — §1.3 can't run without them, but §1.1-1.2 can.
- Output: `{tool/step, rule, verdict: PASS|FAIL, evidence, suggested_fix}`.
- Check every FAIL against §4 Memories before surfacing it.

---

## 3. Fix & re-verify loop (human-gated)

Follows the shared mechanics in `skills/agent-loops/SKILL.md` in full, using the `acceler-post-sales:content-fixer` agent for the apply step. Hands-on-guide-specific only:

- "The artifact" = the guide doc. The fixer may touch only the guide — never the stated objectives or this skill's §1 rubric.
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
- [ ] Every tool in the "required for the day" list has a matching complete step-by-step section
- [ ] No real credential value appears anywhere in the guide text or screenshots
- [ ] Tool list cross-checked against that day's code-demo/deck where available
- [ ] Every finding checked against §4 Memories before being surfaced
- [ ] No fix applied without explicit human approval, none exceeding 2 rounds
