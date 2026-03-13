# 하네스 엔지니어링 - OMX 내부 구조

## 1. 하네스 엔지니어링이란?

### 하네스의 정의

**하네스(harness)**는 핵심 도구를 감싸는 런타임 확장 레이어다. 항공/자동차 공학에서 "하네스"는 복잡한 배선 묶음을 안전하게 고정하고 제어하는 부품을 뜻한다. 소프트웨어에서도 같은 의미다: 핵심 실행 엔진(Codex)을 건드리지 않고 그 주변에 관찰, 제어, 복구, 확장 레이어를 붙이는 것이다.

OMX는 다음 구성요소를 통해 Codex CLI를 하네스한다:

| 하네스 레이어 | OMX 구현체 | 역할 |
|-------------|-----------|------|
| 컨텍스트 주입 | `agents-overlay.ts` + AGENTS.md | 세션 시작 시 동적 컨텍스트 삽입 |
| 설정 자동화 | `config/generator.ts` | `config.toml` 자동 생성 및 병합 |
| 라이프사이클 훅 | `scripts/notify-hook.js` | Codex 이벤트 인터셉트 |
| 플러그인 확장 | `.omx/hooks/*.mjs` | 사용자 정의 훅 플러그인 |
| 네이티브 fast path | `omx explore` + `omx sparkshell` + hydration | 읽기 전용 탐색/셸 검사/릴리스 자산 기반 네이티브 보조 경로 |
| 상태 백플레인 | MCP 서버 5개 | 상태/메모리/추적/팀 관리 |
| 팀 오케스트레이션 | `cli/team.ts` + tmux | 병렬 워커 조율 |

### OMX가 Codex CLI를 하네스하는 방식

Codex CLI는 두 가지 핵심 확장 포인트를 제공한다:

1. **`-c model_instructions_file="<path>"`**: 실행 시 지시 파일을 주입한다.
2. **`config.toml`의 `[mcp_servers.*]`**: MCP 서버를 자동으로 연결한다.

OMX는 이 두 포인트를 완전히 활용한다. `omx setup`은 `config.toml`에 5개의 MCP 서버 항목과 에이전트 롤 설정을 자동으로 기록한다. `omx` 실행 시에는 세션 전용 AGENTS.md를 생성하여 `-c model_instructions_file` 플래그로 주입한다.

### 왜 단순 플러그인이 아닌 "운영 런타임"인가

단순 플러그인은 기능을 추가할 뿐이다. 운영 런타임은 작업의 **지속성**, **가시성**, **복구 가능성**을 보장한다.

- **지속성**: ralph 루프는 작업이 완료될 때까지 반복 실행을 유지한다.
- **가시성**: HUD, status, trace 명령으로 실행 중인 모든 상태를 확인한다.
- **복구 가능성**: `omx resume --last`로 중단된 세션을 재개한다.
- **조율**: 팀 모드는 여러 워커의 라이프사이클을 단일 오퍼레이터 뷰로 통합한다.

### Spark Initiative가 하네스 엔지니어링에 추가한 것

`v0.9.0`의 Spark Initiative는 단순히 새 커맨드를 추가한 것이 아니라, OMX 하네스 자체에 **네이티브 fast path**를 추가했다는 의미가 있다.

- `omx explore`는 제한된 allowlist와 read-only 계약을 가진 탐색 전용 표면이다.
- `omx sparkshell`은 빠른 셸 실행/요약과 tmux pane 캡처를 위한 명시적 오퍼레이터 표면이다.
- 단순 read-only shell 작업은 `explore -> sparkshell` 경로로 라우팅될 수 있다.
- 패키지 설치는 GitHub Release 자산과 `native-release-manifest.json`을 통해 필요한 네이티브 도우미를 hydration한다.
- 런타임 fallback order는 `OMX_*_BIN` override → hydrated cache → repo-local artifact 순서로 더 명확해졌다.

이 변화 덕분에 OMX는 TypeScript 설정/오케스트레이션 하네스이면서도, 일부 read-only/operator workflows에 대해서는 Rust 네이티브 경로를 조합하는 혼합형 런타임이 되었다.

---

## 2. Hook 시스템

### 2.1 훅의 역할

OMX의 훅 시스템은 Codex 라이프사이클 이벤트에 반응하여 사이드이펙트를 실행한다. 핵심 훅은 `scripts/notify-hook.js`다. 이 스크립트는 `config.toml`의 `notify` 키에 등록되어 Codex가 매 턴(turn) 완료 시 자동으로 호출한다.

```toml
# config.toml에 omx setup이 기록하는 훅 등록
notify = ["node", "/path/to/oh-my-codex/scripts/notify-hook.js"]
```

훅이 수신하는 페이로드 구조:

