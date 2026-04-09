# oh-my-codex (OMX) 학습 가이드

> *Your codex is not alone.*

OMX는 OpenAI Codex CLI 위에 얹는 **운영 런타임 하네스**다. 핵심은 Codex를 대체하는 것이 아니라, 그 주변에 **더 강한 시작점, 재사용 가능한 역할/워크플로우, 영속 상태, 팀 조율, 운영자 제어**를 추가하는 데 있다.

이 가이드는 원본 README를 한국어로 옮기는 데서 멈추지 않는다. 2026년 4월 초 기준 upstream 구조와 최근 릴리스 흐름을 다시 읽어, **지금의 OMX를 어떻게 이해하고 어떤 순서로 배우는 게 맞는지**를 학습용으로 재구성한다.

## 버전 기준

- 현재 upstream 워크스페이스 버전: `0.11.11`
- 근거 파일: `Cargo.toml`, `package.json`
- 최근 릴리스 문맥에서 특히 중요한 구간: `0.11.8` → `0.11.9` → `0.11.10`
- 역사적으로 계속 중요한 전환점: `0.9.x`의 **Spark Initiative**

즉 이 가이드는 **Spark Initiative를 “시작점”이 아니라 현재 0.11.x 런타임을 이해하기 위한 배경층**으로 다룬다.

---

## OMX를 한 문장으로 말하면

**Codex를 실행 엔진으로 유지한 채, 역할 프롬프트·스킬·팀 런타임·상태 추적·운영자 제어를 더하는 멀티에이전트 워크플로우 레이어**다.

원본 README의 현재 메시지도 여기에 가깝다.

- 먼저 강한 기본값으로 Codex 세션을 시작하고
- `$architect`, `$plan`, `$team`, `$ralph` 같은 표면으로 작업을 구조화하며
- 필요할 때만 더 무거운 orchestration으로 올라간다

이 관점을 잡지 못하면 OMX를 “명령어 많은 래퍼”로 오해하기 쉽다.

---

## 왜 지금의 OMX를 다시 읽어야 하나

### 1. 기본 온보딩 경로가 더 분명해졌다

최근 upstream README는 초보자에게 `omx team`부터 강요하지 않는다. 오히려:

1. `omx setup`
2. `omx --madmax --high`
3. Codex 안에서 `$architect`, `$plan`

이라는 **Codex-first 기본 경로**를 더 강하게 민다.

즉 Team Mode는 여전히 중요하지만, **모든 사용자의 첫 진입점은 아니다.**

### 2. 0.11.x는 “새 기능 폭발”보다 운영 계약을 다듬는 구간이다

최근 changelog와 release notes를 보면 핵심은 다음 축에 모인다.

- deep-interview와 ralplan handoff 정교화
- HUD / live state visibility 복구
- setup 재실행 안정화
- team supervision / watcher / nudge correctness 강화
- packed install / release metadata / version-sync 정렬

즉 0.11.x의 OMX는 “무엇을 추가했는가”보다 **기존 orchestration을 얼마나 덜 깨지게 운영하느냐**가 중요하다.

### 3. Spark Initiative는 아직 중요하지만, 현재는 배경 지식으로 읽어야 한다

`omx explore`, `omx sparkshell`, `native-release-manifest.json`은 여전히 중요하다. 하지만 지금 OMX의 중심 서사는 더 이상 “Spark가 새로 들어왔다”가 아니다.

지금 학습자가 먼저 잡아야 하는 건:

- 기본 세션 시작법
- 역할/스킬 선택 기준
- Team / Ralph / Ralplan의 관계
- stateful runtime과 HUD
- setup / upgrade / packed install 계약

이다.

---

## OMX의 핵심 구조

### 1. 실행 엔진과 런타임 레이어를 분리해서 봐야 한다

- **Codex**: 실제 에이전트 실행 엔진
- **OMX**: 역할 라우팅, 워크플로우, 상태, 팀 orchestration을 담당하는 런타임

그래서 OMX를 이해할 때는 CLI 옵션 목록보다 아래 계층을 먼저 봐야 한다.

- `AGENTS.md` — 프로젝트 운영 계약
- `prompts/` — 역할 프롬프트 카탈로그
- `skills/` — 재사용 가능한 워크플로우
- `.omx/` — 상태, 메모리, 계획, 로그
- `docs/` — 운영 계약과 통합 문서
- `crates/` + `src/` — TypeScript + native helper 혼합 런타임

### 2. Team / Ralph / Ralplan은 서로 다른 층이다

이 셋을 하나로 섞어 이해하면 금방 헷갈린다.

