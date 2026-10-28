# Live check: config/crew-exclude-tools on Claude workers

Real `bin/fm-spawn.sh` (only the tmux pane faked) built each worker launch from a home's
config/crew-exclude-tools (`fm-spawn-launch-transcript.txt`). The Claude part of those exact
launch strings was then run against the real `claude` 2.1.312 CLI, using the machine's normal login.

| Run | Tools in init | Excluded names still loaded |
|---|---|---|
| scout with list (Artifact, AskUserQuestion, Workflow, ScheduleWakeup, ReportFindings, mcp__firecrawl, mcp__context7) | 362 | 0 |
| ship with empty list (baseline) | 395 | 33 (Workflow, ScheduleWakeup, ReportFindings, 28 firecrawl tools, 2 context7 tools) |

- In both runs the worker's first tool call read the operational-inbox record named by the launch
  prompt, so the variadic `--disallowedTools` did not swallow the trailing prompt.
- Interactive pty run of the exact launch (`claude-scout-interactive-pane.txt`): the worker read its
  launch brief and reported the task done.
- Refusals: codex with a non-empty list and a malformed entry were both refused (see transcript).