```json
{
  "type": "agent-turn-complete",
  "cwd": "/path/to/project",
  "thread-id": "thread-abc123",
  "turn-id": "turn-xyz789",
  "input-messages": ["사용자 메시지"],
  "last-assistant-message": "어시스턴트 응답 텍스트"
}
```

훅이 처리하는 주요 작업:
- tmux-hook: 설정된 tmux 패인에 프롬프트 주입 (ralph/ultrawork/team 모드 지속성 지원)
- 플러그인 디스패치: `OMX_HOOK_PLUGINS=1`일 때 `.omx/hooks/*.mjs` 플러그인 실행
- 상태 갱신: 세션 메트릭 업데이트, 로그 기록

### 2.2 Mermaid: Hook 이벤트 플로우

![Diagram 1](../assets/diagrams/sections__02-harness-engineering__diagram_1.svg)

### 2.3 훅 확장 (omx hooks)

`omx hooks`는 사용자 정의 훅 플러그인을 위한 새로운 확장 포인트다.

**플러그인 파일 구조:**

```bash
# 초기화
omx hooks init
# 생성되는 파일:
# .omx/hooks/sample-plugin.mjs
```

생성되는 스캐폴드 플러그인:

```js
// .omx/hooks/sample-plugin.mjs
export async function onHookEvent(event, sdk) {
  // event.schema_version === "1"
  // event.event: "session-start" | "session-end" | "turn-complete" | "session-idle"
  // event.source: "native" | "derived"
  // event.context: { ... }
  // event.session_id, event.thread_id, event.turn_id, event.mode

  sdk.log.info('이벤트 수신:', event.event);

  // tmux 패인에 키 전송
  // await sdk.tmux.sendKeys('omx-session', 'Continue\n');

  // 플러그인 네임스페이스 범위의 상태 읽기/쓰기
  // await sdk.state.write('my-key', { value: 42 });
  // const data = await sdk.state.read('my-key');
}
```

**SDK 표면:**

| SDK 메서드 | 용도 |
|-----------|------|
| `sdk.tmux.sendKeys(target, keys)` | tmux 패인에 키 전송 |
| `sdk.log.info/warn/error(msg)` | `.omx/logs/hooks-*.jsonl`에 기록 |
| `sdk.state.read(key)` | 플러그인 네임스페이스 상태 읽기 |
| `sdk.state.write(key, value)` | 플러그인 네임스페이스 상태 쓰기 |
| `sdk.state.delete(key)` | 플러그인 상태 삭제 |
| `sdk.state.all()` | 플러그인 전체 상태 조회 |

**활성화 및 튜닝:**

```bash
# 플러그인 디스패치 활성화 (기본값: 비활성)
export OMX_HOOK_PLUGINS=1

# 타임아웃 조정 (기본값: 1500ms)
export OMX_HOOK_PLUGIN_TIMEOUT_MS=3000

# 파생 이벤트 활성화 (기본값: 비활성)
export OMX_HOOK_DERIVED_SIGNALS=1
```

**네이티브 이벤트 목록 (v1):**

| 이벤트 | 설명 | source |
|--------|------|--------|
| `session-start` | 세션 시작 | native |
| `session-end` | 세션 종료 | native |
| `turn-complete` | 에이전트 턴 완료 | native |
| `session-idle` | 세션 유휴 상태 | native |
| `needs-input` | 사용자 입력 대기 (파생) | derived |
| `pre-tool-use` | 도구 사용 직전 (파생) | derived |
| `post-tool-use` | 도구 사용 직후 (파생) | derived |

⚠️ 팀 워커 세션(`OMX_TEAM_WORKER` 환경변수 설정 시)에서는 플러그인 사이드이펙트가 기본으로 스킵된다. 리더 세션이 캐노니컬 이벤트 에미터 역할을 한다.

### 2.4 tmux-hook vs hooks 차이

두 시스템은 서로 다른 목적을 가지며 공존한다.

| 특성 | `omx tmux-hook` | `omx hooks` |
|------|----------------|-------------|
| 도입 시기 | 기존 (original) | 신규 (additive) |
| 설정 파일 | `.omx/tmux-hook.json` | `.omx/hooks/*.mjs` |
| 주요 목적 | ralph/team 루프 지속성 지원 | 사용자 정의 자동화 |
| 활성화 | 기본 활성 | `OMX_HOOK_PLUGINS=1` 필요 |
| 언어 | 내장 (TypeScript 컴파일 결과) | ESM JavaScript (.mjs) |
| 진입점 | notify-hook.js 내부 | `onHookEvent(event, sdk)` export |

`tmux-hook` 설정 파일 예시:

