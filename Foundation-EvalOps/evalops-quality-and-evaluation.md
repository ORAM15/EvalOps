FINAL QUALITY & EVALUATION SYSTEM — EvalOps

Project: EvalOps
Version: 1.0
Status: FINAL
Authority: Project Constitution → Requirements & Constraints → Final Architecture → Technology & Tooling Specification → Master Phase Plan → Checkpoint System → AI Engineering Contract

1. Purpose

This system defines how EvalOps proves that engineering work is actually correct.

The central rule is:

Implementation is not proof. Validation is proof.

Therefore:

Code
 ↓
Test
 ↓
Runtime behavior
 ↓
Evidence
 ↓
Acceptance criteria
 ↓
Gate decision

A successful:

commit,
build,
GitHub Action,
unit test,
PR,

is evidence, but never sufficient proof by itself.

2. What “Correct” Means for EvalOps

Correctness has several layers.

                 CORRECTNESS
                      │
       ┌──────────────┼──────────────┐
       ▼              ▼              ▼
   Software       Evaluation      System
   correctness     correctness    correctness
       │              │              │
       ▼              ▼              ▼
  Does code       Are AI results   Does the
  behave as       measured         product
  intended?       meaningfully?    solve the problem?

For EvalOps, there is an additional distinction:

A technically correct evaluator can still produce a meaningless evaluation.

Therefore both software correctness and evaluation validity must be demonstrated.

3. Evidence Hierarchy

Evidence is evaluated according to what it proves.

Evidence	What it proves	Sufficiency
Code inspection	Implementation exists	Low
Commit	Change was recorded	Low
Successful build	Build integrity	Low–Medium
Unit test	Local behavior	Medium
Integration test	Component interaction	Medium–High
E2E test	User/system workflow	High
Real runtime demonstration	Actual behavior	High
Real AI evaluation	Evaluation behavior	Very High
Human acceptance	Requirement-level acceptance	Final

The project therefore uses layered evidence, not one universal test.

4. Testing System
4.1 Unit Testing

Unit tests validate isolated behavior.

Examples:

Dataset validation
Score calculation
Run-state transitions
Cost calculation
Regression calculation
Authentication rules
Required when

A component contains deterministic logic whose correctness can be isolated.

Evidence
test cases;
actual test execution;
actual output;
failure cases.
Pass

Expected inputs produce expected outputs, including relevant edge cases.

Failure

Any incorrect deterministic result or untested critical branch.

5. Integration Testing

Integration tests prove that components actually communicate correctly.

Examples:

API
 ↓
Database

API
 ↓
Queue
 ↓
Worker

Worker
 ↓
Target AI
 ↓
Evaluator
 ↓
Database
Required evidence

The participating components must actually interact.

Mocking may be used for isolated failure testing, but a mocked integration does not prove the real integration works.

Pass

Real component boundaries exchange valid data and preserve expected semantics.

6. End-to-End Testing

E2E testing validates the complete user/system workflow.

For EvalOps:

Create project
      ↓
Register application
      ↓
Create dataset
      ↓
Start evaluation
      ↓
Worker executes
      ↓
Results generated
      ↓
Results displayed

Later:

Run A
 ↓
Run B
 ↓
Comparison
 ↓
Regression
 ↓
CI gate
Pass

A complete workflow succeeds using the actual deployed/local system as applicable.

Failure

Any critical workflow requires undocumented manual intervention or produces incorrect results.

7. Regression Testing

Regression testing answers:

Did something that previously worked stop working?

It has two levels.

Software regression

Existing application functionality remains operational.

AI/system regression

A new AI version performs worse according to the approved evaluation methodology.

These must not be confused.

Software regression
        ≠
AI quality regression
8. Security Testing

Security validation must cover the actual security requirements.

Test:

authentication;
authorization;
project isolation;
API credentials;
secrets;
sensitive evaluation data;
malicious inputs;
evaluator prompt injection;
access boundaries.

Particularly important:

AI output
   ↓
LLM Judge

The output is untrusted content.

A malicious output such as an instruction to ignore the rubric must not override evaluator instructions.

Pass

