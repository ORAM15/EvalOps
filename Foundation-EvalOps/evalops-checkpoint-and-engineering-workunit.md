FINAL CHECKPOINT & ENGINEERING-WORK UNIT SYSTEM

Project: EvalOps
Version: 1.0
Status: FINAL
Authority: Project Constitution → Requirements → Final Architecture → Technology Specification → Master Phase Plan

1. Core Principle

A checkpoint is a verification boundary, not a coding milestone.

The lifecycle is:

Requirement
    ↓
Checkpoint
    ↓
Bounded implementation
    ↓
Evidence
    ↓
Validation
    ↓
Human/automated acceptance
    ↓
Checkpoint complete

Therefore:

Code exists        ❌
Commit exists      ❌
PR exists          ❌
Tests attempted    ❌

Acceptance proven  ✅

This is particularly important for EvalOps because a large part of the project can be made to look functional without actually evaluating AI systems.

2. Engineering-Work Unit

A Work Unit is the smallest autonomous implementation task an AI engineering agent is permitted to execute.

Each work unit must have:

ONE objective
ONE bounded scope
DEFINED inputs
DEFINED outputs
DEFINED validation
DEFINED stop conditions

An agent must not expand a work unit because it discovers something “interesting.”

If additional work is required:

Current checkpoint
       ↓
STOP
       ↓
Create/update next checkpoint
3. Checkpoint Hierarchy

The project hierarchy is:

PROJECT
   │
   └── PHASE
         │
         ├── CHECKPOINT
         │      │
         │      └── ENGINEERING WORK UNITS
         │
         └── CHECKPOINT

Example:

P1 — Evaluation Core
│
├── P1-C1 — Evaluation Domain Foundation
├── P1-C2 — Dataset Management
├── P1-C3 — Target Application Integration
├── P1-C4 — Evaluation Run Execution
├── P1-C5 — Deterministic Scoring
└── P1-C6 — Evaluation Truth Validation

The final checkpoint of a phase is a phase acceptance checkpoint.

4. Autonomous-Agent Safety Rules

An autonomous agent may:

inspect the repository;
implement the explicitly permitted work;
create tests;
run tests;
update documentation directly related to the checkpoint;
create commits;
prepare a PR;
provide validation evidence.

It may not:

redesign the architecture;
change requirements;
introduce an unapproved technology;
expand scope;
silently change a metric definition;
fabricate evaluation results;
weaken acceptance criteria;
mark the checkpoint complete merely because tests pass.

If blocked by an architectural or product decision:

STOP and request human decision.

5. GitHub Representation

Every checkpoint gets one GitHub Issue.

Recommended format:

[CP] P1-C2 — Dataset Management

The Issue becomes the control record for that checkpoint.

It contains:

Objective
Prerequisites
Allowed work
Expected artifacts
Acceptance criteria
Validation commands
Failure conditions
Completion evidence
Dependencies
Human approval
6. Branch Strategy

A checkpoint gets its own branch.

main
 │
 ├── cp/P0-C1-foundation
 ├── cp/P0-C2-runtime
 ├── cp/P1-C1-evaluation-domain
 └── ...

Branch naming:

cp/<checkpoint-id>-<short-name>

Example:

cp/P1-C4-evaluation-run

An AI agent must not work directly on main for checkpoint implementation.

7. Commit Strategy

Commits should represent meaningful engineering units.

Example:

feat(eval): implement evaluation run lifecycle
test(eval): validate run lifecycle transitions
docs(eval): document run lifecycle

Avoid:

update
changes
stuff
final
working

A checkpoint may have multiple commits.

A commit is not evidence of checkpoint completion.

8. Pull Request

Each checkpoint should normally produce one PR.

Example:

[CP] P1-C4 — Evaluation Run Execution

The PR should contain:

Checkpoint
Objective
Implemented
Not implemented
Tests
Validation evidence
Known limitations
Screenshots/logs where useful

