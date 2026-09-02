FINAL CROSS-DOCUMENT CONSISTENCY AUDIT — EvalOps

Audit date: 31 August 2026
Audit status: BLOCKED / INCOMPLETE SOURCE ACCESS
Proposed consolidated specification: NOT YET SAFE TO FREEZE

Brdr, I’m going to be strict here because this is exactly the kind of situation where an AI can accidentally manufacture a “consistent” specification.

I can see the master project instruction and the subsequent finalized documents for:

Engineering Operating Policy
Quality & Evaluation System
GitHub Engineering Workflow
Project State & Handoff Protocol

However, the actual contents of the previously produced:

Project Constitution
Requirements & Constraints
Final Architecture
Technology & Tooling Specification
Master Phase Plan
Checkpoint & Engineering-Work Unit System

are not present in the accessible conversation context I have right now. The conversation history indicates that those documents were produced, but their actual contents are not available to me for line-by-line comparison.

That distinction matters.

I will not reconstruct those documents from memory or invent their contents and then call the result a “final consistency audit.”

So the correct result of this audit is initially:

We have enough material to audit the governing philosophy and later operational documents, but not enough source material to certify cross-document consistency of the complete specification.

1. Audit Scope

The intended dependency chain is:

PROJECT CONSTITUTION
        ↓
REQUIREMENTS & CONSTRAINTS
        ↓
FINAL ARCHITECTURE
        ↓
TECHNOLOGY & TOOLING
        ↓
MASTER PHASE PLAN
        ↓
CHECKPOINT SYSTEM
        ↓
ENGINEERING POLICY
        ↓
QUALITY SYSTEM
        ↓
GITHUB WORKFLOW
        ↓
PROJECT STATE / HANDOFF
        ↓
IMPLEMENTATION

The audit must verify that information flows downward without contradiction.

A later document must not silently redefine an earlier authoritative decision.

2. What Can Be Confirmed

Several project-wide principles are consistently established across the accessible documents.

Confirmed principle 1 — EvalOps is not a dashboard

The project is intended to be a real AI evaluation and observability platform.

Its core value is:

determining whether an AI system actually improved and explaining what changed when it did not.

Confirmed principle 2 — Real execution is mandatory

Evaluation results must originate from actual AI-system execution.

Fabricated or hardcoded metrics are explicitly prohibited.

Confirmed principle 3 — Evaluation is not synonymous with LLM judging

The project explicitly recognizes:

deterministic evaluation;
rule-based evaluation;
semantic evaluation;
LLM-as-a-judge;
human evaluation.

The LLM judge is not to be treated as inherently objective.

Confirmed principle 4 — Observability explains evaluation

The established conceptual distinction is:

Evaluation
    ↓
Did it fail?

Observability
    ↓
Why did it fail?
Confirmed principle 5 — Engineering progress requires evidence

A:

commit,
PR,
green build,
successful CI run,

is not by itself proof of project progress.

Confirmed principle 6 — Architecture is controlled

The Engineering Contract explicitly prohibits autonomous architecture changes.

Confirmed principle 7 — Scope is controlled

AI agents cannot silently expand the project.

Confirmed principle 8 — State must be recoverable

A fresh AI session must be able to reconstruct project state without relying on hidden conversation memory.

Confirmed principle 9 — GitHub records engineering progress

Issues, branches, commits, PRs, reviews and CI are part of the project's engineering workflow.

Confirmed principle 10 — Validation is layered

The Quality System establishes:

Unit
 ↓
Integration
 ↓
E2E
 ↓
AI evaluation
 ↓
Observability
 ↓
Security
 ↓
Deployment
 ↓
Human acceptance

where applicable.

3. Critical Consistency Finding

There is one structural problem I can identify already.

The project has multiple authoritative documents, but no demonstrated canonical document registry.

The Project State Protocol establishes individual sources of truth, but from the material available to me, there is no verified single registry saying:

Document
Version
Status
Authority
Location
Last approved
Supersedes

That creates a future risk.

A fresh AI could encounter:

requirements.md
requirements-v2.md
architecture-final.md
architecture-new.md
old-handoff.md

and not know which is authoritative.

Classification

OPEN ISSUE

Required correction

