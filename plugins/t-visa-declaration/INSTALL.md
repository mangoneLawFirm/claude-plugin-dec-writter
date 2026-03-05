# Installation Guide

## Prerequisites

1. **Claude desktop app** with Cowork mode enabled
2. **n8n instance** accessible via HTTPS with the T-visa extraction workflow active

## Step 1 — Install the Plugin

Open Claude desktop → Settings → Plugins → Install from file → select `t-visa-declaration.plugin`.

Or drag the `.plugin` file into the Cowork chat window.

## Step 2 — Configure n8n

The plugin connects to n8n via MCP (Server-Sent Events). The default URL is:

```
https://n8n.n8nmanguito.lat/mcp/extract-dec-events
```

If your n8n instance is at a different URL, edit `.mcp.json` inside the plugin directory:

```json
{
  "mcpServers": {
    "n8n-tvisa": {
      "type": "sse",
      "url": "YOUR_N8N_URL_HERE"
    }
  }
}
```

### n8n Workflow Requirements

The n8n workflow must expose a tool called `extract_and_classify` that accepts a `transcript` string and returns JSON with these keys:

- `section_assignments` — facts organized by section (background, trafficking, physical_presence, law_enforcement, extreme_hardship, moral_character, introduction, conclusion)
- `gap_analysis` — critical and non-critical gaps with placeholder text
- `attorney_decision_points` — flags for multiple episodes, background boundary issues, etc.

The trafficking section assignment must also include `emp_mapping` and `paragraph_plan`.

## Step 3 — Verify

1. Open a new Cowork session
2. Type: "Generate a T-visa declaration"
3. The plugin should ask for the client's name and trafficking type
4. When it calls the n8n tool, verify the extraction returns valid JSON

If the n8n call fails, check:
- Is the n8n workflow active (not paused)?
- Is the MCP endpoint URL correct?
- Can your machine reach the n8n server?

## Troubleshooting

| Problem | Solution |
|---------|----------|
| "n8n extraction tool failed to respond" | Check n8n is running and workflow is active |
| Plugin not appearing in Cowork | Reinstall the `.plugin` file |
| Sections missing from output | Check n8n returns all required `section_assignments` keys |
| Trafficking section too VAWA-like | Review the trafficking-writer agent rules; ensure n8n EMP mapping is correct |
