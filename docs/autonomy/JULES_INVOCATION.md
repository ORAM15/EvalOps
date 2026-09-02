# JULES_INVOCATION.md

## Agent Initialization Protocol

Whenever Jules (or any AI agent) is freshly initialized to work on EvalOps, it must execute the following "Fresh AI Resume Sequence":

### Step 0: DO NOT CODE
Do not make any modifications until the state is fully reconstructed.

### Step 1: Verify Repository & Git State
* Confirm you are in `ORAM15/EvalOps`.
* Inspect `git status`, `git branch`, and `git log`. Find out if the working tree is clean and what branch you are on.

### Step 2: Reconstruct Project State
* Read `docs/project-state/PROJECT_STATE.md`.
* Read `docs/project-state/HANDOFF_RECORD.md`.
* Identify the exact checkpoint you are permitted to work on.

### Step 3: Verify Checkpoint Details
* Read `Foundation-EvalOps/evalops-checkpoint-and-engineering-workunit.md` or the relevant checkpoint issue.
* Cross-check `CHECKPOINT_LOG.md` to ensure prerequisites are actually completed.

### Step 4: Check Open Decisions
* Read `docs/project-state/DECISION_LOG.md`. Ensure your checkpoint is not blocked by a `DEC-OPEN-*` decision.

### Step 5: Action & Bounded Work
* Once the state is verified, state your plan (e.g., "I am working on Px-Cy. I am allowed to do Z. My next permitted action is A.")
* Implement within the strict bounds of the checkpoint.
* Perform validation and update `VALIDATION_RECORD.md`.

For the complete protocol, refer to `Foundation-EvalOps/evalops-project-state-and-handoff-protocol.md`.
