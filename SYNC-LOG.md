# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `ed78b878e59f1d65a2f1626d63f293e8d40ebe81`
- current source sha: `fabb3ce0b96e42c20feb2940c74f2aa5addb8cee`
- mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 소스코드

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: README/소개, 설치/설정, CLI/명령어, 문서 구조, 소스코드.

## upstream commits reviewed

- `fabb3ce Merge pull request #1241 from Yeachan-Heo/release/0.11.13`
- `2b644d2 Keep dispatch notification tests stable across CI shadow-state lag`
- `204c024 Keep mailbox notification assertions deterministic across full CI lanes`
- `ce930ba Keep tmux-heal verification stable on GitHub runners`
- `cc1b814 Make mailbox notification assertions deterministic under GitHub CI`
- `26eaec0 Keep tmux-heal verification aligned with the active pane injection path`
- `130a1b9 Keep CI typecheck unblocked after the live-injection fix`
- `6300c9f Keep leader mailbox nudges injectable through the active pane path`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.vi.md`
- `RELEASE_BODY.md`
- `crates/omx-runtime-core/src/engine.rs`
- `crates/omx-runtime-core/src/lib.rs`
- `docs/contracts/ralph-state-contract.md`
- `docs/qa/ralph-persistence-gate.md`
- `docs/qa/release-readiness-0.11.13.md`
- `docs/reference/ralph-parity-matrix.md`
- `docs/release-notes-0.11.13.md`
- `package-lock.json`
- `package.json`
- `src/cli/__tests__/autoresearch.test.ts`
- `src/cli/__tests__/cleanup.test.ts`
- `src/cli/__tests__/error-handling-warnings.test.ts`
- `src/cli/__tests__/exec.test.ts`
- `src/cli/__tests__/index.test.ts`
- `src/cli/__tests__/launch-fallback.test.ts`
