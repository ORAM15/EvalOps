# EvalOps Contribution & Development Conventions

## Development Contract
All development on EvalOps strictly follows the guidelines in `Foundation-EvalOps/evalops-ai-engineering-contract.md` and `Foundation-EvalOps/evalops-github-engineering-workflow.md`.

* Do NOT bypass human approval gates.
* Do NOT implement features outside the scope of the current permitted checkpoint.
* Read the `AGENTS.md` and `JULES_INVOCATION.md` before making any changes.

## Engineering Workflow
* All work must be attached to an explicit checkpoint branch (`cp/<checkpoint-id>-<short-name>`).
* Commits should represent meaningful engineering units.
* Pull requests (PRs) must be used. Direct commits to `main` are not allowed.
* A checkpoint is only completed when its specific acceptance criteria are validated and recorded.

## Quality Standards
* Evidence of runtime behavior is required before acceptance.
* Real evaluation results are preferred over fabricated metrics.
* Meaningful metrics are preferred over mere dashboard decoration.

Please review the full project constitution in `Foundation-EvalOps/` for detailed rules.