- **Ralplan**: 계획과 수용 기준을 만드는 층
- **Team Mode**: 병렬 워커를 조율하는 층
- **Ralph**: 검증이 끝날 때까지 루프를 유지하는 층

즉 권장 조합은 여전히:

`ralplan → team → ralph`

이다. 다만 작은 작업이라면 이 전부가 필요하지 않다.

### 3. OMX는 단순한 설정 스크립트가 아니라 상태를 가진 시스템이다

최근 upstream에서 반복적으로 강화되는 것도 이 부분이다.

- HUD가 stateful mode를 제대로 보여 주는가
- deep-interview 상태가 잘 잠기고 해제되는가
- fallback watcher와 team supervision이 live session을 올바르게 다루는가
- setup을 다시 돌렸을 때 기존 환경을 재파괴하지 않는가

즉 OMX는 “설치 후 명령 몇 개 외우는 툴”이 아니라, **지속 실행과 회복을 전제로 한 운영 시스템**으로 읽는 편이 맞다.

---

## 빠른 시작: 지금 가장 현실적인 첫 성공 루프

### 1단계 — 요구사항 확인

```bash
node --version
npm install -g @openai/codex oh-my-codex
```

- Node.js 20+
- Codex CLI 인증 완료
- Team Mode를 쓸 생각이면 tmux 호환 환경 준비

### 2단계 — 설치와 진단

```bash
omx setup
omx doctor
omx doctor --team
```

여기서 중요한 건 설치 자체보다 **재실행 가능한 setup/doctor 계약**을 이해하는 것이다. 최근 0.11.x는 이 구간을 계속 다듬고 있다.

### 3단계 — 기본 세션 시작

```bash
omx --madmax --high
```

그리고 Codex 안에서:

```text
$architect "analyze the authentication flow"
$plan "map the safest implementation path"
```

이것이 현재 upstream README가 밀고 있는 **기본 UX**다.

### 4단계 — 필요할 때만 더 무거운 레이어로 올라가기

```text
$team 3:executor "implement the approved plan"
$ralph "stay in the loop until verification is real"
```

작업이 작다면 여기까지 갈 필요가 없다. **OMX의 장점은 모든 기능을 항상 켜는 게 아니라, 필요한 순간에 orchestration 강도를 높일 수 있다는 점**이다.

---

## 현재 릴리스 라인에서 기억할 것

### 0.11.8
- deep-interview 상태가 있을 때 각종 nudge를 억제하는 hotfix
- notify-hook / watcher 계열의 과잉 개입을 줄이는 방향

### 0.11.9
- deep-interview ↔ ralplan coordination 강화
- live ralplan state visibility 복구
- setup이 Codex-managed TUI 설정을 덜 망가뜨리도록 보정
- HUD / watcher / live team supervision 복원

### 0.11.10
- approved handoff alias parsing 회귀 방지
- release metadata / release collateral 동기화
- 릴리스 검증 레인 정리

### 0.11.11
- 현재 워크스페이스 버전 정렬
- 따라서 학습자는 `0.11.8~0.11.10`의 운영 수정사항을 이해한 위에서, **0.11.11을 현재 기준선**으로 보면 된다.

---

## Spark Initiative는 지금 어떻게 읽어야 하나

Spark Initiative를 빼고 OMX를 설명할 수는 없다. 하지만 지금은 **현재 런타임의 역사적 전환점**으로 읽는 편이 맞다.

Spark Initiative가 남긴 핵심 자산:

- `omx explore`
- `omx sparkshell`
- 네이티브 helper distribution
- `native-release-manifest.json` 기반 hydration 계약
- `build:full`, `test:explore`, `test:sparkshell` 같은 릴리스 검증 표면

이 전환 덕분에 OMX는 TypeScript orchestration만 있는 CLI가 아니라,
**native helper를 함께 배포·검증하는 하이브리드 런타임**이 되었다.

다만 현재 학습 순서에서는 이걸 초반 주인공으로 두기보다:

1. 기본 Codex-first 루프
2. 계획/팀/검증 레이어
3. setup / state / HUD / packed install
4. 그 뒤에 Spark 계열의 fast-path 해석

순으로 읽는 편이 더 이해가 빠르다.

---

## 원본 저장소를 읽을 때 우선 볼 곳

### 먼저 볼 파일

- `README.md` — 현재 onboarding 메시지
- `CHANGELOG.md` — 릴리스별 운영 수정 흐름
- `Cargo.toml`, `package.json` — 현재 버전 기준
- `docs/release-notes-0.11.8.md`
- `docs/release-notes-0.11.9.md`
- `docs/release-notes-0.11.10.md`
- `docs/openclaw-integration.md`

