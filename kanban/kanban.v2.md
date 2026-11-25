# Handoff: Lifecycle-Aware File-Based Agent-Native Project Structure V2

## 1. Mission

Establish a lightweight, reusable, lifecycle-aware, agent-native engineering environment that allows humans and AI agents to collaboratively develop, maintain, and evolve software projects.

The repository should function as a persistent, auditable project control plane without requiring a dedicated project management server, database, or proprietary agent orchestration platform.

An agent entering the repository without previous conversation history should be able to determine:

- What the project is.
- Why it exists.
- What the project constraints are.
- What operating rules apply.
- What work should happen next.
- What work is currently in progress.
- What is blocked.
- What has already been completed.
- Why important architectural decisions were made.
- How to implement and validate changes.
- Where durable knowledge belongs.
- What information can be archived or discarded.

The environment must support both individual agents and future concurrent multi-agent workflows.

The goal is not to maximise the amount of persistent information. The goal is to maintain the smallest useful representation of project intent, current execution state, durable knowledge, and reproducible implementation.

---

## 2. Core Philosophy

Preserve the following principles:

- The filesystem is the project state.
- Git is the audit and history layer.
- AGENTS.md defines agent operating policy.
- PROJECT.md defines project intent.
- ROADMAP.md defines future direction.
- File-based Kanban tracks active work.
- Individual Markdown files represent active tasks.
- Documentation stores durable institutional knowledge.
- Source code and tests represent executable truth.
- Generated artifacts have explicit lifecycles.
- Completed work is consolidated and archived.
- Agents should operate with minimal human prompting.

### Guiding Mental Model

| Component | Responsibility |
|---|---|
| Repository | Persistent workspace |
| Filesystem | Project state |
| Git | Audit log and historical memory |
| AGENTS.md | Operating policy |
| PROJECT.md | Mission and context |
| ROADMAP.md | Strategic direction |
| Kanban board | Execution queue |
| Task files | Work orders |
| Documentation | Institutional knowledge |
| Source code | Executable implementation |
| Tests | Executable verification |
| Agent | Worker |
| Task selection | Scheduler |
| Dependency resolution | Execution coordination |
| Knowledge consolidation | Memory compaction |
| Repository maintenance | Garbage collection |
| Artifact management | Log rotation and evidence retention |
| Task archival | Completed work lifecycle |

Treat these responsibilities as conceptual boundaries, not requirements to build separate software components.

---

## 3. Operating Modes

Determine the operating mode before making changes.

### Mode A: Project Bootstrap

Adopt the agent-native conventions within a new or existing product repository.

This is the default mode when this handoff accompanies a product or engineering specification.

- Inspect the existing repository and product requirements.
- Establish the minimum necessary project structure.
- Document project intent and constraints.
- Create an initial roadmap.
- Prepare actionable near-term tasks.
- Preserve existing application architecture and conventions where appropriate.

Do not embed a complete reusable framework, example repository, or starter project inside the product repository.

### Mode B: Project Execution

Continue developing an existing project using its established conventions.

- Read existing operating instructions.
- Resolve project context and task dependencies.
- Select and execute suitable work.
- Validate changes.
- Consolidate knowledge.
- Maintain repository hygiene.

Do not recreate the project structure or regenerate the roadmap unnecessarily.

### Mode C: Repository Maintenance

Improve the maintainability of an existing project.

- Consolidate overlapping documentation.
- Archive eligible completed tasks.
- Remove temporary and generated outputs.
- Review obsolete experiments.
- Resolve stale references.
- Improve repository structure without changing product behaviour unnecessarily.

### Mode D: Framework Development

Develop reusable templates, tooling, or conventions for agent-native engineering.

Only enter this mode when explicitly requested.

Framework development should normally happen in a dedicated framework or template repository rather than inside an unrelated product repository.

---

## 4. Progressive Scaffolding

Use the following structure as a reference convention, not a mandatory directory checklist.

    project/
    ├── AGENTS.md
    ├── README.md
    ├── PROJECT.md
    ├── ROADMAP.md
    │
    ├── board/
    │   ├── backlog/
    │   ├── ready/
    │   ├── in-progress/
    │   ├── blocked/
    │   ├── done/
    │   └── completed.jsonl
    │
    ├── docs/
    │   ├── architecture/
    │   ├── decisions/
    │   └── specs/
    │
    ├── artifacts/
    ├── experiments/
    ├── tests/
    └── src/

The exact source layout may vary according to the language, framework, and project requirements.

For example, a monorepo may use apps/ and packages/ instead of src/.

