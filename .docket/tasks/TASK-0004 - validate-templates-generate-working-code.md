---
id: TASK-0004
title: Validate templates generate working code
status: To Do
created: '2026-03-24'
priority: medium
milestone: MS-0001
tags:
  - templates
  - validation
dependencies:
  - TASK-0001
acceptance-criteria:
  - Generated features.py extracts real features
  - Generated train.py trains and exports weights JSON
  - Generated model.py loads weights and predicts with zero deps
  - Generated evaluate.py produces a quality report
---
Take the output of a battle-test (whichever completes first) and actually generate code from the templates. Verify it runs, trains, exports weights, and the runtime model infers correctly.
