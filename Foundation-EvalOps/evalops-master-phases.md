FINAL MASTER PHASE PLAN — EvalOps

Status: FINAL
Baseline: v1.0
Authority: Project Constitution → Requirements & Constraints → Final Architecture → Technology & Tooling Specification

Phase overview
P0  Foundation & Environment
             │
             ▼
P1  Evaluation Core
             │
             ▼
P2  Application Integration + Observability
             │
             ▼
P3  Product Surface + Analysis
             │
             ▼
P4  Reliability, Security & Productionization
             │
             ▼
P5  Release, CI Evaluation Gates & Portfolio Validation

There are 6 phases total.

The important dependency is:

We do not build the polished dashboard before we have a real evaluation engine producing trustworthy data.

And:

We do not claim production readiness before reliability, security and real-data validation.

P0 — FOUNDATION & ENVIRONMENT
Purpose

Establish the actual development foundation required to begin implementation safely.

Why it exists

The repository exists, but the runtime/tooling foundation has not yet been established as an operational development environment.

We must know that the project can be built, tested and run locally before implementing product functionality.

Prerequisites
Project Constitution established.
Requirements established.
Final Architecture established.
Technology Specification established.
GitHub repository exists.
Inputs
Final Architecture
Technology Specification
Existing ORAM15/EvalOps repository
Objectives

Establish:

local project structure;
Python environment;
Node/Next.js environment;
backend foundation;
frontend foundation;
PostgreSQL development environment;
Redis development environment;
worker foundation;
Docker configuration;
migration mechanism;
testing foundation;
environment configuration;
basic CI foundation.
Major workstreams
Repository

Establish the authoritative structure:

EvalOps/
├── backend/
├── frontend/
├── tests/
├── docs/
├── infrastructure/
└── README.md

Exact structure will follow the architecture rather than being invented independently.

Backend

Create the minimal FastAPI application.

Frontend

Create the minimal Next.js application.

Database

Connect PostgreSQL.

Migrations

Establish Alembic.

Worker

Establish Redis + Celery.

Testing

Establish pytest and initial test execution.

Configuration

Create environment-variable conventions.

CI

Create basic GitHub Actions validation.

Expected artifacts
runnable backend;
runnable frontend;
database connection;
Redis connection;
worker process;
migration system;
test runner;
CI workflow;
development documentation.
Validation

The following must work:

Backend starts
      ↓
Frontend starts
      ↓
PostgreSQL connects
      ↓
Redis connects
      ↓
Worker starts
      ↓
Migration executes
      ↓
Tests execute
      ↓
CI passes
Dependencies

None beyond the approved foundation.

Risks
incorrect local environment;
dependency conflicts;
Docker configuration problems;
configuration/secrets accidentally committed;
unnecessary infrastructure introduced during setup.
Human decisions required

Only decisions that remain open in the Technology Specification, particularly:

exact authentication provider;
eventual LLM provider.

They do not need to block the basic project skeleton if the relevant interfaces can remain provider-neutral.

Exit criteria

P0 is complete when:

the repository builds;
frontend runs;
backend runs;
database works;
worker works;
migrations work;
tests run;
CI validates the repository;
no secrets are committed.
Definition of complete

EvalOps exists as a runnable software system skeleton with all approved foundational runtime boundaries operational.

Next-phase dependency

P1 cannot begin until P0 passes.

P1 — EVALUATION CORE
Purpose

Build the actual heart of EvalOps:

Run a real AI application against a real dataset and determine whether its outputs satisfy an evaluation criterion.

Why it exists

Everything else depends on trustworthy evaluation data.

A dashboard without this would violate the central product requirement.

Prerequisites

P0 complete.

Inputs
Evaluation architecture
Data architecture
Scorer architecture
Evaluation requirements
Database architecture
Objectives

Implement the minimum complete evaluation pipeline:

Dataset
   ↓
Dataset Version
   ↓
Test Cases
   ↓
Evaluation Run
   ↓
Target Application
   ↓
Actual Output
   ↓
Scorer
   ↓
Evaluation Result
   ↓
Run Metrics
Major workstreams
Project model

Projects become persistent entities.

Application registration

Register the target AI application.

Dataset management

Support:

dataset;
dataset version;
test cases;
inputs;
expected outputs;
metadata.
Evaluation runs

Implement run lifecycle:

CREATED
   ↓
