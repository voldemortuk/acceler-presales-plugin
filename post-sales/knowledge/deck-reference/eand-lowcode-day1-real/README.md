# e& Low-Code Day 1, real delivered deck (104 slides)

A written summary of the actual, real class deck delivered for e& AI Builder Program (Low Code) Day 1, first captured 2026-09-25. Previously only reachable by a human manually sharing it, since the Curriculum Graph never indexed it (see the missing-content finding below).

**Where the deck itself lives (changed 2026-10-01):** the full PDF is no longer kept in this repo, real client decks stay in the shared Drive only, under `B2B AI Programs` > the e& AI Builder (Low Code) Q1'26 program folder > Day 1 live class slides. No skill reads the PDF directly, everything the pipeline needs from it is already written into this README, `slide-types-contact-sheet.png` in this folder, and `live-session-deck/SKILL.md`. If the real deck is needed again, a human opens it from Drive.

**Real instructor:** Anshaj Khare (shown directly on the cover and instructor slides).

## Real structure, 4 parts, confirmed by reading all 104 pages directly

1. **"How AI is reshaping the Tech World"** — a Codex demo, Andrej Karpathy's "Software 1.0/2.0/3.0" framing, the "Map of GitHub" visualization, the Hugging Face Model Atlas. **This entire part is missing from what this pipeline built for e&-TESTRUN's Day 1**, confirmed by checking all real slide headings in the generated deck, none match. A real, substantial content gap, not a styling issue.
2. **"Prompt Engineering - The Right Way to Talk to AI"** — taught entirely through **Expertex**, not generic ChatGPT. Covers Expertex's interface, model parameters (temperature etc.), then the four prompting techniques (instructional, role-based, few-shot, chain-of-thought), each practiced live inside Expertex. Includes the real "Introducing Personas" section and the real "Maya" persona demo (a Senior Backend/Platform Engineer at e&, the exact same demo this session's Demo 1 covers), confirmed here as genuine, real, taught Day 1 content, not unused/off-curriculum material as earlier assumed this session. Closes with a separate "Prompt Engineering For Software Engineers" sub-section (code prompting, refactoring, debugging, docs).
3. **"Building AI automations in n8n"** — the real n8n build (Intelligent Client Inquiry Response System). This is the part that lines up reasonably well with what e&-TESTRUN's Day 1 already has (Pod 1.3b).
4. **"Responsible AI"** — hallucination, prompt injection (including a real GitHub MCP vulnerability writeup), and a dedicated privacy/PII section. Richer than what e&-TESTRUN's Day 1 currently covers.

## Real colors confirmed by direct pixel sampling, not assumed

Cover/section-divider slides use a softer pastel palette (blush/pink background, soft purple/peach/pink gradient circles) than the indigo-heavy Day 3 default reference, real difference worth knowing about if this deck is ever used as the primary visual reference instead of Day 3. The demo link-out slide's "Click Here" button text is `#0097A7`, confirmed identical to the Day 3 reference's own value, now in `live-session-deck/SKILL.md` §2.0 as `--link-accent`.

## Why the Curriculum Graph never found this on its own

Checked directly: this file isn't in `curriculum-graph.json` at all. The graph's real data for this specific program (e& Low Code Q1'26) only reaches a folder called "Modules," it never expanded into Day 1-4 individually, unlike some other e& programs which do have that depth. Separately, even where individual files under "Modules" did get indexed, this specific file (45MB) is large enough that the sync tool's content-reading step likely fails/times out on it silently, a real, separate bug flagged to Utkarsh, parked for a later fix pass. Until that's fixed, a real deck like this one has to be found and shared by a human, same as this one was.
