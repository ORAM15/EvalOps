TECHNOLOGY AND TOOLING SPECIFICATION

Project: EvalOps
Baseline: Final Architecture v1.0
Status: Technology/tooling baseline
Normative inputs: Final Architecture + Requirements & Constraints

1. Decision Framework

Every tool is classified as one of:

ALREADY CONFIGURED — demonstrably configured in this project.

AVAILABLE — available to the project/user through an existing environment or account, but not necessarily configured for EvalOps.

REQUIRES SETUP — needed/approved, but configuration has not yet occurred.

PROPOSED — useful candidate, but not required/approved as a project dependency.

NOT REQUIRED — explicitly unnecessary for the current architecture.

There is also an architectural status:

APPROVED DECISION
PROPOSAL
OPEN QUESTION
NOT REQUIRED

These are deliberately kept separate.

2. Programming Language
Python

Purpose: Backend, evaluation engine, scorers, workers and AI integrations.

Why needed: The architecture requires a backend evaluation engine, pluggable scorers, asynchronous evaluation and LLM integration. Python is a strong fit for those workloads.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Alternatives considered:

TypeScript/Node.js
Java
Go

Reason for selection: Python gives the evaluation layer direct access to the AI/ML ecosystem while remaining suitable for API and worker development.

Constraints:

Must not turn the backend into a collection of ML notebooks.
Evaluation logic must remain testable application code.
Python version must be pinned.

Installation/configuration:

Python 3.x
project virtual environment
dependency lock/requirements mechanism

Security: Dependencies must be pinned/reviewed; secrets remain outside source.

Cost: Open-source/runtime cost is effectively zero.

Operational: Backend and worker use the same Python ecosystem.

3. TypeScript

Purpose: Frontend.

Why needed: The architecture explicitly separates a web UI from backend/evaluation logic.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Alternatives:

JavaScript
Python frontend frameworks

Reason: Strong typing is useful for API contracts and a non-trivial developer-facing UI.

Constraints: TypeScript belongs primarily to the presentation layer.

Security: Never place backend secrets or LLM API keys in client-side TypeScript.

Cost: None.

4. Backend Framework — FastAPI

Purpose: HTTP API and backend application boundary.

Why needed: EvalOps needs APIs for projects, datasets, applications, runs, results and programmatic integrations.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Alternatives:

Flask
Django
Node.js/Express
Node.js/NestJS

Reason: FastAPI fits the Python evaluation engine while providing typed API contracts and async HTTP support.

Constraints: It must remain an API/application layer, not become the evaluation engine itself.

Security: Input validation, authentication and authorization at the API boundary.

Operational: Long-running evaluation work must go to the worker rather than blocking HTTP requests.

5. Frontend — Next.js

Purpose: EvalOps developer dashboard.

Why needed: Requirements require a usable frontend and final public deployment.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Alternatives:

React + Vite
Vue
SvelteKit

Reason: Next.js provides a mature React application framework suitable for a polished developer-facing product.

Constraints: No evaluation business logic should live exclusively in the frontend.

Security: Secrets never enter browser bundles.

Operational: Frontend communicates with the versioned backend API.

6. Database — PostgreSQL

Purpose: Primary system of record.

Why needed: The architecture contains strongly related entities:

Project
Application
Dataset
DatasetVersion
TestCase
Run
Result
Trace
Span

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Alternatives:

MongoDB
SQLite
Multiple databases
Elasticsearch/OpenSearch

Reason for selection: Relational integrity and traceability are central requirements.

Constraints:

One primary database initially.
No second database without architectural change approval.

Security:

Credentials outside source.
Network access restricted.
Least-privilege database credentials.

Cost: Local PostgreSQL is free; hosted PostgreSQL has provider-dependent cost.

Operational: Database migrations are mandatory.

7. SQLAlchemy

Purpose: Python database access/ORM layer.

Why needed: Keeps database access separate from application/domain logic.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Alternatives:

Raw SQL
SQLModel
Django ORM

Reason: Fits the Python/FastAPI architecture and provides explicit relational modeling.

Constraint: ORM abstractions must not hide important query/performance behavior.

8. Alembic

Purpose: Database schema migrations.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Why needed: Dataset/run/result schemas must evolve without destroying existing evaluation history.

Alternative: Manual SQL migrations.