QUEUED
   ↓
RUNNING
   ↓
COMPLETED / FAILED
Worker execution

Worker loads cases and executes them.

Deterministic scorer

Implement exact evaluation first.

This is deliberate.

We should prove the evaluation architecture using the most transparent evaluator before adding subjective evaluation.

Result persistence

Persist actual outputs and scores.

Basic metrics

Derive real metrics such as:

pass rate;
latency;
failures.

No cosmetic metrics.

Expected artifacts

A developer can perform something equivalent to:

Create project
      ↓
Register application
      ↓
Upload dataset
      ↓
Create run
      ↓
Worker executes cases
      ↓
Actual outputs recorded
      ↓
Scores generated
      ↓
Run completed
Validation

Use a real target AI application.

Example:

Input:
"What is the attendance requirement?"

Expected:
"75%"

The target application must actually produce an output.

EvalOps must then determine:

Expected
Actual
Score
Pass/Fail
Latency
Dependencies
P0 infrastructure.
Risks
Biggest risk

Building an evaluator that appears functional but does not actually evaluate real executions.

Other risks
incorrect scoring;
duplicate results;
worker failures;
unclear run state;
misleading metrics;
poor dataset versioning.
Human decisions required

Selection of the first target AI application if still unresolved.

This is an OPEN QUESTION from the architecture.

Exit criteria

A real evaluation can be completed end-to-end and its results can be inspected from persisted data.

Definition of complete

EvalOps can execute a real AI application against a real dataset and produce genuine, reproducible evaluation results.

Next-phase dependency

P2 requires P1.

P2 — APPLICATION INTEGRATION & OBSERVABILITY
Purpose

Make EvalOps useful beyond simple pass/fail evaluation by establishing the execution evidence needed to answer:

Why did the system fail?

Why it exists

Evaluation tells us what happened.

Observability tells us what happened inside the execution.

This phase implements that distinction.

Prerequisites

P1 complete.

Inputs
Trace architecture;
OpenTelemetry decision;
integration architecture;
observability requirements;
security trust boundaries.
Objectives

Implement:

target application integration;
trace capture;
spans;
execution metadata;
latency measurement;
token measurement where available;
cost estimation;
failure classification.
Major workstreams
Application integration

Formalize the target-AI integration contract.

Trace

Capture the journey of a request.

Trace
 ├── Retrieval
 ├── Tool
 ├── LLM
 └── Validation

Only spans that actually exist should be recorded.

OpenTelemetry

Instrument relevant execution boundaries.

Token usage

Capture:

input tokens
output tokens
total tokens

when the target model/provider exposes them.

Cost

Calculate estimated cost from:

actual usage
+
pricing configuration
Failure classification

Separate:

AI failure
Evaluator failure
Infrastructure failure
Retry/idempotency

Ensure worker retries cannot corrupt run results.

Expected artifacts
target integration;
trace model;
span model;
real traces;
latency measurements;
token measurements where available;
cost calculation;
failure classification;
retry handling.
Validation

A deliberately failing evaluation should allow us to answer:

Which test failed?
       ↓
What output was produced?
       ↓
How long did it take?
       ↓
What execution steps occurred?
       ↓
Did the target AI fail?
       ↓
Did the evaluator fail?
Dependencies

P1 evaluation execution.

Risks
capturing too much sensitive data;
confusing traces with evaluation results;
incorrect token/cost calculations;
telemetry becoming unnecessarily complex;
duplicate trace records.
Human decisions required
exact LLM/provider integration;
telemetry retention details if required.
Exit criteria

A real evaluation result can be connected to meaningful execution evidence.

Definition of complete

EvalOps can tell both whether an AI output failed and provide execution evidence that helps investigate the failure.

Next-phase dependency

P3 requires P2.

P3 — EVALUATION INTELLIGENCE & PRODUCT SURFACE
Purpose

Turn the working evaluation engine into the actual developer product.

This is where the platform becomes useful for comparison, analysis and decision-making.

Why it exists

P1 gives us evaluation.

P2 gives us evidence.

P3 turns that evidence into engineering decisions.

Prerequisites

P2 complete.

Inputs
quality requirements;
experiment requirements;
regression requirements;
UI requirements;
evaluation methodology.
Objectives

Implement:

additional justified scorers;
LLM judge;
judge calibration;
evaluation metrics;
run comparison;
regression detection;
failure review;
dashboard;
experiment comparison.
Major workstreams
Rule-based evaluation