Unauthorized behavior is rejected and untrusted content cannot alter evaluator control logic.

Failure

Any unauthorized access, credential exposure or evaluator-control bypass.

9. Performance Testing

Performance testing must answer meaningful engineering questions.

Relevant measurements include:

API latency;
evaluation execution latency;
worker processing behavior;
database performance;
queue behavior;
target-AI latency;
throughput where applicable.

Do not invent arbitrary “enterprise-scale” performance targets.

Pass

The system satisfies the performance requirements established for the actual deployment/use case.

Failure

Performance prevents the defined workflows from functioning acceptably or causes reliability problems.

10. AI Evaluation

AI evaluation is a distinct validation layer.

The project must use appropriate evaluation methods rather than one universal scorer.

                  Test Case
                      │
        ┌─────────────┼──────────────┐
        ▼             ▼              ▼
   Exact/rule     Semantic       LLM Judge
   evaluation     evaluation
        │             │              │
        └─────────────┼──────────────┘
                      ▼
                   Result

The method must match the task.

11. Deterministic Evaluation

Use when the expected result is unambiguous.

Example:

Expected = 75%
Actual   = 75%
Pass

The deterministic scorer produces the correct result.

Failure

The scorer incorrectly accepts/rejects known cases.

12. Semantic Evaluation

Use when multiple textual answers can legitimately express the same answer.

Example:

Expected:
"Students require 75% attendance."

Actual:
"At least seventy-five percent attendance is required."

Exact matching could incorrectly fail.

Semantic evaluation should therefore assess the relevant meaning.

13. LLM-as-a-Judge Evaluation

An LLM judge is an automated evaluator, not an objective authority.

The validation system must consider:

rubric correctness;
consistency;
bias;
prompt sensitivity;
adversarial outputs;
agreement with human labels.

The judge's output must itself be testable.

14. Judge Calibration

A representative set of cases should contain:

Human judgment
       +
LLM judgment

Then measure agreement/disagreement.

Calibration does not prove the judge is perfect.

It provides evidence about whether the judge is sufficiently reliable for the selected use case.

15. Human Evaluation

Human evaluation is required where automated evaluation cannot be trusted alone.

Humans can provide:

reference labels;
quality judgments;
calibration labels;
disputed-case resolution.

The system should preserve disagreement rather than automatically treating the machine result as correct.

16. Evaluation Validity Rules

Every important metric must answer:

What engineering question does this metric answer?

For example:

Accuracy
→ Did correctness improve?

Latency
→ Did the change make execution slower?

Cost
→ Did the change become more expensive?

Groundedness
→ Is the response adequately supported?

A metric without a defined engineering interpretation should not be promoted to a primary product metric.

17. Trace Validation

A trace is valid only if it corresponds to a real execution.

Validation:

Evaluation case
      ↓
Execution
      ↓
Trace ID
      ↓
Spans
      ↓
Actual operations
Pass

A real execution can be followed through its recorded trace.

Failure
fabricated trace;
orphan trace;
missing critical execution relationship;
telemetry claiming operations that did not occur.
18. Observability Validation

EvalOps must validate both:

Target observability

Can EvalOps explain AI application behavior?

EvalOps observability

Can we detect problems inside EvalOps?

AI Application
      │
      ▼
    EvalOps
      │
      ▼
EvalOps itself must be observable
19. Failure Testing

Failure is a first-class test condition.

We deliberately test:

Target AI failure
Evaluator failure
Database failure
Queue failure
Worker failure
Timeout
External API failure
Invalid input
Authentication failure

The system must distinguish these failures.

For example:

Target AI unavailable
        ↓
Evaluation could not execute
        ↓
NOT
        ↓
"AI quality = 0"

An infrastructure failure must not silently become an evaluation failure.

20. Deployment Validation

Deployment validation proves that the deployed system is the same functional system that passed local validation.

Validate:

Frontend
   ↓
Backend
   ↓
Database
   ↓
Queue
   ↓
Worker
   ↓
Evaluation

The deployed environment must support a real evaluation.

Pass

An external user can access the system and complete the defined critical workflow.

21. UX Validation