Selection reason: Versioned schema migration fits reproducibility and maintainability requirements.

9. Redis

Purpose: Evaluation job coordination.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Why needed: The architecture deliberately separates long-running evaluation work from HTTP requests.

API
 ↓
Job
 ↓
Redis
 ↓
Worker

Alternatives:

Synchronous processing
PostgreSQL-based job polling
Kafka
RabbitMQ

Reason: Redis is sufficient for the currently established worker/job requirement without introducing event-stream infrastructure.

Constraints: Redis is not a second system of record.

Security: Authentication and network restrictions required in deployment.

Cost: Managed Redis may incur usage cost.

10. Celery

Purpose: Background evaluation execution.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Why needed: Evaluation runs can involve hundreds or thousands of test cases.

Alternatives:

FastAPI background tasks
RQ
Dramatiq
cloud-native job queues
synchronous execution

Reason: A dedicated worker abstraction matches the architecture's explicit run lifecycle.

Constraint: Do not add distributed workers before the evaluation workload actually needs them, but the architecture has reserved this boundary.

11. Docker

Purpose: Reproducible local/deployment environments.

Current status: REQUIRES SETUP

Architecture status: APPROVED DECISION

Why needed: Backend and worker need reproducible runtime environments.

Alternatives:

Native installation
provider-specific build environments

Reason: Containerization provides consistency without requiring Kubernetes.

Important constraint:

Docker is approved; Kubernetes is not.

12. AI Models

There are two separate model roles.

Target AI Model
      ≠
Judge Model
Target AI Model

Purpose: Produce the AI behavior EvalOps evaluates.

Status: REQUIRES SETUP / project-dependent.

Architecture status: OPEN QUESTION

The first target AI application/model has not yet been selected.

Judge Model

Purpose: Evaluate outputs where deterministic/rule-based evaluation is insufficient.

Status: PROPOSED.

Architecture status: OPEN QUESTION

The architecture requires LLM judging as a capability but does not mandate a specific provider/model.

13. OpenAI API

Purpose: Candidate initial LLM-judge provider.

Status: PROPOSED.

Architecture status: PROPOSAL

Why considered: The architecture requires an external LLM capability for judge-based evaluation.

Alternatives:

Gemini API
Anthropic API
local models
other hosted models

Reason for not marking it APPROVED: The requirements do not mandate OpenAI.

Constraint: EvalOps must use an LLM-provider abstraction.

Security:

API key server-side only.
Never expose in frontend.
Avoid sending sensitive evaluation data unnecessarily.

Cost: Judge calls incur model usage costs.

Operational: Provider availability, rate limits and model-version changes can affect reproducibility.

14. Gemini API

Purpose: Alternative LLM provider for target applications or judge models.

Google AI Studio is currently positioned as a direct way to obtain Gemini API access and API keys.

Status: PROPOSED.

Architecture status: PROPOSAL

Why considered: It provides another LLM provider without changing the core provider abstraction.

Alternatives: OpenAI, Anthropic, local models.

Selection: Not selected as the mandatory production provider.

Security: API key must remain server-side.

Cost: Depends on model and usage.

Operational: Model availability, pricing and limits must be tracked.

15. Embedding Models

Status: NOT REQUIRED initially.

Why: The approved requirements do not require a vector-search or retrieval subsystem inside EvalOps itself.

This is an important distinction.

A target AI application may use embeddings internally, but EvalOps does not therefore need to own an embedding model.

16. Vector Database

Status: NOT REQUIRED.

Candidates such as:

Pinecone
Qdrant
Weaviate
pgvector
Milvus

are deliberately excluded from the initial EvalOps architecture.

Why: No EvalOps requirement requires vector retrieval.

Introducing one would violate the "no unnecessary infrastructure" constraint.

17. API Architecture

Technology: REST/HTTP + JSON.

Status: REQUIRES SETUP.

Architecture status: APPROVED DECISION

Why needed:

dashboard communication;
target-application integration;
future SDK;
programmatic evaluation/trace ingestion.

Alternative: GraphQL, gRPC.

Reason for REST: The current requirements do not justify the additional complexity of GraphQL/gRPC.

API namespace:

/api/v1/
18. Authentication

The architecture requires authentication for the public production system, but does not select an identity provider.

