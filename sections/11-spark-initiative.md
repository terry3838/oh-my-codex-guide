# Spark Initiative와 v0.9.x 업데이트

`v0.9.0`은 OMX의 **Spark Initiative** 릴리즈입니다. 이 시점부터 OMX는 Team Mode 오케스트레이션 중심 도구를 넘어서, **read-only repository discovery와 shell-native inspection을 위한 native fast path**를 함께 제공하는 런타임으로 설명하는 편이 맞습니다.

---

## 1. 먼저 버전 상태를 구분하기

2026년 3월 13일 기준으로 학습자가 봐야 할 버전 상태는 둘입니다.

- **latest published GitHub release**: `v0.9.0`
- **local source clone**: `0.9.1`

즉, 공개 릴리즈 문맥에서는 `v0.9.0`이 Spark Initiative의 시작점이고, 로컬 소스 기준으로는 `0.9.1` hotfix가 이미 반영되어 있을 수 있습니다.

`0.9.1`은 새 기능 릴리즈가 아니라 다음 의미를 가집니다.

- `v0.9.0`은 역사적으로 red 상태로 남음
- `0.9.1`은 packed-install smoke hydration fix를 포함한 clean superseding hotfix

---

## 2. 무엇이 추가되었나

![Diagram 1](../assets/diagrams/sections__11-spark-initiative__diagram_1.svg)

### `omx explore`

- 읽기 전용 탐색 전용 엔트리포인트
- allowlisted shell-native path를 사용
- 단순 repo lookup, 파일/심볼 탐색, 로그 확인에 적합

### `omx sparkshell`

- JS -> Rust sidecar bridge 기반 operator surface
- 긴 출력을 compact summary로 줄일 수 있음
- tmux pane tail 요약을 명시적으로 지원

예시:

```bash
omx sparkshell --tmux-pane %12 --tail-lines 400
```

### explore ↔ sparkshell integration

모든 `omx explore`가 sparkshell로 가는 것은 아닙니다. 다만 qualifying read-only shell-native prompt는 sparkshell backend를 사용할 수 있습니다.

핵심은:
- 속도를 높이되
- read-only safety를 풀지 않고
- unsupported execution은 normal path에 남겨 두는 것

---

## 3. 설치/배포 관점에서 왜 중요한가

Spark Initiative 이후에는 OMX를 TypeScript CLI만으로 보면 반만 이해한 것입니다.

학습자가 함께 알아야 할 요소:

- `omx-explore-harness`
- `omx-sparkshell`
- `native-release-manifest.json`
- packed-install smoke verification
- `npm run build:full`

즉, OMX는 이제 **TypeScript orchestration + native helper distribution**이 함께 있는 하이브리드 런타임입니다.

---

## 4. 0.9.1 hotfix가 실제로 정리한 것

로컬 소스의 `docs/release-notes-0.9.1.md` 기준으로 보면, 핵심은 packed-install smoke hydration fix입니다.

학습 포인트:

- `v0.9.0`은 기능 릴리즈
- `0.9.1`은 릴리즈 안정화/hydration 정리
- 따라서 기능 설명은 `v0.9.0`, 배포 안정성 설명은 `0.9.1` 문맥으로 읽으면 가장 깔끔합니다

---

## 5. 실무적으로 기억할 다섯 가지

1. `omx explore`는 읽기 전용 탐색 surface다.
2. `omx sparkshell`은 명시적 shell-native inspection surface다.
3. Spark Initiative 이후 OMX는 native helper distribution까지 포함한다.
4. `npm run build:full`이 release-oriented one-shot build path다.
5. project scope 사용자는 업그레이드 후 `omx setup --force --scope project`를 다시 돌리는 편이 안전하다.

