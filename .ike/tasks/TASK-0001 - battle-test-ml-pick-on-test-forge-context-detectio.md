---
id: TASK-0001
title: Battle-test /ml-pick on test-forge context detection
status: In Progress
created: '2026-03-24'
priority: high
milestone: MS-0001
tags:
  - test-forge
  - battle-test
acceptance-criteria:
  - >-
    ml-pick produces a concrete recommendation (L0-L6) for test-forge context
    detection
  - Recommendation is actionable — not just 'use ML'
  - 'If ML is recommended, feature vector is sketched'
visionlog_goal_id: GOAL-003
updated: '2026-03-24'
---
test-forge tested a live site when changes were local. Run /ml-pick to determine the right intelligence level for detecting local vs deployed state. This is GOAL-003's first real use.

## ml-pick Result

**Recommended: L0 — hardcoded rules**

The context detection problem (local vs deployed) is NOT ML-shaped:
- 3-4 binary features (has_uncommitted, has_unpushed, target_is_url, target_is_localhost)
- No training data exists
- Decision tree is ~10 lines of if/else
- Already partially implemented in model.py:175-188 (deployment gap warning)

What DOES exist in test-forge that IS ML-shaped:
- Suite recommendation (DONE — already L1 with learned weights, 49 training examples, 8 history files)
- The recommend/ module already follows ml-forge patterns (features.py, model.py, train.py, weights.json)

**Action needed in test-forge:** Strengthen the L0 deployment gap detection. Currently it injects a warning but doesn't prevent testing the wrong target. Should gate the test: if target is remote URL + local changes exist, ask user to confirm or auto-redirect to localhost.

## Revised after user feedback

User correctly pushed back on L0. Hardcoded rules can't grow. Revised assessment:

**Recommended: L1 — learned weights, same pattern as suite recommendation**

The context detection problem IS ML-shaped when you factor in:
- Accumulated history from user confirm/override signals
- Multi-signal feature vector (git state + target type + deployment config + local server + timing)
- Labels come naturally from user interaction with the deployment gap warning

**Start at L0 as bootstrap, but wire it for L1 growth:**
1. L0 heuristic provides the initial behavior (what exists now)
2. Every decision (confirm/override) is logged as a training example
3. Once enough examples accumulate (>30), train.py learns weights
4. Model replaces the heuristic, continues collecting feedback

This is the hybrid approach from /ml-pick: L0 fallback + L1 that grows.