### Minimum Viable Structure

For a simple project, the initial structure may contain only:

    project/
    ├── AGENTS.md
    ├── README.md
    ├── PROJECT.md
    ├── board/
    ├── src/
    └── tests/

Add additional directories and files only when justified by actual project requirements.

Do not:

- Create unnecessary empty directories.
- Create placeholder implementations.
- Generate speculative packages.
- Introduce infrastructure for hypothetical future requirements.
- Create documentation merely to populate the directory structure.
- Duplicate an existing framework or template repository.

Directories representing unused task states may be created when first needed.

The repository should grow progressively with the product.

---

## 5. Repository Minimalism Policy

Before creating a new tracked file, the agent must determine:

1. Does an existing file already own this responsibility?
2. Can the existing file be extended instead?
3. Is the information durable or temporary?
4. Is the information already represented by source code, tests, documentation, or Git history?
5. Does the file need to be independently maintained?
6. Is Git the appropriate storage location?
7. Will the file improve future maintainability or agent understanding?

Prefer updating existing files over creating redundant files.

A new file must have a distinct responsibility.

Do not create separate documents solely to demonstrate that work has been performed.

Do not create a report, summary, implementation journal, or status document when the information can be recorded concisely in an existing task or authoritative document.

However, do not artificially combine unrelated source modules or remove useful tests merely to reduce file count.

Optimise for:

- Maintainability.
- Clear responsibilities.
- Low cognitive overhead.
- Reproducibility.
- Agent readability.
- Human readability.
- Minimal duplication.

File count alone is not a measure of architectural quality.

---

## 6. Responsibilities of Core Documents

### 6.1 README.md

Audience: humans and developers.

Purpose:

- Explain what the product is.
- Explain why it is useful.
- Describe its main capabilities.
- Provide installation and quick-start instructions.
- Explain how to run and test the project.
- Link to relevant documentation.

The README should focus on the product, not the internal agent management framework.

Do not turn the README into a project management dashboard or historical development journal.

### 6.2 PROJECT.md

Purpose: stable project-level context.

Include:

- Project objective.
- Product vision.
- Scope.
- Non-goals.
- Important constraints.
- High-level architecture.
- Definition of success.
- Current strategic context where necessary.

PROJECT.md describes what is being built and why.

Do not include:

- Individual task statuses.
- Routine test results.
- Detailed implementation journals.
- Duplicated technical specifications.
- Frequently changing milestone progress.

Detailed architecture and requirements should be referenced rather than copied.

### 6.3 ROADMAP.md

Purpose: strategic planning and milestone management.

Include, where relevant:

- Current milestone.
- Upcoming milestones.
- High-level capabilities.
- Major dependencies.
- Important delivery risks.
- Release gates.
- Future directions.

ROADMAP.md should be the authoritative source for milestone-level project progress.

Avoid maintaining separate project plans, timelines, milestone trackers, dependency documents, and release plans unless the project's complexity genuinely requires them.

### 6.4 AGENTS.md

Purpose: concise operating manual for AI agents.

Include:

1. What to read first.
2. How to select work.
3. How to claim tasks.
4. How task dependencies operate.
5. Implementation constraints.
6. Testing expectations.
7. Documentation expectations.
8. Repository minimalism policy.
9. Artifact handling policy.
10. Completion criteria.
11. Blocker handling.
12. Task archival rules.
13. Multi-agent safety rules.

AGENTS.md should primarily act as an entry point and operating policy.

Do not turn it into a complete project encyclopedia.

Prefer concise instructions and references to authoritative documentation.

---

## 7. Canonical Knowledge Ownership

Maintain one authoritative source for each category of information.

| Information | Authoritative location |
|---|---|
| Product introduction and usage | README.md |
| Project objective and constraints | PROJECT.md |
| Milestones and future direction | ROADMAP.md |
| Agent operating rules | AGENTS.md |
| Active tasks and blockers | board/ |
| Completed task outcomes | board/done/ and completion index |
| System architecture | docs/architecture/ |
| Architectural decisions | docs/decisions/ |
| Detailed requirements | docs/specs/ |
| Current implementation | Source code |
| Verified behaviour | Tests and applicable evidence |
| Historical changes | Git history |
| Generated execution evidence | CI or release artifacts |

Agents should reference information instead of duplicating it.

Where information changes, update its authoritative source.

If conflicting documentation is discovered:

- Determine the intended authoritative source.
- Verify the current implementation where applicable.
- Update or remove stale information.
- Record significant unresolved discrepancies.

Do not silently change product requirements merely to make documentation consistent with an incorrect implementation.

