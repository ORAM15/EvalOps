# DECISION_LOG.md

The authoritative history of approved engineering decisions and their consequences.

## Approved Decisions

* **DEC-APP-001 (Project Identity):** EvalOps is the project/product identity.
* **DEC-APP-002 (Product Vision):** The product is about AI evaluation + observability, not merely dashboards.
* **DEC-APP-003 (Core Principle):** Real AI executions and real evaluation results are required.
* **DEC-APP-004 (Prohibition):** Fabricated metrics are prohibited.
* **DEC-APP-005 (Evaluation Constraint):** LLM judges must not be called objective.
* **DEC-APP-006 (Evaluation Constraint):** Automated evaluation should be treated critically.
* **DEC-APP-007 (Value Principle):** Metrics must have a meaningful engineering purpose.
* **DEC-APP-008 (Engineering Constraint):** Overengineering is prohibited.
* **DEC-APP-009 (Process Constraint):** Implementation should proceed incrementally based on checkpoints.
* **DEC-APP-010 (Milestone):** The first implementation milestone is the Real Evaluation Loop.
* **DEC-APP-011 (Repository):** The GitHub repository is the project's official repository.

## Open Decisions (Requiring Owner Approval)

* **DEC-OPEN-001 (Tech Stack):** Final technology stack. (Currently proposed: TypeScript, Next.js, Python, FastAPI, PostgreSQL, Redis)
* **DEC-OPEN-002 (Target App):** Exact first target AI application.
* **DEC-OPEN-003 (Dataset):** Initial golden dataset and its size/criteria.
* **DEC-OPEN-004 (Judge):** LLM judge model and rubric.
* **DEC-OPEN-005 (Methodology):** Calibration methodology, regression methodology, confidence/statistical requirements.
* **DEC-OPEN-006 (Contracts):** API style/contracts, database schema, trace representation.
* **DEC-OPEN-007 (Infrastructure):** Async-processing requirement for MVP, deployment providers, authentication requirement.
* **DEC-OPEN-008 (Licensing):** Open-source license selection.