The PR must link the checkpoint Issue.

9. Review

Review asks:

Scope

Did the agent implement only the checkpoint?

Architecture

Does implementation conform to the approved architecture?

Correctness

Do acceptance tests demonstrate the required behavior?

Evidence

Is the result actually demonstrated?

Integrity

Are any numbers, traces or evaluation results fabricated?

Security

Were secrets or sensitive data mishandled?

10. Merge Rule

The sequence is:

Issue
 ↓
Branch
 ↓
Implementation
 ↓
Tests
 ↓
Validation
 ↓
PR
 ↓
Review
 ↓
Acceptance evidence
 ↓
Merge
 ↓
Issue closure

Merge is not itself the acceptance event.

The acceptance event is:

The checkpoint's acceptance criteria have been demonstrated.

11. FINAL CHECKPOINT MAP

The six master phases become the following bounded checkpoints.

P0 — FOUNDATION
P0-C1 — Repository & Development Contract

Objective: Establish the repository's engineering conventions and checkpoint operating model.

Prerequisites: Existing GitHub repository.

Allowed work:

repository structure;
contribution/development conventions;
environment documentation;
checkpoint documentation convention;
basic project metadata.

Expected artifacts:

repository structure;
README foundation;
development documentation;
checkpoint convention.

Validation: Repository inspection.

Acceptance criteria:

project can be understood by a new contributor;
authoritative documents are clearly identified;
no unrelated project material exists;
checkpoint workflow is documented.

DoD: Repository is an unambiguous engineering workspace.

Failure conditions:

undocumented conventions;
unrelated architecture;
secrets;
scope changes.

Human approval: No, unless repository scope changes.

GitHub: Issue → branch → PR → review → merge.

Next: P0-C2.

P0-C2 — Local Runtime Foundation

Objective: Make the approved runtime stack executable locally.

Prerequisites: P0-C1.

Allowed work:

Python environment;
TypeScript/Next.js foundation;
FastAPI foundation;
PostgreSQL connection;
Redis connection;
Celery worker;
Docker configuration;
environment configuration.

Expected artifacts:

Frontend
Backend
PostgreSQL
Redis
Worker

Validation:

frontend starts
backend starts
database connects
redis connects
worker starts

Acceptance criteria: All required components operate locally without manual hidden steps.

DoD: EvalOps skeleton is runnable.

Failure conditions:

broken dependency chain;
secrets committed;
undocumented manual configuration;
unapproved infrastructure.

Human approval: Only if tooling must change.

Next: P0-C3.

P0-C3 — Persistence & Migration Foundation

Objective: Establish safe relational persistence.

Prerequisites: P0-C2.

Allowed work:

SQLAlchemy;
Alembic;
initial schema mechanism;
migration execution;
database test connection.

Expected artifacts:

DB layer;
migration system;
initial schema foundation.

Validation:

fresh database
 ↓
migration
 ↓
application connection
 ↓
test

Acceptance: A fresh database can be initialized deterministically.

DoD: Database evolution is controlled through migrations.

Failure conditions:

schema manually required;
migration irreproducible;
application bypasses approved persistence boundary.

Next: P0-C4.

P0-C4 — Testing & CI Foundation

Objective: Establish automated validation before meaningful product development.

Prerequisites: P0-C2 and P0-C3.

Allowed work:

pytest;
backend tests;
basic frontend/build validation;
GitHub Actions;
lint/type/build checks where appropriate.

Expected artifacts:

test configuration;
CI workflow;
baseline tests.

Validation: Push a controlled change and observe CI.

Acceptance: CI reliably identifies a deliberately introduced validation failure and passes after correction.

DoD: The repository can automatically validate basic software integrity.

Next: P0-C5.

P0-C5 — Foundation Acceptance

Objective: Prove the entire P0 foundation works together.

Prerequisites: P0-C1 through P0-C4.

Allowed work: Fix only foundation-level defects.

Validation:

Clone
 ↓
Install
 ↓