Status: REQUIRES SETUP.

Architecture status: OPEN QUESTION

Possible alternatives:

Auth.js
Clerk
Auth0
Firebase Authentication
Supabase Auth
custom authentication

No provider should be installed yet merely because it is popular.

Machine-to-machine authentication

An API credential mechanism is required for application integration.

Human authentication

An authenticated session is required for dashboard access in the public deployment.

Authorization

Project-level access boundaries are mandatory.

19. Testing Toolchain

The testing architecture requires:

Unit
 ↓
Integration
 ↓
End-to-End
 ↓
Adversarial AI evaluation
pytest

Purpose: Python backend/evaluation tests.

Status: REQUIRES SETUP.

Architecture status: APPROVED DECISION

Alternatives: unittest, nose2.

Reason: Fits Python evaluation/worker testing.

Playwright

Purpose: End-to-end browser testing.

Status: PROPOSED.

Architecture status: PROPOSAL

Why: The final product has a real browser workflow.

Alternatives:

Cypress
Selenium

Reason for selection: Modern browser automation and good fit for a Next.js application.

It is not needed before the frontend exists.

Frontend unit testing

A specific library such as Vitest/Jest is:

Status: PROPOSED.

Architecture status: OPEN QUESTION

We should select it when frontend implementation begins based on actual component/testing needs.

20. CI/CD
GitHub Actions

Purpose:

automated tests;
linting;
type checking;
build validation;
eventually evaluation gates.

Status: REQUIRES SETUP.

Architecture status: APPROVED DECISION for CI capability

Alternatives:

GitLab CI
Jenkins
provider-native CI

Reason: EvalOps already uses GitHub as its source-control system.

Constraint: CI evaluation gates are DEFERRED as a product capability, but ordinary software CI should exist during development.

Important distinction:

Software CI
    ≠
AI Evaluation Gate

Ordinary CI can be established earlier.

AI-quality gating comes later.

21. GitHub

Purpose:

source control;
pull requests;
issues;
project history;
documentation;
CI/CD.

Status: ALREADY CONFIGURED

The repository ORAM15/EvalOps has already been created and connected locally. The main branch and initial foundation commit are established.

Architecture status: APPROVED DECISION

Alternatives: GitLab, Bitbucket.

Reason: Already established as the official project repository.

Security:

protect secrets;
review PRs;
use branch protections when appropriate;
avoid pushing .env.

Operational: Git history becomes part of the project's technical record.

22. GitHub CLI (gh)

Purpose:

issue management;
PR management;
repository automation;
CI inspection.

Status: PROPOSED / availability not verified.

Architecture status: PROPOSAL

Why useful: Makes GitHub workflows scriptable.

Alternative: GitHub web UI.

Decision: Not required for EvalOps runtime or initial implementation.

23. Jules

This one deserves special treatment.

Jules is Google's autonomous coding agent with GitHub integration; its current documentation says it can clone repositories into fresh cloud VMs, modify code, run tests and create PRs. It also has a CLI and API.

Purpose: AI-assisted development.

Status: REQUIRES SETUP if we choose to use it; otherwise PROPOSED.

Architecture status: PROPOSAL

Why useful: It can operate on the actual EvalOps GitHub repository and produce reviewable changes.

Alternatives:

Gemini CLI
Antigravity
Codex
manual development
GitHub Copilot

Reason for selection: Strong GitHub/PR-oriented workflow.

Critical constraint:

Jules is not part of EvalOps's production architecture.

It is a development tool.

Security:

Jules requires GitHub repository access. Its documentation describes scoped integrations and secure credential storage, but repository permissions still need to be treated as sensitive.

Operational: Jules operates in a cloud VM, so we should review generated diffs before merging.

Cost: Jules currently has multiple usage plans, including a no-cost tier, with paid higher-capacity plans.

24. Jules CLI

Purpose: Control Jules from terminal/automation.

Status: PROPOSED.

Architecture status: PROPOSAL

The official documentation currently supports installation through npm and commands such as jules login and jules remote ....

Installation if selected:

npm install -g @google/jules

Constraint: Do not install it merely because it exists.

We should first decide whether Jules adds value compared with Gemini CLI/Antigravity.

25. Gemini CLI

Purpose: Local AI-assisted development, repository analysis, implementation assistance and testing assistance.

