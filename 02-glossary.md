# OMX 핵심 용어 사전

oh-my-codex(OMX)에서 사용하는 핵심 용어와 개념을 한국어로 설명합니다. 기술 용어는 영문 그대로 표기하며, 의미와 사용 맥락을 함께 설명합니다.

---

## A

### AGENTS.md

**정의:** Codex CLI의 최상위 운영 계약(operating contract) 파일. 에이전트의 역할, 위임 규칙, 스킬, 팀 파이프라인, 상태 관리를 정의하는 마크다운 문서입니다.

**위치:** 프로젝트 루트 디렉토리 (`./AGENTS.md`)

**역할:**
- OMX 런타임의 오케스트레이션 브레인
- `prompts/*.md`의 상위 계약 (프롬프트 파일은 AGENTS.md를 따라야 함)
- 키워드 감지 및 스킬 라우팅 정의

**관련 커맨드:**
```bash
omx setup --scope project  # AGENTS.md 생성
omx agents-init .          # 서브디렉토리 AGENTS.md 부트스트랩
```

**주입 방식:**

OMX는 Codex 실행 시 자동으로 AGENTS.md를 주입합니다:
```
-c model_instructions_file="<cwd>/AGENTS.md"
```

**OMX 런타임 마커:**

AGENTS.md 내 런타임이 관리하는 섹션은 마커로 구분됩니다:
```
<!-- OMX:RUNTIME:START --> ... <!-- OMX:RUNTIME:END -->
<!-- OMX:TEAM:WORKER:START --> ... <!-- OMX:TEAM:WORKER:END -->
```

---

## C

### cancel (취소 스킬)

**정의:** 활성 실행 모드(ralph, ultrawork, team 등)를 종료하는 스킬 및 커맨드.

**사용 방법:**
```bash
# 터미널에서
omx cancel

# Codex 내에서
$cancel
```

**취소 시점:**
- 모든 작업이 완료되고 검증됨
- 사용자가 중단을 요청함
- 복구 불가능한 블로커 발생

**취소하지 말아야 할 때:**
- 복구 가능한 작업이 남아 있을 때

---

### config.toml

**정의:** Codex CLI의 설정 파일. OMX가 `omx setup` 시 자동으로 구성합니다.

**위치:**
- user 스코프의 Codex 설정 파일
- project 스코프의 `.codex/config.toml`

**OMX가 설정하는 주요 항목:**

```toml
notify = ["node", "..."]               # 알림 설정
model_reasoning_effort = "high"        # 추론 노력 수준
model = "gpt-5.4"                      # 기본 모델
model_context_window = 1000000         # 컨텍스트 창 (gpt-5.4 전용)
model_auto_compact_token_limit = 900000

[features]
multi_agent = true
child_agents_md = true

# MCP 서버 항목
# omx_state, omx_memory, omx_code_intel, omx_trace
```

---

## D

### deep-worker (포스처)

**정의:** 구현 우선(implementation-first) 운영 스타일의 포스처. 실행, 빌드, 테스트에 집중합니다.

**해당 역할:** executor, build-fixer, test-engineer

**추론 티어:** STANDARD

**대비:** `frontier-orchestrator`(리더/전략), `fast-lane`(경량/검색)

---

### .omx/ 디렉토리

**정의:** OMX의 모든 런타임 상태, 메모리, 플랜, 로그가 저장되는 디렉토리.

**구조:**

```
.omx/
├── state/                          # 모드 상태 파일
│   ├── team-state.json            # Team Mode 상태
│   ├── ralph-state.json           # Ralph 루프 상태
│   └── ultrawork-state.json       # Ultrawork 상태
├── notepad.md                     # 세션 노트 (7일 자동 정리)
├── project-memory.json            # 크로스 세션 메모리
├── plans/                         # 계획 문서
│   ├── prd-*.md                   # 제품 요구 사항
│   └── test-spec-*.md             # 테스트 명세
├── logs/                          # 감사 로그
├── backups/setup/<timestamp>/     # 설정 백업
├── hooks/                         # 플러그인 훅 파일 (*.mjs)
└── setup-scope.json               # 설치 스코프 설정
```

**생성 방법:**

```bash
omx setup          # 전체 설정과 함께 생성
omx setup --scope project  # 프로젝트 스코프로 생성
```

