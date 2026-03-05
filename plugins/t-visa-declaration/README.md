# T-Visa Declaration Generator — v3.0.0

A Cowork plugin that generates T-visa personal declarations from interview transcripts using a 4-phase pipeline. Built on 9 rounds of attorney feedback, encoding 40+ legal rules that prevent common drafting mistakes.

---

## How It Works — Pipeline Overview

| Phase | What happens | Where it runs |
|-------|-------------|---------------|
| **Phase 0** | Gather client name, trafficking type, transcript, supplemental notes | Local (AskUserQuestion) |
| **Phases 1–2** | Extract facts from transcript, classify into sections, map EMP framework, identify gaps | n8n via MCP |
| **Stopping Point** | Ask attorney pre-writing questions about background selection, scope, and critical gaps | Local (AskUserQuestion) |
| **Phase 3** | Write all 9 declaration sections using dedicated agents (parallel + sequential) | Local (Sonnet agents) |
| **Phase 4** | Review assembled draft against 40+ quality rules and attorney-verified checks | Local (command-level review) |

---

## Phase-by-Phase Detail

### Phase 0 — Gather Information

Collects everything needed before extraction begins. Uses a single `AskUserQuestion` call with 4 questions:

| # | Question | Header | Type | Options |
|---|----------|--------|------|---------|
| 1 | Client's full legal name | `Client name` | Free text | "Type below" |
| 2 | What type of trafficking? | `Case type` | Single select | Labor trafficking / Sex trafficking / Both |
| 3 | Transcript source | `Transcript URL` | Single select | Paste URL (fetched via n8n) / Paste manually |
| 4 | Supplemental case notes? | `Case notes` | Single select | No additional notes / Yes — paste below |

After collecting answers, the transcript is fetched either via the n8n MCP tool (`mcp__n8n-tvisa__get_transcription`) or read from a file/pasted text.

**Error handling:** If the n8n fetch fails, times out, or returns unusable content, the pipeline stops and asks the user to verify the URL or paste manually.

---

### Phases 1–2 — Extraction + Classification (n8n)

A single call to `mcp__n8n-tvisa__extract_and_classify` sends the full transcript to the n8n workflow, which runs two phases server-side:

- **Phase 1 (Extraction):** Reads the raw transcript and extracts ALL facts as structured JSON. Maximum granularity — no filtering, no writing. Includes specialized extraction for psychological coercion, procedural task detail, baseline-vs-reality contrasts, and multiple trafficking episode detection.

- **Phase 2 (Classification):** Assigns each extracted fact to one of 9 declaration sections. Maps the Ends-Means-Process (EMP) legal framework. Generates a paragraph plan for the Trafficking section. Produces a gap analysis identifying missing information critical for the legal case.

**Output keys required:** `section_assignments`, `gap_analysis`, `attorney_decision_points`. The pipeline stops if any are missing.

---

### Stopping Point — Pre-Writing Questions

**The pipeline pauses here to ask the attorney 3–8 questions before writing begins.** This ensures attorney judgment guides the draft rather than AI assumptions.

#### Question 1 — Background Facts (always asked)

Background is limited to 2 short paragraphs. The system presents the extracted vulnerability facts and asks which are most important:

| Header | Type | What it asks |
|--------|------|-------------|
| `Background` | Multi-select | "I found these key facts: [list]. Which are most important? Anything else about the client's life before meeting the trafficker?" |

Options are generated dynamically from `section_assignments.background` (up to 4 facts), plus an "Include all of them" option.

#### Question 2 — Trafficking Scope (conditional)

Only asked when `attorney_decision_points.multiple_episodes.count > 1`:

| Header | Type | What it asks |
|--------|------|-------------|
| `Scope` | Single select | "I found [N] separate trafficking situations: [list]. Which should be the focus?" |

Options: one per episode + "All episodes."

#### Question 3 — Background Boundary (conditional)

Only asked when `attorney_decision_points.background_boundary_issue = true`:

| Header | Type | What it asks |
|--------|------|-------------|
| `Timeline` | Single select | "It looks like the client met the trafficker before coming to the US. Where does the Background end and trafficking begin?" |

Options: Met abroad, trafficking in US / Trafficking started abroad / Met in US, home country as context.

#### Questions 4–8 — Critical Gaps (top 3–5 gaps)

Each critical gap from `gap_analysis.critical_gaps` is translated into a plain-English question:

| Header | Type | What it asks |
|--------|------|-------------|
| `[2-3 word label]` | Single select | Plain-English question about the missing information |

Each gap question includes 2 likely answers plus an "I don't know — add placeholder" option. Choosing "I don't know" results in an `[ATTORNEY REVIEW]` tag in the final declaration.

---

### Phase 3 — Section Writing

Writes all 9 sections using two specialized agents. Writing happens in two waves to balance speed with section dependencies.

#### Wave 1 — Independent Sections (parallel)

All of these launch simultaneously as parallel `Task` calls:

| Section | Agent | Key rules |
|---------|-------|-----------|
| **Trafficking** | `trafficking-writer` | Follows Phase 2 paragraph plan exactly. VAWA-vs-trafficking filter on every paragraph. Means-Ends pairing mandatory. Recruitment progression (2–4 ¶s for gradual cases). Labor paragraph granularity. Servitude contrast. Isolation integrity. |
| **Header** | `section-writer` | Case caption format |
| **Introduction** | `section-writer` | 1 paragraph, NO heading, flows under preamble |
| **Background** | `section-writer` | Strictly 2 paragraphs. Every fact must pass vulnerability-connection test. No intent to remain. |
| **Law Enforcement** | `section-writer` | 1 concise paragraph. Altruistic framing. Delayed cooperation explanation if applicable. |
| **Moral Character** | `section-writer` | 2–3 paragraphs |
| **Conclusion** | `section-writer` | 3–5 sentences. Plea, not summary. |

