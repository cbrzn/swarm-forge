---
tags: [brainstorm, swarmforge, ui, orchestration]
status: raw
date: 2026-05-24
---

# SwarmForge UI — Raw Brainstorm

## The Problem

SwarmForge runs agents in tmux sessions but gives no visibility into what each agent is doing. You can't tell if an agent is idle, working, or done without attaching to its session manually. Running multiple feature swarms in parallel is also not supported.

## Core Ideas

### 1. Agent Status Visibility
A UI that shows per-agent state at a glance:
- **Idle** — waiting for input
- **Working** — actively doing something
- **Awaiting handoff** — sent a notify, waiting for response
- **Done** — completed its task

State inference could come from parsing `logs/agent_messages.log` or adding explicit state writes to `notify-agent.sh`.

### 2. Multiple Parallel Swarms (per project)
Run one swarm per feature branch simultaneously. Each swarm gets namespaced sessions and worktrees (e.g. `swarmforge-<project>-<feature>-<role>`). A UI could show all active swarms grouped by feature.

### 3. UI Approaches (rough options)
- **TUI** — Node.js with `ink` or `blessed`, lives in terminal, reads log/state files
- **Web UI** — tiny local server (Express/Fastify) + browser dashboard, more visual
- **`watch` script** — simplest possible, zero deps, just refreshes a terminal view

### 4. What the UI Would Show
- List of active swarms + their features
- Per-agent status panel
- Recent messages from `agent_messages.log`
- Which agent is currently the "blocker" (needs input)

## Open Questions (not urgent)
- How to reliably detect idle vs working without modifying SwarmForge internals?
- Should this be a separate tool or built into SwarmForge?
- Web UI vs TUI — depends on workflow preference

## Related Tools Explored
- **Webmux** — web dashboard on top of Claude Code/Codex, closest existing solution
- **Composio Agent Orchestrator** — handles parallel agents + PRs autonomously
- **OpenHands** — most mature, but opinionated
- **AgentOps** — observability layer, could complement this
