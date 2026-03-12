# 에이전트 프롬프트 카탈로그

OMX는 `~/.codex/prompts/` 에 설치되는 전문화된 에이전트 프롬프트 카탈로그를 제공한다. 각 프롬프트는 특정 역할, 책임 범위, 실행 깊이를 정의하며, Codex 세션에서 `/prompts:<name>` 또는 `omx ask --agent-prompt <name>` 으로 호출된다.

---

## 포스처 라우팅 시스템

OMX는 실험적 포스처(posture) 라우팅 레이어를 도입하여 에이전트를 역할(role), 추론 깊이(tier), 운영 스타일(posture)로 분류한다.

![Diagram 1](../assets/diagrams/sections__06-prompts-agents__diagram_1.svg)

### 포스처별 특징

| 포스처 | 목적 | 대상 에이전트 | Tier |
|--------|------|--------------|------|
| `frontier-orchestrator` | 계획, 라우팅, 비판적 검토 | planner, architect, critic | THOROUGH |
| `deep-worker` | 구현, 수정, 테스트 | executor, build-fixer, test-engineer | STANDARD |
| `fast-lane` | 탐색, 문서, 빠른 조회 | explore, writer, researcher | LOW |

> 💡 포스처 시스템은 실험적 기능이다. `~/.omx/agents/` 에 생성된 native agent config 파일에서 `## OMX Posture Overlay` 섹션으로 확인할 수 있다.

---

## 에이전트 역할별 분류

### frontier-orchestrator 그룹 (계획·분석·검토)

| 프롬프트 | 설명 |
|---------|------|
| `analyst` | 사전 계획 컨설턴트 — 요구사항 분석, 수용 기준 정의 (THOROUGH) |
| `planner` | 전략적 계획 컨설턴트 — 인터뷰 워크플로우 기반 실행 계획 수립 (THOROUGH) |
| `architect` | 전략적 아키텍처 & 디버깅 어드바이저 — 읽기 전용 (THOROUGH) |
| `critic` | 작업 계획 검토 전문가 — 비판적 리뷰 (THOROUGH) |
| `product-manager` | 문제 프레이밍, 가치 가설, 우선순위 지정, PRD 생성 (STANDARD) |
| `product-analyst` | 제품 지표, 이벤트 스키마, 퍼널 분석, 실험 설계 (STANDARD) |

### deep-worker 그룹 (구현·수정·검증)

| 프롬프트 | 설명 |
|---------|------|
| `executor` | 자율적 심층 실행기 — 목표 지향 구현 (STANDARD) |
| `build-fixer` | 빌드/컴파일 오류 해결 전문가 — 최소 변경, 아키텍처 변경 금지 |
| `test-engineer` | 테스트 전략, 통합/e2e 커버리지, 플레이키 테스트 강화, TDD |
| `verifier` | 완료 증거 수집, 주장 검증, 테스트 충분성 평가 |
| `debugger` | 근본 원인 분석, 회귀 격리, 스택 트레이스 분석 |
| `team-executor` | 팀 실행 전문가 — 감독하의 보수적 팀 납품 |
| `team-orchestrator` | 팀 조율 전문가 — 멀티 에이전트 조율 |
| `code-simplifier` | 기능 보존 상태에서 코드 명확성·일관성·유지보수성 개선 |

### 리뷰 그룹 (품질·보안·스타일)

| 프롬프트 | 설명 |
|---------|------|
| `code-reviewer` | 심각도 등급 피드백이 있는 전문 코드 리뷰 |
| `api-reviewer` | API 계약, 하위 호환성, 버전 관리, 오류 시맨틱스 |
| `quality-reviewer` | 논리 결함, 유지보수성, 안티패턴, SOLID 원칙 |
| `quality-strategist` | 품질 전략, 릴리스 준비성, 리스크 평가, 품질 게이트 |
| `security-reviewer` | 보안 취약점 탐지 전문가 (OWASP Top 10, 시크릿, 안전하지 않은 패턴) |
| `performance-reviewer` | 핫스팟, 알고리즘 복잡도, 메모리/레이턴시 트레이드오프, 프로파일링 계획 |
| `style-reviewer` | 포맷팅, 명명 규칙, 관용구, 린트/스타일 규칙 |