---

## E

### ecomode (에코 모드)

**정의:** 토큰 효율적인 실행 모드. 최소한의 토큰으로 작업을 완료합니다.

**트리거 키워드:** "ecomode", "eco", "budget"

**사용:**
```text
$ecomode
```

---

## F

### fast-lane (포스처)

**정의:** 빠른 모델을 위한 경량 트리아지/검색 포스처.

**해당 역할:** explore, writer

**추론 티어:** LOW (Spark 모델 사용)

**대비:** `deep-worker`(구현), `frontier-orchestrator`(전략)

---

### frontier-orchestrator (포스처)

**정의:** 스티어러블(steerable) 프론티어 모델의 리더/라우터 포스처.

**해당 역할:** planner, architect, critic

**추론 티어:** THOROUGH

**특징:** 전략적 결정, 설계 검토, 계획 수립에 적합

---

## H

### HUD (Heads-Up Display)

**정의:** OMX의 실시간 상태 모니터링 인터페이스. tmux와 통합되어 팀 상태, 활성 모드, 진행 상황을 시각화합니다.

**커맨드:**
```bash
omx hud --watch        # 실시간 모니터링
omx hud --json         # JSON 형식 출력
omx hud --preset       # 프리셋 레이아웃 사용
```

**자동 활성화:**

tmux 세션 내에서 `omx` 실행 시 HUD가 자동으로 표시됩니다.

---

## M

### madmax

**정의:** Codex CLI의 `--dangerously-bypass-approvals-and-sandbox` 플래그의 OMX 약어.

**사용:**
```bash
omx --madmax          # 승인 및 샌드박스 우회
omx --xhigh --madmax  # 최고 추론 + 승인 우회 (권장 신뢰 환경 프로파일)
```

> **경고:** 신뢰된 환경 또는 외부 샌드박스에서만 사용하세요. 일반 개발 환경에서는 사용을 피하세요.

---

### MCP 서버 (Model Context Protocol 서버)

**정의:** OMX의 상태 플레인을 구성하는 4개의 Model Context Protocol 서버. Codex 에이전트에게 영속 상태, 메모리, 추적 기능을 제공합니다.

**4개 MCP 서버:**

| 서버명 | 역할 | 저장 위치 |
|--------|------|-----------|
| `omx_state` | 실행 모드 상태 (team, ralph, ultrawork) | `.omx/state/` |
| `omx_memory` | 크로스 세션 프로젝트 메모리 | `.omx/project-memory.json` |
| `omx_trace` | 추적, 진단, 감사 로그 | `.omx/logs/` |
| `omx_code_intel` | 코드 인텔리전스 (인메모리) | — |

**설정 위치:** `config.toml`의 MCP 서버 항목

**workingDirectory 보안 설정:**
```bash
export OMX_MCP_WORKDIR_ROOTS="/path/to/project:/another-root"
```

---

## N

### native-release-manifest.json

**정의:** Spark Initiative 이후 GitHub Release 자산과 함께 배포되는 네이티브 릴리스 매니페스트 파일. 플랫폼별 다운로드 정보와 SHA-256 체크섬을 담아 `omx explore`/`omx sparkshell`의 hydration 경로를 설명한다.

**역할:**
- packaged install에서 어떤 네이티브 자산을 가져와야 하는지 결정
- hydration 시 무결성 검증 정보 제공
- npm 패키지가 네이티브 바이너리를 직접 모두 번들하지 않는 배포 계약을 보완

---

## O

### oh-my-codex (OMX)

**정의:** OpenAI Codex CLI를 위한 운영 런타임 하네스(Operational Runtime Harness). Codex를 실행 엔진으로 유지하면서 팀 조율, 영속 상태, 운영자 제어를 추가합니다.

**공식 슬로건:** *"Your codex is not alone."*

**설치:**
```bash
npm install -g oh-my-codex
```

**핵심 명제:**
- Ultrawork는 병렬성(parallelism)이다.
- Team Mode는 오케스트레이션(orchestration)이다.

**요구사항:**
- Node.js >= 20
- Codex CLI (`@openai/codex`)
- tmux (Team Mode에 필요)

