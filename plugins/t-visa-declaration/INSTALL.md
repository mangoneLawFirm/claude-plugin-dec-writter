# T-Visa Declaration Plugin — Installation Guide

## What This Plugin Does

Generates T-visa personal declarations from interview transcripts using a hybrid pipeline:

1. **Extract** (n8n MCP) — pulls all facts from the raw transcript into structured JSON
2. **Classify** (n8n MCP) — assigns facts to sections, creates trafficking paragraph plan, identifies gaps
3. **Ask** (local) — asks you 3-5 critical gap questions in the conversation
4. **Write** (local) — writes all 9 declaration sections using only their assigned facts
5. **Review** (local) — coherence review + quality checks from 8 rounds of attorney feedback
6. **Notes** — generates attorney action items document

**Outputs:** Two files — the declaration + attorney review notes.

---

## Prerequisites

- Claude Cowork account (Pro, Max, Team, or Enterprise plan)
- Claude Code version 1.0.33 or later (for Claude Code users)
- Access to local plugin installation (currently in research preview)
- **n8n Cloud instance** with Phase 1 and Phase 2 workflows deployed (MCP Server Trigger)

---

## Installation

### Option A — Claude Cowork (Web)

1. Open [claude.ai](https://claude.ai) → Cowork tab
2. Click the `+` button or type `/` to open the command palette
3. Select **Manage plugins** → **Install from local folder**
4. Browse to and select the `plugins/t-visa-declaration/` folder
5. Confirm installation

**Verify:** Type `/` in any conversation. You should see `/t-visa-declaration:generate-declaration` in the list.

### Option B — Claude Code (CLI)

Load the plugin in your session:

```bash
claude --plugin-dir ./plugins/t-visa-declaration
```

Or install permanently by adding to your Claude Code settings:

```json
{
  "plugins": ["path/to/plugins/t-visa-declaration"]
}
```

**Verify:** Run `/t-visa-declaration:generate-declaration --help` or type `/` to see the command.

---

## n8n MCP Server Setup

The plugin bundles the MCP server configuration in `.mcp.json` at the plugin root. When you install the plugin, the MCP connection is registered automatically. You only need to update the URL to point to your n8n instance.

### 1. n8n Workflow Requirements

Your n8n instance must have a workflow with an **MCP Server Trigger** node:

| Workflow | Tool Name | Input Parameters | Output |
|----------|-----------|-----------------|--------|
| Extract & Classify | `extract_and_classify` | `transcript` (string), `supplemental_notes` (string) | JSON with `section_assignments`, `gap_analysis`, `attorney_decision_points` |

Configure the tool name and input schema on the MCP Server Trigger node to match the table above.

### 2. Update the MCP URL

After installing the plugin, edit the `.mcp.json` file inside the plugin folder and replace the placeholder URL with your actual n8n Cloud MCP endpoint:

```json
{
  "mcpServers": {
    "n8n-tvisa": {
      "type": "sse",
      "url": "https://YOUR-N8N-INSTANCE.app.n8n.cloud/mcp/sse"
    }
  }
}
```

### 3. Verify Connection

Restart your Cowork session and check that the tool `mcp__n8n-tvisa__extract_and_classify` is available.

---

## Running Your First Declaration

1. Open a Claude Cowork (or Claude Code) conversation
2. Type: `/t-visa-declaration:generate-declaration`
3. When prompted, either:
   - Paste the transcript directly into the conversation, OR
   - Provide the file path (e.g., `examples/transcription.md`)
4. Optionally provide supplemental case notes when asked
5. Claude will run Phase 1 and Phase 2 automatically, then ask you **3-5 critical questions** about information gaps
6. Answer the questions in plain language — Claude understands natural responses
7. Claude writes all 9 sections, runs the coherence review, and generates attorney notes
8. Find the outputs in the `output/` folder:
   - `output/[client_name]_declaration.md`
   - `output/[client_name]_attorney_notes.md`

---

## Updating Pipeline Rules (After Attorney Feedback)

The plugin's rules live in editable files. To update them after attorney feedback:

1. Run the existing skill: `/feedback-to-system-improvement` (in your main `.claude/skills/`)
2. Or manually edit the relevant agent file:
   - `agents/phase-3-writer.md` — writing rules for each section
   - `agents/phase-4-reviewer.md` — quality checks and corrections
   - `skills/quality-benchmarks/SKILL.md` — pass/fail criteria
   - `skills/system-rules/SKILL.md` — all attorney-verified rules
3. In Cowork: click **Reload plugin** (or restart the session) — changes take effect immediately
4. In Claude Code: restart the session with `--plugin-dir`

**No reinstallation needed** for rule updates — just edit the `.md` files.

---

## Sharing the Plugin with Your Team

**Individual sharing:**
1. Zip the `plugins/t-visa-declaration/` folder
2. Share the zip
3. Each team member extracts and installs following steps above

**Enterprise (Team/Enterprise Cowork plan):**
- Upload to your private plugin marketplace
- Team members install with one click from the marketplace

---

## Folder Structure Reference

```
claude-plugin-dec-writter/              ← Repo root (marketplace)
├── .claude-plugin/
│   └── marketplace.json                ← Marketplace manifest
│
├── plugins/
│   └── t-visa-declaration/             ← Plugin root
│       ├── .claude-plugin/
│       │   └── plugin.json             ← Plugin manifest (name, version)
│       ├── .mcp.json                   ← n8n MCP server config (bundled)
│       ├── commands/
│       │   └── generate-declaration.md ← /t-visa-declaration:generate-declaration
│       ├── agents/
│       │   ├── phase-3-writer.md       ← Section writer (called 9 times, once per section)
│       │   ├── phase-4-reviewer.md     ← Coherence review + quality corrections
│       │   └── phase-5-notes.md        ← Attorney notes generator
│       ├── skills/
│       │   ├── quality-benchmarks/
│       │   │   └── SKILL.md            ← Always-active quality rules
│       │   └── system-rules/
│       │       └── SKILL.md            ← All rules from 8 attorney feedback rounds
│       └── INSTALL.md                  ← This file
│
└── README.md
```

---

## Troubleshooting

**Plugin not appearing in command list:**
- Check that `plugins/t-visa-declaration/.claude-plugin/plugin.json` exists and is valid JSON
- Restart the Cowork session or Claude Code session

**Declaration is too long:**
- Check if the transcript has multiple trafficking episodes — the plugin will ask you which to focus on
- Review the attorney notes for "over-expansion" flags

**Spanish words appearing in the output:**
- This is a HARD FAIL caught by Phase 4 review
- If it still happens, manually remove and report as a bug by opening an issue

**Phase 1 or 2 not responding (n8n MCP):**
- Check that your n8n workflows are active (not paused)
- Verify the MCP server URL in the plugin's `.mcp.json` matches your n8n instance
- Check n8n execution logs for errors
- Ensure the MCP Server Trigger node has the tool name `extract_and_classify`
- Restart the Cowork session after any `.mcp.json` changes

**Phase 3 or 4 not responding (local agents):**
- These run as Claude Code subagents — if one fails, the main conversation will show an error
- Try re-running the command; if the issue persists, run phases individually using the agent names directly

---

## Related Resources

These files in the main project (outside the plugin) support the system:

| Resource | Purpose |
|----------|---------|
| `resources/feedback-to-system-improvement.md` | How to convert attorney feedback into permanent rules |
| `resources/quality-benchmarks.md` | Full quality benchmark reference (source for skill file) |
| `memory/system-rules-log.md` | Complete history of all rules added from each feedback round |
| `examples/golden-outputs/` | Reference JSON for phases 1 and 2 |
| `examples/decs - jose mauricio/` | Example case with all draft versions + attorney feedback |