Configure
 ↓
Start
 ↓
Migrate
 ↓
Run
 ↓
Test

Acceptance: A clean environment can reproduce the foundation using documented instructions.

DoD: P0 complete.

Failure: Any undocumented dependency or broken foundation component.

Human approval: YES — phase acceptance.

Next: P1-C1.

P1 — EVALUATION CORE
P1-C1 — Evaluation Domain Foundation

Objective: Implement the core evaluation concepts as explicit domain objects.

Prerequisite: P0 complete.

Allowed work:

project;
application;
evaluator/scorer abstractions;
evaluation run;
result concepts.

Not allowed:

dashboard;
LLM judge;
unrelated analytics.

Acceptance: Domain model represents an evaluation lifecycle without requiring UI assumptions.

Next: P1-C2.

P1-C2 — Dataset Management

Objective: Make real evaluation datasets usable.

Allowed:

dataset;
dataset version;
test case;
input;
expected output;
metadata;
ingestion.

Acceptance:

A real dataset can be:

created/imported
 ↓
versioned
 ↓
retrieved
 ↓
used by a run

Failure:

fake records;
untraceable versions;
dataset mutation without traceability.

Next: P1-C3.

P1-C3 — Target Application Integration

Objective: Establish the real interface through which EvalOps executes/evaluates an AI application.

Allowed:

target application adapter/interface;
request;
response;
execution metadata.

Acceptance: EvalOps can invoke the selected real target application and obtain its actual output.

Critical rule:

A mock target may be used for automated tests, but cannot be the sole evidence of checkpoint completion.

Next: P1-C4.

P1-C4 — Evaluation Run Execution

Objective: Execute real test cases through the evaluation pipeline.

Flow:

Dataset
 ↓
Run
 ↓
Worker
 ↓
Target AI
 ↓
Output
 ↓
Persist result

Acceptance:

A real run reaches a terminal state and produces persisted results for real executions.

Next: P1-C5.

P1-C5 — Deterministic Evaluation

Objective: Implement the first transparent scoring method.

Allowed:

exact matching;
explicit deterministic rules where justified;
score calculation;
pass/fail.

Acceptance: Known expected/actual pairs produce correctly calculated scores.

Required validation: Include both passing and failing cases.

Failure: Hardcoded scores.

Next: P1-C6.

P1-C6 — Evaluation Truth Acceptance

This is the most important P1 checkpoint.

Objective: Prove EvalOps is genuinely evaluating an AI system.

Prerequisites: P1-C1 through P1-C5.

Validation:

Use a real application and real dataset.

Demonstrate:

Input
 ↓
Real AI execution
 ↓
Actual output
 ↓
Real scorer
 ↓
Real result

Then deliberately introduce a failing case.

Acceptance:

EvalOps must correctly distinguish:

PASS
FAIL

based on actual execution output.

Failure conditions:

fabricated score;
hardcoded output;
score generated without target execution;
dashboard-only demonstration.

Human approval: YES.

DoD: P1 is complete.

P2 — INTEGRATION & OBSERVABILITY
P2-C1 — Application Integration Contract

Formalize how external AI applications communicate with EvalOps.

Acceptance: An external application can send required information through the approved integration boundary.

P2-C2 — Trace & Span Capture

Objective: Capture execution structure.

Trace
 ├── Span
 ├── Span
 └── Span

Acceptance: A real execution produces a trace associated with the relevant evaluation/run context.

P2-C3 — Execution Metrics

Implement actual:

latency;
token usage where available;
execution metadata.

Acceptance: Metrics originate from actual execution data.

P2-C4 — Cost Estimation

Objective: Estimate request/evaluation cost from real token usage and configurable pricing.

Acceptance: Changing pricing configuration changes calculated cost appropriately.

Failure: permanently hardcoded pricing.

P2-C5 — Failure Classification & Recovery

Distinguish:

Target AI failure
Evaluator failure
Infrastructure failure

