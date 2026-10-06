---
name: dev-lesson
description: Use when doing programmatic development tasks — writing scripts, building small tools, making game mods, building GUI tools, etc. Not triggered for chat or Q&A.
---

# Summarizing and reusing development experience

## Experience categories

The user may ask you to develop all kinds of things. First figure out what kind of task it is: a crawler? a small local tool? a game mod?

Maintain a dictionary in `lessons/class.md` (each entry: identification, boundary, experience file, cross-category reuse — format per the "Maintaining class.md" section of `summarize-experience.md`) to classify task types.

Note that a category is not a one-to-one match with a task: one task can span several categories (e.g. "translating a Godot game" = Godot unpacking/repacking + translation, two categories). File each under its own category and read each category's experience separately.

## Summarizing experience after a task

Read `summarize-experience.md`, distill the lessons per it, and write them into the matching category's file.

A one-off project with no reuse value (done-and-used, never rebuilt) is not distilled into lessons; instead write a wrap-up report in `project_summary/` (what was done, final approach, leftover issues), and do not add it to class.md.

### Quality check before writing

Do not write experience drafts directly. For each lesson, attach its source (tested / inferred) and hand it to an independent subagent, which decides per `experience-quality-check.md`. The subagent receives only the rules and the lesson content — not the task process or the pitfall stories — to keep a clean context.

## Reusing experience

Before starting a task, if you find a matching category in class.md, read the corresponding file to avoid repeating mistakes. Filter out lessons with similar meaning and check each against this task: applicable or not, and if not, say why.

## Keeping the working directory tidy

When you decide to write one-off scripts (.py/.sh, etc.) to files, follow `working-directory.md`. The same goes for other temporary files — don't scatter files under the current path.
