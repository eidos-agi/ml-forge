---
id: TASK-0002
title: Battle-test /ml-pick on ship-forge preflight intelligence
status: To Do
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
---
ship-forge missed lint because nobody hardcoded it. Run /ml-pick to determine the right intelligence level for auto-detecting which preflight checks a project needs.