Validate retries/idempotency where applicable.

Acceptance: Failures are classified correctly and retries do not create contradictory authoritative results.

P2-C6 — Observability Acceptance

Objective: Prove an evaluation failure can be investigated.

Demonstration:

Failed case
 ↓
Actual output
 ↓
Trace
 ↓
Relevant spans
 ↓
Latency/token evidence

Human approval: YES.

DoD: P2 complete.

P3 — EVALUATION INTELLIGENCE & PRODUCT
P3-C1 — Additional Scorer Framework

Implement additional evaluation approaches only where justified by real cases.

Potentially:

rule-based;
semantic.

Acceptance: Each scorer has a defined purpose and test cases demonstrating correctness.

P3-C2 — LLM Judge

Objective: Introduce LLM-as-a-judge responsibly.

Allowed:

rubric;
judge prompt;
structured judgment;
provider abstraction;
evaluator output.

Mandatory security behavior:

The evaluated AI output is treated as untrusted content.

Acceptance: The judge can evaluate semantically different but equivalent answers.

P3-C3 — Judge Calibration

Objective: Determine whether automated judging agrees sufficiently with human labels for the selected evaluation scenario.

Acceptance: Calibration dataset contains human labels and automated judgments, with agreement/disagreement measured.

Critical rule:

We do not claim:

“LLM judge is objective.”

P3-C4 — Run Comparison & Experiments

Compare actual runs:

Version A
vs
Version B

with:

quality;
latency;
cost;
failures.

Acceptance: Differences derive from actual runs.

P3-C5 — Regression Detection

Objective: Detect meaningful degradation.

Acceptance:

Given deliberately degraded evaluation results:

Previous > Current

EvalOps identifies the relevant regression according to the configured comparison policy.

Failure: arbitrary alerting without defined methodology.

P3-C6 — Developer Dashboard

Objective: Expose useful engineering information.

Dashboard must answer:

Did it improve?

and:

Why did it change?

Acceptance: A developer can navigate from run summary → failed case → trace.

P3-C7 — Product Acceptance

Demonstrate the complete analytical workflow:

Run A
 ↓
Run B
 ↓
Comparison
 ↓
Regression
 ↓
Failed cases
 ↓
Trace
 ↓
Cost/latency trade-off

Human approval: YES.

DoD: P3 complete.

P4 — RELIABILITY, SECURITY & PRODUCTION
P4-C1 — Authentication & Authorization

Objective: Protect public access and project boundaries.

Acceptance:

Unauthorized access is rejected and authorized users can access permitted project resources.

P4-C2 — Data & Secret Security

Validate:

API key protection;
environment secrets;
sensitive evaluation data;
access controls.

Acceptance: No production secret is present in repository/source/client bundles.

P4-C3 — API Reliability

Validate:

input validation;
error responses;
rate limiting where required;
failure behavior.
P4-C4 — Worker Reliability

Test:

timeout
failure
retry
duplicate attempt
worker interruption

Acceptance: System does not silently create false authoritative results.

P4-C5 — EvalOps Self-Observability

Observe:

API
Worker
Queue
Database
Evaluation jobs

Acceptance: Operational failures can be distinguished from target-AI evaluation failures.

P4-C6 — Production Deployment

Deploy the actual system:

Frontend
Backend
Worker
PostgreSQL
Redis

Acceptance: A clean external user can access the deployed application and perform a real evaluation.

P4-C7 — Production Acceptance

Run the defined security/reliability/deployment validation suite.

Human approval: YES.

DoD: P4 complete.

P5 — CI GATES & FINAL RELEASE
P5-C1 — Evaluation CI Interface

Connect GitHub CI to actual EvalOps evaluation.

Acceptance: CI can trigger a genuine evaluation.

P5-C2 — Quality Gate

Define and implement the approved threshold mechanism.

Example:

Requirement:
Groundedness ≥ 90%

Actual:
87%

Result:
FAIL

