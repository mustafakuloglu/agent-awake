Working style:
- Treat the user as a serious collaborator, but do not blindly trust them. Users can be mistaken, so fact-check claims against the available context, files, tools, or reliable sources.
- If you have a strong, well-supported opinion and think the user is wrong, say so directly and explain why.

Tool discovery:
- Prefer `agent-awake` from `PATH` when available.
- If it is not on `PATH`, locate the `agent-awake` repository and use its `bin/agent-awake` script directly.
- If the repository is checked out beside the current project, a typical relative path is `../agent-awake/bin/agent-awake`.
- If `agent-awake` is not installed and no local checkout exists, clone `https://github.com/mustafakuloglu/agent-awake.git` and use `bin/agent-awake` from that checkout.
- If the user gives you the `agent-awake` repository as context, treat its `AGENTS.md` as the policy for long-running local tasks in the current task, even when the command runs in a different project.

Long-running local tasks:
- Before starting a local command or operation that is likely to run for a long time, decide whether it may need to continue after the user closes the laptop lid. If yes, ask: "Do you want me to keep this running even if the lid is closed?"
- If the user says yes, use `agent-awake --lid -- <command>`.
- Before relying on lid mode on an unknown machine, run `agent-awake --preflight`. If sudo is not ready, ask the user to run `sudo -v` in an interactive terminal first.
- Warn that lid mode can keep the local command running but does not prove Wi-Fi or an agent API connection will survive lid close. If network continuity matters, recommend a short lid-closed smoke test first.
- If the user says no, or if lid-closed continuation is unnecessary, use normal awake mode.
- For commands that should survive normal idle sleep, run them through `agent-awake -- <command>`.
- Also use lid mode directly if the user already explicitly asked for the machine to continue with the lid closed. This temporarily sets `pmset -a disablesleep 1`, keeps sudo alive, and restores `disablesleep 0` when the command exits.
- Do not leave lid-closed sleep disabled globally. Warn if the machine is not on AC power or could be placed in a bag while running.
