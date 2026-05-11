# agent-awake

`agent-awake` is a macOS helper for AI agents and other long-running local
work. It wraps a command with `caffeinate` so normal idle sleep does not stop
the work. When explicitly requested, it can also temporarily disable lid-close
sleep.

## Give This To An Agent

Read and follow the agent-awake instructions at https://github.com/mustafakuloglu/agent-awake#readme for long-running local work; if agent-awake is not available locally, clone https://github.com/mustafakuloglu/agent-awake.git and use its `bin/agent-awake`.

## Install

From this repository:

```sh
chmod +x bin/agent-awake
```

Optional, put it somewhere on `PATH`:

```sh
sudo cp bin/agent-awake /usr/local/bin/agent-awake
```

## Usage

Prevent normal idle sleep while a command runs:

```sh
agent-awake -- ./long-running-job.sh
agent-awake -- ./backup.sh
```

Continue while the lid is closed:

```sh
agent-awake --lid -- ./backup.sh
```

Check whether the machine looks ready for lid mode:

```sh
agent-awake --preflight
```

Hold the machine awake without running a command:

```sh
agent-awake --hold
agent-awake --lid --hold
```

Inspect current power assertions:

```sh
agent-awake --status
```

Restore lid-close sleep manually if needed:

```sh
agent-awake --restore-lid
```

## Agent Instructions

This repo includes an `AGENTS.md` and a reusable snippet at
`docs/AGENTS.snippet.md`. You can either give the agent this repo as context,
or add something like this to other repos:

```md
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
- Treat jobs like model training, large data processing, multi-hour builds, video rendering, backups, and large downloads as likely long-running. Do not ask for ordinary short commands unless the user says they are stepping away or closing the lid.
- If the user says yes, use `agent-awake --lid -- <command>`.
- Before relying on lid mode on an unknown machine, run `agent-awake --preflight`. If sudo is not ready, ask the user to run `sudo -v` in an interactive terminal first.
- Warn that lid mode can keep the local command running but does not prove Wi-Fi or an agent API connection will survive lid close. If network continuity matters, recommend a short lid-closed smoke test first.
- If the user says no, or if lid-closed continuation is unnecessary, use normal awake mode.
- For commands that should survive normal idle sleep, run them through `agent-awake -- <command>`.
- Also use lid mode directly if the user already explicitly asked for the machine to continue with the lid closed.
- Do not leave lid-closed sleep disabled globally. Prefer command-scoped use and verify it is restored afterward.
```

## Safety

`caffeinate` does not reliably override lid-close sleep by itself. The `--lid`
mode uses `sudo pmset -a disablesleep 1`, keeps sudo fresh while the command
runs, and restores `disablesleep 0` when the command exits.

`--lid` keeps the machine from sleeping, but it does not guarantee that Wi-Fi,
VPNs, or an AI agent's API connection will survive lid close on every Mac and
network setup. Treat `agent-awake --preflight` as a readiness check, not a
proof. The real proof is a lid-closed smoke test on the target machine.

Use `--lid` only when the laptop is on AC power, on a hard surface, and has
reasonable airflow. Do not put a closed running laptop in a bag.

For non-interactive agents, run `sudo -v` once in an interactive terminal before
using `--lid`; otherwise the script will fail fast instead of hanging on a
password prompt.

Quick smoke test:

```sh
agent-awake --preflight
sudo -v
agent-awake --lid -- sleep 60 &
agent-awake --status
# Close the lid, wait 90 seconds, reopen, then check that sleep finished.
```

The background `&` is only for this smoke test so the shell can stay available
for `--status`. For real work, run the command in the foreground unless you have
another reason to background it.

If the `agent-awake` process is killed with `SIGKILL` or the machine loses power,
cleanup cannot run. Use `agent-awake --restore-lid` after reboot or recovery if
lid-close sleep appears to remain disabled.
