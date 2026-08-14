---
name: cell-docs
description: Use when a Jupyter notebook has substantive code cells missing preceding markdown spec cells, or when asked to document what notebook cells do. Triggered by /cell-docs, "add cell docs", "document notebook cells", or noticing a code cell with no markdown above it.
---

# cell-docs

## Overview

Add a markdown spec cell before each substantive code cell in a Jupyter notebook. Each spec is 3-5 bullets covering what the cell does, what it expects as input, what it produces, and any key decisions embedded in the code.

## When to Use

- `/cell-docs` is invoked
- A notebook has code cells with no preceding markdown, or only a heading
- Retroactively documenting an existing notebook
- After writing new cells that were added without specs

## Process

1. **Identify the target notebook** — use the file the user mentions, or the most recently opened `.ipynb` in the session
2. **Read the notebook** — load all cells, note which code cells are missing a substantive preceding markdown cell (a heading like `## Cell 2` doesn't count)
3. **For each undocumented code cell**, generate a markdown cell with 3-5 bullets:
   - **What it does** — one sentence summary of the cell's purpose
   - **Inputs** — variables, files, or connections it expects to exist
   - **Outputs** — what it produces (variables, files, printed stats)
   - **Key decisions** — any non-obvious choices (why a column is excluded, why a threshold was chosen, what a filter removes)
4. **Insert the markdown cell** immediately before the code cell in the notebook JSON — the markdown cell index must be lower than the code cell index. Never append it after.
5. **Write the updated notebook** back to disk

## Spec Format

```markdown
- **What:** Detect pull events from the snapshot table by finding weeks where `Credit_lh_midscore` changed
- **Inputs:** `_snap_pd` (full snapshot DataFrame sorted by opp + age)
- **Outputs:** `pull_events` DataFrame (one row per pull event), `df` (pull events with label column)
- **Label:** `Credit_lh_midscore >= 620` at the exact pull snapshot — point-in-time, not forward-looking
- **Excluded:** `Credit_lh_midscore` removed from feature cols since it is the y
```

## What Counts as Substantive

A code cell is substantive if it does any of the following:
- Runs a query or reads data
- Computes a label or feature
- Trains or evaluates a model
- Exports or saves a file
- Contains a non-obvious filter, join, or decision

Single-line imports, constants, or utility definitions do not need a spec cell.

## What Does NOT Count as a Spec Cell

A preceding markdown cell is NOT sufficient if it is only:
- A section heading (`## Cell 3`)
- A one-liner with no bullets
- A generic label with no actual description of inputs/outputs/decisions

## Applying Going Forward

When writing new notebook cells, always write the markdown spec cell first, then the code cell. The spec is written before the code — it clarifies intent and surfaces decisions before they get buried in implementation.