```json
{
  "enabled": true,
  "target": { "type": "pane", "value": "%42" },
  "allowed_modes": ["ralph", "ultrawork", "team"],
  "cooldown_ms": 15000,
  "max_injections_per_session": 200,
  "prompt_template": "Continue from current mode state. [OMX_TMUX_INJECT]",
  "marker": "[OMX_TMUX_INJECT]",
  "dry_run": false,
  "log_level": "info",
  "skip_if_scrolling": true
}
```

💡 `omx tmux-hook validate`로 tmux 타겟 패인의 접근 가능성을 검증할 수 있다.

💡 `omx tmux-hook test`로 실제 notify-hook 페이로드를 합성하여 end-to-end 테스트를 실행할 수 있다.

---

## 3. MCP 서버 통합

### 3.1 MCP가 하네스 엔지니어링에서 하는 역할

MCP(Model Context Protocol) 서버는 Codex 에이전트가 도구(tool)를 호출하는 방식으로 OMX의 런타임 기능에 접근하는 채널이다. Codex는 도구 호출을 통해 상태를 읽고 쓰고, 메모리를 조회하고, 실행 흔적을 추적한다.

OMX가 MCP를 선택한 이유:
- Codex CLI의 네이티브 확장 메커니즘이다.
- 추가 프로세스 없이 표준화된 인터페이스를 제공한다.
- 에이전트와 런타임 간 경계를 명확히 유지한다.
- `config.toml`에 서버 항목만 추가하면 자동으로 활성화된다.

### 3.2 5개의 MCP 서버

`omx setup`은 `config.toml`에 다음 5개의 MCP 서버를 등록한다.

#### omx_state - 실행 모드 상태 관리

파일: `dist/mcp/state-server.js`

지원 모드: `autopilot`, `team`, `ralph`, `ultrawork`, `ultraqa`, `ralplan`

제공 도구:

| 도구 | 설명 |
|------|------|
| `state_read` | 특정 모드의 상태 파일 읽기 |
| `state_write` | 모드 상태 쓰기 (직렬화된 쓰기 락 사용) |
| `state_clear` | 모드 상태 초기화 |
| `state_list_active` | 현재 활성 모드 목록 조회 |
| `state_get_status` | 상태 요약 (active, iteration, phase 등) |

상태 파일은 `.omx/state/{mode}-state.json` 또는 세션 범위 경로 `.omx/state/sessions/{sessionId}/{mode}-state.json`에 저장된다.

#### omx_memory - 영구 메모리 및 노트패드

파일: `dist/mcp/memory-server.js`

제공 도구:

| 도구 | 설명 |
|------|------|
| `project_memory_read` | 프로젝트 메모리 읽기 (섹션별: techStack, build, conventions, structure, notes, directives) |
| `project_memory_write` | 메모리 쓰기 또는 병합 |
| `project_memory_add_note` | 카테고리별 노트 추가 |
| `project_memory_add_directive` | 우선순위 디렉티브 추가 |
| `notepad_read` | 노트패드 읽기 (all/priority/working/manual 섹션) |
| `notepad_write_priority` | 우선순위 섹션 기록 (최대 500자, 세션 시작 시 로드) |
| `notepad_write_working` | 타임스탬프 작업 노트 기록 (7일 후 자동 삭제) |
| `notepad_write_manual` | 영구 노트 기록 (자동 삭제 없음) |
| `notepad_prune` | 오래된 노트 정리 |
| `notepad_stats` | 노트패드 통계 조회 |

저장 위치:
- `.omx/project-memory.json` (프로젝트 메모리)
- `.omx/notepad.md` (노트패드)

#### omx_code_intel - 코드 분석

파일: `dist/mcp/code-intel-server.js`

TypeScript 컴파일러와 `ast-grep` CLI를 래핑하여 LSP 수준의 코드 분석을 제공한다.

제공 도구:
- TypeScript 타입 진단 (`tsc --noEmit` 래퍼)
- 심볼 검색
- AST 패턴 매칭 (`ast-grep` 래퍼)

#### omx_trace - 실행 추적

파일: `dist/mcp/trace-server.js`

`.omx/logs/turns-*.jsonl` 파일을 읽어 에이전트 실행 흐름의 타임라인과 통계를 제공한다.

제공 도구:

| 도구 | 설명 |
|------|------|
| `trace_timeline` | 에이전트 턴 타임라인 조회 |
| `trace_summary` | 실행 통계 요약 |

#### omx_team_run - 팀 작업 라이프사이클

파일: `dist/mcp/team-server.js`

팀 모드 작업의 생성, 상태 확인, 대기, 정리를 담당한다.

제공 도구:
- `omc_run_team_start`
- `omc_run_team_status`
- `omc_run_team_wait`
- `omc_run_team_cleanup`