---

## 8. File-Based Kanban

Use directory location as the authoritative state of an active task.

    board/
    ├── backlog/
    ├── ready/
    ├── in-progress/
    ├── blocked/
    ├── done/
    └── completed.jsonl

### State Definitions

| State | Meaning |
|---|---|
| backlog | Identified work that is not yet ready |
| ready | Sufficiently scoped work with satisfied prerequisites |
| in-progress | Work currently claimed by an agent or human |
| blocked | Work that cannot proceed safely |
| done | Implemented and validated work awaiting archival |
| archived | Completed work represented by a completion record |

Archived is a lifecycle state, not necessarily another directory.

### State Transitions

    backlog → ready → in-progress → done → archived
                         │
                         └──→ blocked → ready

Task status should not be duplicated in YAML front matter.

A task must have exactly one authoritative active location.

Task IDs must remain stable throughout the lifecycle.

---

## 9. Task Model

Use one Markdown file per active task.

Recommended filename:

    TASK-<number>-<slug>.md

Example:

    TASK-004-fix-api-pagination.md

This supports:

- Independent task ownership.
- Reduced Git merge conflicts.
- Machine-readable dependencies.
- Parallel execution.
- Clear auditability.
- Future automation.

Do not maintain all active tasks inside one large mutable KANBAN.md file.

However, completed tasks do not need to remain as individual Markdown files indefinitely.

### Task Metadata

Keep metadata deliberately small.

    ---
    id: TASK-004
    title: Fix API pagination
    priority: high
    created: YYYY-MM-DD
    depends_on:
      - TASK-002
    labels:
      - api
      - bug
    ---

Recommended priorities:

- critical
- high
- medium
- low

Do not add unnecessary metadata until there is a concrete operational requirement.

Task identifiers must be unique across active tasks, completed tasks, and archived completion records.

Do not reuse historical task IDs.

---

## 10. Task File Template

Each task should contain only the context required to understand, implement, and validate the work.

    ---
    id: TASK-004
    title: Fix API pagination
    priority: high
    created: YYYY-MM-DD
    depends_on:
      - TASK-002
    labels:
      - api
      - bug
    ---

    # Goal

    Describe the expected outcome.

    # Context

    Explain why this work is required.

    Reference relevant source files, specifications,
    architectural decisions, and dependencies.

    # Scope

    Describe the boundaries of the task where necessary.

    # Acceptance Criteria

    - Observable outcome.
    - Required behaviour.
    - Relevant verification requirements.

    # Implementation Notes

    Record significant implementation decisions concisely.

    Do not reproduce the complete development process.

    # Validation

    Record checks actually executed, outcomes,
    and outstanding limitations.

    # Blocker

    Include only when applicable.

Task documents are work orders, not transcripts of agent activity.

Do not include:

- Full terminal output.
- Complete generated test reports.
- Repeated architecture descriptions.
- Unnecessary reasoning transcripts.
- Large code excerpts already available in source files.

---

## 11. Rolling-Wave Planning

Plan work at different levels of granularity according to its execution horizon.

| Planning horizon | Representation |
|---|---|
| Product vision | Objectives |
| Long-term direction | Milestones |
| Future capabilities | Epics or feature descriptions |
| Upcoming milestone | Candidate tasks |
| Current milestone | Detailed tasks |
| Active work | Explicit acceptance criteria |

Do not materialise the entire long-term roadmap into detailed task files during project bootstrap.

Create detailed tasks only when:

- The requirements are sufficiently understood.
- The work is relevant to the current or upcoming milestone.
- Dependencies can be identified.
- Acceptance criteria can be meaningfully defined.

Future ideas may remain concise roadmap entries until they become actionable.

Prefer a small, healthy queue of executable work over a large speculative backlog.

Review and refine upcoming tasks as implementation produces new knowledge.

---

## 12. Agent Work Selection

Agents should normally operate using a pull model.

The repository should support instructions such as:

    Continue working on the project.

An agent should then:

1. Read AGENTS.md.
2. Read PROJECT.md.
3. Inspect ROADMAP.md where available.
4. Inspect board/ready/.
5. Resolve task dependencies.
6. Identify the highest-priority suitable task.
7. Confirm the task is not already claimed.
8. Claim the task.
9. Read task-specific context.
10. Implement the required changes.
11. Validate the implementation.
12. Consolidate durable knowledge.
13. Perform repository hygiene checks.
14. Update task state.

Do not select a task simply because it is easy to complete.

Consider:

- Priority.
- Dependency readiness.
- Current milestone.
- Release blockers.
- Risk.
- Scope.
- Existing work in progress.

If no suitable ready task exists, report the reason or prepare appropriately scoped work from the current roadmap when authorised.

Do not invent new product requirements to keep the agent occupied.

---

## 13. Dependency Handling

Tasks may declare dependencies:

    depends_on:
      - TASK-002
      - TASK-003

A dependency is satisfied only when its task is:

- Validated and present in board/done/, or
- Recorded as successfully completed in the archived completion index.

Before beginning a task:

- Verify every dependency exists.
- Verify task IDs are unique.
- Verify prerequisites are satisfied.
- Detect circular dependencies.
- Detect invalid or missing dependency references.

Do not silently ignore dependency metadata.

A cancelled, abandoned, or superseded task must not automatically satisfy a dependency.

If a prerequisite is replaced by another task, update the dependent tasks explicitly.

At initial implementation, dependency resolution may be performed by inspecting Markdown files and the completion index.

Do not build a complex dependency engine unless the project demonstrates a need for one.

---

## 14. Task Claiming and Multi-Agent Safety

An agent must not claim a task that is already in progress unless explicitly instructed to collaborate.

For a shared local workspace, claim tasks using a non-overwriting move from ready/ to in-progress/.

Verify that the task was successfully claimed before beginning implementation.

A local filesystem move is not sufficient to coordinate agents operating in separate Git branches, worktrees, machines, or cloned repositories.

When concurrent agents operate across isolated workspaces:

- Use a designated shared coordination mechanism.
- Ensure task ownership is visible to participating agents.
- Avoid duplicate task IDs.
- Avoid conflicting changes to shared project state.
- Coordinate updates to the completion index.
- Coordinate milestone archival.
- Do not overwrite another agent's active work.

If no reliable shared coordination mechanism exists, do not assume distributed task claiming is safe. Use explicitly assigned tasks or coordinated sequential execution.

Do not introduce a distributed lock service or orchestration platform by default.

Start with simple coordination and add infrastructure only when justified.

---

## 15. Blocked Tasks

When an agent cannot continue safely:

1. Add or update the Blocker section.
2. Explain the missing information, dependency, permission, or capability.
3. Record what has already been attempted where relevant.
4. Describe the action required to unblock the task.
5. Move the task to board/blocked/.

Example:

    # Blocker

    The external API contract is undefined.

    Required information:

    - Authentication mechanism.
    - Response schema.
    - Rate limits.

    # Unblock Action

    Obtain and validate the external API specification.

Agents must not invent important missing requirements or claim that unexecuted verification has passed.

A partially implemented task is not a completed task.

If implementation is complete but mandatory validation remains outstanding, retain the task in an appropriate non-completed state and record the remaining validation requirement.

---

## 16. Artifact Lifecycle Management

The repository is not an unlimited archive of agent execution.

Classify files by purpose before deciding where they belong.

### Category A: Production Source

Examples:

- Application source code.
- Configuration.
- Reusable scripts.
- Test implementations.
- Required fixtures.
- Dependency lockfiles.

Policy:

Track in Git.

### Category B: Durable Knowledge

Examples:

- Architectural findings.
- Important technical constraints.
- Compatibility limitations.
- Product requirements.
- Significant decisions.

Policy:

Consolidate into authoritative documentation.

### Category C: Execution Evidence

Examples:

- Test execution reports.
- Benchmark measurements.
- Browser verification results.
- Integration test journals.
- Release candidate checksums.
- Certification results.

Policy:

Store in an appropriate CI artifact system, release archive, or versioned evidence store.

Record concise outcomes and evidence references in the relevant task or release record.

Evidence that cannot easily be reproduced must have an explicit retention strategy.

Do not rely exclusively on short-lived CI artifacts for important historical or release certification evidence.

### Category D: Temporary State

Examples:

- Logs.
- Cache files.
- Build output.
- Temporary package tarballs.
- Intermediate reports.
- Scratch files.
- Agent-generated temporary scripts.

Policy:

Ignore in Git and remove when no longer needed.

### Default Artifact Rules

The artifacts/ directory should be ignored by Git by default.

Reusable verification scripts belong in tests/, tools/, or another appropriate source directory.

Experimental source code belongs in experiments/ only when retaining it has a clear purpose.

Generated outputs should not be committed unless explicitly justified.

Exceptions may include:

- Golden test fixtures.
- Required generated source or assets.
- Non-reproducible technical evidence.
- Important release certification records.
- Files required by a documented distribution mechanism.

Every exception should have a clear rationale.

The guiding principle is:

> Commit reproducible methods, not every execution result.

---

## 17. Git Tracking and Ignore Policy

Configure .gitignore during project bootstrap according to the actual technology stack.

A generic starting point may include:

    # Generated artifacts
    /artifacts/
    /reports/
    /test-results/
    /coverage/

    # Temporary files
    *.log
    .cache/
    .tmp/

    # Build output
    dist/
    build/

    # Dependencies
    node_modules/
    .venv/
    .pnpm-store/
    __pycache__/

    # Generated packages
    *.tgz

    # Local configuration and secrets
    .env
    .env.*
    !.env.example

    # Operating system files
    .DS_Store

Adapt these rules to the project.

Do not blindly ignore directories that contain required source code.

Do not ignore dependency lockfiles merely because they are generated.

Do not commit credentials, secrets, or private environment configuration.

Before completing a task, inspect both tracked changes and relevant untracked files.

Do not use broad destructive cleanup commands that could remove user-owned files or another agent's work.

---

## 18. Technical Spikes and Experiments

Technical spikes exist to resolve uncertainty.

A spike should identify:

- The question being investigated.
- The success or evaluation criteria.
- The minimum implementation required.
- The findings.
- The resulting architectural decision or next action.

Keep experimental work appropriately isolated.

    experiments/
    └── <experiment-name>/

After completing a spike, classify its contents:

- Promote successful reusable code into production.
- Promote reusable verification into tests.
- Consolidate technical findings into documentation or an ADR.
- Retain useful reproducible experiments where justified.
- Archive important historical evidence.
- Remove temporary or obsolete experimental outputs.

Do not allow experiments/ to become another unlimited artifact archive.

Avoid maintaining duplicate copies of experimental code after equivalent production implementations exist.

---

## 19. Documentation Structure

Use the following documentation conventions when needed:

    docs/
    ├── architecture/
    ├── decisions/
    └── specs/

Additional documentation categories may be introduced when justified by product complexity.

### Architecture Documentation

Describe long-lived system structure and behaviour.

Examples:

- Data flow.
- Component responsibilities.
- Storage.
- Networking.
- Deployment.
- Security boundaries.

Update existing documents when the architecture evolves.

Do not generate a separate architecture document for every task.

### Specifications

Store detailed product or system requirements when they are too large or important to belong inside individual tasks.

Tasks should reference specifications instead of duplicating them.

Do not maintain obsolete copies of superseded specifications unless required for historical or compatibility purposes.

### Architectural Decision Records

Use ADRs for significant decisions that affect:

- System boundaries.
- Important dependencies.
- Data formats.
- Public interfaces.
- Security guarantees.
- Runtime compatibility.
- Deployment models.
- Long-term maintainability.

Do not create an ADR for every minor implementation choice.

An ADR should contain:

    # ADR-XXX: Decision Title

    Status:
    Date:

    ## Context

    What problem requires a decision?

    ## Decision

    What was selected?

    ## Alternatives

    What meaningful alternatives were considered?

    ## Consequences

    What trade-offs or limitations follow?

    ## References

    Relevant specifications or evidence.

Accepted ADRs provide durable decision history.

When a significant decision changes, preserve its historical meaning and explicitly mark or reference its successor.

Do not rewrite decision history to make the original choice appear different.

---

## 20. Knowledge Consolidation

Every completed task should leave the project with useful, appropriately located knowledge.

Before marking a task complete:

1. Identify newly discovered durable knowledge.
2. Determine its authoritative destination.
3. Update existing documentation where possible.
4. Remove unnecessary duplication introduced by the task.
5. Update stale references.
6. Record important limitations.
7. Preserve relevant verification evidence.

Examples:

| Newly discovered information | Destination |
|---|---|
| Database compatibility limitation | Architecture or compatibility documentation |
| Changed product requirement | Specification |
| Significant design decision | ADR |
| New installation command | README or development guide |
| Test execution output | CI artifact |
| Implementation detail | Source code or concise task notes |
| Future feature idea | Roadmap or backlog |
| Temporary debugging information | Discard after use |

Knowledge consolidation should reduce the amount of context future agents need to read.

Do not preserve information merely because an agent generated it.

---

## 21. Completed Task Archival

Completed tasks may temporarily remain under:

    board/done/

At milestone boundaries, or when completed task accumulation becomes excessive, consolidate eligible completed tasks.

### Completion Index

Use a lightweight machine-readable completion index:

    board/completed.jsonl

Each record should preserve sufficient information for dependency resolution and historical retrieval.

