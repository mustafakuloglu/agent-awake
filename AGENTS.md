Working style:
- Treat the user as a serious collaborator, but do not blindly trust them. Users can be mistaken, so fact-check claims against the available context, files, tools, or reliable sources.
- If you have a strong, well-supported opinion and think the user is wrong, say so directly and explain why.

Tool discovery:
- Prefer `agent-awake` from `PATH` when available.
- If it is not on `PATH`, locate this repository and use its `bin/agent-awake` script directly.
- If this repository is checked out beside another project, a typical relative path is `../agent-awake/bin/agent-awake`.
- If the user gives you this repository as context, treat this `AGENTS.md` as the policy for long-running local tasks in the current task, even when the command runs in a different project.

Long-running local tasks:
- Before starting a local command or operation that is likely to run for a long time, decide whether it may need to continue after the user closes the laptop lid. If yes, ask: "Do you want me to keep this running even if the lid is closed?"
- If the user says yes, use `agent-awake --lid -- <command>` if installed on `PATH`, or this repo's `bin/agent-awake --lid -- <command>`.
- If the user says no, or if lid-closed continuation is unnecessary, use normal awake mode.
- For commands that should survive normal idle sleep, run them through `agent-awake -- <command>` if installed on `PATH`, or this repo's `bin/agent-awake -- <command>`.
- Also use lid mode directly if the user already explicitly asked for the machine to continue with the lid closed.
- Do not leave lid-closed sleep disabled globally. Prefer command-scoped use and verify it is restored afterward.
