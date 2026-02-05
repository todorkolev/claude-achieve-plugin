# Achieve Plugin

Goal-driven iterative development with multi-session support.

## Commands

| Command | Description |
|---------|-------------|
| `/achieve:goal <goal>` | Start a new goal session |
| `/achieve:list-goals` | List all sessions and their status |
| `/achieve:cancel-goal [id]` | Cancel a session (active if no id) |

## How It Works

1. **Parse goal**: Extract success criteria from goal text
2. **Create session**: Generate unique ID, create state files
3. **Loop**: Stop hook intercepts exit, continues until done
4. **Complete**: Output `<promise>ACHIEVED</promise>` when criteria met

## Session Management

Sessions are stored in `.claude/achieve-sessions/`:

```
.claude/achieve-sessions/
├── index.yaml              # Active session + list
├── abc123/
│   ├── state.yaml          # Goal, criteria, attempts
│   ├── loop.md             # Loop state
│   ├── research.md         # Research findings
│   ├── spec.yaml           # Requirements spec
│   └── plan.yaml           # Implementation plan
└── def456/
    └── ...
```

## Multi-Session Support

- Start multiple sessions (each gets unique ID)
- Only one session active at a time per terminal
- Previous session paused when new one starts

## Parallel Sessions (Multiple Terminals)

Run multiple goals in parallel using separate terminal sessions:

```bash
# Terminal 1
claude
/achieve:goal "Build REST API"

# Terminal 2
claude
/achieve:goal "Fix auth bug"
```

Each terminal runs its own independent loop. Session tracking happens transparently via transcript markers - no environment variables needed.

## Workflow Phases

Structured development through automatic phases:
- **Research**: Explore codebase, identify patterns and dependencies
- **Plan**: Create spec.yaml and plan.yaml with requirements and tasks
- **Build**: Implement with progress tracking and deviation logging
- **Validate**: Test against success criteria, iterate until achieved

## Stop Criteria

The loop stops when:
1. You output `<promise>ACHIEVED</promise>` (goal complete)
2. Max iterations reached (default: 50)

## Installation

Use Claude Code's built-in `/plugin` command for installation.

### Step 1: Add the Marketplace

In Claude Code, run:

```
/plugin marketplace add todorkolev/claude-achieve-plugin
```

### Step 2: Install the Plugin

```
/plugin install achieve@claude-achieve-plugin
```

That's it! The plugin is now installed.

## Acknowledgments

The phase-based workflow (research, plan, build, validate) is inspired by [Buildforce CLI](https://github.com/berserkdisruptors/buildforce-cli).

