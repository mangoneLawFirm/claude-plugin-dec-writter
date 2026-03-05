---
name: generate-declaration
description: Generate a complete T-visa personal declaration from an interview transcript
allowed-tools: AskUserQuestion, Read, Write, Task, Bash, Glob, mcp__n8n-tvisa__extract_and_classify, mcp__n8n-tvisa__get_transcription
---

# Generate T-Visa Declaration

Generate a complete T-visa personal declaration from an interview transcript using a 4-phase pipeline.

---

## CRITICAL RULE — URL HANDLING

**NEVER use browser tools, Google Drive tools, WebFetch, or any other method to open or fetch URLs provided by the user.** ALL URLs (Google Docs, GoTo Connect, or any other) MUST be passed directly to the n8n MCP tool `mcp__n8n-tvisa__get_transcription`. The n8n workflow handles authentication and document fetching on the server side. Do NOT attempt to read, open, or fetch the URL yourself under any circumstances.

---

## PHASE 0 — Gather Information

### Step 0.1 — Ask for client details and transcript URL

Call `AskUserQuestion` with these questions:

```
[
  {
    "question": "What is the client's full legal name? (used for file names and the declaration heading)",
    "header": "Client name",
    "multiSelect": false,
    "options": [
      { "label": "Type below", "description": "Enter the client's full name in the text field" }
    ]
  },
  {
    "question": "What type of trafficking is this case?",
    "header": "Case type",
    "multiSelect": false,
    "options": [
      { "label": "Labor trafficking", "description": "Forced labor, domestic servitude, wage theft" },
      { "label": "Sex trafficking", "description": "Commercial sex acts by force/fraud/coercion" },
      { "label": "Both labor and sex trafficking", "description": "Elements of both types" }
    ]
  },
  {
    "question": "Paste the URL to the interview transcript (or choose to provide the transcript text manually).",
    "header": "Transcript URL",
    "multiSelect": false,
    "options": [
      { "label": "Paste URL below", "description": "Provide the transcript URL and I'll fetch it automatically via n8n" },
      { "label": "I'll paste the transcript manually", "description": "Skip automatic fetch — I'll provide the transcript text or file path" }
    ]
  },
  {
    "question": "Do you have supplemental case notes to add? (reporting dates, medical records, post-interview updates)",
    "header": "Case notes",
    "multiSelect": false,
    "options": [
      { "label": "No additional notes", "description": "Proceed with the transcript only" },
      { "label": "Yes — I'll add them below", "description": "Paste case notes in the text field" }
    ]
  }
]
```

Save: `client_name`, `trafficking_type`, `transcript_url` (or manual flag), `supplemental_notes`.

### Step 0.2 — Get the transcript

**Option A — Transcript URL provided:**

If the user provided a transcript URL (Google Docs, GoTo Connect, or any other URL):

**DO NOT open, browse, or fetch the URL yourself. DO NOT use WebFetch, Google Drive tools, Chrome tools, or any other tool to access the URL. ALWAYS pass the raw URL directly to n8n.**

Call the n8n MCP tool:

```
mcp__n8n-tvisa__get_transcription({ "url": transcript_url })
```

**MANDATORY ERROR CHECK — After the tool call returns:**

1. If the tool call **failed**, **timed out**, or **returned empty/null**: STOP. Tell the user:
   > "I couldn't fetch the transcript from the URL provided. The URL may be invalid or the n8n workflow may be down. Please check the URL and try again, or paste the transcript manually."
   DO NOT silently continue without a transcript.

2. If the tool call **returned data but it's not a usable transcript** (too short, HTML error page, etc.): STOP. Tell the user what was returned and ask them to verify the URL or paste manually.

3. If the tool call **succeeded and returned a transcript**: Save the full text as `transcript`. Tell the user: "Transcript fetched successfully. Proceeding with extraction."

**Option B — Manual transcript:**

If the user chose to provide the transcript manually:
- Tell the user: "Please paste the interview transcript now, or give me the file path."
- Wait for the transcript.
- If a file path was given, read it with the `Read` tool.

Save the full text as `transcript`.

---

