# DevFlow

> **A markdown-first workflow for AI-assisted software development.**

DevFlow is an open-source workflow that transforms user intent into executable software tasks through structured Markdown artifacts.

Instead of relying on long chat histories, DevFlow separates planning, execution, runtime state, durable memory, and project context into independent documents. This allows AI assistants to pause, resume, switch models, collaborate across tools, or even rebuild an entire project workflow while preserving continuity.

---

# Why DevFlow?

Long-running AI-assisted development commonly encounters four practical problems.

## 1. Long Agent Context Growth

As sessions grow longer, important information becomes compressed, dropped, or overshadowed by newer conversation turns.

## 2. Interruption And Resume Gaps

When an agent session stops unexpectedly, resuming work often depends on reconstructing intent from incomplete chat history.

## 3. Cross-Model Communication Gaps

Different models and tools do not naturally share the same working memory, assumptions, or operating style, which makes handoff and collaboration fragile.

## 4. PoC Evolution Drift

During PoC work, features are frequently added, changed, removed, or refactored. When the project later moves toward MVP, new technology-stack constraints and non-functional requirements often appear, and the original development context becomes difficult to reconstruct.

DevFlow addresses these problems by making every development iteration explicit, persistent, resumable, and model-readable.

---

# Core Workflow

Every development iteration follows the same workflow.

```text
Intent　->　Interview　->　Objective　->　Task　->　Execution　->　Evidence　->　Status
```

The workflow begins when the user expresses an intent through a prompt, a request, or a pre-written intent file. Each artifact has a single responsibility and becomes part of the project's persistent state.

---

# Workflow Artifacts

## Intent

Intent is a record of the original purpose behind a development iteration.

It is not a workflow step. It may be written in advance by the user, or created automatically when the workflow begins.

Its role is to preserve the original motivation so that future AI sessions can understand **why** something was built, not just what was built.

## Interview

Interview collects the information required to complete the current intent.

**Interview is the entry point of every workflow iteration.**

The user expresses a need through a prompt or a pre-written intent file, and the AI asks clarifying questions to reduce uncertainty before planning begins.

Its purpose is to reduce uncertainty rather than define implementation details.

## Objective

Objective transforms the clarified Intent into an executable development objective.

Each Objective defines:

- Scope
- Constraints
- Deliverables
- Success Criteria

Each development iteration owns its own Objective.

## Task

Each Objective is decomposed into multiple executable Tasks.

Every Task should:

- Have one responsibility
- Have explicit boundaries
- Have acceptance criteria
- Be executable by a fresh AI session

## Evidence

Evidence records what has actually been completed.

Typical evidence includes:

- Implementation summary
- Validation results
- Test results
- Important observations

## Status

Status represents the runtime state of the workflow.

It tracks:

- Current Intent
- Current Objective
- Current Task
- Current Role
- Progress
- Resume Point

Status is the single source of truth for continuing development.

At repository bootstrap, `status.md` should begin as a clean initialization template rather than a pre-filled project history.

## Memory

Memory preserves durable project knowledge that should survive across iterations, interruptions, and model handoffs.

Unlike `status.md`, which represents current runtime state, `memory.md` stores medium-term or long-term working knowledge that remains useful across multiple objectives.

Typical memory content includes:

- durable product decisions
- stable domain assumptions
- recurring implementation constraints
- known tradeoffs
- handoff notes useful across models or tools

Memory is not a substitute for context files, and it is not a task log. It exists to reduce re-discovery cost across sessions.

---

# Project Context

Execution should not depend on historical conversations.

Instead, DevFlow separates project context into independent documents that can be loaded when required.

Typical project context includes:

- Architecture
- Technology Stack
- Dependencies
- Coding Conventions
- Repository Structure

Project Context is independent from Intent, Objective, and Tasks, allowing projects to be rebuilt, refactored, migrated, or handed across models without replaying previous conversations.

