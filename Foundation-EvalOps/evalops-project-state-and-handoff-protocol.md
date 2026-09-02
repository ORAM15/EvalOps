FINAL PROJECT STATE & HANDOFF PROTOCOL — EvalOps

Project: EvalOps
Version: 1.0
Status: FINAL
Purpose: Ensure any fresh AI session can safely resume EvalOps without relying on hidden conversational memory.

This protocol is the continuity layer of the project.

The fundamental rule is:

Conversation memory is not project state. Repository/GitHub state is project state.

A fresh AI must be able to reconstruct the current engineering situation from authoritative artifacts before touching code.

1. The Canonical State Model

EvalOps should maintain one canonical project-state representation:

PROJECT_STATE
│
├── Project identity
├── Current phase
├── Current checkpoint
├── Completed checkpoints
├── Active work
├── Blocked work
├── Failed attempts
├── Open decisions
├── Required approvals
├── Latest validation
├── Repository state
├── Known defects
├── Assumptions
├── Risks
└── Next permitted action

The state model exists to answer one question:

“If I know nothing about the previous conversation, what am I legally and technically allowed to do next?”

2. Source-of-Truth Hierarchy

A fresh AI must use the following authority order:

                    PROJECT AUTHORITY
                           │
                           ▼
              Approved project documents
                           │
             ┌─────────────┴─────────────┐
             ▼                           ▼
        Requirements                 Architecture
             │                           │
             └─────────────┬─────────────┘
                           ▼
                  Technology Specification
                           │
                           ▼
                    Master Phase Plan
                           │
                           ▼
                  Checkpoint System
                           │
                           ▼
                Engineering Operating Policy
                           │
                           ▼
                 Quality & Evaluation System
                           │
                           ▼
                 GitHub Engineering Workflow
                           │
                           ▼
                    PROJECT_STATE
                           │
                           ▼
               Current repository state
                           │
                           ▼
                    AI inference

AI inference is the lowest authority.

The AI may interpret state but cannot redefine it.

3. PROJECT_STATE

PROJECT_STATE is the canonical snapshot of the project's current condition.

Recommended location:

docs/project/PROJECT_STATE.md

It should remain concise enough that a fresh AI can read it quickly.

Required structure
# PROJECT_STATE

## Project
EvalOps

## State Version
<version>

## Last Updated
<date>

## Current Phase
<Px — Phase Name>

## Current Checkpoint
<Px-Cy — Checkpoint Name>

## Project Status
<PLANNED / IN PROGRESS / BLOCKED / COMPLETE>

## Completed Checkpoints
- <checkpoint>
- <checkpoint>

## Active Work
- <work item>
- <work item>

## Blocked Work
- <blocker>
- <required decision>

## Failed Attempts
- <attempt>
- <result>
- <lesson>

## Open Decisions
- <decision ID>
- <decision required>

## Required Approvals
- <approval>
- <owner>

## Latest Validation
- <validation ID>
- <result>
- <date>

## Repository State
- Branch:
- Working tree:
- Latest commit:
- PR:
- CI:

## Known Defects
- <defect>

## Known Risks
- <risk>

## Assumptions
- <assumption>

## Next Permitted Action
<exact next action>

## Forbidden Next Actions
- <action>
- <action>
4. Why NEXT PERMITTED ACTION Matters

This is one of the most important fields.

A fresh AI should not merely learn:

“We are on P1.”

It should know:

“P1-C4 is accepted; P1-C5 is next; create branch cp/P1-C5-... and implement only the deterministic scorer.”

Therefore the state must explicitly define:

NEXT PERMITTED ACTION

and, where useful:

FORBIDDEN NEXT ACTIONS

This dramatically reduces autonomous scope creep.

5. DECISION_LOG

Location:

docs/project/DECISION_LOG.md

This records decisions that affect engineering direction.

A decision must be distinguishable from a proposal.

Structure
# DECISION LOG

## DEC-001

### Decision
<decision>

### Context
<why the decision was needed>

### Alternatives
- ...
- ...

### Selected
<chosen approach>

### Reason
<why>

### Consequences
<impact>