**현재 읽는 법:**
- Spark Initiative는 OMX를 hybrid runtime으로 바꾼 중요한 전환점이다.
- 다만 현재 학습 기준선은 `0.11.11`이며, Spark는 지금 버전의 배경층으로 읽는 편이 맞다.

---

### omx explore

**정의:** OMX의 기본 읽기 전용 탐색 엔트리포인트. 단순한 저장소 조사·파일 찾기·로그 확인 같은 작업을 더 안전하고 저렴한 경로로 처리한다.

**특징:**
- shell-only, read-only, allowlisted 제약을 가진다
- 필요 시 `omx sparkshell`을 백엔드로 사용할 수 있다
- 일반 Codex read-only 모드의 완전한 기능 파리티(web, MCP, 이미지)는 보장하지 않는다

**예시:**
```bash
omx explore --prompt "git log --oneline -10"
omx explore --prompt-file prompts/explore-task.md
```

---

### omx setup

**정의:** OMX 구성 요소를 설치하고 환경을 구성하는 커맨드.

**스코프:**

```bash
omx setup
omx setup --scope user
omx setup --scope project
```

**무엇을 하냐:**
- user 스코프에서는 Codex 프롬프트/스킬/설정과 OMX 관리 파일을 사용자 범위에 설치한다.
- project 스코프에서는 현재 저장소에 맞는 `AGENTS.md`, 관리 설정, `.omx/` 상태 디렉터리를 정렬한다.

**백업 정책:**

관리된 파일이 변경될 경우 설치 전 백업을 만든다. 핵심은 정확한 위치를 외우는 게 아니라, **setup이 재실행 가능한 관리 표면**이라는 점을 이해하는 것이다.

**업그레이드 팁:**
```bash
omx setup --force --scope project
```
project scope 사용자는 릴리스 라인이 바뀐 뒤 위 명령으로 관리 파일을 refresh하는 편이 안전하다.

---

### omx sparkshell

**정의:** Spark Initiative에서 추가된 오퍼레이터용 셸 네이티브 검사 표면. JS -> Rust sidecar bridge를 통해 빠르게 명령을 실행하고, 출력이 길면 적응형 요약을 만든다.

**주요 기능:**
- 직접 명령 실행: `omx sparkshell git --version`
- tmux pane tail 요약: `omx sparkshell --tmux-pane %12 --tail-lines 400`
- `omx explore`의 qualifying read-only shell task를 위한 백엔드 역할

**바이너리 탐색 우선순위:**
1. `OMX_SPARKSHELL_BIN`
2. hydrated native cache
3. packaged dev artifacts / repo-local build output

---

### omx doctor

**정의:** OMX 설치 및 런타임 환경을 진단하는 커맨드.

**사용:**
```bash
omx doctor          # 기본 진단
omx doctor --team   # Team Mode 진단 (tmux 포함)
```

**진단 항목:**
- Node.js 버전
- Codex CLI 설치 여부
- OMX 구성 요소 설치 상태
- tmux 설치 및 버전 (--team 시)
- MCP 서버 설정

---

### OpenClaw

**정의:** OMX의 알림 게이트웨이 통합 대상 중 하나. Discord, Telegram, Slack과 함께 지원되는 커스텀 웹훅/CLI 명령 게이트웨이입니다.

**설정:**
```bash
export OMX_OPENCLAW=1
export OMX_OPENCLAW_COMMAND=1
```

**설정 스킬:**
```text
$configure-notifications "openclaw 알림 설정"
```

**명령 게이트웨이 예시:**
```bash
# clawdbot 에이전트 턴 방식
clawdbot agent --deliver --reply-channel ... --reply-to ...
```

**지원 훅 이벤트:**
- `session-start`
- `session-idle`
- `ask-user-question`
- `session-stop`
- `session-end`

---

## P

### 포스처 (Posture)

**정의:** 에이전트의 운영 스타일을 정의하는 실험적 라우팅 레이어. `role`(책임)과 `tier`(추론 깊이)와 결합됩니다.

**3가지 포스처:**

| 포스처 | 설명 | 해당 역할 |
|--------|------|-----------|
| `frontier-orchestrator` | 스티어러블 프론티어 모델의 리더/라우터 | planner, architect, critic |
| `deep-worker` | 구현 우선 실행자 | executor, build-fixer, test-engineer |
| `fast-lane` | 경량 트리아지/검색 | explore, writer |

