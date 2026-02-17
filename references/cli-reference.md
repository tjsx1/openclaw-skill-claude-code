# Claude Code CLI Reference

## Core Commands
- `/add-dir`: Adds another working directory.
- `/context`: Displays current context usage (Grid view).
- `/compact`: Compresses history while keeping essential info (saves tokens).
- `/config`: Opens the settings panel.
- `/doctor`: Checks the installation for errors.
- `/clear`: Clears history for a fresh start.
- `/exit`: Ends the session.

## Shortcuts & Interaction
- `!`: Bash mode for direct terminal commands.
- `@`: Rapid file path selection.
- `&`: Backgrounds processes.
- `Shift + Tab`: Auto-accept code edits.
- `Ctrl + Shift + -`: Undo last change.

## Operation Strategies
- **Exploration**: Use `@` initially to understand file structure.
- **Precision**: Task schema: `"[Action] in [File], ensure [Condition]"`.
- **Validation**: Use `/review` after major changes.
- **Maintenance**: Use `/compact` during long sessions to maintain performance.