## PHASES 1-2 — Extraction + Classification (n8n MCP)

### Step 1 — Call n8n

**STRICT CHECKPOINT — DO NOT SKIP THIS STEP.**

Call `mcp__n8n-tvisa__extract_and_classify` with:
- `transcript`: the full transcript text

**MANDATORY ERROR CHECK — After the tool call returns:**

1. If the tool call **failed**, **timed out**, or **returned empty/null**: STOP IMMEDIATELY. Tell the user:
   > "The n8n extraction tool failed to respond. This could mean the n8n server is down or the workflow is paused. Please check that your n8n instance at the URL in `.mcp.json` is running and the workflow is active. I cannot continue without the extraction results."
   DO NOT attempt to extract locally. DO NOT silently continue. STOP.

2. If the tool call **returned data but it's not valid JSON** or is missing required keys (`section_assignments`, `gap_analysis`, `attorney_decision_points`): STOP. Tell the user what was returned and that it's malformed.

3. If the tool call **succeeded and returned valid JSON with all 3 keys**: Save as `phase2_output`. Continue.

Tell the user: "Extraction and classification complete. Let me review the results and ask a few questions before writing."

---

## STOPPING POINT — Pre-Writing Questions

**STOP. Build questions and call `AskUserQuestion`. Do NOT write any sections until answers are received.**

### Question 1 — Background events (ALWAYS include)

Look at `phase2_output.section_assignments.background`. Generate one option per key fact (up to 4). Pick the most vulnerability-relevant.

```
{
  "question": "The Background section is limited to 2 short paragraphs about what made [client_name] vulnerable. I found these key facts: [list each]. Which are most important? Anything else about [client_name]'s life before meeting the trafficker?",
  "header": "Background",
  "multiSelect": true,
  "options": [
    { "label": "[Fact 1 short label]", "description": "[Why this shows vulnerability]" },
    { "label": "[Fact 2 short label]", "description": "[Why this shows vulnerability]" },
    { "label": "[Fact 3 short label]", "description": "[Why this shows vulnerability]" },
    { "label": "Include all of them", "description": "Use judgment to fit the most important in 2 paragraphs" }
  ]
}
```

### Question 2 — Trafficking scope (ONLY if `attorney_decision_points.multiple_episodes.count > 1`)

```
{
  "question": "I found [N] separate trafficking situations: [list each]. Which should be the focus?",
  "header": "Scope",
  "multiSelect": false,
  "options": [
    { "label": "[Episode 1]", "description": "Focus on this only" },
    { "label": "[Episode 2]", "description": "Focus on this only" },
    { "label": "All episodes", "description": "Cover all situations" }
  ]
}
```

### Question 3 — Background boundary (ONLY if `attorney_decision_points.background_boundary_issue = true`)

```
{
  "question": "It looks like [client_name] met the trafficker before coming to the US. Where does the Background end and trafficking begin?",
  "header": "Timeline",
  "multiSelect": false,
  "options": [
    { "label": "Met abroad, trafficking in US", "description": "Background = home country; trafficking = after US arrival" },
    { "label": "Trafficking started abroad", "description": "Trafficking began before US entry" },
    { "label": "Met in US, home country as context", "description": "Background = home country only; met trafficker after arrival" }
  ]
}
```

### Questions 4-8 — Critical gaps (top 3-5 from `gap_analysis.critical_gaps`)

For each critical gap, translate to plain English:

```
{
  "question": "[Plain-English question — e.g., 'Were there times the trafficker physically hurt or threatened [client_name]?']",
  "header": "[2-3 word label]",
  "multiSelect": false,
  "options": [
    { "label": "[Likely answer 1]", "description": "..." },
    { "label": "[Likely answer 2]", "description": "..." },
    { "label": "I don't know — add placeholder", "description": "Will add [ATTORNEY REVIEW] tag" }
  ]
}
```

**Call `AskUserQuestion` with all questions. Wait for answers. Do NOT proceed until answers are received.**

After answers:
- Save background selections as `background_confirmed_facts`
- Update extracted data with gap answers
- For "I don't know" answers: note the placeholder text from `gap_analysis` for use during writing

