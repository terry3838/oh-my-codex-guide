# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `09d2126204a605b23cb37d72e3005dd4b571ad12`
- current source sha: `435a3399732268f39e3d1f6f357cc9f7c66b744b`
- mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: README/소개, 설치/설정, CLI/명령어, 문서 구조, 스킬/플러그인, 소스코드, 테스트/검증.

## upstream commits reviewed

- `435a3399 Merge dev into main for v0.13.2 release`
- `328470c4 Prepare the 0.13.2 release cut`
- `a8312914 Merge pull request #1707 from Yeachan-Heo/fix/persistent-hooks`
- `61f406f0 Let native Stop auto-nudge run without OMX runtime gating`
- `e562820f Keep Stop-hook cleanup green under no-unused CI checks`
- `fb893f92 Keep active OMX workflows blocking Stop until they truly finish`
- `d9437f2e Merge pull request #1700 from Yeachan-Heo/fix/issue-1698-explore-reentry-recursion`
- `4136347b Keep explore startup-hook tests aligned with rustfmt\n\nThe PR's Rust changes only needed the formatter-applied line wrap in the new explore harness test so CI can pass without widening the change set.\n\nConstraint: Limit the fix to the explore-only Rust diff already in PR #1699\nRejected: Touch unrelated TypeScript or runtime behavior | outside requested scope\nConfidence: high\nScope-risk: narrow\nReversibility: clean\nDirective: Keep follow-up CI fixes confined to the new explore harness coverage unless a failing check proves otherwise\nTested: cargo fmt --all --check; cargo clippy -p omx-explore-harness --all-targets -- -D warnings\nNot-tested: Full workspace test matrix\n`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `.github/workflows/release.yml`
- `AGENTS.md`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `crates/omx-explore/src/main.rs`
- `docs/codex-native-hooks.md`
- `docs/getting-started.html`
- `docs/integrations.html`
- `docs/troubleshooting.md`
- `package-lock.json`
- `package.json`
- `skills/analyze/SKILL.md`
- `src/catalog/__tests__/generator.test.ts`
- `src/catalog/generated/public-catalog.json`
- `src/catalog/manifest.json`
- `src/cli/__tests__/index.test.ts`
- `src/cli/__tests__/setup-skills-overwrite.test.ts`
