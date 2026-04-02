# Obsidian Vault Map

이 문서는 `oh-my-codex-guide` 의 repo guide 문서를 Obsidian note pack과 어떻게 대응시키는지 정리한다.

## 출력 모드

- mode: `hybrid`
- repo guide: `oh-my-codex-guide/`
- repo-local note pack: `obsidian/oh-my-codex Guide/`
- live vault target: `unset`
- live sync status: `not applied`

## 안전 원칙

- repo-local pack이 정본이다.
- live vault sync는 **의도된 vault target이 명시적으로 확인된 뒤에만** 수행한다.
- default vault나 CLI 반환값은 참고 정보일 뿐, 목적지 확정 근거가 아니다.

## 매핑

| repo guide | note pack |
|---|---|
| `README.md` | `oh-my-codex Guide/oh-my-codex Guide - MOC.md` |
| `01-learning-paths.md` | `oh-my-codex Guide/02 Learning Paths.md` |
| `02-glossary.md` | `oh-my-codex Guide/03 Glossary.md` |
| `sections/01-core-concepts.md` | `oh-my-codex Guide/Concepts/Core concepts.md` |
| `sections/01-overview.md` | `oh-my-codex Guide/01 Overview.md` |
| `sections/02-harness-engineering.md` | `oh-my-codex Guide/Concepts/Harness engineering.md` |
| `sections/03-team-mode.md` | `oh-my-codex Guide/Concepts/Team mode.md` |
| `sections/04-ralph-ultrawork.md` | `oh-my-codex Guide/Concepts/Ralph ultrawork.md` |
| `sections/05-skills-catalog.md` | `oh-my-codex Guide/Concepts/Skills catalog.md` |
| `sections/06-prompts-agents.md` | `oh-my-codex Guide/Concepts/Prompts agents.md` |
| `sections/07-mcp-state.md` | `oh-my-codex Guide/Concepts/Mcp state.md` |
| `sections/08-setup-config.md` | `oh-my-codex Guide/Concepts/Setup config.md` |
| `sections/09-notifications.md` | `oh-my-codex Guide/Concepts/Notifications.md` |
| `sections/10-vs-oh-my-claudecode.md` | `oh-my-codex Guide/Concepts/Vs oh my claudecode.md` |
| `sections/11-spark-initiative.md` | `oh-my-codex Guide/Concepts/Spark initiative.md` |

## note map 의도

- MOC가 frontdoor 역할을 한다.
- Learning Paths / Glossary가 기본 3축이 된다.
- deep dive는 source-backed reading surface로 붙인다.
- References와 Workflows는 길찾기/증거를 붙잡아 둔다.