### fast-lane 그룹 (탐색·문서·연구)

| 프롬프트 | 설명 |
|---------|------|
| `explore` | 코드베이스 검색 전문가 — 파일 및 코드 패턴 탐색 |
| `writer` | 문서 작성자 — README, API 문서, 사용자 가이드, 코드 주석 |
| `researcher` | 외부 문서 & 참조 자료 연구원 |
| `dependency-expert` | 외부 SDK/API/패키지 평가 전문가 |

### 도메인 전문가 그룹

| 프롬프트 | 설명 |
|---------|------|
| `designer` | UI/UX 디자이너-개발자 — 인터페이스 설계 (STANDARD) |
| `qa-tester` | 인터랙티브 CLI 테스트 전문가 — tmux 세션 관리 활용 |
| `ux-researcher` | 사용성 연구, 휴리스틱 감사, 사용자 증거 합성 (STANDARD) |
| `information-architect` | 정보 계층, 분류, 탐색 모델, 명명 일관성 (STANDARD) |
| `git-master` | 원자적 커밋, 리베이스, 히스토리 관리 — 스타일 감지 포함 |
| `sisyphus-lite` | 경량 Sisyphus 스타일 전문 워커 — 빠른 제한 작업용 |

---

## 각 에이전트 상세

### analyst (분석가)

**역할:** 결정된 제품 범위를 구현 가능한 수용 기준으로 변환. 계획 시작 전 갭을 포착.

**언제 사용:** 요구사항이 모호하거나 검증되지 않은 가정이 있을 때. `planner` 실행 전 단계.

**특징:**
- 읽기 전용 (Write/Edit 도구 차단)
- 누락된 질문, 정의되지 않은 가드레일, 범위 리스크 식별에 집중
- 시장/사용자 가치 우선순위 지정은 담당하지 않음
- 결과를 리더에게 라우팅하여 다음 단계로 전달

---

### planner (계획가)

**역할:** 인터뷰 워크플로우 기반의 전략적 실행 계획 수립.

**언제 사용:** 복잡한 기능 개발, 멀티 파일 리팩터링, 장기 프로젝트 시작 시.

**특징:**
- THOROUGH 추론 깊이 (고비용, 고정확도)
- 인터뷰 방식으로 요구사항을 구체화
- `analyst` → `planner` → `executor` 파이프라인의 핵심
- frontier-orchestrator 포스처

---

### architect (아키텍트)

**역할:** 전략적 아키텍처 & 디버깅 어드바이저.

**언제 사용:** 시스템 설계 결정, 경계/인터페이스 정의, 장기 트레이드오프 분석 시.

**특징:**
- 읽기 전용 — 코드 구현은 하지 않음
- 시스템 경계, 인터페이스, 트레이드오프에 집중
- 아키텍처 문서 작성 및 리뷰
- frontier-orchestrator 포스처

---

### executor (실행기)

**역할:** 자율적 심층 실행기 — 목표 지향 구현.

**언제 사용:** 실제 코드 구현, 기능 개발, 리팩터링이 필요한 모든 상황.

**특징:**
- STANDARD 추론 깊이
- `deep-worker` 포스처 — 구현 우선
- 멀티 파일 편집, 기능 추가, 버그 수정 모두 처리
- 팀 모드에서 가장 많이 사용되는 에이전트

---

### verifier (검증자)

**역할:** 완료 증거 수집, 주장 검증, 테스트 충분성 평가.

**언제 사용:** 작업 완료 후 결과물 검증, 배포 전 최종 점검 시.

**특징:**
- 작업이 실제로 완료되었는지 증거 기반으로 확인
- 테스트 통과 여부, 빌드 성공 여부를 명시적으로 검증
- 불완전한 작업에 대해 계속 반복

---

### debugger (디버거)

**역할:** 근본 원인 분석, 회귀 격리, 스택 트레이스 분석.