Add only where actual test cases require it.

Semantic evaluation

Add where exact comparison is insufficient.

LLM-as-a-judge

Implement a controlled evaluator.

The judge must:

use a defined rubric;
treat evaluated output as untrusted;
produce structured judgment;
expose limitations.
Judge calibration

Compare judge outputs with human labels on representative cases.

Run comparison

Example:

Prompt v1
     vs
Prompt v2

Show:

Quality
Latency
Cost
Failures
Regression detection

Compare equivalent runs.

Previous
   ↓
Comparison
   ↑
Current

Identify degraded metrics and affected cases.

The mathematical threshold methodology remains an explicit implementation decision within the architecture's open question rather than being invented as a universal rule.

Dashboard

The dashboard must answer:

"Did my AI system get better?"

and:

"What changed when it got worse?"

Expected artifacts
evaluation method framework;
deterministic/rule/semantic scoring where justified;
LLM judge;
calibration evidence;
run comparison;
regression detection;
quality dashboard;
failure investigation UI;
experiment representation.
Validation

Demonstrate:

Version A
Accuracy: 91%

Version B
Accuracy: 84%

        ↓

REGRESSION DETECTED

Then inspect the failed cases and traces.

Also demonstrate a case where:

Quality ↑
Cost ↑
Latency ↑

The product should allow the developer to make an informed engineering decision rather than merely showing a larger score.

Dependencies
P2 traces and execution evidence;
P1 evaluation results.
Risks
LLM judge bias

The judge may systematically favor certain responses.

Metric gaming

A system may optimize for one metric while becoming worse overall.

Dashboard distortion

Too many metrics can hide the actual engineering signal.

False regression

Poor comparison methodology can produce misleading alerts.

Human decisions required
first judge model/provider;
calibration methodology;
regression threshold methodology;
which metrics are meaningful for the selected demonstration application.
Exit criteria

A developer can compare real AI system versions and determine whether one is genuinely better, including trade-offs.

Definition of complete

EvalOps has become a real evaluation product rather than an evaluation backend.

Next-phase dependency

P4 requires P3.

P4 — RELIABILITY, SECURITY & PRODUCTIONIZATION
Purpose

Make the system trustworthy enough for public use.

Why it exists

A working prototype is not automatically a production-oriented engineering system.

This phase validates:

reliability;
security;
deployment;
self-observability;
data protection;
operational behavior.
Prerequisites

P3 complete.

Inputs
security architecture;
deployment architecture;
reliability requirements;
testing architecture;
observability architecture.
Objectives

Implement and validate:

authentication;
authorization;
project isolation;
secret management;
API security;
rate limiting where justified;
failure handling;
worker reliability;
self-observability;
production database;
production deployment.
Major workstreams
Authentication

Implement the selected provider/mechanism.

Authorization

Enforce project-level boundaries.

Sensitive data

Review:

prompts
outputs
datasets
traces
API credentials
API security

Validate inputs and protect endpoints.

Worker reliability

Test:

transient failures;
retries;
timeouts;
worker crashes;
duplicate execution.
Self-observability

Observe EvalOps itself:

API
Worker
Queue
Database
Evaluation jobs
Deployment

Deploy:

Frontend
Backend
Worker
PostgreSQL
Redis
Production configuration

Separate:

development
test
production
Expected artifacts
authenticated application;
authorization;
production database;
production worker;
secure deployment;
self-observability;
reliability tests;
security documentation;
deployment documentation.
Validation

Perform failure and security scenarios:

Unauthorized user
      ↓
Access denied
Worker failure
      ↓
Recovery/retry
      ↓
No duplicate authoritative result
Database failure
      ↓
System reports infrastructure failure
      ↓
No fake evaluation result
Dependencies

P3.

Risks
deployment-specific failures;
leaked secrets;
incorrect authorization;
treating infrastructure failures as AI failures;
overengineering security infrastructure.
Human decisions required
authentication provider;
hosting providers;
production retention/configuration policies.
Exit criteria

The system is publicly accessible, authenticated, operationally observable and able to survive the defined failure/security scenarios.

Definition of complete

EvalOps is a functioning deployed system, not merely a local development project.

Next-phase dependency

P5 requires P4.

P5 — RELEASE, CI EVALUATION GATES & PORTFOLIO VALIDATION
Purpose

