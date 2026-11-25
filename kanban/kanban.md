# Handoff: Implement a File-Based Agent-Native Project Structure

## Objective

Create a reusable, agent-native project structure that can be adopted across future repositories.

The core idea is:

- The **filesystem is the project state**
- **Git is the audit/history layer**
- `AGENTS.md` defines how agents operate
- File-based Kanban tracks work execution
- Each task is represented by an individual Markdown file
- Documentation stores durable project knowledge
- Agents should be able to enter the repository and determine what to do next with minimal human prompting

The system should work well for both single-agent and future multi-agent workflows.

---

## Core Design

Use the following project structure as the default:

```text
project/
├── AGENTS.md
├── README.md
├── PROJECT.md
│
├── board/
│   ├── backlog/
│   ├── ready/
│   ├── in-progress/
│   ├── blocked/
│   └── done/
│
├── docs/
│   ├── architecture/
│   ├── decisions/
│   └── specs/
│
├── artifacts/
│
└── src/
```

The exact application/source layout may vary by project, but the agent-management structure above should remain consistent.

---

# Responsibilities of Each Component

## `README.md`

Audience: humans.

Purpose:

- Explain what the project is
- Explain how to install/run it
- Provide normal developer onboarding information
- Keep this separate from agent workflow instructions

---

## `PROJECT.md`

Purpose: stable project-level context.

It should contain:

- Project objective
- Scope
- Non-goals
- Important constraints
- High-level architecture
- Current project phase
- Definition of success

This file describes **what we are building and why**.

It should not contain volatile task status.

---

## `AGENTS.md`

Purpose: operating manual for AI agents.

It should remain relatively short and act primarily as the agent's entry point.

It should explain:

1. What files to read first
2. How to select work
3. How task states operate
4. What agents may or may not modify
5. Testing expectations
6. Documentation expectations
7. Completion criteria
8. What to do when blocked
9. How to handle architectural decisions

Avoid turning `AGENTS.md` into a complete project encyclopedia.

Instead, link to the appropriate files under `docs/`, `PROJECT.md`, and task files.

Recommended execution flow:

```text
Read AGENTS.md
       ↓
Read PROJECT.md
       ↓
Inspect board/ready/
       ↓
Select highest-priority unblocked task
       ↓
Move task to board/in-progress/
       ↓
Read task context and referenced documentation
       ↓
Implement
       ↓
Test
       ↓
Update task with implementation notes
       ↓
Move task to board/done/
```

---

# File-Based Kanban

The board directories represent task state:

```text
board/backlog/
board/ready/
board/in-progress/
board/blocked/
board/done/
```

Task state should be determined primarily by **file location**, rather than a mutable `status:` field.

For example:

```text
board/ready/TASK-012-add-authentication.md
```

becomes:

```text
board/in-progress/TASK-012-add-authentication.md
```

when work begins.

This avoids maintaining two competing sources of truth.

---

# Task Model

Use **one Markdown file per task**.

Do not maintain all tasks inside one large `KANBAN.md`.

Reasons:

- Reduces Git merge conflicts
- Supports parallel agents
- Makes task history easy to audit
- Makes individual tasks self-contained
- Allows future tooling to parse tasks easily

Recommended task filename:

```text
TASK-<number>-<slug>.md
```

Example:

```text
TASK-004-fix-api-pagination.md
```

---

# Task File Schema

Use YAML front matter for machine-readable metadata.

Example:

```markdown
---
id: TASK-004
title: Fix API pagination
priority: high
created: 2026-10-06
depends_on:
  - TASK-002
labels:
  - api
  - bug
---

# Goal

Fix pagination for `/api/users`.

# Context

Pagination currently skips records when multiple rows share the same timestamp.

Relevant files:

- `src/api/users.ts`
- `docs/architecture/api.md`

# Acceptance Criteria

- Cursor pagination works correctly
- No records are skipped
- Existing API behaviour remains backwards compatible
- Unit tests are included
- Integration tests pass

# Implementation Notes

To be completed by the implementing agent.

# Validation

To be completed before moving the task to `done`.
```

---

# Recommended Metadata

Keep metadata deliberately small.

Initial schema:

```yaml
id:
title:
priority:
created:
depends_on:
labels:
```

Recommended priority values:

```text
critical
high
medium
low
```

Avoid adding unnecessary workflow metadata until there is a concrete need.

In particular, do not duplicate the directory state using:

```yaml
status: ready
```

unless tooling later requires it.

---

# Agent Work Selection

Agents should normally operate using a pull model.

Instead of requiring:

```text
Implement TASK-023.
```

the system should support instructions such as:

```text
Continue working on the project.
```

The agent should then:

1. Read `AGENTS.md`
2. Read relevant project context
3. Inspect `board/ready/`
4. Resolve task dependencies
5. Select the highest-priority suitable task
6. Move it into `board/in-progress/`
7. Execute it
8. Validate the implementation
9. Move it into `board/done/`

This repository should therefore function as a persistent workspace rather than requiring the human to reconstruct context in every prompt.

---

