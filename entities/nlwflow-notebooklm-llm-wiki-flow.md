---
title: nlwflow (notebooklm-llm-wiki-flow)
category: entities
tags: [tool, notebooklm, hermes, obsidian, qmd]
aliases: [notebooklm-llm-wiki-flow, note-wiki]
relationships:
  - target: "[[skills/windows-korean-locale-python-cli-install]]"
    type: requires
  - target: "[[projects/llmwikipsy/llmwikipsy]]"
    type: used-by
sources: ["claude-history/23-01_LLMWiki-PSY memory: nlwflow-hermes-install"]
summary: reallygood83/notebooklm-llm-wiki-flow — a NotebookLM → LLM-Wiki → Obsidian pipeline, installed as a Hermes CLI "note-wiki" skill and pointed at the 23-01_LLMWiki-PSY vault.
provenance:
  extracted: 0.8
  inferred: 0.2
  ambiguous: 0.0
base_confidence: 0.42
lifecycle: draft
lifecycle_changed: 2026-07-12
tier: supporting
created: 2026-07-12T02:10:00Z
updated: 2026-07-12T02:10:00Z
---

# nlwflow (notebooklm-llm-wiki-flow)

`reallygood83/notebooklm-llm-wiki-flow` — a pipeline that runs NotebookLM research and writes the results directly into an Obsidian LLM-Wiki-style vault. Installed on this machine as a Hermes agent CLI skill (`note-wiki` / `/note-wiki`), not just as a Claude Code skill.

## Key Ideas

- Installed at `C:\Users\yeonr\tools\notebooklm-llm-wiki-flow` — an ASCII-only path deliberately chosen to avoid the encoding bug documented in [[skills/windows-korean-locale-python-cli-install]].
- Config (`.env` in that folder) points `NLWFLOW_OBSIDIAN_VAULT` / `NLWFLOW_WIKI_PATH` at `C:\Users\yeonr\Documents\01-문서1\23-01_LLMWiki-PSY\LLMWiki-PSY` — pages are written at vault root (no separate `LLM-Wiki` subfolder), matching the convention used by the user's other wikis (root-level `entities/`, `concepts/`, etc.).
- Invoked via `nlwflow.sh` (bash) / `nlwflow.ps1` (PowerShell) wrapper scripts, not the raw `.venv/Scripts/nlwflow.exe` — the wrappers set `PYTHONUTF8=1`.
- Registered as a Hermes skill at `~/.hermes/skills/note-wiki/SKILL.md` (hand-written to mirror the tool's native `nlwflow install-claude-skill`, which only supports Claude Code's `~/.claude/commands/note-wiki.md`).
- `qmd` (semantic search indexer) is installed globally via `npm i -g @tobilu/qmd`. The 23-01_LLMWiki-PSY vault is registered as a qmd collection: `qmd collection add <vault> --name llmwikipsy`.
- `notebooklm login` completed for account `yeonrigi@gmail.com`; `nlwflow doctor --json` reported all green (notebooklm, qmd, obsidian_vault_exists, wiki_path_exists).

## Verified End-to-End Run

A real (non-dry-run) `note-wiki` run — an Anthropic vs OpenAI usage-policy comparison — successfully wrote a comparison page, a checklist page, a raw source capture, and an inbox note into the 23-01_LLMWiki-PSY vault, with Korean text intact (thanks to `PYTHONUTF8=1`). See [[projects/llmwikipsy/llmwikipsy]] for the resulting pages.

## Open Questions

- Whether/how this pipeline should be re-pointed at this vault (LW-PSY) or kept scoped to the older 23-01_LLMWiki-PSY vault. ^[inferred]

## Sources

- Claude Code memory file `nlwflow-hermes-install` (project: 23-01_LLMWiki-PSY), 2026-07-06.
