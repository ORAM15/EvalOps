FINAL GITHUB ENGINEERING WORKFLOW — EvalOps

Project: EvalOps
Document: GitHub Engineering Workflow
Version: 1.0
Status: FINAL
Authority: Project Constitution → Requirements → Final Architecture → Technology Specification → Master Phase Plan → Checkpoint System → AI Engineering Contract → Quality & Evaluation System

This document defines how EvalOps is developed, reviewed, validated, merged, released, and tracked through GitHub.

The governing principle is:

GitHub records engineering progress; it does not manufacture engineering progress.

A green contribution graph, commit count, PR count, or CI badge is never a substitute for demonstrated functionality.

1. Repository Structure

The repository is the authoritative engineering home of EvalOps.

The structure should remain aligned with the approved architecture rather than becoming a dumping ground for unrelated experiments.

The approved high-level organization is:

EvalOps/
│
├── backend/
│   ├── app/
│   └── tests/
│
├── frontend/
│
├── tests/
│   ├── integration/
│   └── e2e/
│
├── docs/
│   ├── architecture/
│   ├── evaluation/
│   ├── decisions/
│   ├── security/
│   └── development/
│
├── infrastructure/
│
├── .github/
│   └── workflows/
│
├── README.md
├── .gitignore
└── project configuration files

The exact internal organization may evolve inside the approved architecture.

The repository must not accumulate:

generated build artifacts;
secrets;
temporary experiments;
unrelated projects;
abandoned implementations presented as active;
duplicate application implementations.
2. Repository Authority

The repository contains several kinds of information.

Project authority
      │
      ├── Requirements
      ├── Architecture
      ├── Technology specification
      ├── Phase plan
      ├── Checkpoints
      └── Decision records

Implementation must remain traceable to these sources.

Where documentation conflicts with implementation, the discrepancy must be surfaced rather than silently hidden.

3. Branch Strategy

main is the protected integration branch.

No autonomous agent should perform ordinary checkpoint development directly on main.

The normal flow is:

main
 │
 └── checkpoint branch
          │
          ├── implementation
          ├── tests
          └── validation
                  │
                  ▼
                 PR
                  │
                  ▼
                Review
                  │
                  ▼
                Merge
                  │
                  ▼
                main
4. Branch Naming

Checkpoint branches use:

cp/<checkpoint-id>-<short-description>

Examples:

cp/P0-C2-local-runtime
cp/P1-C4-evaluation-runs
cp/P2-C2-trace-capture
cp/P3-C5-regression-detection
cp/P5-C3-regression-gate

Bug fixes that are not part of an active checkpoint may use:

fix/<short-description>

Documentation-only changes:

docs/<short-description>

Infrastructure changes:

infra/<short-description>

However, specialized branches must not become a way to bypass checkpoint boundaries.

5. One Branch = One Bounded Change

A branch should have one coherent purpose.

Bad:

P1 evaluation
+
authentication
+
dashboard redesign
+
Docker refactor
+
new database

Good:

P1-C4 — Evaluation Run Execution

If implementation discovers unrelated work:

Stop, record it, and create or identify the appropriate future work unit.

6. Issue System

Every checkpoint has one authoritative GitHub Issue.

Naming:

[CP] P1-C4 — Evaluation Run Execution

The Issue must contain:

checkpoint ID;
phase;
objective;
prerequisites;
allowed work;
out-of-scope work;
expected artifacts;
validation method;
acceptance criteria;
failure conditions;
dependencies;
human approval requirement;
completion evidence;
next checkpoint.

The Issue is the execution contract for that checkpoint.

7. Checkpoint-to-Issue Mapping

The mapping is one-to-one:

Checkpoint
    ↕
GitHub Issue

For example:

P1-C4
  ↕
GitHub Issue #XX

A checkpoint must not be represented by a vague issue such as:

“Build evaluation system.”

That is too broad to safely delegate to an autonomous agent.

8. Issue Lifecycle
PLANNED
   ↓
READY
   ↓
IN PROGRESS
   ↓
VALIDATING
   ├── BLOCKED
   ├── FAILED
   └── ACCEPTED

Only ACCEPTED unlocks dependent checkpoints.

Closing an Issue without satisfying acceptance criteria is prohibited.

9. Commit Expectations

Commits must represent meaningful engineering changes.

A commit should explain what changed and why.

Examples:

feat(eval): implement evaluation run lifecycle
test(eval): validate run state transitions
fix(worker): prevent duplicate result persistence
docs(eval): document scorer contract