Acceptance: The gate responds to actual evaluation results.

P5-C3 — Regression Release Gate

Objective: Demonstrate that an AI change can be rejected because evaluation shows degradation.

Acceptance:

Change
 ↓
Evaluation
 ↓
Regression
 ↓
CI failure/block

No simulated CI result.

P5-C4 — Documentation & Reproducibility

Finalize:

setup;
architecture;
methodology;
metrics;
judge methodology;
API;
deployment;
security;
limitations;
technical decisions.

Acceptance: Another engineer can understand and reproduce the system using repository documentation.

P5-C5 — Final Demonstration

Run the complete product workflow:

Register AI app
       ↓
Dataset
       ↓
Evaluation
       ↓
Results
       ↓
Trace
       ↓
Cost
       ↓
Latency
       ↓
Version comparison
       ↓
Regression
       ↓
CI gate

All evidence must come from the real system.

P5-C6 — FINAL PROJECT ACCEPTANCE

This is the terminal checkpoint.

It validates the entire Definition of Done.

Acceptance requires:
real AI application integration;
real datasets;
real evaluations;
multiple justified evaluation methods;
responsible LLM judging;
human calibration;
traces;
latency;
tokens;
cost;
comparison;
regression detection;
failure investigation;
CI gate;
deployed frontend/backend/database/worker infrastructure;
security;
documentation;
professional repository;
demonstrable technical understanding.

Human approval: REQUIRED.

DoD: EvalOps v1.0 complete.

12. Complete Checkpoint Dependency Graph
P0
│
├── C1 Repository
│     ↓
├── C2 Runtime
│     ↓
├── C3 Persistence
│     ↓
├── C4 Testing/CI
│     ↓
└── C5 Foundation Acceptance
              │
              ▼
P1
│
├── C1 Evaluation Domain
│     ↓
├── C2 Dataset
│     ↓
├── C3 Target Integration
│     ↓
├── C4 Run Execution
│     ↓
├── C5 Deterministic Scoring
│     ↓
└── C6 Evaluation Truth
              │
              ▼
P2
│
├── C1 Integration Contract
│     ↓
├── C2 Trace/Spans
│     ↓
├── C3 Execution Metrics
│     ↓
├── C4 Cost
│     ↓
├── C5 Failure/Recovery
│     ↓
└── C6 Observability Acceptance
              │
              ▼
P3
│
├── C1 Scorer Expansion
│     ↓
├── C2 LLM Judge
│     ↓
├── C3 Calibration
│     ↓
├── C4 Experiments
│     ↓
├── C5 Regression
│     ↓
├── C6 Dashboard
│     ↓
└── C7 Product Acceptance
              │
              ▼
P4
│
├── C1 Auth
│     ↓
├── C2 Security
│     ↓
├── C3 API Reliability
│     ↓
├── C4 Worker Reliability
│     ↓
├── C5 Self-Observability
│     ↓
├── C6 Deployment
│     ↓
└── C7 Production Acceptance
              │
              ▼
P5
│
├── C1 CI Interface
│     ↓
├── C2 Quality Gate
│     ↓
├── C3 Regression Gate
│     ↓
├── C4 Documentation
│     ↓
├── C5 Final Demo
│     ↓
└── C6 FINAL ACCEPTANCE
13. Standard CHECKPOINT TEMPLATE

This should be copied into every checkpoint Issue.

# CHECKPOINT: <ID> — <NAME>

## Phase
<Px — Phase Name>

## Objective
<One precise sentence describing what this checkpoint proves.>

## Prerequisites
- <Required previous checkpoint>
- <Required decision>
- <Required environment>

## Allowed Work
- <Explicit permitted work>
- <Explicit permitted work>

## Explicitly Out of Scope
- <Work that must not happen here>
- <Architecture changes>
- <Unapproved technology>

## Expected Artifacts
- <Artifact>
- <Artifact>
- <Artifact>

## Validation Method
<Exact method used to demonstrate the checkpoint.>