**3가지 추론 티어:**

| 티어 | 설명 | 적합 포스처 |
|------|------|------------|
| `LOW` | 저복잡도 | fast-lane |
| `STANDARD` | 표준 | deep-worker |
| `THOROUGH` | 심층 | frontier-orchestrator |

**상태:** 실험적 (experimental)

---

### 프롬프트 (Prompt)

**정의:** 특정 역할의 에이전트 행동을 정의하는 마크다운 파일. user 스코프 또는 project 스코프의 Codex 프롬프트 디렉터리에 설치됩니다.

**호출 방법:**
```text
/prompts:executor "다음 작업 수행"
/prompts:architect "설계 검토"
/prompts:planner "계획 수립"
```

**핵심 프롬프트:**
- `executor` — 구현 및 리팩토링
- `planner` — 작업 계획 및 시퀀싱
- `architect` — 시스템 설계 분석
- `debugger` — 근본 원인 분석
- `verifier` — 완료 검증
- `security-reviewer` — 보안 감사
- `test-engineer` — 테스트 전략

**AGENTS.md와의 관계:**

프롬프트는 AGENTS.md의 하위 계약입니다. AGENTS.md를 따라야 하며, 그 권한을 재정의할 수 없습니다.

---

## R

### ralph / ralph 루프

**정의:** 검증이 실재하고 증거가 신선해질 때까지 실행 루프를 유지하는 영속 실행 모드.

**트리거 키워드:** "ralph", "don't stop", "must complete", "keep going"

**사용:**
```bash
# 터미널에서
omx ralph               # ralph 모드로 Codex 실행

# Codex 내에서
$ralph "이 작업을 완료될 때까지 지속하라"
```

**ralph 루프 로직:**

```
루프 시작
  -> 작업 실행
  -> 검증 (PRD + test-spec 존재 확인)
  -> 증거 수집
  -> 증거가 실재하면 완료
  -> 아니면 루프 반복
```

**ralph 계획 게이트:**

ralph가 활성화된 상태에서는 `.omx/plans/prd-*.md`와 `.omx/plans/test-spec-*.md`가 존재해야만 구현 작업을 시작할 수 있습니다.

**omx team ralph와의 관계:** [`omx team ralph` 항목 참조]

---

### ralplan / RALPLAN-DR

**정의:** 구조화된 숙의(deliberation) 방식의 계획 스킬. 거친 요청을 스펙, 수용 기준, 레인별 분해로 변환합니다.

**트리거 키워드:** "ralplan", "consensus plan"

**RALPLAN-DR 구조:**

- 기본: 단기 숙의 (`--deliberate` 없이)
- `--deliberate` 옵션: 고위험 작업을 위한 확장 숙의 (사전 부검 + 확장된 유닛/통합/e2e/관측성 테스트 계획 포함)

**사용:**
```text
$ralplan "이 기능을 구현 레인으로 분해하라"
```

터미널에서:
```bash
omx ask --agent-prompt planner "ralplan: OAuth 콜백을 워커 레인과 수용 기준으로 분해하라"
```

**결과물:**
- `.omx/plans/prd-*.md` — 제품 요구 사항 문서
- `.omx/plans/test-spec-*.md` — 테스트 명세

**권장 워크플로우:** `ralplan → team → ralph`

---

### 런타임 하네스 (Runtime Harness)

**정의:** OMX의 아키텍처 역할을 설명하는 용어. Codex CLI를 직접 실행하는 대신, 그 주변을 감싸서 조율, 상태 관리, 운영자 제어를 추가합니다.

**하네스가 추가하는 것:**

| 기능 | 설명 |
|------|------|
| 팀 조율 | 멀티 에이전트 워크플로우 관리 |
| 영속 상태 | 세션 간 상태 유지 |
| HUD 통합 | 실시간 모니터링 |
| 복구 경로 | ralph, resume, retry |
| 운영자 제어 | start, inspect, cancel, shutdown |

**핵심 원칙:** Codex는 실행 엔진으로 유지되고, OMX는 그 주변 런타임을 제공합니다.

---

## S

### Spark Initiative