Example:

    {"id":"TASK-001","title":"Initialise project","outcome":"completed","revision":"abc123","path":"board/done/TASK-001-initialise-project.md"}
    {"id":"TASK-002","title":"Implement runtime","outcome":"completed","revision":"def456","path":"board/done/TASK-002-implement-runtime.md"}

The example revisions are placeholders. Actual records must reference real committed revisions containing the complete task document.

Additional metadata may be introduced only when required.

### Archival Procedure

1. Confirm the task satisfies the Definition of Done.
2. Confirm its complete final task document exists in a durable Git revision.
3. Confirm important external evidence has sufficient retention.
4. Record its stable ID, title, outcome, revision, and historical file path.
5. Verify the completion record resolves to the historical task document.
6. Validate all dependent tasks.
7. Remove the redundant active-tree task file.
8. Commit the archival changes.

Perform archival through a coordinated maintenance operation.

Do not allow multiple agents to independently rewrite shared archival indexes without coordination.

Do not archive incomplete tasks as completed.

Do not permanently discard unresolved blockers.

### Historical Retrieval

The archived task should remain recoverable using Git.

For example:

    git show <revision>:<historical-task-path>

A shallow clone may need additional Git history to retrieve historical task documents.

Do not depend on uncommitted files or inaccessible local-only revisions as permanent historical records.

### Reopening Work

If previously completed work needs further changes, normally create a new task that references the original task ID.

Preserve the original completion record as historical evidence.

Do not silently reuse old task IDs.

---

## 22. Agent Execution Lifecycle

The standard execution lifecycle is:

    Discover
       ↓
    Select
       ↓
    Claim
       ↓
    Understand
       ↓
    Implement
       ↓
    Verify
       ↓
    Consolidate
       ↓
    Garbage Collect
       ↓
    Complete

### Discover

Read project context, operating instructions, and relevant task state.

### Select

Choose meaningful, unblocked work.

### Claim

Establish ownership safely.

### Understand

Inspect relevant requirements, source code, tests, and constraints.

### Implement

Make the smallest coherent change that satisfies the task.

### Verify

Execute relevant tests and acceptance checks.

### Consolidate

Integrate durable knowledge into its authoritative location.

### Garbage Collect

Remove unnecessary temporary outputs, duplication, and superseded material introduced by the work.

### Complete

Record validation outcomes and update task state.

Completion means the work is both validated and maintainable.

---

## 23. Extended Definition of Done

A task may move to board/done/ only when all applicable requirements are satisfied.

### Functional Completion

- Acceptance criteria are satisfied.
- Required functionality is implemented.
- Important edge cases are addressed.
- Relevant compatibility requirements are preserved.

### Verification

- Relevant tests have been executed.
- Required checks pass.
- Validation outcomes are recorded.
- Known limitations are explicitly documented.
- No mandatory validation remains outstanding.

### Documentation

- Required documentation is current.
- Significant architectural decisions are captured.
- Durable knowledge is consolidated.
- Unnecessary documentation duplication is avoided.

### Repository Hygiene

- Temporary files are removed where safe.
- Generated artifacts are not accidentally tracked.
- Obsolete files introduced or superseded by the task are handled.
- Unnecessary placeholder files are removed.
- Relevant documentation links remain valid.
- Task dependencies remain valid.
- The Git diff has been reviewed for unrelated changes.

### Scope and Safety

- No unrelated features were introduced.
- No unnecessary infrastructure was added.
- No other agent's active work was overwritten.
- No required historical evidence was discarded.

Do not claim successful completion based solely on implementation progress.

A task is complete when its intended outcome has been validated and the repository remains in a maintainable state.

---

## 24. Repository Garbage Collection

Repository maintenance is an explicit part of the engineering lifecycle.

### Task-Level Maintenance

Before completing each task:

- Remove temporary files generated by the task.
- Consolidate relevant documentation.
- Remove unnecessary duplicated implementation introduced by the task.
- Inspect generated outputs.
- Review the Git diff.

Keep maintenance scoped to the current work.

Do not perform broad unrelated cleanup during a feature task.

### Milestone-Level Maintenance

At milestone boundaries:

- Archive eligible completed tasks.
- Review stale documentation.
- Consolidate overlapping project documents.
- Review retained experiments.
- Remove unnecessary generated outputs.
- Review unused dependencies.
- Review package and module boundaries.
- Verify important evidence remains accessible.
- Verify task dependency integrity.
- Update ROADMAP.md.
- Review unresolved blockers and release gates.

### Safety Rules

Repository garbage collection does not mean indiscriminate deletion.