UX validation is behavioral rather than aesthetic.

The primary question is:

Can a developer understand what happened and make a decision?

The critical journey:

Run
 ↓
Result
 ↓
Comparison
 ↓
Regression
 ↓
Failure
 ↓
Trace
Pass

A developer can determine:

whether quality improved;
what changed;
which cases failed;
why they failed;
what happened operationally;
what cost/latency trade-off occurred.
Failure

The UI displays data but does not allow the user to understand the engineering implication.

22. Phase Quality Gates

Now the testing system becomes a formal progression.

GATE P0 — FOUNDATION
What must be validated
repository foundation;
runtime;
frontend;
backend;
database;
Redis;
worker;
migrations;
testing;
CI.
How

Local clean-start validation plus automated tests.

Required evidence
startup logs
migration result
test result
CI result
documented setup
Pass

A clean environment can reproduce the foundation.

Fail

Any foundational component cannot reliably start or integrate.

Approver

Human phase acceptance + engineering evidence.

GATE P1 — EVALUATION TRUTH

This is a critical gate.

What must be validated
dataset;
test cases;
target application;
evaluation run;
worker;
scorer;
result persistence.
How

Real target application + real dataset.

Required evidence
Input
Actual AI output
Expected/reference
Score
Pass/fail
Persisted run
Pass

The score is demonstrably derived from actual execution.

Fail

Any fabricated, hardcoded or disconnected result.

Approver

Human review required.

GATE P2 — OBSERVABILITY
What must be validated
integration contract;
traces;
spans;
latency;
token usage;
cost;
failure classification;
recovery behavior.
How

Execute real evaluations, including deliberately failing cases.

Required evidence
Evaluation result
+
Trace
+
Spans
+
Latency
+
Token usage where available
+
Cost
+
Failure classification
Pass

A meaningful failed evaluation can be investigated.

Fail

The platform reports a failure but cannot provide reliable execution evidence.

Approver

Human review required.

GATE P3 — EVALUATION INTELLIGENCE
What must be validated
additional scoring methods;
LLM judge;
calibration;
run comparison;
experiments;
regression detection;
dashboard;
failure investigation.
How

Use real AI versions/runs with intentionally different behavior.

Required evidence

Example:

Run A
91%

Run B
84%

       ↓

Regression detected
       ↓
Affected cases
       ↓
Trace
       ↓
Cost/latency comparison
Pass

The system produces meaningful, traceable engineering conclusions from real data.

Fail
fabricated metrics;
invalid comparison;
unjustified regression;
unvalidated judge;
dashboard-only evidence.
Approver

Human review required.

GATE P4 — PRODUCTION TRUST
What must be validated
authentication;
authorization;
secrets;
data protection;
API reliability;
worker reliability;
self-observability;
deployment.
How

Security, failure and deployment tests.

Required evidence
access-control tests;
failure tests;
deployment logs;
production runtime demonstration;
observability evidence.
Pass

The deployed system functions securely and predictably under defined failure conditions.

Fail

Security violation, unrecoverable critical failure, secret exposure or inability to operate the deployed system.

Approver

Human review required.

GATE P5 — RELEASE & CI
What must be validated
CI integration;
evaluation execution;
quality thresholds;
regression gates;
documentation;
final demonstration.
How

Introduce an actual AI change that produces a measurable regression.

Required evidence
Code change
 ↓
CI
 ↓
EvalOps evaluation
 ↓
Real regression
 ↓
Gate failure

And a separate improvement case:

Code change
 ↓
Evaluation
 ↓
Acceptable result
 ↓
Gate passes
Pass

CI decisions originate from real EvalOps evaluation results.

Fail

Any simulated or hardcoded CI decision.

Approver

Human final project acceptance.

23. Gate Status Model

Every phase gate has exactly four possible outcomes.

GATE PASS

All mandatory acceptance criteria are satisfied.

Evidence
   ↓
Criteria satisfied
   ↓
PASS
   ↓
Next phase unlocked
GATE FAIL

Evidence demonstrates that required behavior does not work.

Evidence
   ↓
Criteria violated
   ↓
FAIL
   ↓
Fix within checkpoint
   ↓