Status: PROPOSED — availability on the user's machine is not verified.

Architecture status: PROPOSAL

Google's current documentation provides npm installation and Google-account authentication, and supports Windows/PowerShell with Node.js 20+.

Installation:

npm install -g @google/gemini-cli

Then:

gemini

Alternatives:

Jules
Antigravity
Codex
GitHub Copilot

Reason for consideration: Terminal-native workflow fits the existing PowerShell/Git workflow.

Security: Gemini CLI can execute tools/commands against the local environment, so trusted-folder/security controls matter. Its documentation explicitly covers execution security and trusted folders.

Cost: Current Gemini CLI access depends on account/license/usage limits.

Operational: Do not let an AI coding agent autonomously modify main without review.

26. Google Antigravity

Purpose: Agent-oriented software development environment.

Status: PROPOSED.

Architecture status: PROPOSAL

Google currently presents Antigravity as an agent-first development platform in its developer ecosystem.

Alternatives:

Gemini CLI
Jules
VS Code + AI extension
Codex

Reason for consideration: It may provide a more integrated agentic development workflow.

Important: It is a development tool, not an EvalOps runtime dependency.

Security: Agent access to the local repository must be treated as privileged.

Cost: Account/product-plan dependent; verify before committing to it.

Operational: Avoid simultaneously letting multiple autonomous agents modify the same branch.

27. Google AI Studio

Purpose:

Gemini experimentation;
prompt/evaluator prototyping;
model exploration;
API-key acquisition;
optional prototype generation.

Status: AVAILABLE as a web service, but no EvalOps-specific configuration has been established.

Architecture status: PROPOSAL as development tooling; NOT REQUIRED as runtime tooling.

Google describes AI Studio as a direct starting point for Gemini API development and API-key creation.

Alternatives:

direct Gemini API development;
OpenAI API;
local model environments.

Reason for consideration: Useful for rapidly testing evaluator prompts before encoding them into EvalOps.

Security: API keys must remain secret. AI Studio's build mode currently uses server-side secrets for Gemini API keys rather than exposing them in browser code.

Cost: Gemini API usage can incur cost depending on model/usage.

Operational: Experimental prompt work in AI Studio must eventually be transferred into version-controlled EvalOps evaluator configuration.

28. Gemini Web App

Purpose: General reasoning, research, prompt drafting and technical exploration.

Status: AVAILABLE as a general development aid.

Architecture status: PROPOSAL / development-only

Not required for EvalOps runtime.

It should not become the authoritative source of implementation decisions; those belong in the repository.

29. Stitch

Purpose: UI/UX ideation and high-fidelity interface design.

Status: PROPOSED

Architecture status: PROPOSAL

Google currently describes Stitch as an AI-native design canvas for high-fidelity UI generation, iteration, collaboration and prototypes. It can export designs toward development workflows.

Alternatives:

Figma
manual design
direct implementation in Next.js

Reason for consideration: EvalOps needs a polished developer dashboard, but Stitch can be used only for design exploration.

Constraint: Stitch designs must not become a second source of truth for application behavior.

Security: Do not upload sensitive production data to design tools.

Cost: Product/account dependent.

30. Opal

Purpose: Rapid AI mini-app/workflow experimentation.

Status: PROPOSED.

Architecture status: NOT REQUIRED for EvalOps implementation.

Google currently describes Opal-powered Gemini mini-apps as an experimental capability.

Why not required: EvalOps itself is the application being built. Creating another AI mini-app would not solve a required architectural problem.

Possible use: Educational/prototyping experimentation outside the runtime.

Security: Do not put sensitive EvalOps data into experimental tooling.

31. Google Cloud

Purpose: Potential future infrastructure.

Status: PROPOSED.

Architecture status: PROPOSAL

Google's current developer ecosystem positions Cloud Run as a deployment option for Gemini-built applications.

Alternatives:

Render
Fly.io
Railway
AWS
Azure
Vercel for frontend

Why not selected: The architecture requires public deployment, not a specific cloud.

32. Google Cloud Run

Purpose: Potential backend/worker hosting.

Status: PROPOSED.

Architecture status: PROPOSAL

Reason: Could provide container-based deployment without Kubernetes.

Alternative: Render.

