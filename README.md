# OpenClaw Skill: Secure Claude CLI

A hardened [OpenClaw](https://openclaw.ai) skill for running Anthropic **Claude Code CLI** (`claude`) in a safe, non-interactive way.

## What this skill solves

The default `claude` command starts an interactive REPL, which can block/lock automations.
This skill enforces one-shot and background patterns so OpenClaw agents can run coding tasks reliably.

## Core Principles

- **Never run `claude` alone** (no interactive REPL in automation flows)
- **Always use `-p`** for one-shot prompts
- **Use `timeout` for sync runs** to avoid indefinite hangs
- **Use `tmux` + logfile for long jobs**
- **Restrict scope with `--allowedTools`** when needed
- **No external log/code exfiltration** unless explicitly requested by the user

## Quick Usage

### 1) Synchronous (quick/scoped)

```bash
timeout 300 claude -p "Refactor src/auth.js to use JWT" --cwd /path/to/project
```

Scoped tools example:

```bash
timeout 300 claude -p "Add JSDoc to src/utils.js" \
  --cwd /path/to/project \
  --allowedTools "Read,Write,Edit"
```

Machine-readable output:

```bash
timeout 300 claude -p "List all TODO comments" \
  --cwd /path/to/project \
  --output-format json
```

### 2) Asynchronous (long-running)

```bash
tmux new-session -d -s claude-bg-task \
  "claude -p 'Build a new dashboard component in React' --cwd /path/to/project > /path/to/project/claude-run.log 2>&1"
```

Check progress:

```bash
tail -n 20 /path/to/project/claude-run.log
```

Check running status:

```bash
tmux has-session -t claude-bg-task 2>/dev/null; echo "Exit: $?"
# 0 = running, 1 = finished/not found
```

## Error Handling

- On transient API/rate-limit errors: wait 60s and retry once.
- On timeout (`124`): treat as too complex for sync; switch to async `tmux` pattern.
- On repeated failure: report exact command + last 20 logfile lines.

## Files

- `SKILL.md` → canonical behavior/instructions used by OpenClaw
- `references/cli-reference.md` → optional CLI command reference

---
Built for practical, production-safe Claude Code delegation in OpenClaw.
