# Changelog

All notable changes to this plugin are documented in this file.

## Unreleased

- no unreleased changes

## 1.1.3

- synced plugin skills with the current `/home/dreamtcs/skills` AI Services updates, including Summarizer and Translator
- updated Zoom MCP guidance for the seven-tool main server surface and cross-Zoom `search_zoom` workflows
- added the optional Team Chat MCP child skill while keeping `.mcp.json` limited to the existing bundled servers
- refreshed Video SDK Web references and Meeting SDK example updates from the canonical skills tree

## Earlier

- aligned the repository with the current Claude plugin structure around `.claude-plugin/plugin.json`, `skills/`, and `.mcp.json`
- added Claude-facing installation and connector documentation
- converted command-style workflows into `SKILL.md`-based workflows under `skills/`
- bundled Zoom MCP server configuration in `.mcp.json`
- tightened skill metadata and reduced maintainer-facing wording in user-facing docs