Commit messages should not be used to manufacture activity.

Avoid:

update
changes
final
fix
test
work
progress

unless the repository convention makes the context genuinely obvious.

10. Commit Granularity

There is no required number of commits.

A checkpoint may have:

1 meaningful commit

or:

5 meaningful commits

depending on the work.

The project does not optimize for commit frequency.

The metric is:

Accepted engineering capability.

11. No Green-Dot Farming

The following behavior is prohibited:

Tiny change
 ↓
Commit
 ↓
Tiny change
 ↓
Commit
 ↓
Tiny change
 ↓
Commit

solely to increase GitHub activity.

A commit must exist because an actual engineering change occurred.

12. Pull Request Structure

Each checkpoint normally produces one PR.

Recommended title:

[CP] P1-C4 — Evaluation Run Execution

The PR body should contain:

## Checkpoint

P1-C4

## Objective

<what this PR proves>

## Implemented

- ...
- ...

## Not Implemented

- ...

## Architecture Impact

None / explicitly approved change

## Tests

<tests actually executed>

## Validation

<actual runtime/integration evidence>

## Acceptance Criteria

- [x] ...
- [x] ...
- [ ] ...

## Evidence

<logs/screenshots/output/results>

## Known Limitations

...

## Failures / Remaining Issues

...

## Related Issue

Closes #XX
13. PR Requirements

A PR is mergeable only when:

Scope

The PR corresponds to a defined checkpoint or explicitly authorized work unit.

Implementation

The intended functionality actually exists.

Tests

Relevant tests have actually been executed.

Validation

The checkpoint's validation method has been executed.

Evidence

The PR contains evidence sufficient to verify the acceptance criteria.

Architecture

No unauthorized architectural changes exist.

Security

No secrets or sensitive data have been introduced.

Documentation

Required documentation changes are included.

Honesty

Failures and limitations are disclosed.

14. Required Evidence Before Merge

This is the most important GitHub rule.

A PR is not mergeable merely because:

✓ CI green
✓ build successful
✓ tests passed

The PR must contain appropriate evidence.

At minimum:

Scope evidence
+
Test evidence
+
Behavior evidence
+
Acceptance evidence

Depending on the checkpoint, this may include:

test output;
API responses;
runtime logs;
screenshots;
evaluation results;
trace IDs;
real dataset execution;
comparison results;
regression evidence;
deployment URL;
security test results.
15. Evidence Must Be Real

Evidence must originate from actual execution.

For example:

Accuracy: 91.4%

is acceptable only if the PR can establish:

dataset
 ↓
actual run
 ↓
actual outputs
 ↓
actual scorer
 ↓
actual calculation

A manually typed number is not evidence.

16. Tests Before PR

Before opening a PR, the AI must run the relevant validation available for the checkpoint.

At minimum:

Code-level changes

Relevant unit tests.

Component interaction

Relevant integration tests.

User workflow

Relevant E2E validation.

Evaluation changes

Real evaluation cases.

Infrastructure changes

Actual startup/deployment validation.

Security changes

Relevant security tests.

The checkpoint itself determines the exact required evidence.

17. Review Requirements

Every PR must be reviewed according to its risk.

Review must examine:

1. Scope
2. Correctness
3. Architecture
4. Tests
5. Evidence
6. Security
7. Maintainability
8. Documentation
9. Regression risk

For evaluation-related changes, additionally:

10. Evaluation validity
11. Metric interpretation
12. Judge behavior
13. Data integrity
18. Human Review

Human review is mandatory when the change affects:

architecture;
API contracts;
database contracts;
evaluation methodology;
LLM judge methodology;
security boundaries;
authentication;
production deployment;
phase acceptance;
final project acceptance.

The AI may prepare the change but cannot grant itself owner-level approval.

19. Merge Requirements

A PR may merge only when:

Checkpoint scope satisfied
        +
Required tests passed
        +
Required validation passed
        +
Evidence supplied
        +
Review completed
        +
No unresolved blocking issue
        +
Required human approval obtained

Then:

MERGE
20. Merge Does Not Equal Checkpoint Completion

This distinction is mandatory.

PR merged
    ↓
Implementation integrated

does not necessarily mean:

Checkpoint accepted

The checkpoint becomes ACCEPTED only after its acceptance criteria have been demonstrated.

21. Phase Completion

A phase completes only when its phase acceptance checkpoint passes.

Example:

P1-C1 ✓
P1-C2 ✓
P1-C3 ✓
P1-C4 ✓
P1-C5 ✓
P1-C6 ✓
       ↓
P1 GATE PASS

The next phase remains locked until then.

22. Failed CI

A failed CI run is information.

The AI must:

inspect the failure;
identify whether it is related to the change;
diagnose;
modify only within scope;
rerun validation.

Repeatedly pushing speculative fixes without understanding the failure is prohibited.

23. Repeated Failure Rule

If the same class of failure occurs repeatedly:

Attempt 1
 ↓
Failure
 ↓
Diagnosis
 ↓
Attempt 2
 ↓
Failure

the AI must not continue blindly.

It must stop and determine:

what was learned;
whether the hypothesis was wrong;
whether the environment is responsible;
whether the architecture is implicated;
whether owner input is required.

This prevents:

“try random fix → commit → CI → random fix → commit.”

24. Huge Uncontrolled Changes

If a PR becomes substantially larger than the checkpoint requires, the AI must stop and reassess.

Warning signs:

unrelated files;
unrelated features;
large refactor;
architecture changes;
multiple checkpoint objectives;
unexplained dependency additions.

The correct response is to split the work rather than hide scope inside one PR.

25. Mixing Unrelated Work

Unrelated changes must not be bundled into a checkpoint PR merely because the agent happens to encounter them.

Example:

While implementing P2-C2, the agent notices a UI spacing issue.

It should record:

Future work:
UI improvement

not modify the dashboard in the same PR.

26. Rollback Strategy

Rollback is determined by the type of failure.

Unmerged branch

Discard/rework the branch if necessary.

No impact on main.

Merged change

Prefer a normal Git revert when the change is isolated and reversible.

bad merge
   ↓
revert
   ↓
main restored
Database migration

Database rollback requires additional care.

Never assume:

“Git revert = database revert.”

A migration's rollback behavior must be explicitly understood before deployment.

Production incident

The priority becomes:

Contain
 ↓
Restore known-good behavior
 ↓
Preserve evidence
 ↓
Investigate
 ↓
Correct
 ↓
Revalidate

Do not destroy logs/evaluation evidence merely to make the dashboard look clean.

27. Release Strategy

Releases are milestone-based, not commit-count-based.

The project may use semantic version tags such as:

v0.1.0
v0.2.0
v1.0.0

A release should correspond to a meaningful validated product state.

The first stable project release should be associated with:

P5-C6 — Final Project Acceptance

and the successful completion of the project's Definition of Done.

28. Pre-Release Validation

Before a release:

All mandatory checkpoints accepted
        ↓
Phase gates passed
        ↓
Final E2E workflow
        ↓
Deployment validation
        ↓
Security validation
        ↓
Evaluation validation
        ↓
Documentation validation
        ↓
Human approval
        ↓
Release tag
29. Documentation Updates

Documentation is part of engineering work.

If implementation changes:

architecture;
API behavior;
evaluation methodology;
setup;
deployment;
security;
operational behavior;

the relevant documentation must be updated.

Documentation must never claim functionality that the repository does not implement.

30. Decision Records

Important architectural or engineering decisions should be recorded under:

docs/decisions/

A decision record should contain:

Decision
Context
Problem
Alternatives considered
Chosen approach
Reason
Trade-offs
Consequences
Requirements affected
Date
Approval

Example:

ADR-001 — Primary Database Selection
ADR-002 — Evaluation Run Execution Model
ADR-003 — LLM Judge Strategy

Only actual decisions should be recorded as decisions.

Proposals remain proposals until approved.

31. State Tracking

EvalOps should track state at three levels.

Checkpoint state
PLANNED
READY
IN PROGRESS
VALIDATING
BLOCKED
FAILED
ACCEPTED
Phase state
LOCKED
IN PROGRESS
GATE REVIEW
PASSED
BLOCKED
Project state
FOUNDATION
EVALUATION CORE
OBSERVABILITY
PRODUCT
PRODUCTION
RELEASE
COMPLETE

GitHub Issues and PRs should make these states visible.

32. AI Agent GitHub Behavior

An AI agent must begin checkpoint work by inspecting:

git status
git branch
git log
repository structure
checkpoint Issue
relevant documentation
relevant implementation
tests

Then it should confirm:

Checkpoint identified
Prerequisites satisfied
Scope understood

Only then should it modify files.

33. AI Agent Must Not

The agent must not:

work directly on main for ordinary checkpoint development;
create commits merely for activity;
open PRs without meaningful changes;
bypass CI;
bypass review;
merge its own work where human approval is required;
modify unrelated components;
hide failures;
rewrite functioning code unnecessarily;
alter architecture silently;
fabricate validation;
fabricate evaluation results.
34. AI Agent Completion Behavior

At the end of implementation:

Inspect diff
     ↓
Run tests
     ↓
Run checkpoint validation
     ↓
Review acceptance criteria
     ↓
Collect evidence
     ↓
Prepare PR
     ↓
STOP

The agent should not continue making changes merely because:

“There is still something I could improve.”

Improvement beyond checkpoint scope belongs to another work unit.

35. PR Evidence Matrix
Change type	Required evidence
Pure deterministic logic	Unit tests + relevant integration validation
API	API/integration tests + actual request/response
Database	Migration + persistence tests
Worker	Worker execution + failure/retry evidence
Evaluation	Real dataset + actual evaluation result
LLM judge	Judge tests + calibration evidence where applicable
Regression	Real before/after runs + detected regression
Trace	Real execution + trace/span evidence
Cost	Actual usage + calculation evidence
Dashboard	Runtime demonstration + representative real data
Security	Security test results
Deployment	Deployed runtime validation
CI gate	Actual CI evaluation pass/fail
Documentation	Review against actual implementation
36. The Mergeability Test

Before a PR can be considered mergeable, the reviewer should be able to answer yes to:

[ ] Do I know which checkpoint this implements?
[ ] Is the scope bounded?
[ ] Do the acceptance criteria exist?
[ ] Were the relevant tests actually run?
[ ] Was runtime behavior actually demonstrated?
[ ] Is the evidence real?
[ ] Does the implementation match the architecture?
[ ] Are failures disclosed?
[ ] Are there no unauthorized changes?
[ ] Is documentation accurate?
[ ] Is the checkpoint actually ready for acceptance?

If any mandatory answer is no:

Not mergeable yet.

37. Anti-Gaming Rules

The following behaviors are explicitly treated as engineering-quality failures:

Green-dot farming

Commits without meaningful engineering purpose.

PR farming

Opening PRs simply to create activity.

Test farming

Creating tests without executing them.

CI farming

Repeatedly changing code until CI turns green without understanding the underlying failure.

Evidence farming

Producing screenshots/logs that do not prove the acceptance criterion.

Metric farming

Displaying more metrics without increasing engineering usefulness.

Scope farming

Expanding the PR to make the work appear larger.

38. GitHub as a State Machine

The entire workflow can be represented as:

                 CHECKPOINT
                     │
                     ▼
                  ISSUE
                     │
                     ▼
                  BRANCH
                     │
                     ▼
               WORK UNITS
                     │
                     ▼
                  COMMITS
                     │
                     ▼
                 VALIDATE
                     │
              ┌──────┴──────┐
              ▼             ▼
           SUCCESS        FAILURE
              │             │
              ▼             ▼
              PR          DIAGNOSE
              │             │
              ▼             └──→ FIX / BLOCK
            REVIEW
              │
              ▼
          ACCEPTANCE
              │
          ┌───┴────┐
          ▼        ▼
        PASS      FAIL
          │        │
          ▼        ▼
        MERGE    REWORK
          │
          ▼
       CLOSE ISSUE
          │
          ▼
   UNLOCK NEXT CHECKPOINT
39. Final GitHub Operating Rule

The repository should tell a truthful story.

Someone examining EvalOps should be able to move from:

Requirement
   ↓
Checkpoint Issue
   ↓
Implementation PR
   ↓
Tests
   ↓
Runtime evidence
   ↓
Acceptance
   ↓
Merged capability

and understand why the change exists, what it proves, and whether it actually works.

That is more valuable than a repository containing hundreds of commits.

40. Final Engineering Principle

The GitHub workflow is ultimately governed by one equation:

Meaningful Progress
=
Accepted Capability
+
Evidence
+
Traceability

Not:

Meaningful Progress
=
Commits
+
PRs
+
Green CI

Therefore the final rule of the EvalOps GitHub workflow is:

Never use GitHub activity to simulate engineering progress. Use GitHub to record and prove engineering progress that has already been demonstrated.

This FINAL GITHUB ENGINEERING WORKFLOW v1.0 is now the authoritative repository workflow for EvalOps. Changes to branch policy, checkpoint mapping, acceptance/merge rules, or release policy should be treated as explicit engineering-policy changes rather than informal workflow adjustments.