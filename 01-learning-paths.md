# OMX 학습 경로

OMX는 지금 **Codex-first 기본 루프**로 배우는 게 가장 빨라요.
핵심 순서는 이거예요.

`$deep-interview → $ralplan → $team / $ralph`

## 트랙 1. 처음 쓰는 사용자

목표:
- 권장 기본 환경이 macOS/Linux라는 점을 알아요
- 첫 세션을 Canonical flow로 끝내 봐요

읽는 순서:
1. `README.md`
2. `sections/01-overview.md`
3. `sections/08-setup-config.md`
4. `02-glossary.md`

실습:
```bash
npm install -g @openai/codex oh-my-codex
omx setup
omx --madmax --high
```

세션 안에서:
```text
$deep-interview "clarify the task"
$ralplan "approve the implementation path"
```

성공 기준:
- 왜 `$deep-interview`가 첫 단계인지 설명할 수 있어요
- 왜 macOS/Linux가 권장 경로인지 설명할 수 있어요

## 트랙 2. 구현 중심 사용자

목표:
- 언제 `$team`을 쓰고 언제 `$ralph`를 쓰는지 구분해요

읽는 순서:
1. `sections/03-team-mode.md`
2. `sections/04-ralph-ultrawork.md`
3. `sections/05-skills-catalog.md`

권장 루프:
```text
$deep-interview → $ralplan → $team 또는 $ralph
```

성공 기준:
- 병렬 실행은 `$team`, persistent completion loop는 `$ralph`라고 설명할 수 있어요

## 트랙 3. 운영자

목표:
- setup, doctor, hud, state 표면을 같이 읽어요

읽는 순서:
1. `sections/08-setup-config.md`
2. `sections/07-mcp-state.md`
3. `sections/03-team-mode.md`
4. upstream `CHANGELOG.md`

확인 포인트:
- native hook ownership
- HUD recovery
- state CLI parity
- team runtime hygiene

## 자주 틀리는 순서

- `omx team`부터 전부 배우기
- Windows 경로를 기본값으로 가정하기
- `$deep-interview` 없이 바로 `$team`부터 쓰기