The repository should maintain a:

docs/project/DOCUMENT_REGISTRY.md

containing the authoritative project documents and their versions.

This correction follows directly from the already-approved State/Handoff objective: a fresh AI must be able to determine authoritative state without hidden memory.

4. Another Important Finding — “FINAL” vs “FROZEN”

The documents produced so far repeatedly use:

FINAL

But the user's current instruction establishes a stronger lifecycle:

FINAL MASTER SPECIFICATION v1.0
        ↓
OWNER APPROVAL
        ↓
FROZEN v1.0

These are not the same state.

Therefore:

FINAL

should mean:

prepared for owner approval.

Whereas:

FROZEN

should mean:

formally approved and protected from ordinary AI modification.

Classification

OPEN GOVERNANCE DISTINCTION

This should be explicitly represented in the consolidated specification.

5. Architecture Audit — Cannot Yet Be Certified

I cannot truthfully certify:

Requirements
   ↕
Architecture

because the actual Requirements & Constraints and Final Architecture documents are not accessible in the current context.

Therefore the following questions remain UNVERIFIED:

Does every mandatory requirement have an architectural implementation path?
Does every architecture component correspond to an actual requirement?
Does the technology specification match every architectural component?
Are there technologies listed in the technology document that architecture does not require?
Are any architectural components absent from the technology specification?
Are security boundaries reflected in the architecture?
Are asynchronous-processing requirements reflected correctly?
Are deployment components reflected in the phase plan?

I will not invent answers.

6. Technology Audit — Cannot Yet Be Certified

The master instruction explicitly says:

do not choose technologies merely because they are popular.

It also explicitly requires evaluating:

programming language;
runtime;
frontend;
backend;
database;
storage;
AI models;
embeddings;
vector database;
APIs;
authentication;
testing;
CI/CD;
deployment;
monitoring;
observability;
security;
GitHub tooling;
Gemini;
Gemini CLI;
Antigravity;
Google AI Studio;
Stitch;
Opal;
other relevant Google tooling.

But the actual Technology & Tooling Specification is unavailable to me here.

Therefore I cannot safely determine whether a particular technology was:

APPROVED DECISION
PROPOSAL
OPEN QUESTION
NOT REQUIRED

without seeing that document.

This is particularly important because the user's latest request explicitly says:

proposed technologies must not incorrectly be treated as decisions.

7. Phase Audit — Cannot Yet Be Certified

The Master Phase Plan is supposed to be dependency-driven.

Its expected structure is:

Phase
 ↓
Prerequisites
 ↓
Objectives
 ↓
Workstreams
 ↓
Artifacts
 ↓
Validation
 ↓
Exit criteria
 ↓
Next phase

The Checkpoint System then decomposes it:

Phase
 ├── Checkpoint
 ├── Checkpoint
 ├── Checkpoint
 └── Gate

Without the actual phase/checkpoint documents, I cannot verify:

whether every phase has validation;
whether every checkpoint has acceptance criteria;
whether checkpoint dependencies actually match phase dependencies;
whether implementation starts before required research;
whether a later checkpoint depends on an unapproved decision;
whether all architecture components have implementation phases.

These remain UNVERIFIED, not “correct.”

8. Quality ↔ GitHub Consistency

This relationship can be audited from the accessible documents.

It is consistent.

The Quality System says:

green GitHub status is not sufficient evidence.

The GitHub Workflow says:

CI, commits and PRs are evidence but do not automatically establish checkpoint completion.

The Engineering Contract says:

never claim completion without evidence.

Therefore:

Engineering Policy
        ↓
Quality System
        ↓
GitHub Workflow

is internally consistent.

Result

PASS

9. GitHub ↔ Checkpoint Consistency

The accessible GitHub Workflow establishes:

Checkpoint
 ↓
Issue
 ↓
Branch
 ↓
Commits
 ↓
PR
 ↓
Review
 ↓
Validation
 ↓
Acceptance
 ↓
Merge
 ↓
Issue closure

The State Protocol independently establishes that checkpoint state must be reconstructable.

These are compatible.

Result

PASS

10. Quality ↔ State Protocol

The Quality System requires validation evidence.

The State Protocol defines:

