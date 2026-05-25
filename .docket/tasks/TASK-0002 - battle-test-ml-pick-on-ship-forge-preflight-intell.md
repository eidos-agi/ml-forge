---
id: TASK-0002
title: Battle-test /ml-pick on ship-forge preflight intelligence
status: Done
created: '2026-03-24'
priority: high
milestone: MS-0001
tags:
  - ship-forge
  - battle-test
acceptance-criteria:
  - ml-pick produces a concrete recommendation for ship-forge preflight
  - Feature vector sketched if ML recommended
  - Compared against the current static checklist approach
visionlog_goal_id: GOAL-003
updated: '2026-03-24'
---
ship-forge missed lint because nobody hardcoded it. Run /ml-pick to determine the right intelligence level for auto-detecting which preflight checks a project needs.

## ml-pick Result

**Recommended: L0 bootstrap with L1 growth (same pattern as test-forge)**

The key insight wasn't ML — it was CI mirroring. The highest-value check is: parse .github/workflows/ci.yml and mirror every `run:` step in preflight. That's L0 (deterministic config parsing), but it logs every run as training data for future L1 weights that learn which checks are most predictive of real issues.

Built `/ship-detect` skill in ship-forge:
- Scans project config files (pyproject.toml, ruff.toml, .eslintrc, CI workflows)
- Builds a check manifest with priority and source
- CI mirroring: any CI step not in the manifest = critical gap
- Feedback loop for L1 growth

Updated `/ship-check` to call `/ship-detect` as Step 0.

Commit: abff4b7 on eidos-agi/ship-forge
