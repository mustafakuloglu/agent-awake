# Agent Bootstrap Prompt

Give this prompt to an agent when you want it to use this repository for
long-running local work:

```md
You have access to the `agent-awake` repository.

Read `agent-awake/AGENTS.md` and follow it for this task, even if the command
you run is in another project.

When you need to run a likely long-running local command or operation, decide
whether it should continue if the laptop lid is closed. If yes, ask me:
"Do you want me to keep this running even if the lid is closed?"

Use `agent-awake -- <command>` when `agent-awake` is installed on PATH.
If it is not on PATH, locate the `agent-awake` repo and use
`bin/agent-awake -- <command>` from that checkout.
```

Short version:

```md
Use the `agent-awake` repo. Read `agent-awake/AGENTS.md` and follow it for all
long-running local work in this task.
```
