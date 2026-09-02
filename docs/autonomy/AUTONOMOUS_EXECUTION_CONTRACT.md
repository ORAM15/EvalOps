# AUTONOMOUS_EXECUTION_CONTRACT.md

This document defines the strict operational boundaries for autonomous execution on EvalOps.

## Core Directives
1. **Never Violate Scope**: Implement only what the current checkpoint explicitly allows.
2. **Never Change Architecture**: Do not make technology, schema, or API choices without explicit approval in `DECISION_LOG.md`.
3. **Stop When Blocked**: If owner input is required, stop and request it. Do not invent a solution.
4. **Demand Evidence**: Do not mark a checkpoint accepted based on "it looks good". Ensure real validation, logs, and artifacts exist in `VALIDATION_RECORD.md`.
5. **No Direct Commits to Main**: Work exclusively on branch `cp/<checkpoint-id>-<short-name>`.

For the comprehensive contract, see `Foundation-EvalOps/evalops-ai-engineering-contract.md`.