Constraint: We should choose one deployment path rather than simultaneously building for every provider.

33. Supabase

Purpose: Potential hosted PostgreSQL.

Status: PROPOSED.

Architecture status: PROPOSAL

Why considered: Managed PostgreSQL could reduce operational burden.

Alternative: Neon, Railway, Render PostgreSQL, Cloud SQL.

Important: PostgreSQL is approved. Supabase is not.

34. Upstash

Purpose: Potential hosted Redis.

Status: PROPOSED.

Architecture status: PROPOSAL

Why considered: Managed Redis can avoid operating Redis ourselves.

Alternative: Redis Cloud, provider-managed Redis.

Important: Redis is architecturally approved; Upstash is not.

35. Monitoring

Monitoring has two levels.

Target AI systems
        ↓
EvalOps observability

EvalOps itself
        ↓
Operational monitoring

A specific monitoring vendor is:

OPEN QUESTION

Possible tools:

Grafana
Prometheus
provider-native monitoring
OpenTelemetry-compatible backend

We should not install all of them.

36. OpenTelemetry

Purpose: Traces/spans and telemetry standard.

Status: REQUIRES SETUP.

Architecture status: APPROVED DECISION

Why needed: Trace/span observability is explicitly part of the final architecture.

Alternatives:

custom telemetry;
vendor-specific tracing.

Reason: Standardized instrumentation avoids building our own tracing protocol.

Security: Telemetry can contain prompts, outputs, IDs and sensitive information. Instrumentation must control what is captured.

Operational: Sampling/retention decisions will matter as data volume increases.

37. Prometheus

Status: PROPOSED.

Architecture status: NOT REQUIRED initially

Why?

OpenTelemetry and application-level metrics can satisfy the current requirements without immediately introducing a separate Prometheus stack.

If deployment requirements later justify dedicated infrastructure monitoring, this can be reconsidered through a change request.

38. Grafana

Status: PROPOSED.

Architecture status: NOT REQUIRED initially

EvalOps's own product dashboard is not a substitute for infrastructure monitoring, but the current requirements do not mandate a Grafana deployment.

39. Security Tooling

No dedicated security platform is currently required.

The baseline is:

Secrets
 ↓
Environment/secret manager
 ↓
Backend only

and:

Internet
 ↓
Authentication
 ↓
Authorization
 ↓
API
 ↓
Data

Potential later tools:

secret manager;
SAST;
dependency scanning;
container scanning.

These are PROPOSED, not mandatory vendor choices.

40. Dependency Security

Recommended development controls:

dependency lockfiles;
automated vulnerability checks;
GitHub security alerts;
dependency updates through controlled PRs.

Specific third-party scanners remain:

PROPOSED

because the requirements mandate secure software, not a particular scanner.

41. Documentation Tooling
Markdown

Status: ALREADY CONFIGURED.

The repository foundation already uses Markdown documentation.

Architecture status: APPROVED.

Primary documentation belongs in Git.

Mermaid

Status: PROPOSED.

Useful for:

architecture diagrams;
data flows;
control flows;
sequence diagrams.

Not a runtime dependency.

OpenAPI

Status: APPROVED CONCEPT / REQUIRES SETUP.

FastAPI can expose API documentation from the backend.

Purpose:

API contracts;
developer integration;
debugging.
42. Design Tool
Stitch

Already covered above.

Classification: PROPOSAL.

It should remain a design aid, not a runtime dependency.

43. AI Development Tool Strategy

This is where I want us to be disciplined.

We do not need all of these simultaneously:

Jules
Gemini CLI
Antigravity
Google AI Studio
Gemini
Codex
Copilot
Stitch
Opal

That would violate the project's own anti-overengineering principle.

A sensible development-tool hierarchy is:

                    EvalOps Repository
                           │
              ┌────────────┴────────────┐
              │                         │
        Primary coding             Design/ideation
              │                         │
       Gemini CLI / Jules              Stitch
              │
              ▼
         GitHub PR
              │
              ▼
           CI tests

Only one or two AI coding agents should be used actively at a time.

44. Recommended Development Tool Roles
Primary AI coding agent

Jules OR Gemini CLI/Antigravity

Not all three simultaneously.

Jules

Best when the task should be:

Issue
 ↓
Agent
 ↓
Plan
 ↓
Code
 ↓
