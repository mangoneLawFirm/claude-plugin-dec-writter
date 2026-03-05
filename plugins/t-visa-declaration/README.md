# T-Visa Declaration Generator — v3.0.0

A Cowork plugin that generates T-visa personal declarations from interview transcripts using a 4-phase pipeline.

## How It Works

| Phase | What happens | Where it runs |
|-------|-------------|---------------|
| **Phase 0** | Gather client name, trafficking type, transcript, supplemental notes | Local (AskUserQuestion) |
| **Phases 1–2** | Extract facts from transcript, classify into sections, map EMP framework, identify gaps | n8n via MCP |
| **Phase 3** | Write all 9 declaration sections using dedicated agents | Local (Sonnet agents) |
| **Phase 4** | Review assembled draft against quality benchmarks and attorney rules | Local (command-level review) |

## Declaration Sections

1. Header (case caption)
2. Introduction (1 paragraph, no heading)
3. Background Information (2 paragraphs)
4. Severe Form of Trafficking in the U.S. (longest section)
5. Physical Presence on Account of Trafficking (1–2 paragraphs)
6. Cooperation with Law Enforcement (1 paragraph)
7. Extreme Hardship (3 paragraphs max)
8. Good Moral Character and Waiver of Inadmissibility (2–3 paragraphs)
9. Conclusion (3–5 sentences)

## Plugin Components

**Command:** `generate-declaration` — runs the full pipeline end-to-end.

**Agents:**
- `trafficking-writer` — dedicated agent for the trafficking section with anti-VAWA rules
- `section-writer` — handles the remaining 8 sections

**Skills (reference):**
- `writing-rules` — shared voice, language, and structural rules + per-section instructions
- `system-rules` — all attorney-verified rules from 9 feedback rounds
- `quality-benchmarks` — pass/fail criteria, word-count targets, prohibited words

**MCP Server:**
- `n8n-tvisa` — SSE connection to n8n workflow for extraction and classification

## Requirements

- Claude desktop app with Cowork mode
- n8n instance running the `extract-dec-events` workflow (see INSTALL.md)

## Usage

1. Run the `generate-declaration` command
2. Provide the client's name, trafficking type, and interview transcript
3. Answer pre-writing questions about background facts and critical gaps
4. The plugin writes all sections, reviews the draft, and outputs a Markdown file

Output is saved to `output/[client_name]_declaration.md`.

## Version History

- **v3.0.0** — Complete rewrite: strict n8n checkpoints, dedicated trafficking-writer agent, progressive-disclosure skills, command-level Phase 4 review
- **v2.0.0** — Original 4-agent pipeline (extractor, classifier, writer, reviewer)
