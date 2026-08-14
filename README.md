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

**Option A: Paste this prompt into Claude Code**

```
Please install the cell-docs skill from https://github.com/luke-edwards-vu/claude-skills:

1. Fetch the raw SKILL.md from https://raw.githubusercontent.com/luke-edwards-vu/claude-skills/main/cell-docs/SKILL.md and write it to ~/.claude/skills/cell-docs/SKILL.md (create the directory if needed).
2. Read ~/.claude/settings.json. Add a PostToolUse hook with matcher "NotebookEdit" and this command: jq -r 'if .tool_input.cell_type == "code" then "{\"hookSpecificOutput\":{\"hookEventName\":\"PostToolUse\",\"additionalContext\":\"You just wrote a code cell to a notebook. Add a markdown spec cell immediately before it with What, Inputs, Outputs, and Key decisions bullets.\"}}" else empty end' 2>/dev/null || true — merge it into the existing hooks section without overwriting anything else.
3. Tell me when done and remind me to open /hooks or restart Claude Code.
```

**Option B: Manual install**

1. Copy the skill file:
```bash
mkdir -p ~/.claude/skills/cell-docs
cp cell-docs/SKILL.md ~/.claude/skills/cell-docs/SKILL.md
```

2. Merge the hook from `cell-docs/hook-snippet.json` into your `~/.claude/settings.json` under the `"hooks"` key. If you already have a `PostToolUse` section, add this entry to the existing array.

3. Restart Claude Code (or open `/hooks`) to pick up the new settings.

### Usage

- Type `/cell-docs` to run it manually on the current notebook
- With the hook installed, it runs automatically when you open a `.ipynb` file