### Requirements Affected
<IDs>

### Architecture Affected
<yes/no>

### Approved By
<owner>

### Date
<date>

### Status
APPROVED

Possible statuses:

PROPOSED
UNDER REVIEW
APPROVED
REJECTED
SUPERSEDED

Only APPROVED decisions may be treated as authoritative decisions.

6. CHECKPOINT_LOG

Location:

docs/project/CHECKPOINT_LOG.md

This provides the historical execution record.

Unlike PROJECT_STATE, which describes now, CHECKPOINT_LOG describes what happened.

Example:

# CHECKPOINT LOG

| ID | Status | Branch | PR | Validation | Accepted |
|---|---|---|---|---|---|
| P0-C1 | ACCEPTED | ... | ... | VAL-001 | Yes |
| P0-C2 | ACCEPTED | ... | ... | VAL-002 | Yes |
| P0-C3 | ACCEPTED | ... | ... | VAL-003 | Yes |
| P1-C1 | IN PROGRESS | ... | ... | ... | No |

For each checkpoint, the log should link to its authoritative Issue/PR/evidence.

7. CHECKPOINT_LOG vs PROJECT_STATE

They must not become duplicates.

PROJECT_STATE
     ↓
"What is true NOW?"

CHECKPOINT_LOG
     ↓
"What happened BEFORE and what is accepted?"

Example:

State
Current checkpoint:
P1-C4
Log
P0-C1 ✓
P0-C2 ✓
P0-C3 ✓
P0-C4 ✓
P0-C5 ✓
P1-C1 ✓
P1-C2 ✓
P1-C3 ✓
P1-C4 IN PROGRESS
8. VALIDATION_RECORD

Validation evidence must be represented independently from general project state.

Location:

docs/project/validation/

Each significant validation receives an ID.

Example:

VAL-P1-C4-001

Structure:

# VALIDATION RECORD

## Validation ID
VAL-P1-C4-001

## Checkpoint
P1-C4

## Objective
<what is being proven>

## Environment
<environment>

## Commit
<commit SHA>

## Dataset
<dataset/version>

## Test / Procedure
<exact validation>

## Expected Result
<expected>

## Actual Result
<actual>

## Evidence
<logs/results/screenshots/links>

## Result
PASS / FAIL / BLOCKED

## Known Limitations
<limitations>

## Validator
<who/what>

## Date
<date>
9. Validation Records Must Be Immutable Evidence

Once a validation record has been used to accept a checkpoint, it should not be casually rewritten.

If the implementation changes and validation becomes invalid:

Old validation
      ↓
superseded
      ↓
New validation

Do not silently edit history to make old validation appear successful.

10. HANDOFF_RECORD

The HANDOFF_RECORD is designed specifically for a fresh AI session.

Location:

docs/project/HANDOFF.md

Its purpose is different from PROJECT_STATE.

PROJECT_STATE is the canonical state.

HANDOFF is the resume briefing.

It should answer:

“What does the incoming AI need to know before continuing?”

11. HANDOFF_RECORD Structure
# EVALOPS HANDOFF RECORD

## Handoff Version
<version>

## Generated
<date>

## Current Phase
<Px>

## Current Checkpoint
<Px-Cy>

## Last Accepted Checkpoint
<Px-Cy>

## Current Objective
<one paragraph>

## What Is Already Working
- ...
- ...

## What Was Recently Changed
- ...
- ...

## What Was Validated
- ...
- ...

## Known Failures
- ...
- ...

## Failed Attempts
- ...
- ...

## Open Decisions
- ...
- ...

## Required Approvals
- ...
- ...

## Repository State
- Branch:
- Working tree:
- Latest commit:
- CI:
- Open PR:

## Known Defects
- ...

## Important Constraints
- ...

## Do Not Do
- ...

## Next Permitted Action
<exact action>

## Resume Procedure
<reference to protocol>
12. HANDOFF Is Not Conversational Memory

The handoff must never say:

“As we discussed earlier…”

Instead:

“P1-C4 is currently in validation. The worker executes evaluation cases but duplicate-result behavior remains unresolved.”

Everything necessary must be explicit.