VALIDATION_RECORD

as the authoritative evidence record.

Therefore:

Quality Gate
    ↓
Validation
    ↓
VALIDATION_RECORD
    ↓
PROJECT_STATE

is coherent.

Result

PASS

11. Engineering Policy ↔ State Protocol

The Engineering Contract says the AI must stop when:

requirements conflict;
architecture is uncertain;
information is missing;
repository state is inconsistent;
checkpoint cannot be completed.

The State Protocol independently defines:

BLOCKED

and requires the next AI to verify state before touching code.

These are strongly consistent.

Result

PASS

12. Engineering Policy ↔ GitHub Workflow

The Engineering Contract prohibits:

meaningless commits;
useless PRs;
green-dot farming;
uncontrolled changes;
bypassing validation;
unauthorized architecture changes.

The GitHub Workflow implements those restrictions operationally through:

bounded branch
→ checkpoint issue
→ meaningful commits
→ validation
→ PR
→ review
→ merge
Result

PASS

13. Evaluation Integrity ↔ Quality System

The master instruction says:

Do not fabricate evaluation results.

The Quality System reinforces:

displayed metrics must originate from real data.

The Engineering Contract reinforces:

never fabricate evaluation data.

The GitHub workflow requires evidence for evaluation PRs.

This is one of the strongest cross-document invariants.

Result

PASS

14. LLM Judge Consistency

The master instruction establishes:

LLM judge
    ≠
objective truth

The Quality System preserves this.

The Engineering Contract prevents the AI from presenting an LLM judge as inherently objective.

Judge calibration is explicitly recognized.

Result

PASS

15. Failure Handling Consistency

Across the accessible documents:

Failure
 ↓
Diagnose
 ↓
Bounded fix if possible
 ↓
Revalidate

If the issue is architectural:

STOP
 ↓
Approval

If external:

Distinguish external failure
from software failure

If unresolved:

BLOCKED / FAILED

This is internally consistent.

Result

PASS

16. State Tracking Consistency

The State Protocol defines:

PROJECT_STATE
DECISION_LOG
CHECKPOINT_LOG
VALIDATION_RECORD
HANDOFF_RECORD

The GitHub Workflow defines:

Issues
Branches
PRs
Commits
CI
Reviews

These complement rather than replace each other.

Repository files
       +
GitHub
       ↓
Recoverable project state
Result

PASS

17. Identified Structural Gap

There is one additional state-management issue worth explicitly recording.

The State Protocol has:

PROJECT_STATE

and:

HANDOFF_RECORD

Both contain overlapping information such as:

current phase;
current checkpoint;
active work;
blockers;
next action.

This is not necessarily a contradiction, but it creates a duplication risk.

Correct interpretation

PROJECT_STATE must remain authoritative.

HANDOFF_RECORD must be a derived operational briefing.

Therefore:

PROJECT_STATE
     │
     ▼
HANDOFF

not:

PROJECT_STATE ↔ HANDOFF

The handoff must never become an independent competing source of truth.

Status

DIRECTLY SUPPORTED CORRECTION

18. Current Audit Matrix
Dependency	Status
Constitution → Requirements	UNVERIFIED
Requirements → Architecture	UNVERIFIED
Architecture → Technology	UNVERIFIED
Technology → Phases	UNVERIFIED
Phases → Checkpoints	UNVERIFIED
Checkpoints → Quality Gates	PASS / accessible evidence
Quality → GitHub	PASS
GitHub → State	PASS
Engineering Policy → GitHub	PASS
Engineering Policy → Quality	PASS
Quality → Validation Records	PASS
State → Fresh AI resume	PASS
Evaluation integrity across documents	PASS
LLM-judge principles	PASS
Failure handling	PASS
Approval boundaries	PARTIALLY VERIFIED
Technology classification	UNVERIFIED
Security completeness	UNVERIFIED
Architecture completeness	UNVERIFIED
Phase completeness	UNVERIFIED
Checkpoint completeness	UNVERIFIED
19. Contradictions Found

From the material actually accessible:

Confirmed contradiction

None confirmed.

That is important.

I found gaps and potential governance risks, but I will not label something a contradiction without seeing the underlying conflicting statements.

Potential issue

