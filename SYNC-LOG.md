# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `b1c39445741b8a0c4c30b22ab0db3e9491215b8b`
- current source sha: `c364f617eccfe3783b8708f5ef53cd11396f76cf`
- mode: `update`
- impact labels: CLI/명령어, 문서 구조, 소스코드

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: CLI/명령어, 문서 구조, 소스코드.

## upstream commits reviewed

- `c364f61 Cut the 0.12.3 patch release on main`
- `010068e Cut the 0.12.3 patch release`
- `9513cc3 Merge pull request #1364 from Yeachan-Heo/hotfix/team-keyword-detector`
- `cacc7f7 Merge remote-tracking branch 'origin/dev' into hotfix/team-keyword-detector`
- `0003a4f Cut the 0.12.2 patch release on main`
- `13b680c Cut the 0.12.2 patch release`
- `93dc6f6 Merge pull request #1369 from Yeachan-Heo/fix/team-silently-delted`
- `98b90ad Merge pull request #1367 from Yeachan-Heo/fix/stale-state-hud`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `RELEASE_BODY.md`
- `docs/qa/release-readiness-0.12.2.md`
- `docs/qa/release-readiness-0.12.3.md`
- `docs/release-notes-0.12.2.md`
- `docs/release-notes-0.12.3.md`
- `package-lock.json`
- `package.json`
- `src/cli/__tests__/index.test.ts`
- `src/cli/index.ts`
- `src/hooks/__tests__/agents-overlay.test.ts`
- `src/hooks/__tests__/keyword-detector.test.ts`
- `src/hooks/agents-overlay.ts`
- `src/hooks/keyword-detector.ts`
- `src/hud/__tests__/render.test.ts`
- `src/hud/__tests__/state.test.ts`
- `src/hud/render.ts`
- `src/hud/state.ts`