Tests
 ↓
PR

Jules explicitly supports GitHub repository workflows and PR generation.

Gemini CLI

Best when we are working directly inside:

PowerShell
+
local repository
+
terminal

It supports repository-aware terminal workflows and command execution.

Antigravity

Best candidate for agent-first IDE-based work.

But it remains PROPOSED until we actually evaluate whether it improves the workflow.

45. Recommended Tooling Policy

For EvalOps:

The AI agent is an assistant, not the architect.

The project constitution, requirements, architecture and repository documentation remain authoritative.

An AI agent must not:

silently change architecture;
add infrastructure because it is fashionable;
invent metrics;
fabricate test results;
commit secrets;
bypass tests;
modify requirements.
46. Local Development Toolchain

The minimum local environment should eventually be:

Windows
 │
 ├── Git
 ├── GitHub access
 ├── Python
 ├── Node.js
 ├── npm
 ├── Docker
 └── VS Code / equivalent IDE

Then:

EvalOps
 ├── Next.js
 ├── FastAPI
 ├── PostgreSQL
 ├── Redis
 └── Worker

AI development tools sit outside the runtime stack.

47. What Is Already Configured?

Based strictly on this workspace:

ALREADY CONFIGURED
Git
GitHub repository
EvalOps local repository
main branch
origin → ORAM15/EvalOps
Initial repository foundation
README.md
.gitignore
LICENSE

We have not established evidence that the following are installed/configured on the local machine:

Python
Node.js
Docker
PostgreSQL
Redis
Jules
Gemini CLI
Antigravity
Google AI Studio API key
OpenAI API key
GitHub CLI

So I will not pretend they are.

48. Available vs Configured

This distinction matters.

For example:

Google AI Studio exists and is publicly available.

That does not mean:

"EvalOps has a configured Gemini API."

Likewise:

Jules exists and can connect to GitHub.

That does not mean:

"Jules is configured for ORAM15/EvalOps."

And:

Gemini CLI can be installed through npm.

That does not mean:

"gemini is installed on your Windows machine."

49. Tool Classification Matrix
Tool	Role	Status	Architecture
Git	Source control	ALREADY CONFIGURED	APPROVED
GitHub	Repository/PR/CI	ALREADY CONFIGURED	APPROVED
Python	Backend/evaluation	REQUIRES SETUP	APPROVED
TypeScript	Frontend	REQUIRES SETUP	APPROVED
Next.js	Frontend	REQUIRES SETUP	APPROVED
FastAPI	Backend API	REQUIRES SETUP	APPROVED
PostgreSQL	Primary DB	REQUIRES SETUP	APPROVED
SQLAlchemy	DB layer	REQUIRES SETUP	APPROVED
Alembic	Migrations	REQUIRES SETUP	APPROVED
Redis	Job coordination	REQUIRES SETUP	APPROVED
Celery	Worker	REQUIRES SETUP	APPROVED
Docker	Packaging	REQUIRES SETUP	APPROVED
OpenTelemetry	Observability	REQUIRES SETUP	APPROVED
OpenAI API	LLM provider	PROPOSED	PROPOSAL
Gemini API	LLM provider	PROPOSED	PROPOSAL
Auth provider	Authentication	REQUIRES DECISION	OPEN QUESTION
GitHub Actions	CI	REQUIRES SETUP	APPROVED
Playwright	E2E	PROPOSED	PROPOSAL
pytest	Python testing	REQUIRES SETUP	APPROVED
GitHub CLI	GitHub automation	PROPOSED	PROPOSAL
Jules	AI coding agent	PROPOSED	PROPOSAL
Jules CLI	AI coding CLI	PROPOSED	PROPOSAL
Gemini CLI	AI coding CLI	PROPOSED	PROPOSAL
Antigravity	AI development	PROPOSED	PROPOSAL
Google AI Studio	AI prototyping	AVAILABLE	PROPOSAL
Gemini web app	AI assistance	AVAILABLE	PROPOSAL
Stitch	UI design	PROPOSED	PROPOSAL
Opal	AI mini-app prototyping	PROPOSED	NOT REQUIRED
Google Cloud	Hosting option	PROPOSED	PROPOSAL
Cloud Run	Hosting option	PROPOSED	PROPOSAL
Supabase	PostgreSQL hosting	PROPOSED	PROPOSAL
Upstash	Redis hosting	PROPOSED	PROPOSAL
Prometheus	Infrastructure metrics	PROPOSED	NOT REQUIRED initially
Grafana	Monitoring UI	PROPOSED	NOT REQUIRED initially
Embedding model	Retrieval	—	NOT REQUIRED
Vector DB	Retrieval	—	NOT REQUIRED
Kafka	Event streaming	—	NOT REQUIRED
Kubernetes	Orchestration	—	NOT REQUIRED
MongoDB	Secondary DB	—	NOT REQUIRED
Elasticsearch	Search/analytics	—	NOT REQUIRED
50. Exact Runtime Toolchain

