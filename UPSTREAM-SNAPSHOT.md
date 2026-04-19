# Upstream Snapshot — oh-my-codex

- source repo: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- previous synced commit: `09d2126204a605b23cb37d72e3005dd4b571ad12`
- current synced commit: `435a3399732268f39e3d1f6f357cc9f7c66b744b`
- sync mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증
- guide repo: `oh-my-codex-guide`

## 원본 한줄 요약

[![npm version](https://img.shields.io/npm/v/oh-my-codex)](https://www.npmjs.com/package/oh-my-codex) [![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![Node.js](https://img.shields.io/badge/node-%3E%3D20-brightgreen)](https://nodejs.org)

## recent upstream commits

- `435a3399 Merge dev into main for v0.13.2 release`
- `328470c4 Prepare the 0.13.2 release cut`
- `a8312914 Merge pull request #1707 from Yeachan-Heo/fix/persistent-hooks`
- `61f406f0 Let native Stop auto-nudge run without OMX runtime gating`
- `e562820f Keep Stop-hook cleanup green under no-unused CI checks`
- `fb893f92 Keep active OMX workflows blocking Stop until they truly finish`
- `d9437f2e Merge pull request #1700 from Yeachan-Heo/fix/issue-1698-explore-reentry-recursion`
- `4136347b Keep explore startup-hook tests aligned with rustfmt\n\nThe PR's Rust changes only needed the formatter-applied line wrap in the new explore harness test so CI can pass without widening the change set.\n\nConstraint: Limit the fix to the explore-only Rust diff already in PR #1699\nRejected: Touch unrelated TypeScript or runtime behavior | outside requested scope\nConfidence: high\nScope-risk: narrow\nReversibility: clean\nDirective: Keep follow-up CI fixes confined to the new explore harness coverage unless a failing check proves otherwise\nTested: cargo fmt --all --check; cargo clippy -p omx-explore-harness --all-targets -- -D warnings\nNot-tested: Full workspace test matrix\n`

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

- `.github/workflows/release.yml`
- `AGENTS.md`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `crates/omx-explore/src/main.rs`
- `docs/codex-native-hooks.md`
- `docs/getting-started.html`
- `docs/integrations.html`
- `docs/troubleshooting.md`
- `package-lock.json`
- `package.json`
- `skills/analyze/SKILL.md`
- `src/catalog/__tests__/generator.test.ts`
- `src/catalog/generated/public-catalog.json`
- `src/catalog/manifest.json`
- `src/cli/__tests__/index.test.ts`
- `src/cli/__tests__/setup-skills-overwrite.test.ts`

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
Before you treat the runtime as ready, run the quick-start smoke test below: `omx doctor` verifies the install shape, while `omx exec` proves the active Codex runtime can actually authenticate and complete a model call from the current environment.
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
- Codex auth configured and visible in the same shell/profile that will run OMX
- `tmux` on macOS/Linux if you want the recommended durable team runtime
- `psmux` on native Windows only if you intentionally want the less-supported Windows team path

### A good first session

After install, check both boundaries:

```bash
omx doctor
codex login status
omx exec --skip-git-repo-check -C . "Reply with exactly OMX-EXEC-OK"
```

`omx doctor` catches missing OMX files, hooks, and runtime prerequisites. The real smoke test catches auth, profile, and provider/base-URL problems that only appear when Codex performs an actual request.

Launch OMX the recommended way:

```bash
omx --madmax --high
```

This starts the interactive leader session directly by default.
If you explicitly want the leader session in tmux, use:

```bash
omx --tmux --madmax --high
```
