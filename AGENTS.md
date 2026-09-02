# AGENTS.md

## EvalOps Agent Operating Guidelines

This file is part of the minimum persistent engineering-control substrate for autonomous checkpoint execution.

Agents MUST read and follow these guidelines when operating in the EvalOps repository.

### 1. Project Constitution
The project is bound by the rules in `Foundation-EvalOps/evalops-project-constitution.md`. Do not make architectural or technological decisions without explicitly checking if they have been approved.

### 2. Checkpoint System
All engineering work MUST follow the checkpoint system described in `Foundation-EvalOps/evalops-checkpoint-and-engineering-workunit.md`.

* Do NOT invent or redefine checkpoints.
* Work ONLY on the currently permitted checkpoint.
* Do NOT implement features outside the explicit scope of the current checkpoint.
* A checkpoint is only complete when all acceptance criteria are met, validated, and recorded.

### 3. State Management
Agents MUST read and update the state documents in `docs/project-state/` before and after their work, as defined in `Foundation-EvalOps/evalops-project-state-and-handoff-protocol.md`.

* `PROJECT_STATE.md`: The authoritative current state.
* `CHECKPOINT_LOG.md`: The history of accepted checkpoints.
* `DECISION_LOG.md`: The history of engineering decisions.
* `VALIDATION_RECORD.md`: Evidence of successful validation.
* `HANDOFF_RECORD.md`: Briefing for the next agent.

### 4. Autonomy Boundaries
Agents operate under the boundaries defined in `Foundation-EvalOps/evalops-ai-engineering-contract.md`.

* Stop and request user input if architectural decisions are needed.
* Do not bypass quality or validation gates.
* Treat human approval gates as hard blockers.

### 5. Git Workflow
Follow the GitHub Engineering Workflow in `Foundation-EvalOps/evalops-github-engineering-workflow.md`.

* Never work directly on `main`.
* Use `cp/<checkpoint-id>-<short-name>` branches.
* Commits should be meaningful.
* PRs require review and acceptance before merging.

### 6. Jules Invocation Protocol
When initializing, a new instance of Jules should consult `docs/autonomy/JULES_INVOCATION.md` to reconstruct the current project state from the substrate, and then proceed to `HANDOFF_RECORD.md` to find its next permitted action.