**정의:** OMX를 단순 TypeScript CLI에서 **native helper를 함께 배포·검증하는 hybrid runtime**으로 확장한 역사적 전환점. `omx explore`, `omx sparkshell`, hydration 계약, release-oriented verification이 핵심이다.

**핵심 변화:**
- 읽기 전용 fast path인 `omx explore`
- operator-facing shell-native inspection surface인 `omx sparkshell`
- `native-release-manifest.json` 기반 hydration 계약
- `build:full`, `test:explore`, `test:sparkshell`, packed install smoke 같은 릴리스 검증 표면

**현재 맥락:**
- Spark Initiative 자체는 `0.9.x`에서 시작된 전환이다.
- 하지만 현재 학습 기준선은 `0.11.11`이며, Spark는 지금 버전의 배경층으로 읽는 편이 맞다.

---

### Spark 모델 (gpt-5.3-codex-spark)

**정의:** Team Mode 워커에 사용할 수 있는 경량 고속 모델. 리더 모델보다 약 1.3배 빠릅니다.

**설정:**
```bash
OMX_SPARK_MODEL=gpt-5.3-codex-spark   # 기본값

# 팀 워커에 Spark 모델 사용
omx --spark team 3:executor "작업"

# Spark + madmax (워커 및 리더 승인 우회)
omx --madmax-spark team 3:executor "작업"
```

**워커 모델 우선순위:**
1. `OMX_TEAM_WORKER_LAUNCH_ARGS`의 명시적 모델
2. 리더 `--model` 상속
3. `OMX_SPARK_MODEL` 기본값

**직접 설정:**
```bash
OMX_TEAM_WORKER_CLI=codex \
OMX_TEAM_WORKER_LAUNCH_ARGS='--model gpt-5.3-codex-spark -c model_reasoning_effort="low"' \
omx team 2:explore "분석 작업"
```

---

### 스킬 (Skill)

**정의:** Codex 내에서 `$name`으로 호출 가능한 워크플로우 커맨드. user 스코프 또는 project 스코프의 스킬 디렉터리에 설치됩니다.

**호출 방법:**
```text
$ralph         # ralph 루프 실행
$team          # 팀 오케스트레이션
$autopilot     # 자율 실행
$ultrawork     # 병렬 에이전트
$plan          # 전략적 계획
$cancel        # 취소
```

**핵심 스킬 카탈로그:**

| 스킬 | 트리거 | 설명 |
|------|--------|------|
| `autopilot` | "autopilot", "build me", "I want a" | 아이디어에서 동작하는 코드까지 |
| `ralph` | "ralph", "don't stop" | 검증 완료까지 루프 유지 |
| `ultrawork` | "ultrawork", "ulw", "parallel" | 병렬 에이전트 최대 활용 |
| `team` | "team", "swarm" | 구조화된 멀티 에이전트 |
| `plan` | "plan this", "plan the" | 전략적 계획 수립 |
| `ralplan` | "ralplan", "consensus plan" | RALPLAN-DR 계획 |
| `ultraqa` | "ultraqa" | QA 사이클 |
| `ecomode` | "ecomode", "eco", "budget" | 토큰 효율 모드 |
| `cancel` | "cancel", "stop", "abort" | 모드 취소 |
| `tdd` | "tdd", "test first" | 테스트 주도 개발 |
| `build-fix` | "fix build", "type errors" | 빌드 오류 수정 |
| `code-review` | "review code", "code review" | 코드 리뷰 |
| `security-review` | "security review" | 보안 감사 |
| `web-clone` | "web-clone", "clone site" | 웹사이트 클론 파이프라인 |
| `visual-verdict` | 직접 호출 | 시각적 QA 검증 |
| `configure-notifications` | 직접 호출 | 알림 설정 |

**v0.5.0에서 제거된 스킬:**

`deep-executor`, `scientist` 프롬프트와 9개의 deprecated 스킬이 제거되었습니다.

---

### 실행 플레인 (Execution Plane)

**정의:** OMX의 3-plane 아키텍처에서 실제 에이전트 작업을 실행하는 레이어. Codex CLI가 이 플레인을 담당합니다.

**구성 요소:** Codex CLI

**역할:** 프롬프트 처리, 코드 생성, 파일 조작, 쉘 명령 실행

---

## T

### Team Mode / omx team