The approved runtime/development foundation is:

                 ┌───────────────────────┐
                 │       Developer       │
                 └───────────┬───────────┘
                             │
                         Next.js
                       TypeScript
                             │
                             ▼
                     FastAPI / Python
                             │
             ┌───────────────┼───────────────┐
             │               │               │
             ▼               ▼               ▼
        PostgreSQL         Redis         External APIs
             │               │               │
             │               ▼               ├── Target AI
             │             Celery             │
             │               │               └── LLM Judge
             │               ▼
             │             Worker
             │               │
             └───────────────┘
                     │
                     ▼
                OpenTelemetry
51. Exact Development Toolchain

My recommended actual working setup is narrower:

Git
 +
GitHub
 +
VS Code
 +
Python
 +
Node.js/npm
 +
Docker
 +
ONE primary AI coding agent

For the AI agent, I recommend we evaluate Jules and Gemini CLI rather than install everything immediately.

Jules is particularly attractive for GitHub/PR-oriented autonomous work.

Gemini CLI is particularly attractive for our PowerShell/local-repository workflow and is currently installable through npm.

We do not need Jules + Gemini CLI + Antigravity all operating simultaneously.

52. Exact AI Toolchain

The project can use:

Research / reasoning
        │
        ├── Gemini
        └── ChatGPT
               │
               ▼
        Architecture decisions
               │
               ▼
        Repository specification
               │
               ▼
        AI coding agent
        ┌──────┼───────┐
        │      │       │
      Jules  Gemini  Antigravity
        │      │       │
        └──────┼───────┘
               │
               ▼
          GitHub PR
               │
               ▼
          CI validation

But the three coding agents are alternatives, not mandatory components.

53. Google Toolchain Decision
Google tool	EvalOps runtime?	Development use?	Status
Gemini API	No	Yes, if selected as provider	PROPOSAL
Gemini CLI	No	Yes	PROPOSAL
Jules	No	Yes	PROPOSAL
Jules CLI	No	Yes, optional	PROPOSAL
Antigravity	No	Yes, optional	PROPOSAL
Google AI Studio	No	Yes	AVAILABLE / PROPOSAL
Stitch	No	UI design	PROPOSAL
Opal	No	Not necessary	NOT REQUIRED
Google Cloud	No	Hosting option	PROPOSAL
Cloud Run	No	Hosting option	PROPOSAL
54. Important Security Boundary for AI Development Tools

AI coding agents receive access to project source code.

Therefore:

AI Agent
   │
   ├── Can read code
   ├── Can modify code
   ├── May execute commands
   └── May access configured integrations

They must not automatically receive:

Production database credentials
Production API keys
LLM provider secrets
Private user data

unless a specific task genuinely requires them.

This is particularly important because both Jules and Gemini CLI can operate beyond simple text generation: Jules runs tasks in cloud VMs and integrates with repositories, while Gemini CLI can execute local tools/commands.

55. Cost Architecture

We need to distinguish:

Free/open-source
Python
FastAPI
Next.js
PostgreSQL
Redis
Celery
SQLAlchemy
Alembic
OpenTelemetry
pytest
Git
Potential usage costs
LLM APIs
Hosted PostgreSQL
Hosted Redis
Cloud hosting
AI coding agents
No unnecessary paid infrastructure

We should not purchase:

Vector DB
GPU server
Kubernetes cluster
Dedicated observability SaaS
Enterprise IAM

without a requirement-driven reason.

56. Operational Rules
Rule 1

One source of truth:

GitHub repository + project documentation.

Rule 2

AI tools generate changes; Git/PR review validates them.

