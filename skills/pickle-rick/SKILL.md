---
name: pickle-rick
description: Autonomous PRD-driven coding loops using pickle-rick-claude. Use when user asks to "run pickle rick", "use pickle", "autonomous coding loop", "PRD-driven development", "meeseeks review", or wants to spawn long-running agentic coding sessions with context clearing, circuit breakers, and automatic rate limit recovery. Requires Claude Code CLI and tmux.
---

# Pickle Rick

Autonomous PRD-driven engineering loops for Claude Code based on the [Ralph Wiggum loop](https://ghuntley.com/ralph/).

Repo: https://github.com/gregorydickson/pickle-rick-claude

## Installation

```bash
git clone https://github.com/gregorydickson/pickle-rick-claude.git
cd pickle-rick-claude
bash install.sh

# Add to project
cat ~/.claude/pickle-rick/persona.md >> /path/to/project/.claude/CLAUDE.md
```

Requires: Node 18+, Claude Code CLI v2.1.49+, jq, rsync, tmux (for /pickle-tmux)

## Core Commands

| Command                           | Purpose                                                                 |
| --------------------------------- | ----------------------------------------------------------------------- |
| `/pickle "task"`                  | Full loop: PRD → breakdown → research → plan → implement → simplify     |
| `/pickle prd.md`                  | Execute existing PRD                                                    |
| `/pickle-tmux "task"`             | tmux mode for long epics (8+ iterations, context clearing)              |
| `/pickle-refine-prd prd.md`       | Refine PRD with 3 parallel analysts, then `/pickle --resume`            |
| `/pickle-refine-prd --run prd.md` | Refine + auto-launch tmux session                                       |
| `/meeseeks`                       | Code review loop (10-50 passes) — audits deps, security, tests, quality |
| `/portal-gun <source>`            | Gene transfusion — extract patterns from another codebase               |
| `/project-mayhem`                 | Chaos engineering — mutation testing, dep downgrades                    |
| `/pickle-metrics`                 | Token usage, commits, lines changed                                     |
| `/eat-pickle`                     | Cancel active loop                                                      |

## Key Flags

```
--max-iterations <N>     Stop after N iterations (default: 100, 0=unlimited)
--max-time <M>           Stop after M minutes (default: 720)
--resume [PATH]          Resume existing session
--meeseeks               Chain Meeseeks review after execution
--completion-promise     Only stop when agent outputs specific text
```

## Workflow Patterns

**Quick task:**

```
/pickle "refactor the auth module"
```

**Complex task (recommended):**

```
/pickle-refine-prd my-prd.md   # Refine with analysts
/pickle --resume                # Execute
```

**Full pipeline:**

```
/pickle-refine-prd --meeseeks my-prd.md  # Refine → execute → review
```

## Architecture

- **Context clearing**: Fresh subprocess per iteration (no drift)
- **Circuit breaker**: Auto-stops sessions stuck in error loops
- **Rate limit recovery**: Detects throttling, computes wait, auto-resumes
- **Pickle Jar**: Queue tasks for batch execution (`/add-to-pickle-jar`, `/pickle-jar-open`)

## tmux Monitor

`/pickle-tmux` creates a 3-pane layout:

- Top-left: Dashboard (ticket status, phase, circuit breaker state)
- Top-right: Iteration logs
- Bottom: Live worker output

```bash
tmux attach -t <session>  # Attach
Ctrl+B d                   # Detach (keeps running)
```

## Configuration

Edit `~/.claude/pickle-rick/pickle_settings.json`:

| Setting                           | Default  | Description               |
| --------------------------------- | -------- | ------------------------- |
| `default_max_iterations`          | 100      | Max loop iterations       |
| `default_max_time_minutes`        | 720      | Session time limit        |
| `default_meeseeks_model`          | "sonnet" | Model for review passes   |
| `default_circuit_breaker_enabled` | true     | Enable runaway protection |

## When to Use

- Building features from PRDs
- Long refactoring epics (8+ iterations)
- Overnight batch coding tasks
- Automated code review cycles
- Cross-codebase pattern extraction

Launch Claude with `claude --dangerously-skip-permissions` for unattended operation.
