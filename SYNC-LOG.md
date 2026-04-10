# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `c364f617eccfe3783b8708f5ef53cd11396f76cf`
- current source sha: `6d737879e8c1992ef045db387044c7dd156cc78d`
- mode: `update`
- impact labels: README/소개, 설치/설정, CLI/명령어, 문서 구조, 소스코드, 테스트/검증

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: README/소개, 설치/설정, CLI/명령어, 문서 구조, 소스코드, 테스트/검증.

## upstream commits reviewed

- `6d737879 Revert "fix: fully strip multiline root notify arrays during setup merge (#1430)"`
- `a588326b Revert "fix: enforce session authority for ownerless Stop Ralph gating (#1431)"`
- `2f4a11a3 Revert "fix: honor active tmux context before availability probe (#1432)"`
- `6f832d07 docs(readme): add strong default environment caution`
- `223085c1 fix: honor active tmux context before availability probe (#1432)`
- `cabb8c57 fix: enforce session authority for ownerless Stop Ralph gating (#1431)`
- `5d979b35 fix: fully strip multiline root notify arrays during setup merge (#1430)`
- `e0f67399 Ship 0.12.4 with release metadata sync and dispatch receipt fix`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `.github/workflows/ci.yml`
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `README.md`
- `RELEASE_BODY.md`
- `docs/codex-native-hooks.md`
- `docs/hooks-extension.md`
- `docs/qa/ci-speedups-after-prompt-worker-fix.md`
- `docs/release-notes-0.12.4.md`
- `docs/reports/open-prs-dev-readiness-2026-04-09.md`
- `package-lock.json`
- `package.json`
- `src/cli/__tests__/index.test.ts`
- `src/cli/__tests__/launch-fallback.test.ts`
- `src/cli/__tests__/mcp-parity.test.ts`
- `src/cli/__tests__/nested-help-routing.test.ts`
- `src/cli/__tests__/package-bin-contract.test.ts`
- `src/cli/__tests__/setup-hooks-shared-ownership.test.ts`
- `src/cli/__tests__/setup-scope.test.ts`
