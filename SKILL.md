---
name: claude-code
description: Use the Claude Code CLI (`claude`) to perform complex coding tasks, file exploration, and terminal-based development. Trigger this skill when the user wants to use Claude Code as a sub-agent for autonomous coding, refactoring, or terminal interaction.
---

# Claude Code

This skill enables interaction with Antropic's Claude Code CLI tool for autonomous coding and terminal operations.

## Basic Workflow

1. **Navigate**: Change to the target project directory.
   ```bash
   cd /path/to/project
   ```
2. **Launch**: Start the Claude Code instance.
   ```bash
   claude
   ```
3. **Execute**: Provide clear, precise instructions to the CLI.

## Interactive Commands

The CLI supports several slash-commands for session management:
- Use `/compact` frequently in long sessions to save tokens and maintain speed.
- Use `/context` to monitor token usage.
- Use `/doctor` if the tool behaves unexpectedly.
- For a full list of commands and shortcuts, see [cli-reference.md](references/cli-reference.md).

## Strategies for Efficient Use

- **Exploration**: Start with `@` to let the tool index/understand the project structure.
- **Precision**: Formulate prompts using the pattern: `"[Action] in [File], pay attention to [Condition]"`.
- **Bash Mode**: Use `!` for direct terminal commands if needed during the session.
- **Undo**: Use `Ctrl + Shift + -` to revert the last change if the tool makes a mistake.

## Safety & Validation

- Support validation by using `/review` after significant changes.
- **Critical Operations**: If the tool suggests deleting files or changing critical system settings, stop and ask for user confirmation before proceeding.
