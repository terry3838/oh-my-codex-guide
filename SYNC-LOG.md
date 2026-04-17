# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `5892224fd684354d1216ee9862d8e12768ebf561`
- current source sha: `09d2126204a605b23cb37d72e3005dd4b571ad12`
- mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증.

## upstream commits reviewed

- `09d21262 Merge remote-tracking branch 'origin/dev'`
- `54813d39 Merge pull request #1633 from Yeachan-Heo/release/0.13.1`
- `0e142412 Prepare the 0.13.1 hotfix release cut`
- `e43009b5 Keep detached tmux leader launches from dropping Codex stdin on startup (#1631)`
- `8a11d124 Publish the 0.13.0 dev line onto main`
- `3e3ab62e Prepare the 0.13.0 release cut from latest dev`
- `23ebc57d Prepare the 0.13.0 release cut from latest dev`
- `72ecedcf Reduce repeated git probes in leader stale polling on macOS (#1619)`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `.github/PULL_REQUEST_TEMPLATE.md`
- `.github/workflows/dev-merge-issue-close.yml`
- `.github/workflows/pr-check.yml`
- `.github/workflows/release.yml`
- `AGENTS.md`
- `CHANGELOG.md`
- `CONTRIBUTING.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `crates/omx-explore/src/main.rs`
- `docs/adapt.md`
- `docs/codex-native-hooks.md`
- `docs/contracts/ralph-state-contract.md`
- `docs/prompt-guidance-contract.md`
- `docs/prompt-guidance-fragments/core-operating-principles.md`
- `docs/qa/release-readiness-0.13.0.md`
- `docs/qa/release-readiness-0.13.1.md`
- `docs/release-notes-0.13.0.md`