### 3.3 Mermaid: MCP 데이터 플로우

![Diagram 2](../assets/diagrams/sections__02-harness-engineering__diagram_2.svg)

### 3.4 config.toml MCP 설정

`omx setup`이 `config.toml`에 기록하는 MCP 서버 항목 구조:

```toml
# ============================================================
# oh-my-codex (OMX) Configuration
# Managed by omx setup - manual edits preserved on next setup
# ============================================================

# OMX State Management MCP Server
[mcp_servers.omx_state]
command = "node"
args = ["/path/to/oh-my-codex/dist/mcp/state-server.js"]
enabled = true
startup_timeout_sec = 5

# OMX Project Memory MCP Server
[mcp_servers.omx_memory]
command = "node"
args = ["/path/to/oh-my-codex/dist/mcp/memory-server.js"]
enabled = true
startup_timeout_sec = 5

# OMX Code Intelligence MCP Server (LSP diagnostics, AST search)
[mcp_servers.omx_code_intel]
command = "node"
args = ["/path/to/oh-my-codex/dist/mcp/code-intel-server.js"]
enabled = true
startup_timeout_sec = 10

# OMX Trace MCP Server (agent flow timeline & statistics)
[mcp_servers.omx_trace]
command = "node"
args = ["/path/to/oh-my-codex/dist/mcp/trace-server.js"]
enabled = true
startup_timeout_sec = 5

# OMX Team MCP Server (team job lifecycle: start, status, wait, cleanup)
[mcp_servers.omx_team_run]
command = "node"
args = ["/path/to/oh-my-codex/dist/mcp/team-server.js"]
enabled = true
startup_timeout_sec = 5
```

⚠️ OMX 마커 블록(`# oh-my-codex (OMX) Configuration` ~ `# End oh-my-codex`) 내부의 항목은 `omx setup` 실행 시 자동으로 갱신된다. 마커 블록 **외부**의 사용자 설정은 보존된다.

🔧 `OMX_MCP_WORKDIR_ROOTS` 환경변수를 설정하면 MCP 서버가 수락하는 `workingDirectory` 값을 지정된 루트 경로로 제한할 수 있다.

---

## 4. AGENTS.md 주입 메커니즘

### 4.1 어떻게 작동하는가

AGENTS.md 주입은 OMX의 가장 핵심적인 하네스 메커니즘이다. `omx` 실행 시 다음 플래그가 Codex에 전달된다:

```bash
codex -c model_instructions_file="<sessionPath>/AGENTS.md" [other-flags]
```

이 플래그는 Codex의 시스템 지시(system instructions)를 AGENTS.md 내용으로 확장한다. Codex 핵심 정책을 대체하지 않고, 그 위에 프로젝트 특화 지시를 추가하는 방식이다.

세션 전용 AGENTS.md는 다음 내용을 합성(compose)한다:

1. **기본 AGENTS.md** (`<cwd>/AGENTS.md`): 프로젝트 오케스트레이션 브레인
2. **런타임 오버레이**: 세션별 동적 컨텍스트 (최대 3500자)
   - 세션 메타데이터 (sessionId, 타임스탬프)
   - 코드베이스 맵
   - 활성 모드 상태 (ralph iteration, team phase 등)
   - 노트패드 PRIORITY 섹션
   - 프로젝트 메모리 요약 (techStack, conventions, 고우선순위 디렉티브)
   - ralph 플래닝 게이트 상태 (UNLOCKED/BLOCKED)
   - 컴팩션 프로토콜

오버레이는 마커로 구분된다:
```markdown
<!-- OMX:RUNTIME:START -->
<session_context>
... 동적 컨텍스트 내용 ...
</session_context>
<!-- OMX:RUNTIME:END -->
```

### 4.2 Mermaid: AGENTS.md 생성 및 주입 플로우

![Diagram 3](../assets/diagrams/sections__02-harness-engineering__diagram_3.svg)

### 4.3 User scope vs Project scope

`omx setup --scope user|project`로 설치 범위를 지정한다.

| 설치 항목 | user scope | project scope |
|----------|-----------|--------------|
| 프롬프트 설치 경로 | `~/.codex/prompts/` | `./.codex/prompts/` |
| 스킬 설치 경로 | `~/.agents/skills/` | `./.agents/skills/` |
| config.toml 경로 | `~/.codex/config.toml` | `./.codex/config.toml` |
| 네이티브 에이전트 경로 | `~/.omx/agents/` | `./.omx/agents/` |
| AGENTS.md 생성 | 생성 안 함 | `<cwd>/AGENTS.md` 생성/갱신 |
| Codex 런치 | 기본 CODEX_HOME | `CODEX_HOME=./.codex` 자동 설정 |