13. Notebook vs GitHub/Repository

This distinction is critical.

GitHub / Repository = Authoritative Engineering State

The following belong in the repository/GitHub:

Requirements
Architecture
Technology decisions
Checkpoint definitions
Decision records
Validation records
Project state
Checkpoint history
Known defects
Engineering documentation
API documentation
Evaluation methodology
Security documentation
Deployment documentation

These must be version-controlled and reviewable.

14. Notebook / Reference Material

Notebook material can contain:

learning notes;
explanations;
exploratory research;
personal understanding;
temporary experiments;
interview preparation;
conceptual diagrams;
scratch analysis;
observations that have not yet become project decisions.

Notebook material is not authoritative.

For example:

Notebook:
"I think Celery might be useful."

does not mean:

Technology Specification:
Celery = APPROVED
15. Promotion Rule

Information moves from notebook/reference material into project authority only after appropriate validation/approval.

Idea
 ↓
Exploration
 ↓
Proposal
 ↓
Review
 ↓
Decision
 ↓
Repository record

Never:

Notebook thought
      ↓
AI assumes decision
16. GitHub vs Repository Files

GitHub provides workflow state.

Repository files provide project knowledge.

GitHub

Best for:

Issues;
PRs;
reviews;
CI;
branches;
commits;
releases;
discussions;
checkpoint status.
Repository

Best for:

architecture;
requirements;
state;
decisions;
validation;
methodology;
documentation.

Together:

GitHub workflow
       +
Repository knowledge
       ↓
Complete project state
17. Canonical State Ownership

Each category should have one authoritative home.

Information	Authority
Requirements	Repository
Architecture	Repository
Technology decisions	Repository
Current checkpoint	PROJECT_STATE + GitHub Issue
Checkpoint status	GitHub Issue + CHECKPOINT_LOG
Implementation	Git branch
Code history	Git
Review state	GitHub PR
CI state	GitHub Actions
Validation	VALIDATION_RECORD
Decisions	DECISION_LOG
Defects	GitHub Issue + project state
Future ideas	Issue / roadmap
Learning notes	Notebook
Temporary exploration	Notebook / local scratch
18. State Update Rules

PROJECT_STATE must be updated whenever a material state transition occurs.

Examples:

Checkpoint started
Checkpoint blocked
Checkpoint failed
Checkpoint accepted
Decision approved
Architecture changed
Major defect discovered
Validation superseded
PR merged
Phase gate passed

The state should not be updated for trivial activity.

19. State Transition Example

Suppose P1-C4 is completed.

Before:

CURRENT CHECKPOINT
P1-C4

STATUS
VALIDATING

After successful acceptance:

CURRENT CHECKPOINT
P1-C5

COMPLETED
P1-C4

LATEST VALIDATION
VAL-P1-C4-001 — PASS

NEXT PERMITTED ACTION
Begin P1-C5

This is what allows the next AI to resume safely.

20. Failed Attempts

Failed attempts are valuable project knowledge.

They must not disappear simply because the final implementation works.

Record:

Attempt
Hypothesis
Action
Result
Root cause
Lesson

Example:

Attempt:
Execute all evaluation cases synchronously.

Result:
Large evaluation runs blocked the API request.

Lesson:
Long-running evaluation execution belongs in background processing.

This prevents the next AI from repeating the same experiment.

21. Open Decisions

Every unresolved decision should have an identifier.

Example:

DEC-OPEN-001

The state should contain:

Question
Why it matters
Options
Current recommendation
Decision owner
Blocking checkpoints

An AI must not convert:

Recommendation

into:

Approved decision

without authorization.

22. Known Defects

Defects must distinguish severity and state.

Example:

DEF-012

Problem:
Duplicate worker retry can produce duplicate telemetry.

Severity:
Medium

Affected:
P2-C5

Status:
OPEN

Workaround:
None

Blocks:
P2-C6

The AI must check blockers before proceeding.

23. Repository State Record

The canonical state should record:

Current branch
Working tree status
Latest commit
Open PR
CI status
Uncommitted changes
Known divergence

Example:

Branch: cp/P1-C4-evaluation-run
Working tree: CLEAN
Latest commit: abc1234
PR: #27
CI: PASS

This does not replace git status.

It is a state snapshot.

Before acting, the AI must verify the snapshot against the actual repository.

24. Fresh AI Resume Sequence

This is the exact sequence a new AI agent must follow.

STEP 0 — Do Nothing

Before touching code:

Do not modify anything.

STEP 1 — Identify Repository

Confirm:

repository
remote
current directory

Verify it is the EvalOps repository.

STEP 2 — Inspect Git State

Run/inspect:

git status
git branch
git log
git remote -v

Determine:

current branch;
clean/dirty state;
latest commit;
divergence;
existing work.
STEP 3 — Read Project State

Read:

docs/project/PROJECT_STATE.md

This establishes the claimed current state.

STEP 4 — Read Handoff

Read:

docs/project/HANDOFF.md

This gives the incoming agent the latest resume context.

STEP 5 — Read Current Checkpoint

Open the corresponding GitHub Issue and checkpoint definition.

Determine:

Objective
Prerequisites
Allowed work
Out-of-scope work
Acceptance criteria
Dependencies
Required approval
STEP 6 — Verify Checkpoint Status

Never trust the state file blindly.

Cross-check:

PROJECT_STATE
+
GitHub Issue
+
CHECKPOINT_LOG
+
Git branch/PR

If they disagree:

STOP.

Resolve the discrepancy before modifying code.

25. STEP 7 — Read Relevant Authority

Read only the relevant portions of:

Requirements
Architecture
Technology Specification
Quality System
Engineering Contract
GitHub Workflow

Do not reread the entire project unnecessarily.

But the agent must understand any rule directly affecting the checkpoint.

26. STEP 8 — Inspect Existing Implementation

Before changing anything:

repository structure
relevant source files
tests
configuration
interfaces
database models
related implementation

Determine:

What already exists?

27. STEP 9 — Inspect Previous Validation

Read the latest relevant:

VALIDATION_RECORD

Determine:

what has already been proven;
what remains unproven;
what failed previously;
what must not be repeated.
28. STEP 10 — Inspect Failed Attempts

If previous attempts exist:

read them
understand them
do not repeat blindly

A fresh agent should inherit lessons, not just files.

29. STEP 11 — Inspect Open Decisions

Check whether the checkpoint depends on an unresolved decision.

If yes:

BLOCKED

unless an explicitly approved decision already exists.

30. STEP 12 — Determine Permitted Action

The agent must now be able to state:

I am working on:
<Px-Cy>

I am allowed to:
<bounded work>

I am not allowed to:
<out-of-scope>

I must prove:
<acceptance criteria>

My next permitted action is:
<one action>

Only after this is established may code modification begin.

31. STEP 13 — Inspect Before Modify

Now inspect the exact files that will change.

Then:

PLAN

The plan should be proportional to the work.

No massive speculative implementation plan is necessary for a small change.

32. STEP 14 — Make Bounded Change

Implement only the checkpoint's allowed work.

Preserve working behavior.

Do not:

redesign;
upgrade unrelated dependencies;
refactor unrelated code;
modify architecture;
modify API contracts;
modify schema outside authorization.
33. STEP 15 — Validate

Execute the validation required by the checkpoint.

Not:

“The code looks correct.”

But:

Test
+
Runtime
+
Integration
+
Evaluation

as applicable.

34. STEP 16 — Record Evidence

Create/update the appropriate:

VALIDATION_RECORD

with actual evidence.

35. STEP 17 — Update State

Only after validation:

Update:

PROJECT_STATE
CHECKPOINT_LOG
HANDOFF

as appropriate.

36. STEP 18 — GitHub Workflow

Then:

Commit
 ↓
Push
 ↓
PR
 ↓
Review
 ↓
Acceptance
 ↓
Merge

according to the GitHub Engineering Workflow.

37. STEP 19 — Determine Next State

After acceptance, explicitly identify:

Completed checkpoint
Current phase
Next checkpoint
Next permitted action

The agent must not automatically begin the next checkpoint unless authorized by the project's operating workflow.

38. STEP 20 — Handoff

