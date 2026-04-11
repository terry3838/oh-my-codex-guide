# Upstream Snapshot — oh-my-codex

- source repo: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- previous synced commit: `6d737879e8c1992ef045db387044c7dd156cc78d`
- current synced commit: `982a7546b7b42518d47de2ac53fa2b47ebd670a4`
- sync mode: `update`
- impact labels: 일반 변경
- guide repo: `oh-my-codex-guide`

## 원본 한줄 요약

[![npm version](https://img.shields.io/npm/v/oh-my-codex)](https://www.npmjs.com/package/oh-my-codex) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org)

## recent upstream commits

- `982a7546 Merge pull request #1456 from Yeachan-Heo/revert/pr-1454-main`
- `9a4171f6 Remove the Windows ps fallback from main after landing on the wrong base branch`
- `c884326d Merge pull request #1454 from Gujiassh/fix/windows-ps-fallback`
- `a250fdab fix(notifications): tolerate missing ps on Windows`

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

- 변경 파일 없음

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

<table>
<tr>
<td><strong>🚨 CAUTION — RECOMMENDED DEFAULT ONLY: macOS or Linux with Codex CLI.</strong><br><br><strong>OMX is primarily designed and actively tuned for that path.</strong><br><strong>Native Windows and Codex App are not the default experience, may break or behave inconsistently, and currently receive less support.</strong></td>
</tr>
</table>

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
- `tmux` on macOS/Linux if you want the recommended durable team runtime
- `psmux` on native Windows only if you intentionally want the less-supported Windows team path

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
```
