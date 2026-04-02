# OMX 학습 경로

이 문서는 oh-my-codex(OMX)를 처음 접하는 사람부터 운영자 관점으로 runtime을 다루려는 사람까지를 위한 **현행 학습 순서**를 제시한다.

핵심 원칙은 하나다.

> **OMX를 Team Mode 명령 모음으로 배우지 말고, Codex-first 기본 루프 → 계획/팀/검증 → 상태/운영 계약 순서로 배워라.**

---

## 학습 경로 개요

![Diagram 1](assets/diagrams/01-learning-paths__diagram_1.svg)

---

## 트랙 1 — 처음 쓰는 사용자

### 목표

- OMX가 Codex를 대체하지 않는다는 점 이해
- `setup → doctor → 첫 세션` 루프 완료
- `$architect`, `$plan`을 기본 진입점으로 익힘
- Team Mode를 “나중에 올리는 레이어”로 이해

### 먼저 읽을 문서

1. `README.md`
2. `sections/01-overview.md`
3. `sections/08-setup-config.md`
4. `02-glossary.md`의 `oh-my-codex`, `omx setup`, `omx doctor`, `ralplan`, `ralph`, `Team Mode`

### 실습 순서

```bash
npm install -g @openai/codex oh-my-codex
omx setup
omx doctor
omx doctor --team
omx --madmax --high
```

Codex 안에서:

```text
$architect "analyze the authentication flow"
$plan "map the safest implementation path"
```

### 성공 기준

- `omx setup` / `omx doctor`를 한 번 직접 실행했다
- `$architect`와 `$plan`의 역할 차이를 설명할 수 있다
- 왜 `omx team`이 기본 첫 단계가 아닌지 이해한다

---

## 트랙 2 — 구현 중심 사용자

### 목표

- Ralplan / Team / Ralph의 역할 분리를 이해
- 큰 작업에서 언제 병렬화해야 하는지 판단
- 검증이 있는 실행 루프를 설계할 수 있음

### 먼저 읽을 문서

1. `sections/03-team-mode.md`
2. `sections/04-ralph-ultrawork.md`
3. `sections/05-skills-catalog.md`
4. `sections/06-prompts-agents.md`
5. `02-glossary.md`의 `ralplan`, `ralph`, `Team Mode`, `executor`, `planner`, `architect`

### 추천 루프

```text
$plan "break the work into safe lanes"
$team 3:executor "implement the approved plan"
$ralph "stay in the loop until verification is real"
```

### 이 트랙에서 반드시 구분할 것

- `ralplan`은 **계획 산출물**을 만든다
- `team`은 **병렬 워커를 조율**한다
- `ralph`는 **증거가 신선해질 때까지 루프를 유지**한다

### 성공 기준

- 작은 작업에서는 Team Mode를 생략할 수 있다
- 큰 작업에서는 `ralplan → team → ralph` 흐름이 왜 안전한지 설명할 수 있다
- 병렬성 자체와 orchestration을 구분할 수 있다

---

## 트랙 3 — 운영자 / 유지보수자

### 목표

- 0.11.x에서 중요한 운영 수정 포인트를 이해
- setup refresh, live supervision, watcher, HUD, state visibility를 함께 읽음
- OMX를 “상태를 가진 시스템”으로 다룰 수 있음

### 먼저 읽을 문서

1. `sections/07-mcp-state.md`
2. `sections/08-setup-config.md`
3. `sections/09-notifications.md`
4. `sections/11-spark-initiative.md`
5. `README.md`의 0.11.x 릴리스 요약

### 이 트랙에서 중점적으로 볼 것

- deep-interview / ralplan handoff
- HUD state visibility
- live team supervision
- notify-hook / watcher correctness
- packed install / release metadata / version sync

### 권장 확인 루프

```bash
omx setup --force --scope project
omx doctor --team
omx hud --watch
```

그리고 최근 release notes를 읽는다.

- `0.11.8` — deep-interview 중 nudge suppression
- `0.11.9` — setup / HUD / supervision 보정
- `0.11.10` — approved handoff parsing / release sync

### 성공 기준