Before deleting or archiving files:

- Check whether they are required by builds.
- Check whether they are required by tests.
- Check whether documentation references them.
- Check whether they contain non-reproducible evidence.
- Check whether other agents are using them.
- Verify historical recovery where required.

Do not rewrite Git history or permanently discard important evidence without explicit approval.

---

## 25. Architecture Minimalism

Distinguish between:

- Logical module boundaries.
- Physical directory boundaries.
- Package boundaries.
- Deployment boundaries.
- Public API boundaries.

These boundaries do not always need to correspond one-to-one.

Create a separate package only when justified by:

- Dependency isolation.
- Runtime compatibility.
- Independent distribution.
- Independent reuse.
- Meaningful build boundaries.
- Clear ownership or lifecycle requirements.

Prefer logical modules within an existing package when a separate package provides no meaningful benefit.

Do not introduce:

- Microservices without operational need.
- Databases for simple local project state.
- Frameworks for trivial functionality.
- Abstract factories without concrete implementations.
- Empty packages for hypothetical future capabilities.
- Complex event systems without clear requirements.
- Custom infrastructure that duplicates existing tools.

Preserve modularity where it improves correctness, testing, or maintainability.

The objective is appropriate architecture, not minimum architecture at any cost.

---

## 26. Git and Pull Request Discipline

Git is the historical audit layer.

Use Git to preserve meaningful changes rather than maintaining duplicate historical files.

### Commits

Prefer coherent commits that represent understandable units of work.

Commit messages should communicate the purpose of the change.

Reference task IDs where useful.

### Pull Requests

A pull request should be focused and independently reviewable.

Where practical:

- Separate unrelated features.
- Separate broad architectural refactoring from functional changes.
- Separate repository cleanup from substantial feature implementation.
- Avoid including historical artifacts unrelated to the change.
- Avoid including generated outputs unnecessarily.
- Explain important architectural decisions.
- Summarise checks actually executed.
- Record outstanding limitations.

Large initial imports may be unavoidable, but should still distinguish production assets from historical development artifacts.

Do not introduce arbitrary file-count limits that compromise source modularity or testing.

### Safety

Do not automatically:

- Push to remote repositories without authorisation.
- Merge pull requests without authorisation.
- Publish packages.
- Deploy to production.
- Rewrite shared Git history.
- Delete branches or tags containing required historical evidence.

Follow the user's explicit authorisation and the repository's established release policies.

---

## 27. Nested AGENTS.md

Support nested operating instructions when the repository genuinely requires domain-specific guidance.

Example:

    AGENTS.md
    backend/
      AGENTS.md
    frontend/
      AGENTS.md
    infra/
      AGENTS.md

The root AGENTS.md defines global operating rules.

Nested AGENTS.md files define additional local conventions.

Examples:

- Language-specific coding conventions.
- Domain-specific test commands.
- Database migration requirements.
- Runtime constraints.
- Deployment restrictions.

Do not duplicate global operating instructions into every directory.

Do not create nested AGENTS.md files when the root instructions are sufficient.

---

## 28. Future Tooling

Keep the filesystem representation simple enough to support future automation.

Potential commands:

    project init
    project status
    project next
    project add-task
    project start TASK-012
    project block TASK-012
    project done TASK-012
    project validate
    project archive
    project gc

Potential tooling capabilities:

- Markdown task parsing.
- Dependency graph validation.
- Duplicate task ID detection.
- Task selection.
- Task claiming.
- Board visualisation.
- Completion index management.
- Documentation link validation.
- Artifact classification.
- Repository hygiene checks.
- Milestone maintenance.

A future graphical interface should remain a projection of authoritative project state rather than becoming a competing datastore.

Do not implement these tools during initial bootstrap unless explicitly requested or necessary for the project's actual operation.

Prefer conventions over custom infrastructure.

---

## 29. Initial Bootstrap Procedure

When applying this handoff to a new or existing product repository, perform the following steps.

### Step 1: Inspect

Inspect:

- Existing repository structure.
- Product requirements.
- Source code.
- Existing documentation.
- Technology stack.
- Existing tests.
- Git configuration.
- Current project state.

Preserve useful existing work.

### Step 2: Establish Project Intent

Create or update PROJECT.md with the actual project objectives, scope, constraints, and definition of success.

Do not invent missing requirements.

### Step 3: Establish Operating Policy

Create or update a concise AGENTS.md containing the essential execution and maintenance rules.

Do not copy the entire handoff into AGENTS.md.

Translate the handoff into practical operating instructions appropriate to the project.

### Step 4: Establish Product Documentation