## Acceptance Criteria

- [ ] <Observable criterion>
- [ ] <Observable criterion>
- [ ] <Observable criterion>

## Definition of Done

The checkpoint is complete only when all acceptance criteria have been
demonstrated and completion evidence has been attached.

## Failure Conditions

- <Condition>
- <Condition>
- <Condition>

## Dependencies

### Requires
- <Checkpoint IDs>

### Blocks
- <Checkpoint IDs>

## Human Approval Required

<YES / NO>

If YES:
<What the owner must approve>

## GitHub Activity

### Issue
<Checkpoint issue>

### Branch
`cp/<checkpoint-id>-<short-name>`

### Commits
<Relevant commits>

### Pull Request
<PR link>

### Review
<Review status>

### Merge
<Merge status>

## Completion Evidence

### Tests
<commands + results>

### Runtime Demonstration
<what was actually demonstrated>

### Screenshots / Logs
<if applicable>

### Evaluation Evidence
<real evaluation evidence, if applicable>

### Known Limitations
<limitations>

## Acceptance Decision

- [ ] ACCEPTED
- [ ] REJECTED
- [ ] BLOCKED

### Reviewer / Owner
<Name>

### Date
<YYYY-MM-DD>

## Next Checkpoint

<Checkpoint ID + name>
14. Autonomous Agent Checkpoint Contract

Every autonomous coding agent working on EvalOps should receive the checkpoint Issue as its scope boundary.

The agent's operating rule is:

READ
 ↓
UNDERSTAND
 ↓
CHECK PREREQUISITES
 ↓
IMPLEMENT ONLY ALLOWED WORK
 ↓
TEST
 ↓
VALIDATE ACCEPTANCE CRITERIA
 ↓
COLLECT EVIDENCE
 ↓
STOP

Not:

Implement
 ↓
Notice another thing
 ↓
Redesign
 ↓
Add technology
 ↓
Refactor everything
 ↓
"Done"
15. Agent Stop Conditions

An agent must stop and request owner input if:

an architectural decision is required;
a technology not in the specification appears necessary;
requirements conflict;
acceptance criteria cannot be satisfied;
a security decision is ambiguous;
evaluation methodology becomes scientifically questionable;
scope expansion is required;
the agent cannot obtain real validation evidence.
16. Evidence Hierarchy

For EvalOps, evidence should be considered in this order:

Real runtime behavior
        ↑
Integration test
        ↑
Unit test
        ↑
Static inspection
        ↑
Code existence

Therefore:

Code is implementation evidence, not product-behavior evidence.

For example, a function named detect_regression() proves almost nothing.

A real demonstration showing:

Run A = 91%
Run B = 84%
        ↓
Regression detected
        ↓
Affected cases identified

is meaningful evidence.

17. Checkpoint Status Model

Every checkpoint should have exactly one state:

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

Only:

ACCEPTED

permits dependency-unblocking.

A merged PR without acceptance remains:

VALIDATING

until evidence is reviewed.

18. The Most Important Rule

For this project, we should treat every checkpoint as answering one question:

“What must we be able to demonstrate before we are allowed to move forward?”

For example:

P1-C6:

Can EvalOps actually evaluate a real AI application?

P2-C6:

Can we investigate why that evaluation failed?

P3-C7:

Can a developer use EvalOps to decide whether one AI version is better?

P4-C7:

Can we trust the deployed system?

P5-C3:

Can EvalOps actually prevent a degraded AI change from passing CI?

That gives the entire project a very clean engineering progression:

           BUILD
             │
             ▼
        DEMONSTRATE
             │
             ▼
          VERIFY
             │
             ▼
          ACCEPT
             │
             ▼
       UNLOCK NEXT

This Checkpoint & Engineering-Work Unit System v1.0 is the authoritative execution-control layer beneath the Master Phase Plan. It does not change the architecture or requirements; it defines how we safely turn them into independently verifiable engineering work.