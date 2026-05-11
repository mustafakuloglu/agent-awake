Working style:
- Treat the user as a serious collaborator, but do not blindly trust them. Users can be mistaken, so fact-check claims against the available context, files, tools, or reliable sources.
- If you have a strong, well-supported opinion and think the user is wrong, say so directly and explain why.

Tool discovery:
- Prefer `agent-awake` from `PATH` when available.
- If it is not on `PATH`, locate this repository and use its `bin/agent-awake` script directly.
- If this repository is checked out beside another project, a typical relative path is `../agent-awake/bin/agent-awake`.
- If `agent-awake` is not installed and no local checkout exists, clone `https://github.com/mustafakuloglu/agent-awake.git` and use `bin/agent-awake` from that checkout.
- If the user gives you this repository as context, treat this `AGENTS.md` as the policy for long-running local tasks in the current task, even when the command runs in a different project.

Long-running local tasks:
- Before starting a local command or operation that is likely to run for a long time, decide whether it may need to continue after the user closes the laptop lid. If yes, ask: "Do you want me to keep this running even if the lid is closed?"
- Treat jobs like model training, large data processing, multi-hour builds, video rendering, backups, and large downloads as likely long-running. Do not ask for ordinary short commands unless the user says they are stepping away or closing the lid.
- If the user says yes, use `agent-awake --lid -- <command>` if installed on `PATH`, or this repo's `bin/agent-awake --lid -- <command>`.
- Before relying on lid mode on an unknown machine, run `agent-awake --preflight` if installed on `PATH`, or this repo's `bin/agent-awake --preflight`. If sudo is not ready, ask the user to run `sudo -v` in an interactive terminal first.
- Warn that lid mode can keep the local command running but does not prove Wi-Fi or an agent API connection will survive lid close. If network continuity matters, recommend a short lid-closed smoke test first.
- If the user says no, or if lid-closed continuation is unnecessary, use normal awake mode.
- For commands that should survive normal idle sleep, run them through `agent-awake -- <command>` if installed on `PATH`, or this repo's `bin/agent-awake -- <command>`.
- Also use lid mode directly if the user already explicitly asked for the machine to continue with the lid closed.
- Do not leave lid-closed sleep disabled globally. Prefer command-scoped use and verify it is restored afterward.
