---
description: Pre-PR quality pass - parallel review + docs check, then tests for the gaps
argument-hint: "[path to the API, default: current folder]"
---
Run a quality pass on the Express API in `$ARGUMENTS`. If no path is given, use the current folder. Treat that folder as the working directory for every agent.

**Step 1 - in parallel.** Start both of these subagents at the same time. They are independent and read-only:
- `route-reviewer`: review all routes and store helpers.
- `docs-checker`: compare `docs/api.md` with the routes.

Wait until both have finished.

**Step 2 - depends on step 1.** Take the `UNTESTED:` line from route-reviewer.
- If it lists anything, start the `test-writer` subagent and pass it exactly that list.
- If it says `none`, skip this step and say so.

**Step 3 - report.** Merge everything into one short report:
1. high and medium findings from route-reviewer;
2. MISMATCH and UNDOCUMENTED lines from docs-checker;
3. tests added and the `npm test` result from test-writer, including any test marked `FAILS`.

Finish with the three fixes you would make first. Do not change application code in this command.