💡 설치 범위는 `.omx/setup-scope.json`에 저장되어 다음 `omx` 실행 시 자동으로 적용된다.

⚠️ `omx` 세션이 활성 상태일 때 AGENTS.md 덮어쓰기는 차단된다. 활성 세션을 먼저 종료하고 `omx setup`을 재실행해야 한다.

### 4.4 omx agents-init

`omx agents-init [path]`는 전체 setup 없이 AGENTS.md만 빠르게 생성한다.

```bash
# 현재 디렉토리 및 하위 디렉토리에 AGENTS.md 생성
omx agents-init .

# 드라이런으로 미리 확인
omx agents-init ./src --dry-run

# 기존 AGENTS.md도 강제 갱신
omx agents-init . --force
```

동작 규칙:
- 대상 디렉토리와 직계 하위 디렉토리에 AGENTS.md를 생성한다
- `.git`, `.omx`, `.codex`, `.agents`, `node_modules`, `dist`, `build` 디렉토리는 건너뛴다
- `<!-- OMX:AGENTS-MANUAL:* -->` 마커 섹션은 갱신 시 보존된다
- 비관리형(unmanaged) AGENTS.md는 `--force` 없이 수정하지 않는다
- 프롬프트, 스킬, config.toml, 실행 워크플로우는 변경하지 않는다

---

## 5. config.toml 자동 생성

### 5.1 omx setup이 쓰는 것들

`omx setup`은 `config/generator.ts`의 `buildMergedConfig()` 함수를 통해 `config.toml`을 생성한다. 생성 전략은 **병합(merge)**이다: 기존 사용자 설정을 보존하면서 OMX 관리 항목만 갱신한다.

생성되는 항목을 섹션별로 설명한다.

**루트 레벨 키 (TOML 헤더 이전):**

```toml
# oh-my-codex top-level settings (must be before any [table])
notify = ["node", "/path/to/scripts/notify-hook.js"]
model_reasoning_effort = "high"
developer_instructions = "You have oh-my-codex installed. AGENTS.md is your orchestration brain..."
model = "gpt-5.4"              # root model이 없을 때만 추가
model_context_window = 1000000  # gpt-5.4이고 키가 없을 때만 추가 (실험적)
model_auto_compact_token_limit = 900000  # 동일 조건
```

**[features] 섹션:**

```toml
[features]
multi_agent = true       # 멀티 에이전트 활성화
child_agents_md = true   # 하위 에이전트 AGENTS.md 지원
```

**MCP 서버 섹션:** (앞 절 참조)

**네이티브 에이전트 섹션:**

```toml
# OMX Native Agent Roles (Codex multi-agent)
[agents.executor]
description = "Code implementation, refactoring, feature work"
config_file = "/path/to/.omx/agents/executor.toml"

[agents.planner]
description = "Task sequencing, execution plans, risk flags"
config_file = "/path/to/.omx/agents/planner.toml"

# ... 모든 AGENT_DEFINITIONS 항목에 대해 생성
```

**[tui] 섹션:**

```toml
[tui]
status_line = ["model-with-reasoning", "git-branch", "context-remaining",
               "total-input-tokens", "total-output-tokens", "five-hour-limit"]
```

### 5.2 Mermaid: config.toml 생성 플로우

![Diagram 4](../assets/diagrams/sections__02-harness-engineering__diagram_4.svg)

🔧 `omx setup --dry-run`으로 실제 쓰기 없이 변경 내용을 미리 확인할 수 있다.

🔧 `omx setup --verbose`로 파일별 상세 변경 내용을 확인할 수 있다.

---

## 6. 스킬/프롬프트 설치 메커니즘

### 6.1 설치 경로

`omx setup`은 두 종류의 아티팩트를 설치한다: **프롬프트(에이전트 롤)**와 **스킬(워크플로우)**.

**User scope 설치 경로:**

```text
~/.codex/prompts/executor.md        # Codex /prompts:executor 명령으로 로드
~/.codex/prompts/planner.md
~/.codex/prompts/architect.md
~/.codex/prompts/...

~/.agents/skills/ralph/SKILL.md     # $ralph 스킬
~/.agents/skills/team/SKILL.md      # $team 스킬
~/.agents/skills/autopilot/SKILL.md
~/.agents/skills/.../SKILL.md
```

**Project scope 설치 경로:**

```text
./.codex/prompts/executor.md
./.codex/prompts/...

./.agents/skills/ralph/SKILL.md
./.agents/skills/.../SKILL.md
```

카탈로그 매니페스트(`src/catalog/manifest.json`)에 `status: "active"` 또는 `"internal"`인 항목만 설치된다. `"deprecated"` 상태 항목은 `--force` 플래그 없이는 설치되지 않고, `--force`시 기존 deprecated 항목이 정리된다.