Demonstrate the central production-engineering value of EvalOps.

The final question is:

Can EvalOps prevent a bad AI change from silently reaching production?

Why it exists

This is the bridge between:

AI evaluation

and:

AI software engineering
Prerequisites

P4 complete.

Inputs
complete evaluation engine;
regression detection;
production deployment;
GitHub;
CI architecture.
Objectives

Implement the final release workflow:

Code change
     ↓
CI
     ↓
AI evaluation
     ↓
Quality threshold
     ├── PASS
     └── FAIL
Major workstreams
CI evaluation gate

Connect real EvalOps evaluation to CI.

Quality thresholds

Use configured, meaningful thresholds.

Example:

Groundedness ≥ 90%

Result:
87%

❌ Evaluation failed
Regression blocking

A degraded evaluation can warn/block according to the configured policy.

GitHub integration

Produce a developer-readable result.

Documentation

Finalize:

architecture;
evaluation methodology;
metric definitions;
judge methodology;
dataset examples;
API documentation;
setup;
deployment;
threat model;
limitations;
technical decisions;
roadmap.
Demo

Build the complete demonstration.

Expected artifacts

The final demo should demonstrate:

Register application
       ↓
Create/upload dataset
       ↓
Run evaluation
       ↓
Inspect results
       ↓
Compare versions
       ↓
Detect regression
       ↓
Open failed case
       ↓
Inspect trace
       ↓
Inspect cost/latency
       ↓
Run CI gate
       ↓
PASS / FAIL
Validation

The final system must use real evaluation data.

No fabricated dashboard values.

The CI failure must correspond to an actual evaluation threshold violation.

Dependencies

P4 production system.

Risks
fake CI integration;
hardcoded thresholds;
demo-specific hacks;
documentation diverging from implementation;
claiming capabilities that were not actually tested.
Human decisions required
final CI threshold policy;
release demonstration scenario;
portfolio presentation scope.
Exit criteria

The entire intended product workflow works from AI application through evaluation to release decision.

Definition of complete

EvalOps satisfies the project's final Definition of Done and can be technically defended.

Next phase dependency

None.

P5 is the terminal phase of the approved v1.0 roadmap.

Dependency Graph

The authoritative dependency graph is intentionally linear at the phase level:

                    ┌──────────────────────┐
                    │ P0                   │
                    │ Foundation           │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ P1                   │
                    │ Evaluation Core      │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ P2                   │
                    │ Integration +        │
                    │ Observability        │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ P3                   │
                    │ Evaluation           │
                    │ Intelligence + UI    │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ P4                   │
                    │ Security + Reliability│
                    │ + Production         │
                    └──────────┬───────────┘
                               │
                               ▼
                    ┌──────────────────────┐
                    │ P5                   │
                    │ CI Gates + Release   │
                    │ + Portfolio          │
                    └──────────────────────┘

This is deliberately not a calendar.

A phase can take longer or shorter than expected without changing the dependency structure.

Cross-Phase Dependency Logic

The deeper dependency is:

Trustworthy Data
      ↓
Evaluation
      ↓
Execution Evidence
      ↓
Analysis
      ↓
Production Reliability
      ↓
Release Automation

Or, more simply:

Can we measure it?
       ↓
Can we explain it?
       ↓
Can we compare it?
       ↓
Can we trust it?
       ↓
Can we automate decisions from it?

That is the actual progression of EvalOps.

Critical Gates

There are five hard gates.

Gate 1 — Foundation Gate

Before P1:

Can the complete development stack run?

Gate 2 — Evaluation Truth Gate

Before P2:

Can EvalOps produce a genuine evaluation result from a real AI execution?

If not, stop.

Gate 3 — Investigation Gate

Before P3:

Can we connect evaluation outcomes to execution evidence?

If not, the observability architecture is not functioning.

Gate 4 — Production Trust Gate

Before P5:

Is the deployed system secure, reliable and observable?

If not, do not claim production readiness.

Gate 5 — Final Product Gate

Before declaring EvalOps complete:

Can a real AI change pass or fail a real evaluation-based release decision?

If not, the central production-engineering story remains incomplete.

Work That Must NOT Happen Early

The phase structure explicitly prevents these mistakes.

No dashboard-first development

P3 depends on P1/P2.

No LLM-judge-first development

Deterministic evaluation establishes the evaluation foundation first.