**정의:** OMX의 기본 멀티 에이전트 오케스트레이션 표면. 여러 Codex 워커가 공유 가시성, 재개, 복구, 생명주기 제어 아래 협력합니다.

**사용:**
```bash
# 기본 팀 실행
omx team 3:executor "범위가 정해진 작업을 검증과 함께 완료하라"

# 상태 확인
omx team status <team-name>

# 재개
omx team resume <team-name>

# 종료
omx team shutdown <team-name>

# ralph와 연계
omx team ralph 3:executor "구현 후 ralph 검증"
```

**팀 파이프라인:**

```
team-plan -> team-prd -> team-exec -> team-verify -> team-fix (루프)
```

단계별 설명:

| 단계 | 역할 에이전트 | 설명 |
|------|-------------|------|
| `team-plan` | explore, planner | 탐색 및 계획 |
| `team-prd` | analyst, critic | 수용 기준 및 범위 확정 |
| `team-exec` | executor, 전문가 | 구현 실행 |
| `team-verify` | verifier, reviewer | 완료 검증 |
| `team-fix` | executor, debugger | 결함 수정 (루프) |

**터미널 상태:** `complete`, `failed`, `cancelled`

**워커 CLI 선택:**

```bash
OMX_TEAM_WORKER_CLI=auto    # 기본 (워커 모델에 "claude" 포함 시 claude CLI)
OMX_TEAM_WORKER_CLI=codex   # Codex CLI 강제
OMX_TEAM_WORKER_CLI=claude  # Claude CLI 강제
OMX_TEAM_WORKER_CLI_MAP=codex,codex,claude,claude  # 워커별 CLI 지정
```

**Team vs. Ultrawork:**

| 구분 | Team Mode | Ultrawork |
|------|-----------|-----------|
| 목적 | 오케스트레이션 | 병렬성 |
| 적합 상황 | 공유 컨텍스트, 블로커 처리 | 독립적 하위 작업 |
| 상태 관리 | 영속 (durable) | 경량 |
| 핸드오프 | 지원 | 미지원 |

---

## U

### ultrawork

**정의:** 독립적인 하위 작업을 병렬 에이전트로 실행하는 경량 팬아웃 모드.

**트리거 키워드:** "ultrawork", "ulw", "parallel"

**사용:**
```text
$ultrawork "이 5개 모듈을 병렬로 리팩토링하라"
```

**Team Mode와의 차이:**

```
ultrawork = 병렬성 (parallelism)
team = 오케스트레이션 (orchestration)
```

Ultrawork는 하위 작업이 독립적이고 리더가 결과를 병합할 수 있을 때 사용합니다. 작업 간 공유 컨텍스트, 블로커, 핸드오프가 필요하면 Team Mode를 사용합니다.

---

### ultraqa

**정의:** 테스트 → 검증 → 수정 → 반복 사이클을 자동화하는 QA 워크플로우.

**트리거 키워드:** "ultraqa"

**사용:**
```text
$ultraqa
```

**autopilot와의 관계:** autopilot이 내부적으로 ultraqa를 전환할 수 있습니다.

---

## V

### visual-verdict (시각적 판정)

**정의:** 시각적 충실도(visual fidelity)에 의존하는 작업을 위한 QA 스킬. 참조 이미지와 생성된 스크린샷을 비교합니다.

**반환 형식:**

```json
{
  "score": 92,
  "verdict": "pass",
  "category_match": true,
  "differences": [],
  "suggestions": [],
  "reasoning": "..."
}
```

**권장 통과 임계값:** 90+

**사용 규칙:**

시각적 작업의 경우 매 이터레이션 전에 `$visual-verdict`를 실행합니다.

**결과 저장:**

`.omx/state/{scope}/ralph-progress.json`

---

## W

### worktree

**정의:** git worktree 기능. OMX는 `-w` 플래그로 worktree 내에서 Codex를 실행하는 것을 지원합니다.

**사용:**
```bash
omx -w                    # 이름 없이 분리된 worktree 실행
omx -w=feature-branch     # 명명된 worktree 실행
omx --worktree=my-branch  # 동일
```

---

## X

### xhigh (추론 노력)

**정의:** Codex의 최고 수준 추론 노력 설정. `model_reasoning_effort="xhigh"`에 해당합니다.

