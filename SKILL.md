---
name: secure-claude-cli
description: Executes autonomous coding tasks using the Claude Code CLI. Runs tasks safely without getting stuck in interactive REPL loops. Use this skill whenever you need to delegate complex coding tasks to Claude Code, run multi-file refactors, or execute long-running code generation tasks via CLI.
---

# Secure Claude Code CLI

This skill provides a secure, non-interactive way to delegate complex coding tasks to Anthropic's Claude Code CLI.

## Rules for Execution (CRITICAL)

1. **NEVER** run `claude` alone — this opens an interactive REPL and will freeze the current session.
2. **ALWAYS** use `-p` to pass instructions directly as a one-shot command.
3. **No UI interactions.** Use only `stdout`/`stderr` — no keyboard shortcuts.
4. For long tasks, prefer background execution via `tmux` with log output to a file.
5. Use `--allowedTools` to restrict tool access when running untrusted or scoped tasks.
6. Always set a timeout on synchronous tasks to prevent indefinite blocking.
7. Do not exfiltrate logs or code to external APIs/services unless the user explicitly requests it.

---

## Usage Patterns

### 1) Synchronous Task (quick, scoped)

Navigate to the project directory and run Claude Code with `-p`. The process blocks until done. Use `timeout` to prevent indefinite hangs.

> Default shown: `300s` — adjust based on task complexity.

```bash
cd /path/to/project
timeout 300 claude -p "Refactor the authentication logic in src/auth.js to use JWT. Do not ask for confirmation."
```

To restrict which tools Claude Code can use (recommended for scoped tasks):

```bash
timeout 300 claude -p "Add JSDoc comments to all functions in src/utils.js" \
  --allowedTools "Read,Write,Edit"
```

For machine-readable output (useful when the agent needs to parse results):

```bash
timeout 300 claude -p "List all TODO comments in the codebase" \
  --output-format json
```

---

### 2) Asynchronous Task (long-running)

For complex tasks, run Claude Code in an isolated `tmux` session and write logs to a file.

Launch:

```bash
cd /path/to/project
tmux new-session -d -s claude-bg-task \
  "claude -p 'Build a new dashboard component in React' > claude-run.log 2>&1"
```

Check progress:

```bash
tail -n 20 /path/to/project/claude-run.log
```

Check if still running:

```bash
# exit code 0 => session exists (still running)
# exit code 1 => session gone (finished or crashed)
tmux has-session -t claude-bg-task 2>/dev/null; echo "Exit: $?"
```

Wait for completion and retrieve output:

```bash
# Poll until done (checks every 10s)
while tmux has-session -t claude-bg-task 2>/dev/null; do sleep 10; done
cat /path/to/project/claude-run.log
```

---

### 3) Using --cwd instead of cd

Prefer `--cwd` over `cd && ...` for robustness in nested or scripted calls:

```bash
timeout 300 claude -p "Run all tests and fix any failures" --cwd /path/to/project
```

---

## Error Recovery

- If a command fails, inspect console output or `claude-run.log` immediately.
- If Claude hits a rate limit or transient API error, wait 60 seconds and retry once.
- If timeout fires (exit code `124`), the task was too complex — switch to the async `tmux` pattern.
- If retry fails, report the blocker with the exact failing command and the last 20 lines of the log.

---

## Flag Reference

| Flag | Purpose |
|---|---|
| `-p "..."` | One-shot prompt (required — prevents REPL) |
| `--cwd /path` | Set working directory without `cd` |
| `--allowedTools "..."` | Restrict tool access (e.g. `"Read,Write,Edit"`) |
| `--output-format json` | Machine-readable output for downstream parsing |
| `--output-format stream-json` | Streaming JSON for long outputs |
| `timeout N` | Kill process after N seconds (prevents hangs) |