No Kubernetes

No phase requires it.

No vector database

No phase requires it.

No fake CI

CI evaluation gates come only after real evaluation and regression detection exist.

No fake metrics

Every metric originates from actual execution data.

No production claims before P4

Deployment alone does not equal production readiness.

Final Phase-to-Architecture Mapping
Architecture Area	Primary Phase
Repository	P0
Runtime foundation	P0
Frontend foundation	P0
Backend foundation	P0
PostgreSQL	P0
Redis/Celery	P0
Dataset architecture	P1
Evaluation runs	P1
Scorer architecture	P1 → P3
Target application integration	P1/P2
Trace/span architecture	P2
OpenTelemetry	P2
Token/cost measurement	P2
Failure handling	P2 → P4
Semantic evaluation	P3
LLM judge	P3
Human calibration	P3
Experiment comparison	P3
Regression detection	P3
Dashboard	P3
Authentication	P4
Authorization	P4
Security	P4
Production deployment	P4
Self-observability	P4
Reliability	P4
CI evaluation gates	P5
Final documentation	P5
Portfolio/demo	P5
Full Consistency Check Against FINAL ARCHITECTURE
System context

Target AI application ↔ EvalOps boundary is established in P1/P2.

PASS

High-level architecture

Frontend → API → evaluation → worker → database is established in P0–P2.

PASS

Component architecture

Project, dataset, application, evaluation, scorer, metrics and trace responsibilities are progressively implemented.

PASS

Data architecture

Relational entities originate in P1 and become operational before P3 analysis.

PASS

Application architecture

Presentation/API/domain/persistence boundaries begin in P0 and mature through P1–P3.

PASS

Integration architecture

Target-AI integration is established before advanced evaluation analysis.

PASS

Security architecture

Security is deliberately completed before final production/release claims.

PASS

Authentication/authorization

Explicitly placed in P4 because it is not needed to construct the evaluation engine but is required for public production use.

PASS

AI/LLM architecture

LLM provider abstraction exists before provider-specific judge implementation.

PASS

Storage

PostgreSQL established in P0/P1.

No unnecessary secondary datastore introduced.

PASS

API

API foundation in P0; real resource operations in P1/P2.

PASS

Deployment

Deployment follows product functionality rather than preceding it.

PASS

Observability

Target AI observability is P2; EvalOps self-observability is P4.

PASS

Testing

Testing begins in P0 and becomes increasingly meaningful through P1–P5.

PASS

Failure handling

Failure classification begins with evaluation execution and is hardened during productionization.

PASS

Trust boundaries

Target AI, evaluator, external LLM, user and database boundaries are preserved throughout the plan.

PASS

Data flows

Dataset → run → application → result → metrics is implemented before dashboard analysis.

PASS

Control flows

Worker and evaluation lifecycle precede CI release control.

PASS

External dependencies

Providers are introduced only where required.

PASS

Internal interfaces

Scorer, provider, persistence, worker and telemetry boundaries are respected.

PASS

Major invariants

Real data, no fabricated metrics, evaluator humility, idempotency and failure distinction are validation criteria throughout the phases.

PASS

Architectural constraints

No unnecessary:

Kubernetes;
Kafka;
vector database;
MongoDB;
microservice decomposition;
dedicated analytics infrastructure.

PASS

Final Master Phase Structure
P0
FOUNDATION
│
│  Make the system runnable.
▼
P1
EVALUATION CORE
│
│  Make the system measure real AI behavior.
▼
P2
INTEGRATION + OBSERVABILITY
│
│  Make failures explainable.
▼
P3
EVALUATION INTELLIGENCE + PRODUCT
│
│  Make the evidence useful for engineering decisions.
▼
P4
RELIABILITY + SECURITY + PRODUCTION
│
│  Make the system trustworthy.
▼
P5
CI GATES + RELEASE + PORTFOLIO
│
│  Make evaluation part of the software lifecycle.
▼
DONE
The governing principle

The phases are not six arbitrary project-management buckets.

They represent six technical dependencies:

Foundation → Measurement → Explanation → Decision → Trust → Automation

That is the minimum progression needed to transform the current repository into the EvalOps defined by the Constitution and Definition of Done.

This Phase Plan v1.0 is now the authoritative phase structure. Any change to phase boundaries, dependencies, objectives or exit criteria should be treated as an explicit change request rather than an informal roadmap edit.