**사용:**
```bash
omx --xhigh               # xhigh 추론
omx --xhigh --madmax      # 권장 신뢰 환경 프로파일
```

**추론 레벨 비교:**

| 플래그 | 수준 | 설명 |
|--------|------|------|
| (없음) | 기본 | config.toml의 기본값 (보통 "high") |
| `--high` | high | 높은 추론 노력 |
| `--xhigh` | xhigh | 최고 추론 노력 |

**reasoning 커맨드:**
```bash
omx reasoning low     # 낮은 추론
omx reasoning medium  # 중간 추론
omx reasoning high    # 높은 추론
omx reasoning xhigh   # 최고 추론
```

---

## Y

### yolo

**정의:** Codex의 yolo 모드 실행 플래그. 확인 없이 작업을 실행합니다.

**사용:**
```bash
omx --yolo
```

> **주의:** `--yolo`와 `--madmax`는 다릅니다. `--yolo`는 Codex 내 확인 프롬프트를 건너뜁니다. `--madmax`는 샌드박스 자체를 우회합니다.

---

## 제어 플레인 (Control Plane)

**정의:** OMX의 3-plane 아키텍처에서 팀 워커 관리, 생명주기 제어, HUD/tmux 통합, 복구를 담당하는 레이어.

**구성 요소:**
- `omx` 런타임
- Team Mode (`omx team`)
- HUD (`omx hud`)
- 생명주기 커맨드 (start, resume, cancel, shutdown)
- 복구 메커니즘 (ralph, resume)

---

## 상태 플레인 (State Plane)

**정의:** OMX의 3-plane 아키텍처에서 MCP 서버를 통해 상태, 메모리, 진단을 관리하는 레이어.

**구성 요소:**
- `omx_state` MCP 서버
- `omx_memory` MCP 서버
- `omx_trace` MCP 서버
- `omx_code_intel` MCP 서버
- `.omx/` 디렉토리

---

## 용어 빠른 참조 표

| 용어 | 한국어 설명 | 관련 커맨드 |
|------|------------|-------------|
| OMX | Codex CLI 운영 런타임 하네스 | `omx`, `npm install -g oh-my-codex` |
| AGENTS.md | 오케스트레이션 브레인 파일 | `omx setup`, `omx agents-init` |
| config.toml | Codex 설정 파일 | `omx setup` |
| .omx/ | 런타임 상태 디렉토리 | `omx setup` |
| Team Mode | 멀티 에이전트 오케스트레이션 | `omx team N:role "task"` |
| Ralph | 영속 실행 루프 | `$ralph`, `omx ralph` |
| Ralplan | 구조화된 계획 스킬 | `$ralplan` |
| RALPLAN-DR | 구조화된 숙의 계획 | `$ralplan --deliberate` |
| Ultrawork | 경량 병렬 팬아웃 | `$ultrawork` |
| ultraqa | QA 사이클 자동화 | `$ultraqa` |
| HUD | 실시간 상태 모니터 | `omx hud --watch` |
| MCP 서버 | 영속 상태/메모리/추적 | config.toml 자동 설정 |
| Spark 모델 | 경량 고속 워커 모델 | `omx --spark`, `OMX_SPARK_MODEL` |
| madmax | 승인/샌드박스 우회 | `omx --madmax` |
| yolo | 확인 프롬프트 건너뜀 | `omx --yolo` |
| xhigh | 최고 추론 노력 | `omx --xhigh` |
| 포스처 | 에이전트 운영 스타일 | 자동 라우팅 (실험적) |
| frontier-orchestrator | 리더/라우터 포스처 | planner, architect |
| deep-worker | 구현 우선 포스처 | executor, build-fixer |
| fast-lane | 경량 검색 포스처 | explore, writer |
| OpenClaw | 알림 게이트웨이 | `$configure-notifications` |
| omx setup | 환경 구성 커맨드 | `omx setup [--scope]` |
| omx doctor | 설치 진단 커맨드 | `omx doctor [--team]` |
| 실행 플레인 | Codex CLI 레이어 | — |
| 제어 플레인 | omx 런타임 레이어 | `omx team`, `omx hud` |
| 상태 플레인 | MCP 서버 레이어 | `.omx/` 디렉토리 |