Context files should be created as empty template files during initialization.

---

# Design Principles

## Markdown First

Everything is stored as plain Markdown.

No proprietary platform or database is required.

## Separation of Concerns

Planning, execution, runtime state, durable memory, and project context are independent.

Each artifact has a single responsibility.

## Persistent Project State

Project knowledge belongs in files rather than chat history.

Every important artifact is versionable, reviewable, and resumable.

## Objective-Driven Execution

AI executes one Task at a time.

Each Task has explicit boundaries and acceptance criteria.

## Resume Instead Of Restart

Development resumes from project state rather than previous conversations.

AI should reconstruct context from project artifacts instead of chat history.

## Model-Readable Collaboration

Artifacts should be readable and usable across different models, tools, and sessions without relying on hidden memory.

---

# Repository Structure

The following structure is part of the official DevFlow framework.

```text
.devflow/
    # Documentation Records
    intent/
    interview/
    objective/
    tasks/
    evidence/

    # Support And Configuration
    roles/
    skills/
    templates/
    context/
        architecture.md
        stack.md
        dependencies.md
        conventions.md
        repo-structure.md

    # Execution Control
    memory.md
    status.md
```

Repository-level bootstrap control files:

```text
AGENTS.md
CLAUDE.md
```

Notes:

- `context/` files should be created as empty template files during initialization.
- `roles/`, `skills/`, and `templates/` are official DevFlow structure.
- `memory.md` and `status.md` are execution control artifacts.
- `AGENTS.md` and `CLAUDE.md` are repository-level bootstrap control files.
- `status.md` should start from a clean bootstrap state.

---

# Artifact Naming Rules

DevFlow should use stable, sortable, ASCII-safe filenames for workflow artifacts.

General rules:

- use lowercase ASCII only
- use `.md` as the file extension
- use hyphen-separated slugs
- keep IDs stable once assigned
- keep numeric prefixes zero-padded when ordering matters

Recommended naming conventions:

- Intent: `iNN-<slug>.md`
- Interview: `iNN-<slug>.md` or another documented matching scheme
- Objective: `oNN-<slug>.md`
- Task: `tNN-<slug>.md`
- Evidence: `oNN-eNN-<slug>.md`

Recommended paths:

```text
.devflow/intent/i01-user-onboarding-flow.md
.devflow/interview/i01-user-onboarding-flow.md
.devflow/objective/o01-onboarding-mvp.md
.devflow/tasks/o01/t01-create-auth-layout.md
.devflow/evidence/o01-e01-auth-layout.md
```

Encoding principles:

- IDs should be short, stable, and machine-readable
- slugs should describe the artifact in plain English
- filenames should avoid spaces, non-ASCII punctuation, and ambiguous abbreviations
- task files should remain uniquely identifiable even when viewed outside their parent folder

---

# Cold Start

When an AI assistant starts working on a project, it should load information in the following order.

1. `AGENTS.md`
2. `CLAUDE.md` (optional)
3. `.devflow/status.md`
4. `.devflow/memory.md`
5. Current Intent
6. Current Objective
7. Current Task
8. Project Context (as required)
9. Related Evidence (if required)

Execution should depend on the current project state, not historical conversations.

---

# Scope

This initial version of DevFlow focuses on:

- Personal projects
- Open-source projects
- Proof of Concept (PoC)
- Small AI-assisted software projects

It intentionally excludes:

- Enterprise governance
- Compliance workflows
- Large-scale project coordination
- Full multi-agent orchestration frameworks
- Contract-based development workflows

These areas are intentionally left outside the scope of this repository.

---

# Goals

DevFlow aims to make AI-assisted software development:

- Resumable
- Traceable
- Predictable
- Tool-independent
- Model-independent
- Easy to reconstruct

through structured Markdown artifacts and explicit workflow separation.

---

# License

This project is open source.

See the `LICENSE` file for licensing information.