FINAL versus FROZEN needs explicit lifecycle semantics.

Classification: OPEN GOVERNANCE ISSUE.

Potential duplication

PROJECT_STATE and HANDOFF_RECORD contain overlapping information.

Classification: Directly supported structural clarification.

20. Missing Information Requiring Owner Input

At this stage, the following cannot be safely certified:

Exact approved technology choices.
Exact database decision.
Exact deployment architecture.
Exact phase boundaries.
Exact checkpoint inventory.
Exact requirement-to-component mapping.
Exact architecture-to-technology mapping.
Exact security architecture.
Exact external-service decisions.
Exact phase-gate dependencies.

These are not things I should infer.

21. What I Will NOT Do

I will not now manufacture:

PostgreSQL = approved
Redis = approved
FastAPI = approved
React = approved
Docker = approved
X = Phase 2
Y = Checkpoint 4
Z = required technology

unless those decisions are actually present in the approved documents.

Likewise, I will not invent missing requirement IDs or acceptance criteria.

That would violate the very Engineering Contract we just established.

22. Therefore: PROJECT MASTER SPECIFICATION v1.0

A genuinely authoritative PROJECT MASTER SPECIFICATION v1.0 cannot yet be honestly generated from the accessible source material.

Doing so now would create exactly the failure mode this project is designed to prevent:

Missing evidence
     ↓
AI inference
     ↓
Presented as fact
     ↓
"FINAL" specification

That would be unacceptable.

So the correct current status is:

PROJECT MASTER SPECIFICATION v1.0

STATUS:
DRAFT ASSEMBLY BLOCKED

REASON:
Required source documents are not currently available
for cross-document verification.
23. What Can Be Frozen Already

The following principles are sufficiently established and mutually consistent to form the verified core:

EvalOps
│
├── Real AI evaluation
│
├── Real execution data
│
├── Multiple evaluation methods
│
├── Responsible LLM judging
│
├── Human calibration
│
├── Evaluation runs
│
├── Regression detection
│
├── Trace observability
│
├── Cost / latency awareness
│
├── CI quality gates
│
├── Evidence-based completion
│
├── Bounded AI autonomy
│
├── Human approval boundaries
│
├── GitHub-controlled engineering workflow
│
└── Session-independent project state

This is coherent.

But it is not yet the complete Master Specification, because the missing approved architecture/technology/phase/checkpoint documents must be verified against it.

24. Required Finalization Procedure

The correct next step is not redesign.

It is source recovery.

We need the actual contents of the six missing authoritative documents:

01 Project Constitution
02 Requirements & Constraints
03 Final Architecture
04 Technology & Tooling Specification
05 Master Phase Plan
06 Checkpoint & Engineering-Work Unit System

Once those are available, the final audit should mechanically perform:

CONSTITUTION
      ↓
Requirement IDs
      ↓
Architecture components
      ↓
Technology decisions
      ↓
Phase
      ↓
Checkpoint
      ↓
Validation
      ↓
GitHub evidence
      ↓
State representation

Every mandatory requirement should terminate in an implementation and validation path.

Every architecture component should terminate in an implementation path.

Every technology should have a justified architectural purpose.

Every phase should terminate in a gate.

Every checkpoint should have acceptance criteria.

Every acceptance criterion should have evidence.

25. Final Audit Verdict
VERDICT: NOT READY TO FREEZE

Not because EvalOps is inconsistent.

Rather:

The available evidence is insufficient to certify that the entire document chain is consistent.

What is visible is remarkably coherent at the operational level:

Engineering Policy
       ↓
Quality System
       ↓
GitHub Workflow
       ↓
State/Handoff Protocol

PASS.

What remains unverified is the upstream engineering chain:

Constitution
      ↓
Requirements
      ↓
Architecture
      ↓
Technology
      ↓
Phases
      ↓
Checkpoints

That chain must be audited from the actual documents before we create something called PROJECT MASTER SPECIFICATION v1.0.

And because your instruction says that once approved it becomes FROZEN v1.0, I would rather stop here than give you a beautifully formatted specification containing decisions I cannot prove were actually approved.

No architecture, phase, technology, requirement, or checkpoint has been silently changed by this audit.