- 최근 0.11.x의 핵심이 “새 기능 추가”보다 “운영 계약 보강”이라는 점을 설명할 수 있다
- setup 재실행이 왜 운영 표면인지 이해한다
- HUD / watcher / team state를 하나의 시스템으로 읽을 수 있다

---

## 트랙 4 — Spark / native helper 구조를 이해하려는 사용자

### 목표

- Spark Initiative를 현재형 맥락에서 해석
- `omx explore`, `omx sparkshell`, hydration 계약을 이해
- OMX가 TypeScript 단일 CLI가 아니라 hybrid runtime이라는 점을 잡음

### 먼저 읽을 문서

1. `sections/11-spark-initiative.md`
2. `sections/02-harness-engineering.md`
3. `02-glossary.md`의 `Spark Initiative`, `native-release-manifest.json`, `omx explore`, `omx sparkshell`

### 실습 순서

```bash
omx explore --prompt "find where team state is written"
omx sparkshell git --version
omx sparkshell --tmux-pane %12 --tail-lines 400
```

### 여기서 배워야 하는 것

- `omx explore`는 read-only fast path다
- `omx sparkshell`은 operator-facing shell-native inspection surface다
- OMX는 native helper distribution까지 포함한 release contract를 가진다

### 성공 기준

- Spark Initiative를 “지금의 기본 onboarding”이 아니라 “현재 런타임을 만든 중요한 전환점”으로 설명할 수 있다
- `explore`와 `sparkshell`의 역할 차이를 말할 수 있다

---

## 권장 학습 순서 요약

### 빠른 입문 순서

1. `README.md`
2. `sections/01-overview.md`
3. `sections/08-setup-config.md`
4. `sections/03-team-mode.md`
5. `sections/04-ralph-ultrawork.md`
6. `sections/07-mcp-state.md`
7. `sections/11-spark-initiative.md`
8. `02-glossary.md`

### 구현자 순서

1. `README.md`
2. `sections/03-team-mode.md`
3. `sections/04-ralph-ultrawork.md`
4. `sections/05-skills-catalog.md`
5. `sections/06-prompts-agents.md`
6. `02-glossary.md`

### 운영자 순서

1. `sections/01-overview.md`
2. `sections/07-mcp-state.md`
3. `sections/08-setup-config.md`
4. `sections/09-notifications.md`
5. `sections/11-spark-initiative.md`
6. `CHANGELOG / recent release notes` 원문

---

## 자주 틀리는 학습 순서

### 잘못된 순서 1

`Team Mode부터 전부 배우기`

왜 틀리나:
- OMX의 현재 기본 경로는 Codex-first다
- Team은 필요할 때 올리는 orchestration 레이어다

### 잘못된 순서 2

`Spark Initiative부터 먼저 파고들기`

왜 틀리나:
- 역사적으로 중요하지만 지금 onboarding의 첫 축은 아니다
- 현재 0.11.x는 운영 안정성 문맥이 더 중요하다

### 잘못된 순서 3

`setup을 설치 한 번으로 끝난다고 보기`

왜 틀리나:
- 최근 릴리스는 setup refresh safety를 계속 다듬고 있다
- 운영자에게 setup은 재실행 가능한 관리 표면이다

---

## 최종 체크리스트

### 입문자 체크리스트

- [ ] `omx setup`을 직접 실행했다
- [ ] `omx doctor`와 `omx doctor --team`을 확인했다
- [ ] `$architect`와 `$plan`을 한 번씩 썼다

### 구현자 체크리스트

- [ ] `ralplan → team → ralph` 역할 차이를 이해한다
- [ ] 작은 작업과 큰 작업의 시작 경로를 구분한다
- [ ] 병렬화가 항상 정답이 아니라는 점을 안다

### 운영자 체크리스트

- [ ] 0.11.8 ~ 0.11.10의 핵심 수정 포인트를 설명할 수 있다
- [ ] HUD / watcher / setup refresh가 왜 중요한지 안다
- [ ] packed install과 release sync를 별도 운영 계약으로 본다

---

## 다음 문서

- 현재 OMX가 정확히 무엇인지 보려면 `sections/01-overview.md`
- 용어를 한 번에 정리하려면 `02-glossary.md`
- Spark를 현재 버전 맥락으로 다시 읽으려면 `sections/11-spark-initiative.md`
