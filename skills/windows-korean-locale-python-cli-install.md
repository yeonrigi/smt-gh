---
title: Windows Korean-locale Python CLI install gotchas
category: skills
tags: [windows, python, encoding, locale]
aliases: [PYTHONUTF8, cp949 encoding bug]
relationships:
  - target: "[[entities/nlwflow-notebooklm-llm-wiki-flow]]"
    type: applies-to
sources: ["claude-history/23-01_LLMWiki-PSY memory: windows-korean-locale-python-encoding"]
summary: On this Windows machine (Korean/cp949 locale), Python CLI tools break silently on non-ASCII paths/output unless PYTHONUTF8=1 is set and the tool is installed at an ASCII-only path.
provenance:
  extracted: 0.85
  inferred: 0.15
  ambiguous: 0.0
base_confidence: 0.42
lifecycle: draft
lifecycle_changed: 2026-07-12
tier: supporting
created: 2026-07-12T02:10:00Z
updated: 2026-07-12T02:10:00Z
---

# Windows Korean-locale Python CLI install gotchas

This machine's system locale is Korean (cp949). Two related Python bugs surface whenever a path or string contains non-ASCII characters (e.g. Korean folder names like `01-문서1`), unless the process runs with `PYTHONUTF8=1`.

## Key Ideas

- **Editable installs (`pip install -e`) silently fail to import** if the project path contains non-ASCII characters. CPython's `site.py` hardcodes `encoding="locale"` when reading `.pth` files, so a UTF-8-written `.pth` file gets decoded as cp949, the path line is garbled, `os.path.exists()` returns `False`, and the src dir never makes it onto `sys.path` — with no error message at all.
  - **Fix**: install the venv / editable package at an ASCII-only path (e.g. under `C:\Users\yeonr\tools\...`), even if the *data* it operates on lives under a Korean-named path. Runtime file reads that explicitly pass `encoding="utf-8"` are unaffected — only the `.pth` / site-processing step at interpreter startup is broken.
- **stdout/argument text silently corrupts** for Korean characters in CLI output (titles, resolved paths, etc.) unless `PYTHONUTF8=1` is set before the process starts. Without it, Python's default text I/O encoding follows the console locale (cp949), and downstream UTF-8 consumers (files, an agent's Read tool) see mangled text or replacement characters.
  - **Fix**: always export `PYTHONUTF8=1` (bash) / `$env:PYTHONUTF8="1"` (PowerShell) before invoking any Python CLI that might handle Korean text on this machine.

## How to Apply

Whenever installing a new Python-based CLI tool on this machine:

1. Put the venv/repo at an ASCII-only path.
2. Wrap the tool's entrypoint in a small launcher script that sets `PYTHONUTF8=1` before exec'ing the real executable, rather than relying on the raw `.venv/Scripts/<tool>.exe`.

See [[entities/nlwflow-notebooklm-llm-wiki-flow]] for the concrete install that this pattern was worked out for (`nlwflow.sh` / `nlwflow.ps1`).

A related failure mode hit the same install: `subprocess.run(["qmd", ...])` without `shell=True` couldn't resolve the npm-installed `qmd.cmd` shim by bare name on Windows — fixed by pointing the tool's config at the absolute path (`C:/Users/yeonr/AppData/Roaming/npm/qmd.cmd`) instead of the bare command. ^[inferred]

## Open Questions

- Whether this generalizes to other locale-sensitive Windows CLIs beyond Python (untested).

## Sources

- Claude Code memory file `windows-korean-locale-python-encoding` (project: 23-01_LLMWiki-PSY), 2026-07-06.
