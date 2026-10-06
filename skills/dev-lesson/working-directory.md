To complete the user's task you may write a number of scripts: some are temporary, run once and adjusted on the fly; some actually execute the task; and some verify the task environment or check assumptions. Once there is more than one temporary script, they should all go under a directory whose name ends with `work` or `work_dir`.
Each task should have its own subdirectory (e.g. `xx_work\check_curl_cmd\test.py`, or `xx_work\do_work_1\formal.py`, `input.json`, `output.json`).
If there are many files, you can add a `README.md` inside the working directory explaining each script's function and purpose (what problem prompted writing it). This may help when summarizing experience after the task is done. A new task need not try to read it.
Each time you write a new script, briefly mention in the agent output what you're trying to do, so the user is aware (or, when permissions are off, the user can use this to decide whether to allow it).
Script **artifacts** (intermediate results, verification reports, logs, etc. — `.txt`/`.json`/`.log`) should also go into the corresponding work subdirectory, not scattered into the task/game root directory — even if you're just running a bit of Python to verify one conclusion, decide up front to write the output file into the work subdirectory, don't dump it in the root for convenience.

## Global tool/SDK storage (tools not in the task directory)

Large tools/SDKs (Godot, GDRE, model weights, etc.) go in a **global directory** — they don't travel with the task, aren't re-downloaded each time, and don't get stuffed into the task/game directory. Principles:

- **Ask the user the first time you need one**: is it available locally, and which directory to put it in — don't download silently (especially for large ones). A single large download takes tens to hundreds of MB; silently downloading it into a corner is both wasteful and messy.
- **The specific path is each user's / each machine's choice — don't hardcode it**; this file only states the principle.
