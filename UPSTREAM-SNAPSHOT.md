# Upstream Snapshot — oh-my-codex

- source repo: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- previous synced commit: `fb0f8ebb95dcec7aefb3e8bf2e45d977f98b2faf`
- current synced commit: `fb0f8ebb95dcec7aefb3e8bf2e45d977f98b2faf`
- sync mode: `no-change`
- impact labels: 일반 변경
- guide repo: `oh-my-codex-guide`

## 원본 한줄 요약

[![npm version](https://img.shields.io/npm/v/oh-my-codex)](https://www.npmjs.com/package/oh-my-codex) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org)

## recent upstream commits

- `fb0f8eb chore: sync Cargo.lock for 0.11.11 release`
- `4aaf96c Merge pull request #1088 from Yeachan-Heo/dev`
- `e762d86 chore: sync Cargo.toml version to 0.11.11`
- `ef91d8c chore: bump version to 0.11.11`
- `174cb5e docs: add contributors section with maintainer HaD0Yun`
- `19e1540 docs: add contributors section with maintainer HaD0Yun`
- `8ac6ee2 chore(deps): bump @modelcontextprotocol/sdk from 1.27.1 to 1.29.0 (#1086)`
- `b487ee4 chore(deps-dev): bump @biomejs/biome from 2.4.8 to 2.4.10 (#1087)`

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
- `README.de.md`
- `README.es.md`
- `README.fr.md`
- `README.it.md`

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

OMX is a workflow layer for [OpenAI Codex CLI](https://github.com/openai/codex).

It keeps Codex as the execution engine and makes it easier to:
- start a stronger Codex session by default
- reuse good role/task invocations with `$name` keywords
- invoke workflows with skills like `$plan`, `$ralph`, and `$team`
- keep project guidance, plans, logs, and state in `.omx/`

## Recommended default flow

If you want the default OMX experience, start here:

```bash
npm install -g @openai/codex oh-my-codex
omx setup
omx --madmax --high
```

Then work normally inside Codex:

```text
$architect "analyze the authentication flow"
$plan "ship this feature cleanly"
```

That is the main path.
Start OMX strongly, do the work in Codex, and let the agent pull in `$team` or other workflows only when the task actually needs them.

## What OMX is for

Use OMX if you already like Codex and want a better day-to-day runtime around it:
- reusable role/task invocations such as `$architect` and `$executor`
- reusable workflows such as `$plan`, `$ralph`, `$team`, and `$deep-interview`
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

Then try one role keyword and one workflow skill:

```text
$architect "analyze the authentication flow"
$plan "map the safest implementation path"
```

If the task grows, the agent can escalate to heavier workflows such as `$ralph` for persistent execution or `$team` for coordinated parallel work.

## A simple mental model

OMX does **not** replace Codex.

It adds a better working layer around it:
- **Codex** does the actual agent work
- **OMX role keywords** make useful roles reusable
- **OMX skills** make common workflows reusable
- **`.omx/`** stores plans, logs, memory, and runtime state

Most users should think of OMX as **better task routing + better workflow + better runtime**, not as a command surface to operate manually all day.

## Start here if you are new

1. Run `omx setup`
2. Launch with `omx --madmax --high`
3. Ask for analysis with `$architect "..."`
4. Ask for planning with `$plan "..."`
5. Let the agent decide when `$ralph`, `$team`, or another workflow is worth using

## Common in-session surfaces

| Surface | Use it for |
| --- | --- |
| `$architect "..."` | analysis, boundaries, tradeoffs |
| `$executor "..."` | focused implementation work |
| `/skills` | browsing installed skills |
| `$plan "..."` | planning before implementation |
| `$ralph "..."` | persistent sequential execution |
| `$team "..."` | coordinated parallel execution when the task is big enough |

Use `$deep-interview` when the request is still vague, the boundaries are unclear, or you want OMX to keep pressing on intent, non-goals, and decision boundaries before it hands work off to `$plan`, `$ralph`, `$team`, or `$autopilot`.

Typical cases:
- vague greenfield ideas that still need sharper intent and scope
- brownfield changes where OMX should inspect the repo first, then ask cited confirmation questions
- requests where you want a one-question-at-a-time clarification loop instead of immediate planning or implementation
## Advanced / operator surfaces
```
