# oh-my-codex (OMX) 학습 가이드

> Codex를 바꾸지 않고, 그 위에 더 강한 시작 루프와 지속 실행 레이어를 올리는 workflow runtime

지금의 OMX는 “기능이 많은 Codex 래퍼”로 읽으면 아쉬워요. upstream README와 최근 CHANGELOG를 보면, 현재 중심은 더 분명해요.

- **권장 기본 경로는 macOS 또는 Linux + Codex CLI**
- **기본 학습 루트는 `$deep-interview → $ralplan → $team / $ralph`**
- **Windows는 가능하지만 기본 경로가 아니고, 덜 안정적이에요**

이 가이드는 그 현재 기준선을 기준으로 README, learning path, overview를 다시 정리해요.

## 버전 기준

- upstream 기준 커밋: `982a7546`
- current version: `0.12.4`
- 근거: `README.md`, `CHANGELOG.md`, `Cargo.toml`, `package.json`

## OMX를 한 문장으로 보면

**Codex를 실행 엔진으로 유지한 채, 요구 정리, 계획 승인, 병렬 실행, 지속 completion loop, state 저장을 더해 주는 workflow layer**예요.

핵심은 “Codex를 대체한다”가 아니고, **Codex를 더 안정적인 작업 루프로 감싼다**예요.

## 지금 가장 중요한 메시지

### 1. 권장 기본 경로가 분명해졌어요

upstream README는 아주 직접적으로 말해요.

- 권장 기본 경로: **macOS 또는 Linux with Codex CLI**
- native Windows와 Codex App은 기본 경로가 아니에요
- Windows는 부경로예요. 필요하면 WSL2가 더 나아요.

즉 현재 frontdoor는 “어느 환경이 제일 덜 깨지는가”를 먼저 말해 줘야 해요.

### 2. 기본 루트는 `$deep-interview → $ralplan → $team / $ralph`예요

지금 OMX를 시작할 때 가장 먼저 잡아야 하는 mental model은 이거예요.

1. `$deep-interview`로 문제와 경계를 정리해요
2. `$ralplan`으로 구현 계획과 tradeoff를 승인해요
3. 큰 작업이면 `$team`, 한 명이 끝까지 밀어야 하면 `$ralph`로 가요

이 순서가 지금 upstream의 canonical flow예요.

### 3. `omx team`은 중요하지만 기본 첫 단계는 아니에요

`omx team`은 durable tmux/worktree coordination이 필요할 때 올리는 운영 표면이에요.
처음부터 모든 사용자가 team runtime으로 들어갈 필요는 없어요.

## 추천 읽기 순서

1. `sections/01-overview.md`
2. `sections/08-setup-config.md`
3. `sections/03-team-mode.md`
4. `sections/04-ralph-ultrawork.md`
5. `02-glossary.md`

## 가장 현실적인 첫 성공 루프

```bash
npm install -g @openai/codex oh-my-codex
omx setup
omx --madmax --high
```

Codex 안에서:

```text
$deep-interview "clarify the authentication change"
$ralplan "approve the safest implementation path"
$team 3:executor "execute the approved plan in parallel"
$ralph "carry the approved plan to completion"
```

여기서 중요한 건 명령 개수를 외우는 게 아니에요.
**문제 정리 → 계획 승인 → 실행 방식 선택** 순서를 몸에 익히는 게 핵심이에요.

## 자주 생기는 오해

- **오해 1: OMX는 Team Mode부터 배워야 한다**
  - 아니에요. 기본 진입은 Codex-first 루트예요.

- **오해 2: Windows도 기본 경로다**
  - 아니에요. 현재 README는 macOS/Linux를 권장 기본값으로 더 강하게 밀어요.

- **오해 3: `$team`과 `$ralph`를 항상 같이 써야 한다**
  - 아니에요. 병렬 실행이 필요하면 `$team`, 한 명의 persistent loop가 맞으면 `$ralph`예요.

## 누가 이 가이드를 먼저 읽으면 좋은가

- Codex는 이미 쓰지만 더 안정적인 작업 루프가 필요한 사람
- 계획 없이 바로 구현에 들어가다 자주 되감기는 사람
- Team runtime, HUD, state, setup/doctor 표면을 운영자 관점으로 이해하려는 사람

## 다음 행동

- 처음 쓰면 `01-learning-paths.md`를 보고 그대로 첫 세션을 한 번 따라 하세요.
- 운영자면 `sections/08-setup-config.md`, `sections/07-mcp-state.md`를 먼저 보세요.

<!-- GUIDE_SYNC:START -->
## 자동 동기화 상태

- origin repo: `oh-my-codex`
- latest source commit: `5892224fd684`
- sync mode: `update`
- 영향 분류: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증

### 이번 반영 포인트

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증.

### 최근 upstream 커밋

- `5892224f Release 0.12.6`

### 변경 파일 샘플

- `.github/scripts/dev-merge-issue-close.cjs`
- `.github/workflows/dev-merge-issue-close.yml`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `docs/codex-native-hooks.md`
- `docs/qa/recent-bug-regression-hardening-2026-04-11.md`
- `docs/qa/release-readiness-0.12.6.md`
- `docs/readme/README.de.md`
- `docs/readme/README.es.md`
- `docs/readme/README.fr.md`
- `docs/readme/README.it.md`
- `docs/readme/README.ko.md`
- `docs/readme/README.pt.md`
- `docs/readme/README.ru.md`
- `docs/readme/README.tr.md`
- `docs/reference/project-wiki.md`
- `docs/release-notes-0.12.6.md`

> 이 블록은 guide sync가 자동 갱신합니다.
<!-- GUIDE_SYNC:END -->
