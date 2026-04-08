# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `fabb3ce0b96e42c20feb2940c74f2aa5addb8cee`
- current source sha: `b1c39445741b8a0c4c30b22ab0db3e9491215b8b`
- mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드.

## upstream commits reviewed

- `b1c3944 Keep dev aligned after shipping 0.12.1`
- `0ce655b Cut the 0.12.1 patch release on main`
- `1ff91e1 Preserve release-branch guidance and mailbox accuracy before the 0.12.1 cut`
- `38e633e chore: empty CI probe on dev`
- `1fc66b8 Restore detached tmux launches for interactive OMX sessions (#1356)`
- `f1e7f8b Keep PR #1349 mergeable against dev without losing native stop-hook protections`
- `824bb60 Merge pull request #1348 from Yeachan-Heo/fix/stop-hook-misbehavior`
- `4a6feaa Merge pull request #1352 from Yeachan-Heo/fix/release-0-12-1-metadata`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `.github/ISSUE_TEMPLATE/config.yml`
- `.gitignore`
- `AGENTS.md`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `crates/omx-runtime/src/main.rs`
- `docs/codex-native-hooks.md`
- `docs/contracts/team-delivery-state-contract.md`
- `docs/contracts/team-runtime-state-contract.md`
- `docs/hooks-extension.md`
- `docs/index.html`
- `docs/integrations.html`
- `docs/openclaw-integration.de.md`
- `docs/openclaw-integration.es.md`
- `docs/openclaw-integration.fr.md`
- `docs/openclaw-integration.it.md`
- `docs/openclaw-integration.ja.md`
