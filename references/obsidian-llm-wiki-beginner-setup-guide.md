---
title: "Obsidian LLM-Wiki: 초보자 설치부터 사용까지 (clipped guide)"
category: references
tags: [obsidian, llm-wiki, tutorial, setup]
aliases: [LLM Wiki 설치 가이드]
relationships: []
sources: ["23-01_LLMWiki-PSY/Clippings/Obsidian-LLM-Wiki 초보자 설치부터 사용까지 2026-07-07T160225+0900.md"]
source_urls:
  - https://jesusiswith.us/class/llm-wiki/obsidian-llm-wiki-초보자-설치부터-사용까지/
summary: Web-clipped Korean beginner tutorial for setting up Andrej Karpathy's Obsidian+LLM+Wiki pattern using Antigravity IDE — folder setup, Obsidian install, the Ar9av/obsidian-wiki repo, and the ingest → categorize → index workflow.
provenance:
  extracted: 0.85
  inferred: 0.15
  ambiguous: 0.0
base_confidence: 0.55
lifecycle: draft
lifecycle_changed: 2026-07-12
tier: peripheral
created: 2026-07-12T03:05:00Z
updated: 2026-07-12T03:05:00Z
---

# Obsidian LLM-Wiki: 초보자 설치부터 사용까지 (clipped guide)

jesusiswith.us에서 클리핑한 튜토리얼. Antigravity IDE + AI 채팅으로 Andrej Karpathy의 "Obsidian + LLM + Wiki" 패턴을 처음부터 구축하는 절차를 설명한다. 이 vault(`LW-PSY`)를 만들 때 사용한 것과 같은 패턴의 초심자용 설명이다.

## Key Ideas

- **폴더 구성**: 단일 기기면 로컬 폴더(`MyWiki` 등)를, 여러 기기 동기화가 필요하면 Obsidian Sync 또는 iCloud/Google Drive/Dropbox/OneDrive 경유 폴더를 만든다.
- **Obsidian Web Clipper**: 저장 위치를 `_sources/Clipping`으로 지정해 웹 스크랩 자료를 원본 폴더에 모은다.
- **Antigravity IDE**를 통해 AI 채팅으로 `Ar9av/obsidian-wiki` 저장소를 설치하고, `_sources/` 폴더 생성 및 `OBSIDIAN_SOURCES_DIR` 지정까지 자연어 지시로 수행한다.
- **`generate_index.py` 스킬**: `_sources/` 하위 폴더별 `_index.md`와 상위 마스터 인덱스를 자동 생성/갱신하는 스크립트를 `/generate-index` 슬래시 명령으로 등록.

## 7단계 워크플로우 요약

1. **입력** — 영구 보관할 원본(PDF/논문/매뉴얼)은 `_sources/`에, 일회성 메모는 `_raw/`에 둔다. Claude 대화 기록은 `/claude-history-ingest`, 외부 데이터(ChatGPT export, Slack 로그 등)는 `/data-ingest`, 현재 코드 프로젝트는 `/wiki-update`로 가져온다.
2. **Ingest 실행** — `/wiki-ingest`가 `.manifest.json`을 확인해 이미 처리한 파일은 건너뛰고 새/변경분만 델타 처리한다.
3. **내부 파이프라인** — Ingest(원문 그대로 읽기) → Extract(개념/엔티티/절차/수치/관계/미해결 질문 분리) → Resolve(기존 페이지 병합 또는 신규 생성, 중복 방지) → Schema(폴더 일관성 유지, 깨진 링크 방지, index.md 갱신).
4. **출력 분류** — `concepts/`(추상 개념), `entities/`(인물·조직·도구), `skills/`(방법론), `references/`(수치·스펙·원문 요약), `synthesis/`(교차분석 인사이트), `journal/`(날짜 중심 기록), `projects/`(프로젝트별 지식).
5. **부수 기록** — `.manifest.json`(처리 이력), `index.md`(전체 목록), `log.md`(시간순 이력), `_insights.md`(허브/고아 페이지 분석)가 자동 갱신된다.
6. **원본 파일의 운명** — `_sources/`의 원본은 처리 후에도 보존되어 Wiki 페이지의 `source:` 링크로 계속 접근 가능하다. 반면 `_raw/`의 임시 파일은 Wiki 페이지로 승격되면 삭제된다. ^[extracted]
7. **결과 확인** — `/wiki-status`로 델타 확인, `/wiki-query`로 검색/질의.

## Open Questions

- 이 튜토리얼은 `Ar9av/obsidian-wiki` 저장소 기준이며, 이 vault가 실제로 쓰는 Claude Code 스킬 세트(`~/.claude/skills/wiki-*`)와 세부 구현은 다를 수 있다 — 개념적 워크플로우는 동일하다. ^[inferred]

## Sources

- <https://jesusiswith.us/class/llm-wiki/obsidian-llm-wiki-초보자-설치부터-사용까지/>, 클리핑일 2026-07-07.
