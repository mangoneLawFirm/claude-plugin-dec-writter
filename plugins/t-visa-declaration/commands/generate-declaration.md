---
name: generate-declaration
description: Generate a complete T-visa personal declaration from an interview transcript. Phases 1-2 run on n8n (via MCP), phases 3-4 run locally.
disable-model-invocation: true
allowed-tools: AskUserQuestion, Read, Write, Task, mcp__n8n-tvisa__extract_and_classify
---

# Generate T-Visa Declaration

Generate a complete T-visa personal declaration from an interview transcript. Phases 1 and 2 run on n8n via MCP tool calls. Phases 3 and 4 run locally as Claude Code agents.

## Usage

```
/t-visa-declaration:generate-declaration [transcript file path or paste transcript]
```

## Prerequisites

The n8n MCP server must be configured in `.mcp.json` with server name `n8n-tvisa`. The server must expose one tool:
- `extract_and_classify` — extracts all facts from the transcript and classifies them into declaration sections

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

### PHASES 1 & 2 — Extraction + Classification (n8n MCP)

Call the MCP tool `mcp__n8n-tvisa__extract_and_classify` with:
- `transcript`: The full transcript text
- `supplemental_notes`: Any supplemental case notes provided (empty string if none)

The tool extracts all facts from the transcript and classifies them into declaration sections in a single step. It returns a JSON with three keys:
- `section_assignments` (facts assigned to each section + paragraph plan for trafficking)
- `gap_analysis` (critical, important, and minor gaps with follow-up questions)
- `attorney_decision_points` (decisions requiring attorney input)

Save this as `phase2_output`.

Tell the user: "Extraction and classification complete. I have a few questions before I start writing."

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

### PHASE 3 — Section Writing (Parallel Batches)

Sections are split into two batches that run **concurrently**:

**Batch A** — Independent sections (launch ALL 5 simultaneously as parallel Task calls):
- `header`
- `introduction`
- `law_enforcement`
- `moral_character`
- `conclusion`

**Batch B** — Dependent chain (run sequentially, each waits for the prior):
- `background` → `trafficking` → `physical_presence` → `extreme_hardship`

**Launch Batch A and Batch B at the same time.** Batch A sections run in parallel with each other AND with the Batch B chain.

---

#### Batch A — Parallel sections

For each Batch A section, invoke the `phase-3-writer` agent with:
- `section_name`: the section being written
- `assigned_facts`: the facts from `phase2_output.section_assignments.[section_name]`
- `gaps_for_this_section`: relevant gaps from `phase2_output.gap_analysis`
- `previously_written_sections`: "" (empty — these sections have no cross-dependencies)
- `last_paragraph_number`: 0 (temporary — will be renumbered during assembly)

Batch A sections use **temporary paragraph numbering** starting from 1. The assembly step will assign final numbers.

---

#### Batch B — Sequential chain

Run these 4 sections in order. Each section waits for the prior to complete before starting.

**Step B1 — background:**
- `section_name`: "background"
- `assigned_facts`: use `background_confirmed_facts` (from AskUserQuestion response)
- `gaps_for_this_section`: relevant gaps
- `previously_written_sections`: "" (empty — background is the chain start)
- `last_paragraph_number`: 0

Save the background output and its `LAST_PARAGRAPH_NUMBER`.

**Step B2 — trafficking:**
- `section_name`: "trafficking"
- `assigned_facts`: `phase2_output.section_assignments.trafficking`
- `emp_mapping`: `phase2_output.section_assignments.trafficking.emp_mapping`
- `paragraph_plan`: `phase2_output.section_assignments.trafficking.paragraph_plan`
- `gaps_for_this_section`: relevant gaps
- `previously_written_sections`: the **background section text only** (NOT all prior sections)
- `last_paragraph_number`: from background's output

Save the trafficking output and its `LAST_PARAGRAPH_NUMBER`.

**Step B3 — physical_presence:**
- `section_name`: "physical_presence"
- `assigned_facts`: `phase2_output.section_assignments.physical_presence`
- `gaps_for_this_section`: relevant gaps
- `previously_written_sections`: the **trafficking section text only**
- `last_paragraph_number`: from trafficking's output

Save the physical_presence output and its `LAST_PARAGRAPH_NUMBER`.

**Step B4 — extreme_hardship:**
- `section_name`: "extreme_hardship"
- `assigned_facts`: `phase2_output.section_assignments.extreme_hardship`
- `gaps_for_this_section`: relevant gaps
- `previously_written_sections`: the **physical_presence section text only**
- `last_paragraph_number`: from physical_presence's output

---

#### Assembly — After BOTH batches complete

Wait for all Batch A and Batch B tasks to finish. Then assemble the `accumulated_draft` by joining sections in this canonical order:

1. header (from Batch A)
2. introduction (from Batch A)
3. background (from Batch B)
4. trafficking (from Batch B)
5. physical_presence (from Batch B)
6. law_enforcement (from Batch A)
7. extreme_hardship (from Batch B)
8. moral_character (from Batch A)
9. conclusion (from Batch A)

**Renumber paragraphs sequentially.** Batch B sections already have correct relative numbering within the chain (background → trafficking → PP → EH). Batch A sections used temporary numbers. Walk through the assembled draft and renumber all paragraphs (¶1, ¶2, ¶3, ...) sequentially from start to end. This is a simple text replacement — find each paragraph number and replace with the correct sequential number.

---

### PHASE 4 — Coherence Review

Use the `phase-4-reviewer` agent. Provide:
- The complete `accumulated_draft`

The agent returns the corrected final declaration. Save as `final_declaration`.

---

## Output

After Phase 4 completes:

1. Write the declaration to: `output/[client_name]_declaration.md`

Create the `output/` directory if it doesn't exist.

Then tell the user:
> Done! I've generated:
> - **Declaration**: `output/[client_name]_declaration.md` (~[word_count] words)
>
> Quick stats:
> - Total word count: [N]
> - Trafficking section: [N] words ([X]% of total)
> - Sections flagged for attorney review: [list any [ATTORNEY REVIEW] tags found]
>
> If the attorney provides feedback, use `/t-visa-declaration:improve-system` to update the pipeline rules.
