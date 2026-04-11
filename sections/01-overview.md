# oh-my-codex 개요

## repo 역할

- repo: `oh-my-codex`
- source: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- version basis: `0.12.4`
- latest synced commit: `982a7546b7b4`

## 이번에 왜 frontdoor를 고쳤나

기존 가이드는 OMX의 구조를 꽤 잘 잡았지만,
현재 upstream README가 더 강하게 말하는 세 가지를 frontdoor에서 충분히 밀지 못했어요.

1. 권장 기본 경로는 **macOS/Linux + Codex CLI**예요.
2. 기본 학습 루트는 **`$deep-interview → $ralplan → $team / $ralph`**예요.
3. `omx team`은 중요하지만 기본 첫 단계가 아니라 운영자 표면이에요.

## 지금 기준의 핵심 메시지

- OMX는 Codex를 대체하지 않아요
- 먼저 문제를 정리하고, 그 다음 계획을 승인하고, 그 다음 실행 방식을 고르는 구조예요
- 최근 0.12.x는 state/HUD/native-hook/team-runtime 안정화가 큰 축이에요

## 학습자가 먼저 봐야 할 것

- `README.md`
- `sections/08-setup-config.md`
- `sections/03-team-mode.md`
- `sections/04-ralph-ultrawork.md`

## 다음 행동

처음 쓰면 `omx setup`과 첫 세션 루프부터 따라 하세요.
운영자면 `sections/07-mcp-state.md`와 recent `CHANGELOG.md`를 같이 보세요.
