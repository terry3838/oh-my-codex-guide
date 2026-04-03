# Upstream Snapshot — oh-my-codex

- source repo: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- previous synced commit: `fb0f8ebb95dcec7aefb3e8bf2e45d977f98b2faf`
- current synced commit: `4b0e220475c86d4f74136a37bab0996d83c2c271`
- sync mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 소스코드
- guide repo: `oh-my-codex-guide`

## 원본 한줄 요약

[![npm version](https://img.shields.io/npm/v/oh-my-codex)](https://www.npmjs.com/package/oh-my-codex) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org)

## recent upstream commits

- `4b0e220 Merge remote-tracking branch 'origin/dev' into main-merge/0.11.12-cargolock-conflict`
- `b542260 Prepare an accurate 0.11.12 patch cut from the true dev tip`
- `3170ba8 docs: align workflow docs around deep-interview -> ralplan -> team/ralph (#1132)`
- `0467317 fix: treat linked legacy skill roots as a shared canonical root (#1128)`
- `e22a13d fix(team): eliminate dual-write seam gaps in dispatch and mailbox transitions (fixes #1108) (#1126)`
- `910147c docs: add Polish README translation (#1125)`
- `89f4b30 Improve Polish README with natural language`
- `12b620c Add a Polish onboarding path for OMX users`

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
- `README.el.md`
- `README.es.md`
- `README.fr.md`

## changed files

- `AGENTS.md`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.de.md`
- `README.el.md`
- `README.es.md`
- `README.fr.md`
- `README.it.md`
- `README.ja.md`
- `README.ko.md`
- `README.md`
- `README.pl.md`
- `README.pt.md`
- `README.ru.md`
- `README.tr.md`
- `README.vi.md`
- `README.zh-TW.md`
- `README.zh.md`
- `RELEASE_BODY.md`

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
- run one consistent workflow from clarification to completion
- invoke the canonical skills with `$deep-interview`, `$ralplan`, `$team`, and `$ralph`
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

It adds a better working layer around it:
- **Codex** does the actual agent work
- **OMX role keywords** make useful roles reusable
- **OMX skills** make common workflows reusable
- **`.omx/`** stores plans, logs, memory, and runtime state

Most users should think of OMX as **better task routing + better workflow + better runtime**, not as a command surface to operate manually all day.

## Start here if you are new

1. Run `omx setup`
2. Launch with `omx --madmax --high`
3. Use `$deep-interview "..."` when the request or boundaries are still unclear
4. Use `$ralplan "..."` to approve the plan and review tradeoffs
5. Choose `$team` for coordinated parallel execution or `$ralph` for persistent completion loops

## Recommended workflow

1. `$deep-interview` — clarify scope when the request or boundaries are still vague.
2. `$ralplan` — turn that clarified scope into an approved architecture and implementation plan.
3. `$team` or `$ralph` — use `$team` for coordinated parallel execution, or `$ralph` when you want a persistent completion loop with one owner.

## Common in-session surfaces

| Surface | Use it for |
| --- | --- |
| `$deep-interview "..."` | clarifying intent, boundaries, and non-goals |
| `$ralplan "..."` | approving the implementation plan and tradeoffs |
| `$ralph "..."` | persistent completion and verification loops |
| `$team "..."` | coordinated parallel execution when the work is big enough |
| `/skills` | browsing installed skills and supporting helpers |
```