### 6.2 Mermaid: 스킬 호출 체인

![Diagram 5](../assets/diagrams/sections__02-harness-engineering__diagram_5.svg)

스킬은 `$name` 명시 호출 또는 AGENTS.md의 키워드 감지 규칙을 통해 활성화된다. 주요 키워드-스킬 매핑:

| 키워드 | 스킬 | 스킬 파일 |
|--------|------|----------|
| `ralph`, `don't stop`, `must complete` | `$ralph` | `ralph/SKILL.md` |
| `autopilot`, `build me`, `I want a` | `$autopilot` | `autopilot/SKILL.md` |
| `ultrawork`, `ulw`, `parallel` | `$ultrawork` | `ultrawork/SKILL.md` |
| `team`, `swarm` | `$team` | `team/SKILL.md` |
| `plan this`, `let's plan` | `$plan` | `plan/SKILL.md` |
| `ralplan`, `consensus plan` | `$ralplan` | `ralplan/SKILL.md` |
| `cancel`, `stop`, `abort` | `$cancel` | `cancel/SKILL.md` |

에이전트 롤 프롬프트는 `/prompts:<name>` 명령 또는 `omx ask --agent-prompt <name>`으로 호출한다:

```bash
# Codex 세션 내에서
/prompts:executor
/prompts:architect
/prompts:planner

# omx ask 명령으로
omx ask claude --agent-prompt executor "이 기능 구현해줘"
omx ask gemini --agent-prompt planner "롤아웃 계획 작성해줘"
```

---

## 7. 팀 워커 CLI 선택 메커니즘

### 7.1 OMX_TEAM_WORKER_CLI

팀 모드는 여러 워커 프로세스를 병렬로 실행한다. 각 워커가 어떤 CLI를 사용할지 `OMX_TEAM_WORKER_CLI` 환경변수로 제어한다.

**선택 로직:**

```
OMX_TEAM_WORKER_CLI = "auto" (기본값)
  -> 워커 --model 인수에 "claude"가 포함되면 → claude CLI 사용
  -> 그 외 → codex CLI 사용

OMX_TEAM_WORKER_CLI = "codex"
  -> 모든 워커에 codex CLI 강제 사용

OMX_TEAM_WORKER_CLI = "claude"
  -> 모든 워커에 claude CLI 강제 사용
```

**per-worker 선택 (`OMX_TEAM_WORKER_CLI_MAP`):**

```bash
# 4개 워커 중 앞 2개는 codex, 뒤 2개는 claude
OMX_TEAM_WORKER_CLI_MAP=codex,codex,claude,claude omx team 4:executor "작업"

# 길이 1이면 모든 워커에 동일 적용 (OMX_TEAM_WORKER_CLI와 동일 효과)
OMX_TEAM_WORKER_CLI_MAP=codex omx team 3:executor "작업"
```

`OMX_TEAM_WORKER_CLI_MAP`은 `OMX_TEAM_WORKER_CLI`보다 우선순위가 높다.

**관련 환경변수:**

| 변수 | 기본값 | 설명 |
|------|--------|------|
| `OMX_TEAM_WORKER_CLI` | `auto` | 워커 CLI 선택 전략 |
| `OMX_TEAM_WORKER_CLI_MAP` | 없음 | per-worker CLI 지정 |
| `OMX_TEAM_WORKER_LAUNCH_ARGS` | 없음 | 워커 공통 실행 인수 |
| `OMX_TEAM_WORKER_LAUNCH_MODE` | tmux | `prompt` 설정 시 non-tmux 모드 |
| `OMX_SPARK_MODEL` | `gpt-5.3-codex-spark` | 낮은 복잡도 워커 폴백 모델 |
| `OMX_TEAM_AUTO_INTERRUPT_RETRY` | `1` | 적응형 큐→재전송 폴백 활성화 |

⚠️ Claude 워커 모드에서는 `--model`, `--config`, `--effort` 오버라이드가 무시된다. Claude CLI는 기본 `settings.json`을 사용한다.

**저토큰 팀 실행 예시:**

```bash
OMX_TEAM_WORKER_CLI=codex \
OMX_TEAM_WORKER_LAUNCH_ARGS='--model gpt-5.3-codex-spark -c model_reasoning_effort="low"' \
omx team 2:explore "빠른 범위 제한 분석 작업"
```

**--spark 플래그:**

```bash
# 팀 워커에만 spark 모델 사용 (리더는 기본 모델 유지)
omx --spark team 3:executor "작업"

# spark 모델 + dangerously-bypass-approvals
omx --madmax-spark team 3:executor "신뢰 환경 내 작업"
```

