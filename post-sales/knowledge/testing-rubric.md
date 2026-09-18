---
name: post-sales-testing-rubric
description: "A short, reusable checklist for testing any post-sales pipeline stage against real client data. Written after the first real end-to-end test (e& AI Builder Low Code, 2026-09-13) found several distinct kinds of failure on the very first stage. Check every new stage's first real run against this list instead of re-discovering the same failure types by accident each time."
metadata:
  type: reference
---

# Post-Sales Testing Rubric (v1)

**Why this exists.** The first real test run, `discovery-checklist` against e&'s real Low Code proposal, needed four separate rounds before it held up. Each round found a genuinely different kind of problem, not the same one repeated. That's a signal: these are recurring failure shapes, not one-off bugs, worth checking for on purpose in every future stage's first real run, instead of hoping we happen to notice them.

**How to use this.** After running any pipeline command against real client data for the first time, check its output against every item below before trusting it. Don't wait for something to look wrong, actively check, the same way §3's arithmetic check isn't optional just because a number looks plausible.

---

## 1. Facts match the real source, verified by opening it

Don't trust a claim like "not found" or "confirmed" just because the tool said so. Open the actual file it cites and check directly. This is how we caught that a "3-month success metric" claim was wrong, opening `Scope of Work- AI Programs#(1).docx` directly showed only end-of-program metrics, nothing 3-month-out.

## 2. Numbers agree with each other

Any two figures that should multiply or sum to a third (per-person price × headcount = cohort total, session durations summing to a stated day length) actually need to, not just look individually plausible. Caught when AED 14,700/person was paired with a cohort total that was actually built from a different, discounted per-person figure, the two didn't multiply out.

## 3. Document structure read correctly, not just text matched

A real spreadsheet or tracker often has section headers that change what a row means, "Not Selected," "Opted Out," "Draft," "Superseded," "Tentative." Treating every row as equally valid produces wrong totals. Caught when a 16-person confirmed roster became "30 participants" by summing rows under "Not Selected" and "Opted Out" headers along with the real list.

Same failure shape, different sheet: a real name pulled from a tracker isn't automatically playing the role it's being cited for. Caught when three real names credited for "Day N Development" (who wrote the curriculum) got presented as "the instructors who built and delivered it," when the same tracker had a separate tab naming three entirely different people as the actual live delivery instructors. Check what role a row's own label actually says before naming someone by it.

## 4. Output lands in the right place

Every generated artifact saves to `post-sales/Outputs/[Client]/...`, per `content-generation/SKILL.md` §1a, regardless of where the input files happen to live on disk. Caught twice, once from the unstated convention, once from a run finding an old wrong-location file and updating it in place instead of moving to the correct one.

## 5. An old or stale file doesn't get silently extended

If a prior run already produced something at the wrong location (or under an old rule), treat it as reference material for what's already known, not the thing to keep editing. Same root cause as #4, worth checking separately since fixing the rule once didn't stop it from recurring against an existing file.

## 6. A genuine gap doesn't get papered over by something similar

Two things can look alike without being the same thing, an end-of-program assessment score is not a 3-month business-impact metric, even though both are technically "success metrics." When a checklist item asks for something specific, check that what was found actually satisfies it, not just that something in the same category exists.

## 7. Repeated runs on the same input should roughly agree

Running the same command against the same real files more than once produced three different date ranges for the same cohort across three runs. Wide disagreement between runs is itself worth investigating, even before deciding which run (if any) is correct, it usually means the source material itself has conflicting real documents (which is worth flagging to the account team) or the skill isn't anchoring to one clearly authoritative source.

## 8. Unrelated material doesn't get pulled in

Real client folders have documents for other, similar-sounding programs sitting nearby. Confirmed working correctly once already, an unrelated e& RFP for a different program ("Talent Capability Development") was correctly recognized as out of scope and not folded into this program's timeline or budget. Keep checking for this as new engagements get tested, it's easy to get wrong with less careful source material.

## 9. A stated self-check actually happened

If a skill says it self-verifies something (pricing arithmetic, duration math) before presenting it, confirm that check was actually applied to this run's real numbers, not just asserted as done.

## 10. A self-check total is compared against the real source, not itself

A generator or reviewer confirming that rows sum correctly only proves internal consistency, not correctness. Caught when a Lesson Plan added 45 minutes of break time on top of the proposal's stated "6 Hours Each" day, then self-verified against its own 6h45m total, both the generator and the independent reviewer confirmed the rows summed right, neither checked that total against the proposal's actual stated duration. Confirmed against real precedent (the Tech Teams Lesson Plan) that breaks belong inside the stated total, never added on top. Whenever a skill "self-verifies" a total, check what it verified against, its own number or the real one.

## 11. Gate thresholds applied correctly

If a skill defines a score threshold that blocks or allows progress (e.g. <50% is Thin, blocks the pipeline), confirm the actual verdict matches the actual score, and that gaps in the 50-79% band are logged explicitly rather than quietly absorbed into a passing-sounding summary.

## 12. Correct instructions still depend on the run's actual working directory

Item 4 covers a skill's own save-path instruction being missing or ambiguous. This is a different root cause: `mcq-generation.md` already stated the correct path (`Outputs/[Client]/mcq.docx`), matching every other correctly-behaving command, and the file still landed outside the repo entirely, in the standalone client folder. The instruction being right doesn't help if the session executing it is anchored to the wrong directory when it resolves that relative path. Caught the same day this exact confusion (the real repo folder vs. a stale duplicate folder with the same trailing path) had already derailed several unrelated commands. Before trusting a save-location claim, check the actual file landed inside `post-sales/Outputs/[Client]/`, don't assume a correct-looking instruction was enough.

---

## Keeping this current

Add a new item only when a real test surfaces a genuinely new failure shape, not a repeat of one already listed, fold repeats into the existing item's evidence instead. This stays a short, living list, not a growing audit document, the same restraint already applied to `generation-learnings/`.