### 구조 이해용 디렉터리

- `src/` — TypeScript 런타임 본체
- `crates/` — native helper 계층
- `prompts/` — 역할 프롬프트
- `skills/` — 워크플로우 카탈로그
- `docs/` — 운영 계약과 통합 문서
- `templates/` — setup이 쓰는 골격
- `missions/` — 고급 운영/실험 surface

---

## 추천 읽기 순서

1. `sections/01-overview.md` — 지금 OMX가 무엇인지
2. `01-learning-paths.md` — 어떤 경로로 배울지
3. `sections/02-harness-engineering.md` — 런타임 관점
4. `sections/03-team-mode.md` — Team Mode의 실제 역할
5. `sections/07-mcp-state.md` — 상태와 메모리
6. `sections/08-setup-config.md` — setup / config / upgrade
7. `sections/11-spark-initiative.md` — Spark를 현재 맥락에서 다시 읽기
8. `02-glossary.md` — 용어 정리

---

## 이 가이드가 특히 필요한 사람

### 1. Codex는 익숙한데 더 강한 작업 루프가 필요한 사용자

이 경우 핵심은 “OMX 명령을 다 외우기”가 아니라:

- 어떤 작업에서 `$plan`을 먼저 써야 하는지
- 언제 `$team`으로 올려야 하는지
- 언제 `$ralph`가 필요한지

를 배우는 것이다.

### 2. 운영자 시각으로 runtime reliability를 보고 싶은 사용자

최근 0.11.x의 진짜 가치가 여기 있다.

- live state visibility
- nudge suppression correctness
- setup refresh safety
- packed install contract

### 3. OpenClaw 통합이나 팀 운영까지 포함해 보려는 사용자

이 경우 OMX를 단순 프롬프트 팩으로 보면 부족하다. notification / hooks / state / supervision까지 함께 봐야 한다.

---

## 흔한 오해

### 오해 1 — OMX는 Team Mode가 전부다

아니다. Team Mode는 중요한 표면이지만, 현재 기본 진입점은 더 가볍고 Codex-first다.

### 오해 2 — Spark Initiative가 아직도 현재 버전의 중심 서사다

아니다. Spark는 여전히 중요하지만, 지금의 핵심은 **0.11.x 운영 안정성 계약**이다.

### 오해 3 — setup은 한 번 돌리고 끝나는 초기화 단계다

아니다. 최근 release 흐름을 보면 setup은 **재실행 안정성**까지 포함한 운영 표면이다.

---

## 다음 문서

- 학습 순서를 바로 잡으려면 `01-learning-paths.md`
- 현재 버전 기준의 핵심 판단을 먼저 보려면 `sections/01-overview.md`
- Spark를 역사적 전환점이자 현재 배경층으로 읽으려면 `sections/11-spark-initiative.md`

<!-- GUIDE_SYNC:START -->
## 자동 동기화 상태

- origin repo: `oh-my-codex`
- latest source commit: `c364f617eccf`
- sync mode: `update`
- 영향 분류: CLI/명령어, 문서 구조, 소스코드

### 이번 반영 포인트

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: CLI/명령어, 문서 구조, 소스코드.

### 최근 upstream 커밋

- `c364f61 Cut the 0.12.3 patch release on main`
- `010068e Cut the 0.12.3 patch release`
- `9513cc3 Merge pull request #1364 from Yeachan-Heo/hotfix/team-keyword-detector`
- `cacc7f7 Merge remote-tracking branch 'origin/dev' into hotfix/team-keyword-detector`
- `0003a4f Cut the 0.12.2 patch release on main`
- `13b680c Cut the 0.12.2 patch release`

### 변경 파일 샘플

- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `RELEASE_BODY.md`
- `docs/qa/release-readiness-0.12.2.md`
- `docs/qa/release-readiness-0.12.3.md`
- `docs/release-notes-0.12.2.md`
- `docs/release-notes-0.12.3.md`
- `package-lock.json`
- `package.json`
- `src/cli/__tests__/index.test.ts`
- `src/cli/index.ts`
- `src/hooks/__tests__/agents-overlay.test.ts`
- `src/hooks/__tests__/keyword-detector.test.ts`
- `src/hooks/agents-overlay.ts`
- `src/hooks/keyword-detector.ts`
- `src/hud/__tests__/render.test.ts`
- `src/hud/__tests__/state.test.ts`
- `src/hud/render.ts`
- `src/hud/state.ts`

> 이 블록은 guide sync가 자동 갱신합니다.
<!-- GUIDE_SYNC:END -->