Revalidate

A failure is not hidden.

BLOCKED

The team cannot reasonably proceed because something external or unresolved prevents validation.

Examples:

missing owner decision;
unavailable required credential;
unresolved architecture question;
external dependency unavailable.
BLOCKED
   ↓
Owner action
   ↓
Resume validation
REQUIRES HUMAN REVIEW

The automated evidence is insufficient for the decision.

Especially applicable to:

architecture changes;
LLM judge validity;
human calibration;
security acceptance;
phase completion;
final release.
24. Checkpoint-Level Acceptance Formula

A checkpoint should be considered accepted only when:

Checkpoint Accepted
=
Scope satisfied
+
Implementation works
+
Validation executed
+
Evidence captured
+
Acceptance criteria satisfied
+
Required review completed

Not:

Commit exists
+
PR exists
=
Done
25. Meaningful Progress Measurement

The project needs a progress measure that cannot be gamed by code volume.

The authoritative progress unit is:

Accepted capability.

For example:

P1-C4
"Evaluation runs execute real applications."


Progress occurs only when that behavior is demonstrated and accepted.

Therefore:

                    PROJECT PROGRESS

              Accepted Checkpoints
                       │
                       ▼
               Accepted Capabilities
                       │
                       ▼
                 Phase Gates Passed
                       │
                       ▼
                 Product Capability
26. Evidence Ledger

Each checkpoint should maintain a lightweight evidence record.

Checkpoint
│
├── Requirement(s)
├── Acceptance criteria
├── Test results
├── Runtime evidence
├── Evaluation evidence
├── Screenshots/logs
├── Known failures
├── Review
└── Final gate decision

This creates traceability:

Requirement
   ↓
Implementation
   ↓
Validation
   ↓
Evidence
   ↓
Acceptance
27. Quality Rules for the Entire Project

The following are non-negotiable:

Rule 1

A green build is not project progress.

Rule 2

A passing unit test is not system validation.

Rule 3

A successful API call is not proof of evaluation correctness.

Rule 4

A dashboard metric is not valid merely because it is displayed.

Rule 5

An LLM judge is not automatically trustworthy.

Rule 6

A trace must correspond to actual execution.

Rule 7

A regression must be derived from defined comparison methodology.

Rule 8

Infrastructure failures must not be misreported as AI failures.

Rule 9

Negative test cases are mandatory wherever failure behavior matters.

Rule 10

No phase advances without satisfying its gate.

28. Final Quality Model

The entire EvalOps quality system can be represented as:

                         REQUIREMENT
                              │
                              ▼
                         CHECKPOINT
                              │
                              ▼
                        IMPLEMENTATION
                              │
             ┌────────────────┼────────────────┐
             ▼                ▼                ▼
        Unit Testing     Integration       Static Checks
                              │
                              ▼
                         E2E Testing
                              │
                              ▼
                     Real Runtime Evidence
                              │
                 ┌────────────┼─────────────┐
                 ▼            ▼             ▼
             AI Eval     Observability   Failure Tests
                 │            │             │
                 └────────────┼─────────────┘
                              ▼
                         Acceptance
                              │
                              ▼
                           GATE
                    ┌─────────┼─────────┐
                    ▼         ▼         ▼
                  PASS       FAIL    BLOCKED
                    │
                    ▼
               NEXT CHECKPOINT
29. Final Definition of Quality

EvalOps is considered to be making real engineering progress only when it can demonstrate increasingly valuable truths:

P0
"We can reliably run the system."
          ↓
P1
"We can genuinely evaluate an AI application."
          ↓
P2
"We can investigate its execution."
          ↓
P3
"We can determine whether one version is better."
          ↓
P4
"We can trust the deployed platform."
          ↓
P5
"We can use evaluation to influence the software
release lifecycle."

That progression is the project's actual quality trajectory.

This FINAL QUALITY & EVALUATION SYSTEM v1.0 is now the authoritative validation framework for EvalOps. A checkpoint, phase, or final release cannot be considered complete merely from code, commits, PRs, builds, or green CI; completion requires the appropriate behavioral evidence and acceptance defined above.