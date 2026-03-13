# oh-my-codex (OMX) 학습 가이드

> *Your codex is not alone.*

OMX는 OpenAI Codex CLI를 위한 운영 런타임입니다. 이 가이드는 OMX의 전체 아키텍처, 핵심 개념, 워크플로우를 한국어로 설명합니다.

> 버전 기준
> - 최신 published GitHub release는 `v0.9.0`이며, 공개된 Spark Initiative 릴리즈는 2026년 3월 13일에 배포되었습니다.
> - 따라서 이 가이드는 **`v0.9.0`의 Spark Initiative 기능**을 기본 축으로 설명하고, **`0.9.1` hotfix가 무엇을 정리했는지**도 함께 반영합니다.

---

## 개요

### OMX란?

oh-my-codex (OMX)는 [OpenAI Codex CLI](https://github.com/openai/codex)를 감싸는 **운영 런타임 하네스(Operational Runtime Harness)**입니다. Codex CLI 자체를 대체하는 것이 아니라, 그 위에 조율(orchestration), 상태 관리, 팀 협업, 운영자 제어 레이어를 추가합니다.

OMX가 제공하는 핵심 기능:

- **Team Mode 우선** — 공유 가시성, 재개(resume), 복구(recovery), 생명주기 제어를 갖춘 조율된 멀티 에이전트 실행
- **역할 프롬프트 + 스킬** — planner, executor, reviewer 등 생산화된 에이전트 행동 및 재사용 가능한 워크플로우
- **영속 런타임 상태** — MCP 기반 상태, 메모리, 메일박스, 플랜, 진단 정보를 `.omx/` 디렉토리에 저장
- **운영자 제어** — Codex 자체를 교체하지 않고 장시간 작업을 시작, 검사, 검증, 취소, 재개

### oh-my-claudecode와의 관계

OMX는 [oh-my-claudecode (OMC)](https://github.com/Yeachan-Heo/oh-my-claudecode)에서 영감을 받아, 동일한 저자(Yeachan-Heo, HaD0Yun)가 **Codex CLI 버전**으로 제작한 프로젝트입니다.

| 항목 | oh-my-claudecode (OMC) | oh-my-codex (OMX) |
|------|------------------------|-------------------|
| 기반 CLI | Claude Code CLI | OpenAI Codex CLI |
| 주요 모델 | Claude (Anthropic) | GPT-5.4, Codex Spark |
| 상태 디렉토리 | `.omc/` | `.omx/` |
| 팀 모드 | `omc team` | `omx team` |
| 설치 명령 | `omx setup` (npm) | `omx setup` (npm) |

두 프로젝트는 동일한 철학(3-plane 아키텍처, Team Mode, Ralph 루프, Ralplan 워크플로우)을 공유하며, 각각의 CLI 엔진에 최적화되어 있습니다.

### 핵심 가치 제안

Codex CLI는 가볍고 지속적인 오케스트레이션에 적합합니다. 그러나 단독으로는 다음이 부족합니다:

- 영속 상태 (durable state)
- 공유 상황 인식 (shared situational awareness)
- 가시적 복구 경로 (visible recovery paths)
- 타이트한 운영자 제어 (tight operator control)

OMX는 Codex를 실행 엔진으로 유지하면서, 이 모든 것을 런타임으로 추가합니다. 결과적으로 작업이 **검사 가능하고(inspectable), 재개 가능하며(resumable), 반복 가능(repeatable)**해집니다.

---

## 전체 아키텍처

OMX는 3개의 플레인(plane)으로 구성된 레이어드 아키텍처를 사용합니다.

### 3-Plane 아키텍처 다이어그램

![Diagram 1](assets/diagrams/README__diagram_1.svg)

### 사용자에서 실행까지: 전체 흐름

![Diagram 2](assets/diagrams/README__diagram_2.svg)

### 핵심 모델 계층 구조

```
사용자 / 운영자
  -> OMX runtime
    -> Codex CLI (실행 엔진)
    -> AGENTS.md (오케스트레이션 브레인)
    -> ~/.codex/prompts/*.md (에이전트 프롬프트 카탈로그)
    -> ~/.agents/skills/*/SKILL.md (스킬 카탈로그)
    -> ~/.codex/config.toml (기능, 알림, MCP 설정)
    -> .omx/ (런타임 상태, 메모리, 플랜, 로그)
```

---

## New in v0.9.x: Spark Initiative

`v0.9.0`은 OMX가 Team Mode 중심 오케스트레이션 위에, **read-only repository discovery와 shell-native inspection을 위한 native fast path**를 본격적으로 얹은 릴리즈입니다.

핵심 추가점:

- **`omx explore`**: 읽기 전용 탐색 전용 엔트리포인트
- **`omx sparkshell`**: 빠른 shell-native inspection과 explicit tmux-pane summarization
- **Explore ↔ sparkshell integration**: qualifying read-only prompt는 sparkshell backend를 사용할 수 있음
- **Cross-platform native release assets**: `omx-explore-harness`, `omx-sparkshell`, `native-release-manifest.json`
- **Release-oriented verification**: `npm run build:full`, `npm run test:explore`, `npm run test:sparkshell`

`0.9.1`은 새 기능 릴리즈가 아니라, `v0.9.0` 이후 들어온 packed-install smoke hydration fix를 포함한 **clean superseding hotfix**입니다.

빠르게 감 잡는 커맨드:

```bash
npm run build:full
omx explore --prompt "git log --oneline -10"
omx sparkshell git --version
omx sparkshell --tmux-pane %12 --tail-lines 400
```

---

## 빠른 시작 (Quickstart)

3분 안에 OMX를 설치하고 첫 팀을 실행할 수 있습니다.

### 1단계: 사전 요구사항

```bash
# Node.js >= 20 필요
node --version

# Codex CLI 설치
npm install -g @openai/codex

# tmux 설치 (팀 모드에 필요)
# macOS:
brew install tmux
# Ubuntu/Debian:
sudo apt install tmux
```

| 플랫폼 | tmux 제공자 | 설치 명령 |
|--------|------------|-----------|
| macOS | tmux | `brew install tmux` |
| Ubuntu/Debian | tmux | `sudo apt install tmux` |
| Fedora | tmux | `sudo dnf install tmux` |
| Windows (네이티브) | psmux | `winget install psmux` |
| Windows (WSL2) | tmux (WSL 내부) | `sudo apt install tmux` |

### 2단계: OMX 설치 및 설정

```bash
# OMX 설치
npm install -g oh-my-codex

# OMX 설정 (프롬프트, 스킬, config.toml, AGENTS.md 설치)
omx setup

# 설치 진단
omx doctor --team

# project scope를 쓰는 경우 업그레이드 후 재동기화 권장
omx setup --force --scope project
```

### 3단계: 첫 팀 실행

```bash
# 3명의 executor 에이전트로 팀 실행
omx team 3:executor "범위가 정해진 작업을 검증과 함께 완료하라"
```

신뢰 환경에서의 권장 실행 프로파일:

```bash
omx --xhigh --madmax
```

> **주의:** `--madmax`는 Codex 승인 및 샌드박스를 우회합니다. 신뢰된 환경에서만 사용하세요.

### 4단계: Spark Initiative 빠른 연습

`v0.9.0`부터 OMX는 **Spark Initiative** 릴리스 라인을 갖습니다. 학습자는 기본 Team Mode만 보지 말고 아래 두 커맨드도 초기에 익히는 것이 좋습니다.

```bash
omx explore --prompt "git log --oneline -10"
omx sparkshell git --version
omx sparkshell --tmux-pane %12 --tail-lines 400
```

- `omx explore`는 기본 읽기 전용 탐색 진입점입니다.
- `omx sparkshell`은 빠른 셸 네이티브 검사와 tmux pane 요약을 위한 명시적 오퍼레이터 표면입니다.
- 단순 읽기 전용 셸 작업은 `explore -> sparkshell` 경로로 라우팅될 수 있지만, 안전 제약이 완화되는 것은 아닙니다.

### Codex 내부에서 사용하는 첫 번째 세션

Codex 실행 후 다음 명령을 사용합니다:

```text
$plan "OAuth 콜백을 안전하게 구현"
$team 3:executor "공유 검증으로 안전하게 구현"
/prompts:architect "경계 결정사항 검토"
/prompts:executor "다음 범위 지정 작업 수행"
```

터미널에서 팀 관리:

```bash
omx team status <team-name>
omx team resume <team-name>
omx team shutdown <team-name>
```

---

## 현재 릴리스 라인 이해하기

2026년 3월 기준 OMX 학습 시 반드시 구분해야 하는 버전 상태는 다음과 같습니다.

| 관점 | 상태 | 의미 |
|------|------|------|
| 최신 GitHub 게시 릴리스 | `v0.9.0` | Spark Initiative 기능이 처음 공개된 태그드 릴리스 |
| 로컬 소스 저장소 (`/home/terry/guide/Yeachan-Heo/oh-my-codex`) | `0.9.1` | `v0.9.0` 이후의 packed-install smoke hydration hotfix가 포함된 후속 상태 |
| 역사적 주의점 | `v0.9.0` remains red | published release 자체는 역사적으로 red 상태로 남고, `0.9.1`이 clean superseding release 역할을 함 |

### Spark Initiative에서 실제로 달라진 것

- **`omx explore`** — 읽기 전용 저장소 탐색용 기본 엔트리포인트
- **`omx sparkshell`** — 빠른 셸 네이티브 검사, 적응형 요약, tmux pane 캡처를 위한 명시적 커맨드
- **explore ↔ sparkshell 연계** — 단순 read-only shell 작업은 sparkshell 백엔드가 더 적합할 때 그쪽으로 라우팅 가능
- **네이티브 릴리스 배포** — GitHub Release 자산에 `omx-explore-harness`, `omx-sparkshell`, `native-release-manifest.json`이 포함됨
- **업그레이드/설치 의미 변화** — 패키지 설치는 릴리스 자산 hydration과 fallback order를 이해해야 함

### 학습자가 기억할 설치/업그레이드 포인트

- npm 패키지는 모든 네이티브 바이너리를 직접 번들하지 않습니다.
- 런타임은 기본적으로 `OMX_*_BIN` override → 사용자별 hydrated native cache → repo-local 개발 산출물 순서로 바이너리를 찾습니다.
- project scope를 쓰는 경우 업그레이드 후 `omx setup --force --scope project`로 관리 파일을 새 릴리스 라인에 맞춰 갱신하는 습관이 중요합니다.

---

## 이 가이드 사용법

이 학습 가이드는 다음 파일로 구성됩니다:

```
oh-my-codex-guide/
├── README.md              # 이 파일: 전체 개요 및 아키텍처
├── 01-learning-paths.md   # 학습 경로 (초급 → 중급 → 고급)
└── 02-glossary.md         # 핵심 용어 및 개념 사전
```

### 각 섹션 설명

| 파일 | 대상 | 내용 |
|------|------|------|
| `README.md` | 모든 사용자 | OMX 개요, 아키텍처, 빠른 시작 |
| `01-learning-paths.md` | 학습자 | 단계별 학습 경로 및 우선순위 |
| `02-glossary.md` | 모든 사용자 | 용어 정의 및 개념 설명 |

### 추천 학습 순서

1. 이 `README.md`로 전체 그림 파악
2. `01-learning-paths.md`로 자신의 수준에 맞는 경로 선택
3. `02-glossary.md`를 참조 자료로 활용
4. 공식 문서([yeachan-heo.github.io/oh-my-codex-website](https://yeachan-heo.github.io/oh-my-codex-website/))로 심화 학습

---

## 핵심 워크플로우

### ralplan → team → ralph 워크플로우

이것이 OMX의 **가장 강력한 고제어 워크플로우**입니다. `autopilot`보다 더 세밀한 제어가 필요하고, `$ultrawork`보다 더 나은 조율이 필요할 때 사용합니다.

![Diagram 3](assets/diagrams/README__diagram_3.svg)

### 각 단계의 역할

**ralplan** — 계획 단계
- 거친 요청을 스펙, 수용 기준, 레인 준비 분해로 변환
- 워커가 시작하기 전에 명확성 확보
- 결과물: `.omx/plans/prd-*.md`, `.omx/plans/test-spec-*.md`

**team** — 실행 단계
- 영속 워커 조율로 계획 실행
- 가시적 런타임 상태 및 블로커 처리
- 단순 팬아웃보다 더 나은 처리

**ralph** — 검증 단계
- 검증이 실재하고 증거가 신선해질 때까지 루프 유지
- 명시적 정리 보장
- 루프가 끝나지 않으면 완료 선언 금지

### 워크플로우 예시

```bash
# 1단계: ralplan으로 계획 수립
omx ask --agent-prompt planner "ralplan: 이 기능을 워커 레인과 수용 기준으로 분해하라"

# 2단계: team으로 실행
omx team 3:executor "승인된 ralplan을 공유 런타임 조율로 실행하라"

# 팀 상태 모니터링
omx team status <team-name>
```

### Team Mode vs. Ultrawork 선택 기준

![Diagram 4](assets/diagrams/README__diagram_4.svg)

---

## Spark Initiative를 어디서 읽어야 하나

1. 이 `README.md`의 Spark Initiative 요약
2. `01-learning-paths.md`의 초급/중급 업데이트 포인트
3. `02-glossary.md`의 `Spark Initiative`, `omx explore`, `omx sparkshell`, `native-release-manifest.json` 항목
4. `sections/08-setup-config.md`의 native asset / upgrade note
5. 원본 소스의 `README.md`, `CHANGELOG.md`, `docs/release-notes-0.9.0.md`, `docs/release-notes-0.9.1.md`

---

## 주요 커맨드 참조

### 런치 커맨드

```bash
omx                      # HUD와 함께 Codex 실행 (tmux 사용 시)
omx team <args>          # 조율된 팀 워커 시작/상태/재개/종료
omx setup                # 프롬프트/스킬/설정 설치
omx doctor               # 설치/런타임 진단
omx doctor --team        # Team Mode 진단
omx ask <provider> ...   # 로컬 provider advisor 호출 + artifact 기록
omx explore ...          # 읽기 전용 탐색 엔트리포인트 (필요 시 sparkshell 백엔드 사용)
omx sparkshell ...       # 빠른 셸 네이티브 검사 / tmux pane 요약
omx hooks ...            # hooks 플러그인 init/status/validate/test
omx tmux-hook ...        # tmux-hook init/status/validate/test
omx hud ...              # HUD watch/json/preset
omx ralph                # ralph 영속 모드로 Codex 실행
omx resume               # 이전 Codex 세션 재개
omx resume --last        # 마지막 세션 재개
omx status               # 활성 모드 표시
omx cancel               # 활성 실행 모드 취소
omx version              # 버전 정보
omx help                 # 도움말
```

### 런치 플래그

```bash
--yolo              # yolo 모드로 Codex 실행
--high              # 높은 추론 노력
--xhigh             # 최고 추론 노력
--madmax            # 위험: Codex 승인 및 샌드박스 우회
--spark             # 팀 워커에 Codex Spark 모델 사용 (~1.3배 빠름)
--madmax-spark      # Spark 모델 + 리더/워커 승인 우회
-w, --worktree[=<name>]  # git worktree에서 Codex 실행
--verbose           # 상세 출력
--scope <user|project>  # setup 전용
```

### 팀 관리 커맨드

```bash
omx team 3:executor "작업"     # 3명의 executor로 팀 시작
omx team status <team-name>    # 팀 상태 확인
omx team resume <team-name>    # 팀 재개
omx team shutdown <team-name>  # 팀 종료
omx team ralph <args>          # ralph와 연계된 팀 실행
```

### ask 커맨드

```bash
omx ask claude "이 diff를 검토하라"
omx ask gemini "대안을 브레인스토밍하라"
omx ask claude --agent-prompt executor "테스트와 함께 기능 X 구현"
omx ask gemini --agent-prompt=planner --prompt "롤아웃 계획 초안 작성"
```

---

## omx setup이 설치하는 것들

`omx setup` 실행 시 설치되는 항목:

| 항목 | user 스코프 | project 스코프 |
|------|------------|----------------|
| 프롬프트 | `~/.codex/prompts/` | `./.codex/prompts/` |
| 스킬 | `~/.agents/skills/` | `./.agents/skills/` |
| config.toml | `~/.codex/config.toml` | `./.codex/config.toml` |
| 네이티브 에이전트 설정 | `~/.omx/agents/` | `./.omx/agents/` |
| AGENTS.md | 변경 없음 | `./AGENTS.md` 생성/갱신 |
| .omx/ 디렉토리 | — | 프로젝트 루트에 생성 |

`config.toml`에 설정되는 주요 항목:
- `notify = ["node", "..."]` — 알림 설정
- `model_reasoning_effort = "high"` — 추론 노력 수준
- `model = "gpt-5.4"` — 기본 모델
- `[features] multi_agent = true, child_agents_md = true`
- MCP 서버 항목 (`omx_state`, `omx_memory`, `omx_code_intel`, `omx_trace`)

Spark Initiative 이후 설치/배포 관점에서 추가로 알아둘 점:
- `omx setup`은 네이티브 릴리스 자산 자체를 설치하는 명령은 아니지만, 새 릴리스 라인과 맞는 관리 파일·에이전트 설정을 갱신하는 핵심 진입점입니다.
- 패키지 설치 후 `omx explore`/`omx sparkshell`이 바로 repo-local 바이너리를 쓰지 않을 수 있으며, `native-release-manifest.json` 기반 hydration을 통해 적절한 플랫폼 자산을 가져옵니다.
- project scope 사용자는 업그레이드 후 `omx setup --force --scope project`를 다시 실행해 관리 파일과 경로를 refresh하는 편이 안전합니다.

---

## 실험적 기능: 포스처 인식 라우팅

OMX는 실험적 라우팅 레이어를 포함합니다:

- **role**: 에이전트 책임 (`executor`, `planner`, `architect`)
- **tier**: 추론 깊이/비용 (`LOW`, `STANDARD`, `THOROUGH`)
- **posture**: 운영 스타일

| 포스처 | 설명 | 해당 역할 |
|--------|------|-----------|
| `frontier-orchestrator` | 스티어러블 프론티어 모델의 리더/라우터 포스처 | planner, architect, critic |
| `deep-worker` | 구현 우선 포스처 | executor, build-fixer, test-engineer |
| `fast-lane` | 빠른 모델을 위한 경량 트리아지/검색 포스처 | explore, writer |

---

## 관련 링크

- [공식 문서](https://yeachan-heo.github.io/oh-my-codex-website/docs.html)
- [CLI 참조](https://yeachan-heo.github.io/oh-my-codex-website/docs.html#cli-reference)
- [GitHub 저장소](https://github.com/Yeachan-Heo/oh-my-codex)
- [npm 패키지](https://www.npmjs.com/package/oh-my-codex)
- [Spark Initiative 릴리스 노트 v0.9.0](https://github.com/Yeachan-Heo/oh-my-codex/blob/main/docs/release-notes-0.9.0.md)
- [Spark Initiative hotfix 릴리스 노트 v0.9.1](https://github.com/Yeachan-Heo/oh-my-codex/blob/main/docs/release-notes-0.9.1.md)
- [OpenClaw 통합 가이드](https://github.com/Yeachan-Heo/oh-my-codex/blob/main/docs/openclaw-integration.md)