If the session ends before project completion, create/update the handoff record.

The next AI should be able to start at:

PROJECT_STATE
+
HANDOFF
+
CURRENT CHECKPOINT

without asking:

“What did we do previously?”

39. Resume Protocol in One Diagram
                FRESH AI SESSION
                       │
                       ▼
                 DO NOT CODE
                       │
                       ▼
              Verify repository
                       │
                       ▼
                 git status
                       │
                       ▼
              Read PROJECT_STATE
                       │
                       ▼
               Read HANDOFF
                       │
                       ▼
            Read current checkpoint
                       │
                       ▼
        Verify GitHub checkpoint state
                       │
                       ▼
          Read relevant authority docs
                       │
                       ▼
          Inspect existing implementation
                       │
                       ▼
         Inspect previous validation
                       │
                       ▼
          Inspect failed attempts
                       │
                       ▼
           Inspect open decisions
                       │
                ┌──────┴──────┐
                ▼             ▼
             CONSISTENT    INCONSISTENT
                │             │
                ▼             ▼
              PROCEED        STOP
                │
                ▼
         Determine next action
                │
                ▼
          Bounded modification
                │
                ▼
             VALIDATE
                │
         ┌──────┼──────┐
         ▼      ▼      ▼
       PASS   FAIL   BLOCKED
         │      │      │
         ▼      ▼      ▼
      Evidence Diagnose Owner
         │      │      │
         └──────┼──────┘
                ▼
          Update state
                │
                ▼
          GitHub workflow
                │
                ▼
            ACCEPTANCE
                │
                ▼
          Next checkpoint
40. Resume Safety Invariants

A fresh AI must never violate these invariants.

Invariant 1

No code modification before state reconstruction.

Invariant 2

No checkpoint work without satisfied prerequisites.

Invariant 3

No progression based solely on conversational memory.

Invariant 4

No acceptance without validation evidence.

Invariant 5

No architecture change without authorization.

Invariant 6

No unresolved state contradiction may be silently ignored.

Invariant 7

No failed attempt may be repeated without understanding what was learned.

Invariant 8

No state file may override actual Git/repository reality without investigation.

41. Canonical Project Continuity Model

The final system becomes:

                    EVALOPS
                       │
        ┌──────────────┼──────────────┐
        ▼              ▼              ▼
   PROJECT DOCS      GITHUB          CODE
        │              │              │
        ▼              ▼              ▼
 Requirements       Issues         Branches
 Architecture       PRs            Commits
 Decisions          CI             Implementation
        │              │              │
        └──────────────┼──────────────┘
                       ▼
                 PROJECT STATE
                       │
              ┌────────┴────────┐
              ▼                 ▼
        VALIDATION           HANDOFF
              │                 │
              └────────┬────────┘
                       ▼
                  FRESH AI
                       │
                       ▼
               SAFE RESUMPTION
42. Final Definitions
PROJECT_STATE

The authoritative snapshot of what is true about the project right now.

DECISION_LOG

The authoritative history of approved engineering decisions and their consequences.

CHECKPOINT_LOG

The authoritative history of checkpoint progression and acceptance.

VALIDATION_RECORD

The evidence record proving whether a specific behavior or checkpoint was actually validated.

HANDOFF_RECORD

The concise operational briefing that allows a fresh AI session to resume without hidden conversational memory.

43. Final Rule

The most important rule of the entire protocol is:

A fresh AI does not inherit authority from the previous AI. It inherits only verified project state.

Therefore:

Previous AI said:
"Done."

          ≠

Current AI may assume:
"Done."

Instead:

Previous implementation
        ↓
GitHub/repository state
        ↓
Validation evidence
        ↓
Checkpoint acceptance
        ↓
PROJECT_STATE
        ↓
Fresh AI
        ↓
Verify
        ↓
Continue

That makes EvalOps session-independent, auditable, recoverable and safe for autonomous continuation.

This FINAL PROJECT STATE & HANDOFF PROTOCOL v1.0 is now the authoritative continuity mechanism for EvalOps. A new AI session must execute the defined resume sequence and verify repository/GitHub state before touching implementation.