### 7.2 Mermaid: 워커 CLI 라우팅

![Diagram 6](../assets/diagrams/sections__02-harness-engineering__diagram_6.svg)

---

## 8. 포스처 라우팅 시스템 (실험적)

### 8.1 3가지 포스처

포스처(posture) 라우팅은 에이전트 롤을 역할(role), 티어(tier), 운영 스타일(posture)의 세 차원으로 분류하는 실험적 시스템이다.

각 에이전트 정의(`src/agents/definitions.ts`)에는 포스처, 모델 클래스, 라우팅 롤이 명시되어 있다.

**3가지 포스처:**

| 포스처 | 대상 에이전트 | 특성 |
|--------|-------------|------|
| `frontier-orchestrator` | planner, architect, critic, analyst, verifier | 라우팅 우선, 위임 적극적, 가정에 도전 |
| `deep-worker` | executor, debugger, build-fixer, test-engineer, designer | 직접 실행 우선, 테스트/진단 필수 |
| `fast-lane` | explore, writer, style-reviewer, vision | 빠른 분류/검색, 확장 시 에스컬레이션 |

**완전한 에이전트 포스처 분류표:**

| 에이전트 | posture | modelClass | reasoningEffort |
|---------|---------|-----------|----------------|
| `explore` | fast-lane | fast | low |
| `analyst` | frontier-orchestrator | frontier | medium |
| `planner` | frontier-orchestrator | frontier | medium |
| `architect` | frontier-orchestrator | frontier | high |
| `debugger` | deep-worker | standard | medium |
| `executor` | deep-worker | standard | medium |
| `verifier` | frontier-orchestrator | standard | medium |
| `quality-reviewer` | frontier-orchestrator | standard | medium |
| `code-reviewer` | frontier-orchestrator | frontier | high |
| `security-reviewer` | frontier-orchestrator | standard | medium |
| `test-engineer` | deep-worker | standard | medium |
| `build-fixer` | deep-worker | standard | medium |
| `writer` | fast-lane | fast | low |
| `critic` | frontier-orchestrator | frontier | high |
| `code-simplifier` | deep-worker | frontier | high |

### 8.2 Mermaid: 포스처 기반 라우팅

![Diagram 7](../assets/diagrams/sections__02-harness-engineering__diagram_7.svg)

네이티브 에이전트 `.toml` 파일 생성 예시 (`planner.toml`):

```toml
# oh-my-codex agent: planner
model_reasoning_effort = "medium"
developer_instructions = """
[planner.md 프롬프트 내용]

<posture_overlay>

You are operating in the frontier-orchestrator posture.
- Prioritize intent classification before implementation.
- Default to delegation and orchestration when specialists exist.
- Treat the first decision as a routing problem: research vs planning vs implementation vs verification.
- Challenge flawed user assumptions concisely before execution when the design is likely to cause avoidable problems.
- Preserve explicit executor handoff boundaries: do not absorb deep implementation work when a specialized executor is more appropriate.

</posture_overlay>

<model_class_guidance>

This role is tuned for frontier-class models.
- Use the model's steerability for coordination, tradeoff reasoning, and precise delegation.
- Favor clean routing decisions over impulsive implementation.

</model_class_guidance>

## OMX Agent Metadata
- role: planner
- posture: frontier-orchestrator
- model_class: frontier
- routing_role: leader
"""
```

포스처 실험 테스트:

```bash
npm run build
node bin/omx.js setup
# ~/.omx/agents/ 디렉토리에서 생성된 .toml 파일 확인
# OMX Posture Overlay, Model-Class Guidance, OMX Agent Metadata 섹션 존재 확인

node --test dist/agents/__tests__/definitions.test.js dist/agents/__tests__/native-config.test.js
```

---

## 9. 실전 하네스 엔지니어링 팁

### 훅 디버깅 방법

**tmux-hook 로그 확인:**

```bash
# 오늘 날짜 tmux-hook 로그
cat .omx/logs/tmux-hook-$(date +%Y-%m-%d).jsonl | jq .

# 훅 상태 확인
omx tmux-hook status

# 설정 유효성 검사
omx tmux-hook validate

# 합성 페이로드로 end-to-end 테스트
omx tmux-hook test "테스트 메시지"
```

**플러그인 훅 로그 확인:**

```bash
# 오늘 날짜 플러그인 훅 로그
cat .omx/logs/hooks-$(date +%Y-%m-%d).jsonl | jq .

# 플러그인 활성화 후 상태 확인
OMX_HOOK_PLUGINS=1 omx hooks status
OMX_HOOK_PLUGINS=1 omx hooks validate

# 합성 이벤트로 플러그인 테스트
OMX_HOOK_PLUGINS=1 omx hooks test
```

