# Sync Log — oh-my-codex

## latest cycle

- previous source sha: `435a3399732268f39e3d1f6f357cc9f7c66b744b`
- current source sha: `435a3399732268f39e3d1f6f357cc9f7c66b744b`
- mode: `no-change`
- impact labels: 일반 변경

## decision

이번 싸이클에서는 origin 변경이 없어 guide 본문은 유지했고, 동기화 기준점만 재확인했습니다.

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
- 변경 파일 없음
