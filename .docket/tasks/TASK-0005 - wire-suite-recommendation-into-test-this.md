---
id: TASK-0005
title: Wire suite recommendation into test_this
status: Done
created: '2026-03-24'
priority: high
milestone: MS-0001
tags:
  - test-forge
  - recommendation
acceptance-criteria:
  - recommend_suites() called when target is a repo with existing suites
  - Scored menu appears in test_this output
  - 'GUARD-001 respected: recommendations shown, not auto-executed'
visionlog_goal_id: GOAL-003
updated: '2026-03-24'
---
test-forge's recommend/ module (features, model, train, weights) exists but isn't wired into test_this. Users never see the scored menu. Wire it in so that when a repo target is given, the forge recommends which suites to run based on the change fingerprint.

## Done

Wired recommend_suites() into test_this. Live test confirmed:
- Recommendation menu appears in output with scored suites
- Deployment gap warning fires correctly (score 1.0 with uncommitted changes)
- Fixed: scoped suite discovery to repo-only (no global cache leaking)
- Fixed: friendly suite names (strip path prefix)

Commits: b2d521d (wiring), ff9f532 (scoping + naming fix)