Rule 3

No AI agent changes architecture without an approved architecture change.

Rule 4

No AI-generated evaluation numbers are accepted without real execution.

Rule 5

No production secrets enter AI coding prompts or repository files.

Rule 6

No tool is installed simply because it is popular.

Rule 7

Every external service must have a documented purpose.

57. Definitive Toolchain Map
Layer 1 — Source Control
Git
  ↓
GitHub
  ↓
main / branches / PRs

Status: ALREADY CONFIGURED
Architecture: APPROVED

Layer 2 — Development Environment
Windows
 │
 ├── Python
 ├── Node.js
 ├── npm
 ├── Docker
 └── VS Code

Status: Python/Node/Docker/VS Code installation NOT VERIFIED
Architecture: APPROVED where specified

Layer 3 — Frontend
Next.js
   +
TypeScript

Status: REQUIRES SETUP
Architecture: APPROVED

Layer 4 — Backend
Python
   +
FastAPI
   +
SQLAlchemy
   +
Alembic

Status: REQUIRES SETUP
Architecture: APPROVED

Layer 5 — Evaluation
Evaluation Engine
       │
       ▼
Scorer Interface
       │
 ┌─────┼────────┐
 ▼     ▼        ▼
Exact Rules   LLM Judge

Status: REQUIRES SETUP
Architecture: APPROVED

Layer 6 — Async Execution
FastAPI
   ↓
Redis
   ↓
Celery Worker

Status: REQUIRES SETUP
Architecture: APPROVED

Layer 7 — Persistence
PostgreSQL

Status: REQUIRES SETUP
Architecture: APPROVED

Layer 8 — Observability
OpenTelemetry
      ↓
Traces / Spans / Metrics

Status: REQUIRES SETUP
Architecture: APPROVED

Layer 9 — AI Providers
Provider Interface
       │
   ┌───┴────┐
   ▼        ▼
OpenAI    Gemini

Status: PROVIDER NOT YET SELECTED
Architecture: OPEN QUESTION / PROPOSALS

Layer 10 — Authentication
User
 ↓
Authentication
 ↓
Authorization
 ↓
Project

Status: REQUIRES DESIGN/SETUP
Provider: OPEN QUESTION

Layer 11 — Testing
pytest
   +
API integration tests
   +
Playwright
   +
Adversarial evaluation tests

Status: pytest requires setup; Playwright proposed
Architecture: APPROVED testing strategy

Layer 12 — CI/CD
GitHub
   ↓
GitHub Actions
   ↓
Tests
   ↓
Build
   ↓
Validation

Later:

AI Evaluation
   ↓
Quality Gate

Software CI: REQUIRES SETUP / APPROVED
AI-quality gate: DEFERRED

Layer 13 — Deployment

Logical architecture:

Frontend
Backend
Worker
PostgreSQL
Redis

Candidate providers:

Vercel
Render
Supabase
Upstash
Cloud Run

Provider status: PROPOSAL

We will choose the actual providers after checking cost, limits, deployment requirements and the current student/project environment, not before.

Layer 14 — AI Development

Primary candidates:

Jules
Gemini CLI
Antigravity

Secondary:

Google AI Studio
Gemini
Stitch

Not needed:

Opal

All development-agent choices remain PROPOSAL until we explicitly select the workflow.

58. Final Technology Position

The approved technical foundation is therefore:

Python + FastAPI + PostgreSQL + SQLAlchemy + Alembic + Redis + Celery + Next.js + TypeScript + OpenTelemetry + Docker + GitHub/Git.

The not-yet-selected external choices are:

LLM provider, authentication provider, database hosting provider, Redis hosting provider, frontend hosting provider, backend/worker hosting provider, and primary AI coding agent.

The explicitly unnecessary technologies at this stage are:

Vector databases, embedding infrastructure owned by EvalOps, MongoDB, Elasticsearch/OpenSearch, Kafka, Kubernetes, graph databases, dedicated analytics warehouses, and unnecessary observability stacks.

And the most important tooling principle is:

Jules, Gemini CLI, Antigravity, Google AI Studio, Stitch and similar AI tools are development accelerators—not architectural dependencies of EvalOps.

That keeps the actual product architecture clean while still allowing us to use the best available AI-assisted development workflow.