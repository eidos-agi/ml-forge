# ml-extract — Design Feature Extraction

Design a feature extraction pipeline that turns local signals into a model-ready feature vector.

## Trigger

User says `/ml-extract` or needs to design features for an ML model.

### Arguments

- `<project>` — the target project
- `<prediction>` — what the model will predict (from /ml-assess)

## Instructions

### Step 1: Inventory Available Signals

Scan the project and system for everything that could be a feature. Organize by source:

**Filesystem signals:**
- Config files present (pyproject.toml, ruff.toml, .eslintrc, tsconfig.json, etc.)
- Directory structure (src/, tests/, .github/workflows/)
- File types and counts
- File sizes and modification times

**Git signals:**
- Recent commits (messages, files changed, authors)
- Branch state (ahead/behind, merge conflicts)
- Diff stats (lines added/removed, files touched)
- Tags and releases

**System signals (macOS):**
- App bundles: Info.plist, frameworks (otool -L), entitlements, UTIs
- Launch Services database
- Process list, login items

**Project-specific signals:**
- CI config (what checks are defined)
- Test results history
- Dependency graph
- Existing hardcoded rules/lists

### Step 2: Design the Feature Vector

For each candidate feature:
1. **Name**: descriptive, snake_case
2. **Type**: binary (0/1), categorical (one-hot), numerical (float), set (Jaccard-ready)
3. **Extraction**: exact code/command to get it
4. **Missing value strategy**: what to do when it's not available
5. **Specificity**: how much signal it carries (e.g., "has pyproject.toml" is more specific than "has files")

### Step 3: Handle Set Features

For features that are sets (frameworks linked, UTIs registered, file types present):
- Use Jaccard similarity for pairwise models
- Use bag-of-items (binary vector) for classification models
- Weight by inverse frequency (rare items = more signal, common items = noise)

### Step 4: Write the Extractor

Generate a feature extraction module using the `features.py.tmpl` template:
- One function per signal source
- Each returns a dict of feature_name → value
- A `build_vector()` function that combines all sources into a flat list
- Handles missing data gracefully (default values, not crashes)

### Step 5: Validate

- Run the extractor on 3-5 real examples
- Check for: missing values, unexpected types, features that are always the same value
- Drop features with zero variance (they carry no information)

### Output

- Feature extraction module (generated from template)
- Feature dictionary documenting each feature's name, type, source, and rationale
- Validation report showing feature values for sample inputs

## Rules

- Every feature must be extractable locally with no network calls
- Prefer cheap features (file existence checks) over expensive ones (parsing large files)
- Document the extraction cost — subprocess calls are 10-100x slower than file reads
- Never include PII or secrets as features