Create or improve README.md.

Add additional technical documentation only where required.

### Step 5: Establish Roadmap

Create ROADMAP.md when milestone-level planning is useful.

Represent future work at an appropriate level of abstraction.

Do not immediately expand all milestones into detailed tasks.

### Step 6: Prepare Near-Term Work

Create a small initial set of actionable tasks based on the current milestone.

Ensure:

- Unique task IDs.
- Explicit dependencies.
- Clear acceptance criteria.
- Appropriate scope.
- Correct board state.

Do not create artificial example tasks in a real product repository.

### Step 7: Establish Repository Hygiene

Configure appropriate .gitignore rules.

Define artifact retention conventions.

Avoid tracking generated execution outputs.

Identify existing files that may require later consolidation or archival.

### Step 8: Validate

Verify:

- Project instructions are internally consistent.
- Task IDs are unique.
- Dependencies are valid.
- Documentation references resolve.
- Existing project functionality is preserved.
- The repository contains no unnecessary scaffolding.

### Step 9: Deliver

Provide a concise summary of:

- Repository structure.
- Created and updated files.
- Current project milestone.
- Initial actionable tasks.
- Important assumptions.
- Outstanding blockers.
- Relevant verification results.
- Recommended next action.

Avoid creating an additional tracked delivery report unless the project genuinely requires one.

---

## 30. Bootstrap Deliverables

The initial implementation should deliver:

1. A project-appropriate repository structure.
2. A concise AGENTS.md.
3. A PROJECT.md containing real project context.
4. A product-focused README.md.
5. A ROADMAP.md where milestone planning is useful.
6. A minimal file-based task board.
7. A small set of real, actionable near-term tasks.
8. Appropriate Git ignore rules.
9. Necessary architectural documentation.
10. A clear artifact and archival policy.

Task and ADR templates may be embedded concisely in operating documentation or maintained as standalone files when repeatedly needed.

Do not include a complete reusable starter framework inside the product repository.

Do not create synthetic example tasks, duplicate project templates, or empty documentation trees solely to satisfy this deliverable list.

---

## 31. Acceptance Criteria

The project bootstrap is successful when:

- A human can understand the project from its README and project context.
- An agent without previous conversation history can determine what to do next.
- Active work and blockers are discoverable.
- Task dependencies are explicit and valid.
- Relevant project knowledge has clear authoritative locations.
- Generated artifacts are excluded from Git by default.
- Completed tasks have a defined archival lifecycle.
- Important historical evidence remains recoverable.
- The repository does not contain unnecessary framework scaffolding.
- Existing application functionality and tests are preserved.
- The project can evolve without requiring an external orchestration service.
- The operating workflow remains understandable and portable across agent systems.

The initial implementation should demonstrate that Git, the filesystem, and Markdown are sufficient to provide a sustainable project control plane.

---

## 32. Non-Goals

Do not build the following unless explicitly required:

- A dedicated project management database.
- A backend service for Kanban state.
- A distributed orchestration engine.
- A custom agent runtime.
- A complex scheduler.
- A mandatory graphical interface.
- A distributed locking service.
- An unnecessary CLI framework.
- A vector database for project documentation.
- A duplicated knowledge management system.
- A reusable framework embedded within every product repository.

Do not over-engineer the management environment at the expense of delivering the product.

The engineering workflow exists to support the product, not to become the product.

---

## 33. Design Principles

Optimise for:

- Agent readability.
- Human readability.
- Filesystem-native operation.
- Git friendliness.
- Explicit dependencies.
- Low merge-conflict probability.
- Portability across agent systems.
- Minimal hidden state.
- Reproducibility.
- Progressive complexity.
- Single source of truth.
- Knowledge consolidation.
- Sustainable repository growth.
- Safe concurrency.
- Long-term extensibility.

Prefer simple, inspectable conventions over complex infrastructure.

Introduce new mechanisms only when an observed problem justifies their cost.

---

## 34. Final Operating Principle

The repository must serve as a useful persistent memory system rather than an unlimited record of everything an agent has done.

Persist what future agents need to reason correctly.

Preserve what humans need to audit important decisions.

Track what is required to build and reproduce the product.

Consolidate knowledge into authoritative locations.

Archive completed work when it is no longer part of active execution state.

Discard temporary information when it can be safely regenerated.

Every task should improve the product without unnecessarily increasing the complexity of the environment that maintains it.

The system must remain useful as the project grows from ten tasks to one thousand tasks.

Build the product.

Maintain the knowledge.

Manage the lifecycle.

Keep the repository lean.