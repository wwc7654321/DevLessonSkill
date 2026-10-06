# Summarizing Experience After a Task Is Done

## First judge: will this task be reused? (reusability gate, before anything else)

Before summarizing, first answer: will this task (or one of its sub-stages) be done again in the future?

- **It will recur** → worth summarizing. Example: "small-model batch translation" — different games, different packaging formats, different text types all need localization, reusable at any time.
- **One-off tool** → don't summarize. Example: a specific screen translator, used once and then never written a second time; its pitfalls, however real, won't be encountered again. Writing them into the library only dilutes it and slows down reading. Instead, write a completion report for the project in `project_summary/` (what was done, the final approach, leftover issues), and don't put it into class.md.

What's reusable is the **task pattern that will recur**, not **the specific incidents of this task**. "Having stepped on a pitfall" ≠ "reusable".

When a task completes smoothly, think: were there any difficulties this time, any decisions requiring judgment, any outdated knowledge, or any "this path is a dead end" lessons? Distill general experience from them and write it into the corresponding category md (e.g. `DotnetProgramDevelop.md`).
Remember, the distilled experience must be independent of the specific task details, and general enough.

If the whole task had multiple stages (e.g. translating a game certainly has an unpacking stage, a translation-tool debugging stage, the actual translation stage, and a repacking stage), each stage has a different focus and different lessons, so they can be summarized separately.

- **Better few good lessons than many**
If nothing is worth saying — next time you hit a similar task you could handle it with ease — then give up summarizing (you can note in class.md that the task went smoothly, no experience summary needed). Don't force a useless summary: only record pitfalls that "the model couldn't guess from existing knowledge, or would guess wrong"; don't restate common sense the model already knows (basic tool usage, common formats, etc.), otherwise the experience file gets diluted and each read gets slower.
- **Split by "reuse boundary"**: first scan two axes — "task type" and "tech stack × scenario"; the tech-stack axis is the easiest to miss (e.g. a small-tool project implicitly carries the pitfalls of .NET WinForms development) — business features are conspicuous while the tech stack is invisible, yet the reusable value is higher; split out the reusable core (e.g. "local-model translation") for separate summarization, and it must be general without platform-specific logic; things in the same layer but not the same step (unpacking/packing) should belong to the same layer (likewise, if there's no reusable value they can be omitted — e.g. if unpacking has no controversy, only mention packing); things that only appear on one platform and can't be reused after splitting shouldn't be split.
- **Environment preparation is also an experience point**: task dependencies and extra environments that need preparing are prerequisites "an ordinary user may not have", so list them explicitly, don't assume the reader already has them.
- **Mechanisms must be self-explanatory**: write a mechanism only if it has value, and when you do, write out the mechanism itself (what it is / how to do it / why); giving just a noun is unreadable to the next model, and if it has no value, delete it.
- **Examples and wording avoid sensitive content**: experience may be shown to others, so examples and wording must not contain age-inappropriate content; when a grammar point needs an example, pick a neutral sentence without losing the teaching point.
- **Experiences can reference each other, not necessarily each self-contained**: a new experience needn't restate related background, it can reference existing experience (e.g. "online LLM translation" references "local small-model translation"), writing only the difference; experiences can form a reference web.
- **An experience standing alone as a category must have irreplaceable pitfalls of its own**: when a variant task differs so little there's nothing to say, there's no need to make it a separate category — readers who reach the referenced original experience will naturally figure out the variant themselves; adding unnecessary stuff is worse than not writing it.

## Maintain class.md
- Categories are two-dimensionally orthogonal: one axis "task type" (what you do), one axis "tech stack × scenario" (what you use); the same task can fall on multiple dimensions, each going to its own category and read separately.
- class.md only does category routing: identification, boundaries, experience files, cross-category reuse.
- Don't write specific experience, tool parameters, or pitfall details.
- Create a new category only when it has an independent lessons file and can't be covered by an existing category; otherwise merge into the existing category.
- Each category 3–5 lines; if it exceeds that, move content back to lessons.
- Keep category names and paths as stable as possible; when renaming, update all references.
- Don't add last-updated timestamps, keyword fingerprints, severity, or auto-index; for low-frequency tasks these are pure overhead.

## Supplementing Existing Experience
If you want to supplement existing experience, you may modify it and hand it to the user for confirmation. But note: don't pile layer upon layer into a shit mountain; instead review the modified file as a whole, see whether there are redundant or duplicate descriptions, and whether adjusting the structure, order, or wording of the experience descriptions can replace crudely adding new entries. Always stay lean and restrained.
The modified experience should also be handed to experience-quality-check.md for review (explain what was changed).