#### Wave 2 — Dependent Sections (sequential)

These need previously written content to avoid repetition:

| Section | Agent | Depends on | Key rules |
|---------|-------|-----------|-----------|
| **Physical Presence** | `section-writer` | Trafficking section text | 1–2 ¶s (150–300 words, FAIL >350). Must causally connect presence to trafficking. No country fears. |
| **Extreme Hardship** | `section-writer` | Physical Presence text | 3 ¶s max (250–400 words, FAIL >500). Forward-looking only. Must cite INA § 101(a)(15)(T)(i)(IV) + 8 C.F.R. § 214.209. Country conditions required. |

After both waves complete, sections are assembled in canonical order (header → introduction → background → trafficking → physical presence → law enforcement → extreme hardship → moral character → conclusion), paragraphs are renumbered sequentially, and section headings are applied.

---

### Phase 4 — Quality Review

Reviews the assembled draft against 40+ rules derived from 9 rounds of attorney feedback. The reviewer reads the full `quality-benchmarks` and `system-rules` skills before starting.

#### Hard Fail Checks (fix immediately)

| # | Check | What it catches |
|---|-------|----------------|
| 1 | First-person singular | "us/we/our" for victim group → rewrite to "I/me/my" |
| 2 | Firing fear | "afraid they would fire me" → implies voluntary employment |
| 3 | English-only | Any Spanish text remaining in declaration |
| 4 | VAWA filter | Trafficking paragraphs describing abuse without exploitation End |
| 5 | Means-Ends pairing | Orphaned Means (threats without exploitation purpose) |
| 6 | Sexual violence language | "sexually abused/assaulted" → "sexual slave/servitude" |

#### Cross-Section Checks

| # | Check | What it catches |
|---|-------|----------------|
| 7 | Background → Trafficking overlap | Facts restated as new information instead of continuation |
| 8 | Any → Any overlap | Same event appearing in multiple sections |

#### Structure Checks

| # | Check | Target |
|---|-------|--------|
| 9 | Proportions | Trafficking = longest (55–65%). PP 150–300w. EH 250–400w. Total 2,500–3,500w. |
| 10 | Active voice | Fix passive constructions, especially in journey/process narration |
| 11 | Paragraph openings | Max 2–3 trafficking ¶s open with trafficker's name |
| 12 | Consistency | Names, dates, amounts match throughout |
| 13 | Prohibited words | ~35 terms replaced with plain-language equivalents |
| 14–21 | Section-specific | Introduction (no heading), Conclusion (brevity), PP (content boundary), EH (legal citation, country conditions), Enforcement mechanisms, Isolation integrity, Servitude contrast |

Issues that need attorney input are tagged `[ATTORNEY REVIEW: reason]`.

---

## Declaration Sections

| # | Section | Length | Heading |
|---|---------|--------|---------|
| 1 | Header | Case caption | (title block) |
| 2 | Introduction | 1 paragraph | None (flows under preamble) |
| 3 | Background Information | 2 paragraphs | "Background Information" |
| 4 | Severe Form of Trafficking | 10–16+ paragraphs | "Severe Form of Trafficking in the U.S." |
| 5 | Physical Presence | 1–2 paragraphs | "Physical Presence on Account of Trafficking" |
| 6 | Law Enforcement | 1 paragraph | "Cooperation with Law Enforcement" |
| 7 | Extreme Hardship | 3 paragraphs max | "Extreme Hardship in [Country]" |
| 8 | Moral Character | 2–3 paragraphs | "Good Moral Character and Waiver of Inadmissibility" |
| 9 | Conclusion | 3–5 sentences | "Conclusion" |

---

## Plugin Components

**Command:** `generate-declaration` — runs the full pipeline end-to-end.

**Agents:**
- `trafficking-writer` (model: Sonnet, color: red) — dedicated agent for the trafficking section with anti-VAWA rules, EMP mapping, and paragraph plan enforcement
- `section-writer` (model: Sonnet, color: blue) — handles the remaining 8 sections with per-section rule files
- `phase-3-writer` — base writer agent with shared voice, language, and structural rules
- `phase-4-reviewer` (model: Sonnet) — reviews and corrects the full assembled declaration against 40+ checks

**Skills (reference — loaded on demand):**
- `writing-rules` — shared voice, language, and structural rules + per-section instruction files
- `system-rules` — all attorney-verified rules from 9 feedback rounds (organized by round)
- `quality-benchmarks` — pass/fail criteria, word-count targets, prohibited words, per-section checklists

**MCP Server:**
- `n8n-tvisa` — SSE connection to n8n workflow for transcript fetching, extraction, and classification

---

## Requirements

- Claude desktop app with Cowork mode
- n8n instance running the `extract-dec-events` workflow (see INSTALL.md)

---

## Usage

1. Run the `generate-declaration` command
2. **AskUser — Phase 0:** Provide the client's name, trafficking type, transcript source, and any supplemental notes
3. Wait for n8n extraction and classification to complete
4. **AskUser — Pre-Writing:** Answer 3–8 questions about background fact selection, trafficking scope, timeline boundaries, and critical information gaps
5. The plugin writes all sections (parallel + sequential), assembles the draft, and runs a 40+ rule quality review
6. Review any `[ATTORNEY REVIEW]` tags in the output

Output is saved to `output/[client_name]_declaration.md` with word count stats and flagged items.

---

## Version History

- **v3.0.0** — Complete rewrite: strict n8n checkpoints, dedicated trafficking-writer agent, progressive-disclosure skills, command-level Phase 4 review, pre-writing attorney questions
- **v2.0.0** — Original 4-agent pipeline (extractor, classifier, writer, reviewer)
