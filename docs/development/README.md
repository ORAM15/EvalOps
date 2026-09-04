# Development Documentation

Welcome to the EvalOps repository. This folder contains documentation for developers and AI agents working on the project.

## Checkpoint Operating Model

All engineering work in this repository strictly follows the **Checkpoint System**.

- **What is a checkpoint?** A checkpoint is a verification boundary, not a coding milestone. It defines the exact scope of work permitted and the exact acceptance criteria required.
- **Workflow:**
  1. A GitHub Issue defines the checkpoint.
  2. Create a branch (`cp/<checkpoint-id>-<short-name>`).
  3. Implement the **bounded** work.
  4. Test and validate.
  5. Produce evidence of completion.
  6. Create a Pull Request (PR) and link the issue.
  7. Upon human/automated acceptance, the PR is merged, and the next checkpoint is unlocked.
- **Rule of Thumb:** Code existing is not completion. A PR is not completion. Only demonstrated, validated functionality that meets all acceptance criteria is completion.

For full details on the checkpoint lifecycle, refer to the authoritative document: `Foundation-EvalOps/evalops-checkpoint-and-engineering-workunit.md`.

## Engineering Conventions

- **Git Workflow:** Do not work directly on `main`. Use PRs for all feature development. See `Foundation-EvalOps/evalops-github-engineering-workflow.md` for full branch, commit, and PR strategies.
- **Project Autonomy Boundaries:** Agents operate strictly under the rules defined in `Foundation-EvalOps/evalops-ai-engineering-contract.md`. If architectural decisions are required, **stop** and request human input.

## Authoritative Documents

Do **not** modify the foundation documents. They dictate the rules of the project.

- **Constitution & Rules:** `Foundation-EvalOps/evalops-project-constitution.md`
- **Architecture:** `Foundation-EvalOps/evalops-architecture-specification.md`
- **Work Units:** `Foundation-EvalOps/evalops-checkpoint-and-engineering-workunit.md`
- **Git Workflow:** `Foundation-EvalOps/evalops-github-engineering-workflow.md`
- **Project State:** `docs/project-state/` (Update these logs as you start and finish your work!)

Please check the `docs/project-state/PROJECT_STATE.md` to see the current active checkpoint before starting any new work.
