# Upstream Snapshot — oh-my-codex

- source repo: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- previous synced commit: `fabb3ce0b96e42c20feb2940c74f2aa5addb8cee`
- current synced commit: `b1c39445741b8a0c4c30b22ab0db3e9491215b8b`
- sync mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드
- guide repo: `oh-my-codex-guide`

## 원본 한줄 요약

[![npm version](https://img.shields.io/npm/v/oh-my-codex)](https://www.npmjs.com/package/oh-my-codex) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org)

## recent upstream commits

- `b1c3944 Keep dev aligned after shipping 0.12.1`
- `0ce655b Cut the 0.12.1 patch release on main`
- `1ff91e1 Preserve release-branch guidance and mailbox accuracy before the 0.12.1 cut`
- `38e633e chore: empty CI probe on dev`
- `1fc66b8 Restore detached tmux launches for interactive OMX sessions (#1356)`
- `f1e7f8b Keep PR #1349 mergeable against dev without losing native stop-hook protections`
- `824bb60 Merge pull request #1348 from Yeachan-Heo/fix/stop-hook-misbehavior`
- `4a6feaa Merge pull request #1352 from Yeachan-Heo/fix/release-0-12-1-metadata`

## top-level structure

- `AGENTS.md`
- `biome.json`
- `Cargo.lock`
- `Cargo.toml`
- `CHANGELOG.md`
- `CONTRIBUTING.md`
- `COVERAGE.md`
- `crates/`
- `DEMO.md`
- `dist-workspace.toml`
- `docs/`
- `missions/`
- `package-lock.json`
- `package.json`
- `playground/`
- `prompts/`
- `README.md`
- `RELEASE_BODY.md`
- `skills/`
- `src/`

## changed files

- `.github/ISSUE_TEMPLATE/config.yml`
- `.gitignore`
- `AGENTS.md`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `crates/omx-runtime/src/main.rs`
- `docs/codex-native-hooks.md`
- `docs/contracts/team-delivery-state-contract.md`
- `docs/contracts/team-runtime-state-contract.md`
- `docs/hooks-extension.md`
- `docs/index.html`
- `docs/integrations.html`
- `docs/openclaw-integration.de.md`
- `docs/openclaw-integration.es.md`
- `docs/openclaw-integration.fr.md`
- `docs/openclaw-integration.it.md`
- `docs/openclaw-integration.ja.md`

## README excerpt

```md
# oh-my-codex (OMX)

<p align="center">
  <img src="https://yeachan-heo.github.io/oh-my-codex-website/omx-character-nobg.png" alt="oh-my-codex character" width="280">
  <br>
  <em>Start Codex stronger, then let OMX add better prompts, workflows, and runtime help when the work grows.</em>
</p>

[![npm version](https://img.shields.io/npm/v/oh-my-codex)](https://www.npmjs.com/package/oh-my-codex)
[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT)
[![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org)
[![Discord](https://img.shields.io/discord/1452487457085063218?color=5865F2&logo=discord&logoColor=white&label=Discord)](https://discord.gg/PUwSMR9XNk)

**Website:** https://yeachan-heo.github.io/oh-my-codex-website/
**Docs:** [Getting Started](./docs/getting-started.html) · [Agents](./docs/agents.html) · [Skills](./docs/skills.html) · [Integrations](./docs/integrations.html) · [Demo](./DEMO.md) · [OpenClaw guide](./docs/openclaw-integration.md)
**Community:** [Discord](https://discord.gg/PUwSMR9XNk) — shared OMX/community server for oh-my-codex and related tooling.

OMX is a workflow layer for [OpenAI Codex CLI](https://github.com/openai/codex).

It keeps Codex as the execution engine and makes it easier to:
- start a stronger Codex session by default
- run one consistent workflow from clarification to completion
- invoke the canonical skills with `$deep-interview`, `$ralplan`, `$team`, and `$ralph`
- keep project guidance, plans, logs, and state in `.omx/`

## Core Maintainers

| Role | Name | GitHub |
| --- | --- | --- |
| Creator & Lead | Yeachan Heo | [@Yeachan-Heo](https://github.com/Yeachan-Heo) |
| Maintainer | HaD0Yun | [@HaD0Yun](https://github.com/HaD0Yun) |

## Ambassadors

| Name | GitHub |
| --- | --- |
| Sigrid Jin | [@sigridjineth](https://github.com/sigridjineth) |

## Top Collaborators

| Name | GitHub |
| --- | --- |
| HaD0Yun | [@HaD0Yun](https://github.com/HaD0Yun) |
| Junho Yeo | [@junhoyeo](https://github.com/junhoyeo) |
| JiHongKim98 | [@JiHongKim98](https://github.com/JiHongKim98) |
| Lor | — |
| HyunjunJeon | [@HyunjunJeon](https://github.com/HyunjunJeon) |

## Recommended default flow

If you want the default OMX experience, start here:

```bash
npm install -g @openai/codex oh-my-codex
omx setup
omx --madmax --high
```

Then work normally inside Codex:

```text
$deep-interview "clarify the authentication change"
$ralplan "approve the auth plan and review tradeoffs"
$ralph "carry the approved plan to completion"
$team 3:executor "execute the approved plan in parallel"
```

That is the main path.
Start OMX strongly, clarify first when needed, approve the plan, then choose `$team` for coordinated parallel execution or `$ralph` for the persistent completion loop.

## What OMX is for

Use OMX if you already like Codex and want a better day-to-day runtime around it:
- a standard workflow built around `$deep-interview`, `$ralplan`, `$team`, and `$ralph`
- specialist roles and supporting skills when the task needs them
- project guidance through scoped `AGENTS.md`
- durable state under `.omx/` for plans, logs, memory, and mode tracking

If you want plain Codex with no extra workflow layer, you probably do not need OMX.

## Quick start

### Requirements

- Node.js 20+
- Codex CLI installed: `npm install -g @openai/codex`
- Codex auth configured
- `tmux` on macOS/Linux if you later want the durable team runtime
- `psmux` on native Windows if you later want Windows team mode

### A good first session

Launch OMX the recommended way:

```bash
omx --madmax --high
```

This starts the interactive leader session directly by default.
If you explicitly want the leader session in tmux, use:

```bash
omx --tmux --madmax --high
```

Then try the canonical workflow:

```text
$deep-interview "clarify the authentication change"
$ralplan "approve the safest implementation path"
$ralph "carry the approved plan to completion"
$team 3:executor "execute the approved plan in parallel"
```

Use `$team` when the approved plan needs coordinated parallel work, or `$ralph` when one persistent owner should keep pushing to completion.

## A simple mental model

OMX does **not** replace Codex.
```
