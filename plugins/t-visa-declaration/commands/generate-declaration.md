---
name: generate-declaration
description: Generate a complete T-visa personal declaration from an interview transcript using the 5-phase pipeline. Produces two outputs: the declaration and attorney review notes.
disable-model-invocation: true
allowed-tools: AskUserQuestion, Read, Write, Task
---

# Generate T-Visa Declaration

Generate a complete T-visa personal declaration from an interview transcript using the 5-phase pipeline. Produces two outputs: the declaration and attorney review notes.

## Usage

```
/t-visa-declaration:generate-declaration [transcript file path or paste transcript]
```

---

## Pipeline Execution

---

## ⛔ STOPPING POINT 1 — Gather Initial Information

Before running any phase, call the `AskUserQuestion` tool with these two questions:

```
AskUserQuestion([
  {
    question: "What is the client's name? (used for file names and document headings)",
    header: "Client name",
    multiSelect: false,
    options: [
      { label: "Type below", description: "Enter the client's full name in the 'Other' field" }
    ]
  },
  {
    question: "Do you have supplemental case notes to add? (reporting dates, medical records, post-interview updates)",
    header: "Case notes",
    multiSelect: false,
    options: [
      { label: "No additional notes", description: "Proceed with the transcript only" },
      { label: "Yes — I'll add them below", description: "Paste them in the 'Other' field" }
    ]
  }
])
```

Save the responses. If no transcript was provided in `$ARGUMENTS`, after AskUserQuestion returns, tell the user: "Please paste the interview transcript now, or give me the file path." Then wait for the transcript before continuing.

---

### PHASE 1 — Fact Extraction

Use the `phase-1-extractor` agent. Provide:
- The full transcript text
- Any supplemental case notes provided

The agent returns a structured JSON of all extracted facts. Save this as `phase1_output`.

Tell the user: "✓ Phase 1 complete — facts extracted. Running classification..."

---

### PHASE 2 — Classification & Gap Analysis

Use the `phase-2-classifier` agent. Provide:
- The `phase1_output` JSON from Phase 1

The agent returns a JSON with three keys:
- `section_assignments` (facts assigned to each section + paragraph plan for trafficking)
- `gap_analysis` (critical, important, and minor gaps with follow-up questions)
- `attorney_decision_points` (decisions requiring attorney input)

Save this as `phase2_output`.

Tell the user: "✓ Phase 2 complete — classification done. I have a few questions before I start writing."

---

## ⛔ STOPPING POINT 2 — Before Writing Begins

**STOP. Call the `AskUserQuestion` tool with the questions below. Do NOT call any section-writing agents until `AskUserQuestion` returns.**

Build the questions array as follows, then call `AskUserQuestion` with it:

---

### Question 1 — Background events (always include, always first)

Look at `phase2_output.section_assignments.background`. These are the facts assigned to the Background section (strictly 2 short paragraphs). Generate one option per fact (up to 4 max — pick the most vulnerability-relevant ones if there are more).

```
{
  question: "The Background section is limited to 2 short paragraphs covering what made [client name] vulnerable to being trafficked. Based on the transcript, I found these events: [list each background fact as a brief readable phrase]. Which are most important to include? Is there anything about [client name]'s life before meeting [trafficker name] that I should know?",
  header: "Background",
  multiSelect: true,
  options: [
    { label: "[Short label for fact 1]", description: "[One sentence: why this shows vulnerability]" },
    { label: "[Short label for fact 2]", description: "[One sentence: why this shows vulnerability]" },
    { label: "[Short label for fact 3]", description: "[One sentence: why this shows vulnerability]" },
    { label: "Include all of them", description: "Use judgment to fit the most important in 2 paragraphs" }
  ]
}
```

---

### Question 2 — Trafficking scope (add ONLY if `attorney_decision_points.multiple_episodes.count > 1`)

```
{
  question: "I found [N] separate trafficking situations in the transcript: [list each briefly]. Which should be the focus of this declaration?",
  header: "Scope",
  multiSelect: false,
  options: [
    { label: "[Episode 1 short label]", description: "Focus on this situation only" },
    { label: "[Episode 2 short label]", description: "Focus on this situation only" },
    { label: "All episodes", description: "Cover all trafficking situations in the declaration" }
  ]
}
```

---