# Dependency Handling

Tasks may declare:

```yaml
depends_on:
  - TASK-002
  - TASK-003
```

An agent must not begin the task unless all dependencies are complete.

If dependencies are not complete, leave the task in `ready` or move it to `blocked` if appropriate.

Do not silently ignore dependency metadata.

---

# Blocked Tasks

When an agent cannot safely continue:

1. Update the task with a `Blocker` section
2. Clearly state what is missing
3. Move the task to:

```text
board/blocked/
```

Example:

```markdown
# Blocker

The external API contract is still undefined.

Required information:

- authentication mechanism
- response schema
- rate limits
```

Agents should not invent important missing requirements simply to complete a task.

---

# Definition of Done

A task should only move to `board/done/` when:

- Acceptance criteria are satisfied
- Relevant tests pass
- Required documentation has been updated
- No known blocker remains
- Implementation notes are recorded
- Architectural decisions are captured where necessary

Moving the file to `done` is an explicit declaration that the task has been validated.

---

# Documentation Structure

Use:

```text
docs/
├── architecture/
├── decisions/
└── specs/
```

## `docs/architecture/`

Long-lived descriptions of how the system works.

Examples:

```text
data-flow.md
api.md
storage.md
deployment.md
```

## `docs/specs/`

Detailed feature or system specifications.

Use these when requirements are too large to belong inside an individual task.

Tasks should reference specs rather than duplicating them.

## `docs/decisions/`

Store architectural decisions as ADR-style documents.

Example:

```text
ADR-001-use-postgresql.md
ADR-002-adopt-event-driven-ingestion.md
```

Each decision should explain:

- Context
- Decision
- Alternatives considered
- Consequences

---

# Nested `AGENTS.md`

Support nested instructions for large or monorepo projects.

Example:

```text
AGENTS.md

backend/
├── AGENTS.md
└── ...

frontend/
├── AGENTS.md
└── ...

infra/
├── AGENTS.md
└── ...
```

The root `AGENTS.md` should contain global workflow rules.

Nested files should contain domain-specific guidance.

Example:

```text
backend/AGENTS.md
```

may define:

- Python conventions
- test commands
- database migration rules

while:

```text
infra/AGENTS.md
```

may contain:

- Terraform conventions
- deployment restrictions
- rules preventing automatic production changes

Do not duplicate global instructions unnecessarily.

---

# Multi-Agent Considerations

The structure should be designed with future concurrency in mind.

Prefer:

```text
one task = one file
```

because different agents can work on independent files.

Avoid a central mutable task document where every agent must edit the same file.

Agents should avoid taking a task already present under:

```text
board/in-progress/
```

unless explicitly instructed to collaborate on it.

Longer term, task claiming/locking may be added if needed, but keep the first version filesystem-native and simple.

---

# Source of Truth

Use the following hierarchy:

```text
PROJECT.md
    → project intent

AGENTS.md
    → agent operating rules

docs/
    → durable knowledge

board/
    → current execution state

Git history
    → historical audit trail
```

Avoid storing the same information in multiple places.

---

# Future Tooling

The filesystem representation should remain simple enough that additional tooling can be built later.

Possible future commands:

```text
project init
project status
project next
project add-task
project start TASK-012
project block TASK-012
project done TASK-012
```

Possible future UI:

```text
task Markdown files
        ↓
parser
        ↓
task/dependency graph
        ↓
Kanban web UI
```

The GUI should remain a projection of filesystem state.

It must not become the authoritative datastore.

---

# Initial Implementation Requested

Build the first reusable version of this system.

Deliver:

1. A reusable project directory template
2. A concise default `AGENTS.md`
3. A default `PROJECT.md`
4. A task template
5. An ADR template
6. Example board directories
7. At least two example tasks demonstrating:
   - dependencies
   - acceptance criteria
   - task state
8. Documentation explaining how a human or agent should use the workflow

Keep the implementation simple.

Do not build a database or server yet.

Do not build a complex orchestration engine yet.

Do not over-engineer task metadata.

The first milestone is to validate that:

> Git + filesystem + Markdown are sufficient to act as a persistent, auditable project control plane for coding agents.

---

# Design Principles

When making implementation decisions, optimise for:

- Agent readability
- Human readability
- Git friendliness
- Low merge-conflict probability
- Portability between agent systems
- Minimal hidden state
- Explicit dependencies
- Simple automation
- Long-term extensibility

Prefer conventions over custom infrastructure.

The repository itself should contain enough state for a new agent to understand:

```text
What is this project?

What are the rules?

What needs to be done?

What is currently being worked on?

What is blocked?

What has already been completed?

Why were important architectural decisions made?
```

without requiring previous chat history.

---

# Guiding Mental Model

Treat the repository as an agent-native operating environment:

```text
Repository      = workspace
Files           = persistent memory
Kanban folders  = execution state
Task files      = work orders
AGENTS.md       = operating policy
PROJECT.md      = mission/context
docs/           = institutional knowledge
Git             = audit log
Agent           = worker
```

Preserve this model throughout the implementation.