---

## PHASE 3 — Section Writing

**IMPORTANT: Read the writing-rules skill FIRST.** Before launching any writer agents, call `Read` on `${CLAUDE_PLUGIN_ROOT}/skills/writing-rules/SKILL.md` to load the shared voice and language rules. These rules apply to ALL sections.

### Step 3.1 — Write All Independent Sections in Parallel (Wave 1)

Launch ALL of these sections simultaneously via parallel `Task` calls:

**Trafficking** (dedicated agent):
```
Task(subagent_type: "t-visa-declaration:trafficking-writer", prompt: "
Write the trafficking section for [client_name]'s T-visa declaration.

TRAFFICKING TYPE: [trafficking_type]

ASSIGNED FACTS:
[phase2_output.section_assignments.trafficking]

EMP MAPPING:
[phase2_output.section_assignments.trafficking.emp_mapping]

PARAGRAPH PLAN:
[phase2_output.section_assignments.trafficking.paragraph_plan]

GAPS FOR THIS SECTION:
[relevant gaps with placeholder text]

STARTING PARAGRAPH NUMBER: [N] (after background)

SUPPLEMENTAL NOTES:
[supplemental_notes or 'None']
")
```

**Background, Header, Introduction, Law Enforcement, Moral Character, Conclusion** (section-writer agent, one Task per section — all launched in the SAME parallel batch as trafficking):

```
Task(subagent_type: "t-visa-declaration:section-writer", prompt: "
Write the [SECTION_NAME] section for [client_name]'s T-visa declaration.

TRAFFICKING TYPE: [trafficking_type]

Read the section-specific instructions from: ${CLAUDE_PLUGIN_ROOT}/skills/writing-rules/references/section-[SECTION_NAME].md

ASSIGNED FACTS:
[phase2_output.section_assignments.[section_name]]

GAPS FOR THIS SECTION:
[relevant gaps with placeholder text]

STARTING PARAGRAPH NUMBER: [N]
")
```

Wait for ALL Wave 1 sections to complete.

**CHECKPOINT:** Verify every section returned non-empty output. If any failed, tell the user which one and offer to retry.

### Step 3.2 — Write Dependent Sections Sequentially (Wave 2)

These sections need previously written content for continuity. Run them in order:

1. **Physical Presence** — pass the trafficking section text for continuity:
```
Task(subagent_type: "t-visa-declaration:section-writer", prompt: "
Write the physical_presence section for [client_name]'s T-visa declaration.

Read the section-specific instructions from: ${CLAUDE_PLUGIN_ROOT}/skills/writing-rules/references/section-physical-presence.md

TRAFFICKING TYPE: [trafficking_type]

ASSIGNED FACTS:
[phase2_output.section_assignments.physical_presence]

PREVIOUSLY WRITTEN — TRAFFICKING SECTION (for continuity, do NOT repeat):
[trafficking section output]

STARTING PARAGRAPH NUMBER: [N]
")
```

2. **Extreme Hardship** — pass physical presence for continuity:
```
Task(subagent_type: "t-visa-declaration:section-writer", prompt: "
Write the extreme_hardship section for [client_name]'s T-visa declaration.

Read the section-specific instructions from: ${CLAUDE_PLUGIN_ROOT}/skills/writing-rules/references/section-extreme-hardship.md

TRAFFICKING TYPE: [trafficking_type]

ASSIGNED FACTS:
[phase2_output.section_assignments.extreme_hardship]

PREVIOUSLY WRITTEN — PHYSICAL PRESENCE SECTION (for continuity, do NOT repeat):
[physical_presence section output]

STARTING PARAGRAPH NUMBER: [N]
")
```

Wait for Wave 2 to complete.

**CHECKPOINT:** Verify both sections returned non-empty output.

### Step 3.3 — Assemble the Declaration

Join all sections in this canonical order:
1. header
2. introduction
3. background
4. trafficking
5. physical_presence
6. law_enforcement
7. extreme_hardship
8. moral_character
9. conclusion

