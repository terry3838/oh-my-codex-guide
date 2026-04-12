# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `982a7546b7b42518d47de2ac53fa2b47ebd670a4`
- current source sha: `d5975af01a4bb8a7d3c68d4131b31029566380ac`
- mode: `update`
- impact labels: 설치/설정, CLI/명령어, 문서 구조, 소스코드

## decision

origin 변경 파일을 기준으로 guide 문서의 관련 섹션을 다시 읽고 반영했습니다. 핵심 영향 영역: 설치/설정, CLI/명령어, 문서 구조, 소스코드.

## upstream commits reviewed

- `d5975af0 Release 0.12.5`
- `ed38bd17 Add #1473 deep-interview Stop regression to 0.12.5 release notes`
- `17e94583 Merge pull request #1473 from Yeachan-Heo/fix/issue-1472-deep-interview-stop-wait`
- `6912fe59 Cut the 0.12.5 patch release`
- `966263cf Let deep-interview questions stop for user input`
- `fddeece2 Merge pull request #1471 from Yeachan-Heo/fix/multi-skill-state-invoke-state-inits`
- `213675e5 Keep planning state authoritative during multi-skill prompt routing`
- `38a0a414 Merge pull request #1470 from Yeachan-Heo/fix/issue-1353-windows-split-shutdown`

## evidence

- source remote: `https://github.com/Yeachan-Heo/oh-my-codex.git`
- docs/interesting dirs: docs/, skills/, src/
- changed file sample:
- `CHANGELOG.md`
- `Cargo.lock`
- `Cargo.toml`
- `RELEASE_BODY.md`
- `docs/STATE_MODEL.md`
- `docs/codex-native-hooks.md`
- `docs/contracts/multi-state-transition-contract.md`
- `docs/contracts/multi-state-transition-review.md`
- `docs/contracts/ralph-state-contract.md`
- `docs/prompt-guidance-contract.md`
- `docs/qa/release-readiness-0.12.5.md`
- `docs/release-notes-0.12.5.md`
- `package-lock.json`
- `package.json`
- `src/cli/__tests__/doctor-warning-copy.test.ts`
- `src/cli/__tests__/index.test.ts`
- `src/cli/__tests__/setup-refresh.test.ts`
- `src/cli/__tests__/team.test.ts`
- `src/cli/doctor.ts`
- `src/cli/explore.ts`
