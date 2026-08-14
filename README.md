# claude-skills

A collection of Claude Code skills for data science workflows.

---

## cell-docs

Adds a markdown spec cell before each substantive code cell in a Jupyter notebook, documenting what the cell does, its inputs, outputs, and any non-obvious decisions.

**Example output:**

```
- **What:** Distribution of eligible agent pool size per evaluation
- **Inputs:** df['N_ELIGIBLE_AGENTS']
- **Outputs:** histogram + descriptive stats; prints implied positive rate (1 / median pool size)
- **Key decisions:** N_ELIGIBLE_AGENTS is ARRAY_SIZE(eligible_agent_ids) from the view, the post-filter pool, not the full pre-filter count (~454)
```

Only added to substantive cells (queries, feature engineering, model training, exports). Skipped for imports, constants, and short utility lines.

### Installation

**1. Copy the skill file**

```bash
mkdir -p ~/.claude/skills/cell-docs
cp cell-docs/SKILL.md ~/.claude/skills/cell-docs/SKILL.md
```

**2. Add the hook to your settings (optional but recommended)**

The hook automatically triggers cell-docs whenever you open a `.ipynb` file in Claude Code.

Merge the contents of `cell-docs/hook-snippet.json` into your `~/.claude/settings.json` under the `"hooks"` key. If you already have a `PostToolUse` section, add this entry to the existing array.

**3. Restart Claude Code** (or open `/hooks`) to pick up the new settings.

### Usage

- Type `/cell-docs` to run it manually on the current notebook
- With the hook installed, it runs automatically when you open a `.ipynb` file