**notify-hook 직접 실행:**

```bash
# 수동으로 notify-hook 테스트 (JSON payload 전달)
node /path/to/oh-my-codex/scripts/notify-hook.js '{"type":"agent-turn-complete","cwd":"'$(pwd)'","thread-id":"test-1","turn-id":"turn-1","input-messages":["test"],"last-assistant-message":"test response"}'
```

### MCP 상태 직접 확인하는 법

MCP 도구는 Codex 세션 내에서 호출하거나, 상태 파일을 직접 읽어 확인할 수 있다.

```bash
# 활성 모드 상태 직접 확인
cat .omx/state/ralph-state.json | jq .
cat .omx/state/team-state.json | jq .

# 세션 범위 상태 확인
ls .omx/state/sessions/
cat .omx/state/sessions/<sessionId>/ralph-state.json | jq .

# 프로젝트 메모리 확인
cat .omx/project-memory.json | jq .

# 노트패드 확인
cat .omx/notepad.md

# 실행 추적 로그 확인 (최신 10개 턴)
tail -n 10 .omx/logs/turns-$(date +%Y-%m-%d).jsonl | jq .

# omx 상태 명령
omx status
```

```bash
# Codex 세션 내에서 MCP 도구로 상태 확인
# state_read로 ralph 상태 조회
# state_list_active로 활성 모드 목록 확인
# project_memory_read로 프로젝트 메모리 확인
# trace_timeline로 최근 에이전트 플로우 확인
```

### AGENTS.md 커스터마이즈 전략

**수동 섹션 보호:**

AGENTS.md에서 `<!-- OMX:AGENTS-MANUAL:START -->` / `<!-- OMX:AGENTS-MANUAL:END -->` 마커로 감싸진 섹션은 `omx setup` 갱신 시 보존된다.

```markdown
<!-- OMX:AGENTS-MANUAL:START -->
## 프로젝트 특화 규칙

- 이 프로젝트는 TypeScript strict 모드를 사용한다.
- 모든 API 변경은 CHANGELOG.md에 기록한다.
<!-- OMX:AGENTS-MANUAL:END -->
```

**runtime 오버레이 내용 제어:**

오버레이 최대 크기는 3500자다. 섹션은 우선순위 순으로 드롭된다 (optional 섹션부터):

1. optional: codebase_map (최대 1000자)
2. optional: active_modes (최대 600자)
3. optional: priority_notes (최대 600자)
4. optional: project_context (최대 1000자)
5. optional: team_orchestrator
6. required: ralph_planning_gate (ralph 활성 시)
7. required: compaction (항상 포함)
8. required: session (항상 포함)

노트패드 PRIORITY 섹션을 활용하면 오버레이에 우선순위 정보를 주입할 수 있다:

```bash
# Codex 세션 내에서 MCP 도구로:
# notepad_write_priority "중요: 이 PR은 배포 전 QA 검증 필수"
```

### 워커 CLI 최적화

**목적별 권장 설정:**

```bash
# 1. 최고 품질 (기본, 가장 비용 높음)
omx team 3:executor "고품질 구현 작업"

# 2. 비용 절감 (spark 워커)
omx --spark team 3:executor "작업"

# 3. 저토큰 탐색 (explore 롤 + 낮은 reasoning)
OMX_TEAM_WORKER_CLI=codex \
OMX_TEAM_WORKER_LAUNCH_ARGS='--model gpt-5.3-codex-spark -c model_reasoning_effort="low"' \
omx team 2:explore "코드베이스 매핑"

# 4. 혼합 워커 (분석은 claude, 실행은 codex)
OMX_TEAM_WORKER_CLI_MAP=claude,claude,codex,codex \
omx team 4:executor "작업"

# 5. non-tmux 모드 (tmux 없는 환경)
OMX_TEAM_WORKER_LAUNCH_MODE=prompt omx team 2:executor "작업"
```

**팀 라이프사이클 관리:**

```bash
# 팀 상태 확인
omx team status <team-name>

# 팀 재개 (중단 후)
omx team resume <team-name>

# 팀 종료 (진행 중인 작업이 완료된 후)
omx team shutdown <team-name>

# ralph와 연계된 팀 (종료 시 ralph도 함께 처리)
omx team ralph 3:executor "작업"
omx team shutdown <team-name> --ralph
```

⚠️ `in_progress` 상태의 작업이 있는 동안 shutdown하면 작업이 중단된다. 작업이 완료될 때까지 기다리거나, 중단을 의도한 경우에만 실행한다.

💡 `omx doctor --team`으로 팀 모드 진단을 실행할 수 있다. tmux 가용성, config.toml 설정, MCP 서버 상태를 종합적으로 확인한다.
