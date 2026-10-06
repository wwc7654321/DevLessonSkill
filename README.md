# DevLessonSkill

A Claude Code skill for **summarizing and reusing development experience**.
For programmatic tasks (writing scripts, building small tools, making game mods, building GUI tools, etc.), it consults past pitfalls before you start and distills reusable lessons into the experience library when you finish — so you stop repeating the same mistakes.

**Positioning**: not a general memory bank, not a team wiki. It's a "pitfall cheat-sheet" for low-to-mid-frequency development tasks — precise, easy to find, and concise.
Wide in, strict out: no bar for what gets summarized, but each lesson is independently quality-checked before entering the library — keeping only the pitfalls a model can't infer (or would get wrong) from what it already knows.

## What it doesn't do

- Doesn't trigger on chat / Q&A; it only serves programmatic development tasks.
- No auto-retrieval, knowledge graph, layered storage, or self-evolution; at low frequency these are pure overhead.
- Doesn't write one-off project details into the experience library — those go into `project_summary/` as a wrap-up report.
- Doesn't force-summarize common knowledge, basic tool usage, or anything already spelled out in official docs.

## How it differs from general memory / skill projects

General solutions target high-frequency, general-purpose, long-lived knowledge bases, emphasizing auto-retrieval, layered storage, and self-evolution.
DevLessonSkill targets low-to-mid-frequency, one-off / tool-building tasks: category routing + short experience files + independent quality check, prioritizing low context cost and low manual maintenance.
The goal isn't a large, complete wiki — it's "don't step on the same pitfall twice".

## One-line install

Requires [Claude Code](https://claude.com/claude-code).

**Option 1 (newer Claude Code, one command)**:

```
/plugin install dev-lesson --marketplace wwc7654321/DevLessonSkill
```

**Option 2 (two steps, compatible with older versions)**:

```bash
claude plugin marketplace add wwc7654321/DevLessonSkill
claude plugin install dev-lesson@dev-lesson-marketplace
```

Restart Claude Code (or run `/reload-plugins` in a session) for it to take effect.

## Usage

1. **Automatic (recommended)**: add a rule to `CLAUDE.md` (user-level or project-level). It reads experience before a task and summarizes after, with no manual prompting:

```markdown
Applies only to programmatic tasks (writing scripts, building tools, making mods, etc.), not to chat or Q&A. When triggered, follow the dev-lesson skill: read past experience by task category before starting, and summarize experience after finishing.
```

2. **Manual** (same command `/dev-lesson`, two scenarios):

| Scenario | Command | What it does |
|----------|---------|--------------|
| Before a dev task | `/dev-lesson` | Read the matching category's experience to avoid known pitfalls |
| After a dev task | `/dev-lesson` | Distill reusable lessons, quality-check, and write them into the library |

## Structure

```
skills/dev-lesson/
  SKILL.md              # Entry: experience categories, reuse, summarize, quality-check flow
  lessons/
    class.md            # Task-category routing dictionary
    *.md                # Reusable experience by category (local model translation / Godot mods / WinForms…)
  project_summary/      # Wrap-up reports for one-off projects (not in the library)
  summarize-experience.md / experience-quality-check.md / working-directory.md
```

- **Reusable experience** → goes into `lessons/`, routed by `class.md`.
- **One-off projects** (done-and-used, never rebuilt) → only a wrap-up report in `project_summary/`, keeping the library clean.

## License

[MIT](LICENSE)