### Question 3 — Background boundary (add ONLY if `attorney_decision_points.background_boundary_issue = true`)

```
{
  question: "It looks like [client name] met [trafficker name] before coming to the US. I need to know where the Background section ends and the trafficking story begins — can you clarify the timeline?",
  header: "Timeline",
  multiSelect: false,
  options: [
    { label: "Met abroad, trafficking started in US", description: "Background = home country life; trafficking = after US arrival" },
    { label: "Trafficking started abroad too", description: "Trafficking began before they came to the US" },
    { label: "Met in US, home country only as context", description: "Background = home country only; met trafficker after arrival" }
  ]
}
```

---

### Questions 4–8 — Critical gaps (top 3–5 only, from `gap_analysis.critical_gaps`)

For each critical gap selected, translate the field name to a plain-English question. Do NOT use legal field names. Build:

```
{
  question: "[Plain-English question about the gap — e.g. 'Were there times [trafficker] physically hurt or threatened [client]? If so, what happened?']",
  header: "[2-3 word label]",
  multiSelect: false,
  options: [
    { label: "[Likely answer 1 based on case context]", description: "..." },
    { label: "[Likely answer 2 based on case context]", description: "..." },
    { label: "I don't know — add placeholder", description: "Add [ATTORNEY REVIEW] tag so attorney can fill in later" }
  ]
}
```

---

**Call `AskUserQuestion` with all composed questions. Wait for it to return. Do not proceed.**

After `AskUserQuestion` returns:
- Save the user's background selection as `background_confirmed_facts`
- Update `phase1_output` with any new information from gap answers
- For gaps answered "I don't know": note the `suggested_placeholder` from `phase2_output.gap_analysis` for use during writing

---

### PHASE 3 — Section Writing (Loop through 9 sections)

Write sections IN THIS ORDER using the `phase-3-writer` agent for each:
1. header
2. introduction
3. background
4. trafficking ← uses paragraph_plan from Phase 2
5. physical_presence
6. law_enforcement
7. extreme_hardship
8. moral_character
9. conclusion

For EACH section, invoke the `phase-3-writer` agent and provide:
- `section_name`: the section being written
- `assigned_facts`: the facts from `phase2_output.section_assignments.[section_name]`
  - For background: use `background_confirmed_facts` (from AskUserQuestion response) instead of the full assignment
- `emp_mapping`: (trafficking only) `phase2_output.section_assignments.trafficking.emp_mapping`
- `paragraph_plan`: (trafficking only) `phase2_output.section_assignments.trafficking.paragraph_plan`
- `gaps_for_this_section`: relevant gaps from `phase2_output.gap_analysis`
- `previously_written_sections`: accumulated text of all sections written so far
- `last_paragraph_number`: the number of the last paragraph written (start at 0 for header)

After each section, collect the output and update:
- `accumulated_draft` (append the new section)
- `last_paragraph_number` (update from agent's LAST_PARAGRAPH_NUMBER output)

Wait for each section to complete before starting the next.

---

### PHASE 4 — Coherence Review

Use the `phase-4-reviewer` agent. Provide:
- The complete `accumulated_draft`

The agent returns the corrected final declaration. Save as `final_declaration`.

---

### PHASE 5 — Attorney Notes

Use the `phase-5-notes` agent. Provide:
- The `final_declaration`
- The `phase2_output.gap_analysis` (for gaps not resolved by user answers)
- The `phase2_output.attorney_decision_points`
- The `phase1_output` (for reference)

The agent returns the attorney notes document. Save as `attorney_notes`.

---

## Output

After Phase 5 completes:

1. Write the declaration to: `output/[client_name]_declaration.md`
2. Write the attorney notes to: `output/[client_name]_attorney_notes.md`

Create the `output/` directory if it doesn't exist.

Then tell the user:
> Done! I've generated:
> - **Declaration**: `output/[client_name]_declaration.md` (~[word_count] words)
> - **Attorney Notes**: `output/[client_name]_attorney_notes.md`
>
> Quick stats:
> - Total word count: [N]
> - Trafficking section: [N] words ([X]% of total)
> - Sections flagged for attorney review: [list any [ATTORNEY REVIEW] tags found]
>
> If the attorney provides feedback, use `/t-visa-declaration:improve-system` to update the pipeline rules.
