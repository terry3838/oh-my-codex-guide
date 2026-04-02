# oh-my-codex 개요

## 원본 저장소 역할

- repo: `oh-my-codex`
- source: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- latest synced commit: `fb0f8ebb95dc`
- current workspace version basis: `0.11.11`
- one-line summary: **OpenAI Codex CLI 위에 역할 프롬프트, 스킬, 팀 orchestration, 영속 상태, 운영자 제어를 얹는 운영 런타임 하네스**

## 이번 판단

기존 가이드는 Spark Initiative(`0.9.x`)를 너무 앞세웠다.

그 결과 학습자가 먼저 알아야 할 현재형 현실,
즉 **0.11.x의 기본 온보딩, setup refresh 안전성, live supervision, state visibility, packed-install 계약**이 뒤로 밀렸다.

그래서 이번 개정은 관점을 바꿨다.

- 예전 축: `v0.9.0 / 0.9.1` 중심 설명
- 현재 축: `0.11.11` 기준의 운영 런타임 설명
- Spark Initiative: 여전히 중요하지만, 이제는 **역사적 전환점 + 현재 배경층**으로 배치

## 최근 upstream 커밋

- `fb0f8eb chore: sync Cargo.lock for 0.11.11 release`
- `4aaf96c Merge pull request #1088 from Yeachan-Heo/dev`
- `e762d86 chore: sync Cargo.toml version to 0.11.11`
- `ef91d8c chore: bump version to 0.11.11`
- `174cb5e docs: add contributors section with maintainer HaD0Yun`
- `19e1540 docs: add contributors section with maintainer HaD0Yun`

## 원본에서 확인한 핵심 현실

### 1. 현재 README는 Codex-first 온보딩을 민다

최근 README의 기본 경로는 이렇다.

1. `omx setup`
2. `omx --madmax --high`
3. Codex 안에서 `$architect`, `$plan`

즉 Team Mode는 중요하지만, **모든 사용자의 첫 진입점은 아니다.**

### 2. 0.11.x의 중심은 운영 안정성 계약이다

`CHANGELOG.md`, `release-notes-0.11.8.md`, `0.11.9.md`, `0.11.10.md`를 같이 보면 반복되는 축이 선명하다.

- deep-interview / ralplan coordination
- setup refresh safety
- HUD state visibility
- live worker supervision
- watcher / nudge correctness
- release metadata sync

따라서 지금의 OMX는 “새 명령이 무엇이냐”보다 **runtime correctness를 어떻게 유지하느냐**로 읽는 편이 맞다.

### 3. Spark Initiative는 여전히 중요하지만 현재 서사의 중심은 아니다

`omx explore`, `omx sparkshell`, native helper distribution은 여전히 핵심 개념이다.

하지만 지금 학습자는 이걸 이렇게 봐야 한다.

- Spark는 OMX를 hybrid runtime으로 바꾼 전환점
- 현재 0.11.x는 그 위에서 orchestration / setup / supervision 계약을 계속 다듬는 단계

### 4. repo 구조도 이미 “단순 CLI 래퍼” 수준을 넘어간다

확인한 핵심 구조는 다음과 같다.

- `src/` — TypeScript 런타임 본체
- `crates/` — native helper 계층
- `prompts/` — 역할 프롬프트 카탈로그
- `skills/` — 워크플로우 카탈로그
- `docs/` — 릴리스 노트, 통합, 운영 계약
- `templates/` — setup이 쓰는 관리 골격
- `missions/` — 고급 운영/실험 표면

즉 OMX는 “몇 개 명령을 더 붙인 래퍼”가 아니라 **운영 시스템 저장소**로 읽는 게 맞다.

## 이번 가이드에서 고친 것

### 1. 버전 기준을 `0.11.11`로 다시 세움

기존처럼 `0.9.0 / 0.9.1`을 중심 축으로 두지 않고, 현재 워크스페이스 버전과 최근 릴리스 노트를 기준선으로 다시 잡았다.

### 2. 기본 진입 경로를 Codex-first로 재배치

이제 README와 학습 경로는 Team Mode보다 먼저:

- setup
- doctor
- 기본 세션 시작
- `$architect`, `$plan`

을 잡게 만든다.

### 3. Spark Initiative를 역사적 전환점으로 재정렬

Spark를 빼지 않았다. 대신 현재 onboarding을 가리지 않게, **현재형 서사 아래의 배경층**으로 내려 배치했다.

### 4. public-safe 기준을 맞춤

개인 절대경로 같은 로컬 머신 정보는 문서 기준선에서 제거했다. public guide는 저장소 구조와 운영 개념에 집중해야지, 작성자의 로컬 환경을 드러내면 안 된다.

## 학습자가 지금 먼저 기억해야 할 것

1. OMX는 Codex를 대체하지 않는다.
2. 지금의 기본 시작점은 Team Mode가 아니라 Codex-first 세션이다.
3. `ralplan`, `team`, `ralph`는 서로 다른 층이다.
4. 0.11.x는 기능 과시보다 운영 안정성 계약이 더 중요하다.
5. Spark Initiative는 여전히 중요하지만, 현재형 설명의 출발점은 아니다.