**Renumber paragraphs sequentially** from 1 through the entire declaration. Replace section markers with proper headings:
- Introduction: NO heading (flows under title/preamble)
- Background: "Background Information"
- Trafficking: "Severe Form of Trafficking in the U.S."
- Physical Presence: "Physical Presence on Account of Trafficking"
- Law Enforcement: "Cooperation with Law Enforcement"
- Extreme Hardship: "Extreme Hardship in [Country]"
- Moral Character: "Good Moral Character and Waiver of Inadmissibility"
- Conclusion: "Conclusion"

Save as `assembled_draft`.

---

## PHASE 4 — Quality Review

**Read the quality-benchmarks skill** before reviewing: `Read` on `${CLAUDE_PLUGIN_ROOT}/skills/quality-benchmarks/SKILL.md`.
**Read the system-rules skill**: `Read` on `${CLAUDE_PLUGIN_ROOT}/skills/system-rules/SKILL.md`.

Review the assembled draft against ALL rules. Check and fix directly:

### HARD FAIL checks (fix immediately):
1. **First-person singular**: Scan for "us/we/our" in victim experience. Rewrite to "I/me/my".
2. **Firing fear**: Scan for "afraid they would fire me" / "scared of losing the job". Replace with immigration/harm fear.
3. **English-only**: Scan for ANY Spanish text. Translate all to English.
4. **VAWA filter**: Every trafficking paragraph must connect MEANS to ENDS. Flag or reframe paragraphs that only describe abuse.
5. **Means-Ends pairing**: Every Means tied to specific End. Add connection or flag [ATTORNEY REVIEW].
6. **Sexual violence language**: Replace "sexually abused/assaulted" with "sexual slave/servitude".

### Cross-section repetition check (CRITICAL — fix immediately):
7. **Background → Trafficking overlap**: Read the Background and Trafficking sections side by side. If the Trafficking section restates facts already covered in Background (e.g., how the client met the trafficker, home country conditions, family situation, journey details), those facts MUST be rewritten in Trafficking to reference them as prior context, NOT introduced as new information. Use bridging phrases like "As described above," or "Having already [fact from background]," or simply omit the repeated detail and pick up the narrative where Background left off. The Trafficking section must read as a CONTINUATION, not a restart.
8. **Any section → Any section overlap**: Scan the full assembled draft for facts, events, or details that appear in more than one section. For each duplicate: keep it in the section where it is most legally relevant, and remove or compress it to a brief reference in the other section. No fact should be presented as new information if the reader has already encountered it earlier in the declaration.

### Structure checks:
9. **Proportions**: Trafficking MUST be longest. PP 150-300 words. EH 250-400 words. Total 2,500-3,500.
10. **Active voice**: Fix passive constructions. Special attention to journey/process narration.
11. **Paragraph openings**: Max 2-3 trafficking paragraphs open with trafficker's name as first-clause subject.
12. **Consistency**: Names, dates, amounts match throughout. No contradictions.
13. **Prohibited words**: Replace all (~35 prohibited terms).
14. **Introduction**: NO heading. Flows under title/preamble.
15. **Conclusion**: 3-5 sentences. Plea, not summary.
16. **PP content boundary**: Only trafficking-nexus content. No country fears.
17. **EH legal standard**: Must cite INA § 101(a)(15)(T)(i)(IV) and 8 C.F.R. § 214.209.
18. **Country conditions**: EH must include dedicated paragraph.
19. **Enforcement mechanisms**: Every "forced me" must explain HOW.
20. **Isolation integrity**: No details that prove victim had outside support.
21. **Servitude contrast**: Trafficker's idleness vs victim's forced labor.

For issues you cannot fix (needs attorney input), mark: `[ATTORNEY REVIEW: reason]`

Save the corrected version as `final_declaration`.

---

## Output

Write the final declaration to: `output/[client_name_sanitized]_declaration.md`

Create the `output/` directory if needed.

Tell the user:

> Done! Declaration generated for [client_name].
>
> **Stats:**
> - Total words: [N]
> - Trafficking section: [N] words ([X]% of total)
> - Sections flagged for attorney review: [list any [ATTORNEY REVIEW] tags]
>
> The file is ready at `output/[filename]`.
