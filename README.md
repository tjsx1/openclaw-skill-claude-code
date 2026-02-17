# OpenClaw Skill: Claude Code

A specialized [OpenClaw](https://openclaw.ai) skill for controlling Antropic's **Claude Code CLI** (`claude`) as a sub-agent.

## Overview

This skill transforms your OpenClaw agents into expert DevOps and Fullstack operators capable of using Claude Code for autonomous coding tasks, terminal-based development, and complex refactorings.

## Features

- **Standardized Workflow**: Optimized commands for navigating, launching, and interacting with the `claude` CLI.
- **Deep CLI Integration**: Built-in reference for all major Claude Code shortcuts (`!`, `@`, `&`, etc.) and slash-commands (`/compact`, `/context`, `/doctor`).
- **Safety First**: Explicit strategies for validation and confirmation before critical system changes.
- **Token Efficiency**: Pre-defined usage of `/compact` and session management to keep performance high.

## Installation

Packaged skills can be installed via the OpenClaw CLI:

```bash
openclaw skill install claude-code.skill [--target agent-id]
```

## Usage

Once installed, trigger the skill by mentioning coding tasks or specifically requesting the use of the Claude Code CLI.

### Basic Workflow
1. **Navigate**: Go to your project folder.
2. **Launch**: The agent starts `claude`.
3. **Tasking**: Provide high-level goals; the agent will use Claude Code to execute them.

## Strategies

- **Exploration**: Uses `@` indexing for project-wide understanding.
- **Precision**: Prompting follow the schema: `"[Action] in [File], ensure [Condition]"`.
- **Validation**: Uses `/review` after major changes.

---
Created for use with OpenClaw. "Siri's competent cousin."
