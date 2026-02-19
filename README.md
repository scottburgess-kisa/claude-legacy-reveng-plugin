# Claude Legacy Reverse Engineering Plugin

A Claude Code plugin for Defra's Legacy Application Programme (LAP) to aid in the reverse engineering of legacy applications.

## Purpose

This plugin extends Claude Code with specialised tooling and prompts to assist engineers in understanding, documenting, and modernising legacy systems within Defra's estate.

## Prerequisites

- [Claude Code](https://docs.anthropic.com/en/docs/claude-code) installed and authenticated

## Local Development

The plugin is entirely file-based (Markdown and JSON) — there is no build step. Changes to skills, hooks, and configuration are picked up on the next session start.

### Running the plugin locally

```bash
claude --plugin-dir /path/to/claude-legacy-reveng-plugin
```

Or add a shell alias for convenience:

```bash
alias claude-lap='claude --plugin-dir /path/to/claude-legacy-reveng-plugin'
```

### Development workflow

1. Edit a skill, hook, or config file in your editor
2. Start a new Claude Code session with `--plugin-dir` pointing at your local clone
3. Verify the plugin loaded with `/skills` or `/mcp` inside the session
4. Test your changes (e.g. `/defra-legacy-reveng:skill-name`)
5. Iterate — exit the session, tweak files, relaunch

### Tips

- **Skills** are Markdown files — edit and relaunch, nothing to compile.
- **Hooks** run shell commands — test them standalone in your terminal before wiring them into `hooks/hooks.json`.
- **MCP servers**, if added later, are the only component that may require a build step.

## Project Structure

```
claude-legacy-reveng-plugin/
├── .claude-plugin/
│   └── plugin.json       # Plugin manifest
├── skills/               # Reverse engineering skills (slash commands)
├── hooks/
│   └── hooks.json        # Hook configuration
├── agents/               # Custom subagent definitions
├── CLAUDE.md             # Plugin-level context for Claude
└── README.md
```

## Skills

| Skill | Description |
|-------|-------------|
| `image-to-html` | Converts a legacy UI screenshot into semantic HTML |
| `sanitise-transcript` | Removes off-topic content from interview transcripts |
| `extract-workflows` | Extracts user workflows from a sanitised transcript, cross-referencing HTML screens to produce documented workflows with mermaid diagrams |

## Agents

| Agent | Description |
|-------|-------------|
| `interaction-analyst` | Analyses legacy application screens and user workflows, answering questions about screen content, interrelations, and user journeys |

## Status

Early development.
