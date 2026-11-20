# SmartDock instructions for coding agents

These files tell a coding agent how to use SmartDock in a WPF app: layouts, pane content, saving and loading, MVVM binding, toolbars, themes, and events. Pick the file for your agent and copy it into your project.

| Agent | File in this folder | Copy it to |
| --- | --- | --- |
| Claude Code | `skills/smartdock/` | `.claude/skills/smartdock/` |
| Other agents that read [Agent Skills](https://agentskills.io/) | `skills/smartdock/` | the skills folder your agent reads |
| Cursor | `cursor/smartdock.mdc` | `.cursor/rules/` |
| GitHub Copilot | `copilot/smartdock.instructions.md` | `.github/instructions/` |
| Codex and other agents that read `AGENTS.md` | `agents/AGENTS.md` | paste it into your `AGENTS.md`, or link to it from there |

All files carry the same text. Only the header at the top differs:

- `SKILL.md` has a `name` and a `description`. The agent loads the skill when a task matches the description.
- The Cursor rule has a `description` and `globs` for `.cs` and `.xaml` files, with `alwaysApply: false`.
- The Copilot file has `applyTo` for `.cs` and `.xaml` files.

Change the globs if your project needs a narrower scope. For anything the instructions do not cover, see the documentation at https://arcticnoise.github.io/SmartDock/.
