---
id: TASK-0003
title: Battle-test /ml-assess + /ml-extract on apple-a-day similarity
status: To Do
created: '2026-03-24'
priority: medium
milestone: MS-0001
tags:
  - apple-a-day
  - battle-test
acceptance-criteria:
  - ml-assess confirms or denies ML is worth it for this problem
  - 'If yes, ml-extract produces a concrete feature vector with extraction code'
  - Feature vector includes framework fingerprints from otool -L
visionlog_goal_id: GOAL-003
updated: '2026-03-24'
blocked_reason: Another session is doing ML work in apple-a-day. Don't duplicate.
---
apple-a-day uses hardcoded synonym groups for app similarity. Run the full pipeline: /ml-assess to confirm it's ML-shaped, /ml-extract to design the feature vector (framework fingerprints, UTIs, entitlements, etc.).