**언제 사용:** 버그 조사, 테스트 실패 원인 파악, 회귀 격리 시.

**특징:**
- 증거 기반 진단 — 추측이 아닌 사실 확인
- 스택 트레이스, 로그 분석 전문
- 수정보다 진단에 집중 (수정은 `executor` 에 위임)

---

### build-fixer (빌드 수정기)

**역할:** 빌드/컴파일 오류 해결 전문가.

**언제 사용:** TypeScript 타입 에러, 빌드 실패, 컴파일 오류 발생 시.

**특징:**
- 최소 변경 원칙 — 아키텍처 변경은 하지 않음
- 빌드 오류 범위에만 집중
- `deep-worker` 포스처

---

### critic (비평가)

**역할:** 작업 계획 검토 전문가.

**언제 사용:** 계획이 완성된 후 비판적 리뷰가 필요할 때. `planner` 이후 단계.

**특징:**
- THOROUGH 추론 깊이
- 계획의 약점, 리스크, 누락된 항목을 식별
- frontier-orchestrator 포스처
- 건설적인 비판으로 계획 품질 향상

---

## omx ask 커맨드로 에이전트 직접 사용

`omx ask` 커맨드를 사용하면 Claude 또는 Gemini 제공자에게 에이전트 프롬프트를 적용하여 작업을 위임할 수 있다.

### 기본 사용법

```bash
# Claude에게 executor 프롬프트로 구현 요청
omx ask claude --agent-prompt executor "implement feature X with tests"

# Gemini에게 planner 프롬프트로 계획 수립 요청
omx ask gemini --agent-prompt planner "draft a rollout plan for auth refactor"

# Claude에게 architect 프롬프트로 설계 리뷰 요청
omx ask claude --agent-prompt architect "review the boundary decisions for this module"

# Gemini에게 단순 질문 (에이전트 프롬프트 없이)
omx ask gemini "brainstorm alternatives for this API design"
```

### 플래그 상세

```bash
# Claude 플래그
omx ask claude -p|--print "<prompt>"
omx ask claude --agent-prompt <agent-name> "<task>"

# Gemini 플래그
omx ask gemini -p|--prompt "<prompt>"
omx ask gemini --agent-prompt=<agent-name> --prompt "<task>"
```

### 결과물 저장 위치

`omx ask` 실행 결과물은 `.omx/artifacts/` 에 저장된다.

```bash
# 실행 후 결과 확인
ls .omx/artifacts/
```

### Codex 세션 내에서 프롬프트 직접 호출

Codex 세션 내부에서는 `/prompts:` 접두어로 에이전트 프롬프트를 직접 호출할 수 있다.

```text
/prompts:architect "review the boundary decisions"
/prompts:executor "take the next scoped task"
/prompts:verifier "confirm all acceptance criteria are met"
/prompts:planner "create execution plan for OAuth implementation"
```

### 스킬 vs 프롬프트 차이

| 구분 | 스킬 (`$skill`) | 프롬프트 (`/prompts:`) |
|------|----------------|----------------------|
| 목적 | 재사용 가능한 워크플로우 | 에이전트 역할/페르소나 |
| 예시 | `$team`, `$plan`, `$ralph` | `executor`, `planner`, `architect` |
| 설치 위치 | `~/.agents/skills/` | `~/.codex/prompts/` |
| 호출 방식 | `$skill-name` | `/prompts:agent-name` |

> 💡 팁: 복잡한 작업은 먼저 `$plan` 스킬로 계획을 세운 다음, `/prompts:executor` 로 구현을 위임하는 패턴이 효과적이다.

---

## 프롬프트 설치 및 업데이트

```bash
# 사용자 범위로 설치 (기본값)
omx setup

# 설치된 프롬프트 확인
ls ~/.codex/prompts/

# 특정 프롬프트 내용 확인
cat ~/.codex/prompts/executor.md | head -30
```

설치 후 프롬프트 파일은 `~/.codex/prompts/*.md` 에 위치하며, Codex 가 세션 시작 시 자동으로 로드한다.
