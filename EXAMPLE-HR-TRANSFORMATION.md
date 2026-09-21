---
title: Example HR Transformation Delivery
description: A production-oriented walkthrough of the AI product lifecycle harness for an enterprise HR transformation
author: AI Product Engineering
ms.date: 2026-09-20
ms.topic: tutorial
keywords:
  - HR transformation
  - AI product delivery
  - responsible AI
  - enterprise pilot
  - HR service delivery
estimated_reading_time: 20
---

# Example HR Transformation Delivery

## Scenario

The Head of HR asks:

> Transform HR with AI. We have several promising use cases, but we need a safe way to identify the right problem, design the solution, and deliver a production pilot that can become a supported enterprise product.

The harness must not respond with a chatbot demo. It must create an evidence trail from the business problem through production operation.

This walkthrough assumes a fictional enterprise with:

* 25,000 employees in multiple countries
* Workday as the HR system of record
* ServiceNow HR Service Delivery for cases
* SharePoint and an HR knowledge base for policies
* Microsoft Entra ID for identity and group-based access
* Azure as the approved cloud platform
* Microsoft Teams as the primary employee channel
* Existing security, privacy, legal, works council, accessibility, and responsible AI review processes

These assumptions are placeholders. The discovery phase must replace them with verified facts.

## Phase 0: Turn the executive request into a discovery mission

The product lead creates an opportunity record, not an implementation ticket.

### Discovery mission

Determine which HR problem should receive the first production slice, what measurable outcome it can improve, which users and data are involved, and what controls are required to operate it safely.

### Discovery questions for the Head of HR

The discovery or design-thinking agent asks questions in rounds and records answers. It does not generate a solution after one prompt.

#### Business outcome

* Which HR outcome is currently below target?
* Is the priority employee experience, HR operating cost, manager effectiveness, retention, compliance, or speed?
* What executive metric would change if we solved this?
* What is the cost of leaving the problem unchanged for twelve months?

#### Users and workflow

* Which employees, managers, HR service agents, recruiters, or HR business partners experience the problem?
* Where does the current process start and end?
* Which steps are manual, repeated, delayed, or inconsistent?
* What do users do when the process fails today?

#### Evidence

* How many cases, requests, or transactions occur each month?
* What are the top request categories and failure reasons?
* What is the current response time and first-contact resolution rate?
* Can we inspect anonymized examples and representative edge cases?
* Which teams have already tried to solve this?

#### Risk and authority

* Does the use case influence employment, pay, promotion, performance, discipline, hiring, termination, or access to benefits?
* Which countries and worker groups are in scope?
* What data classifications apply?
* What must always remain a human decision?
* Which actions may the system take, and which require approval?

#### Adoption and change

* Who owns the process after launch?
* Which groups will sponsor and challenge the pilot?
* What behavior must change for the benefit to appear?
* What training and support capacity exists?

### Discovery artifacts

The harness creates:

* `opportunity-hr-transformation.md`: request, sponsor, outcome hypothesis, and scope.
* `hr-discovery-research.md`: interviews, workflow evidence, baselines, and unresolved questions.
* `hr-context.md`: shared language, systems, roles, policy concepts, and domain boundaries.
* `hr-risk-register.md`: initial privacy, employment, security, safety, and adoption risks.

Recommended harness behavior:

* Use the Matt Pocock `grill-with-docs` pattern for iterative questioning and durable vocabulary.
* Use HVE-Core research discipline. Research is read-only and records evidence rather than inventing requirements.
* Use a design-thinking agent to facilitate user-centered discovery, but require a human product owner to accept the problem statement.

## Phase 1: Compare candidate use cases

Assume the Head of HR proposes three candidates.

### Candidate A: Employee HR policy and process guidance

Employees and managers ask questions about leave, benefits, workplace policies, and HR processes. The service provides grounded answers with citations and routes unresolved questions to HR Service Delivery.

Potential value:

* Faster answers
* Lower repetitive case volume
* More consistent policy interpretation

Primary risks:

* Outdated or conflicting policy content
* Country or worker-group misapplication
* Disclosure of personal information
* Users treating guidance as a binding employment decision

### Candidate B: HR case triage and routing

The system classifies incoming HR cases, extracts structured fields, suggests routing, and identifies missing information. A human HR service agent approves the classification and routing.

Potential value:

* Lower queue time
* Better first assignment
* More consistent categorization

Primary risks:

* Sensitive employee-relations content
* Misclassification of urgent or protected cases
* Inappropriate access across HR teams
* Hidden bias in routing or prioritization

### Candidate C: Internal mobility and learning recommendations

The system recommends roles, learning, or career resources based on employee interests and skills.

Potential value:

* Increased internal movement
* Better learning participation
* Improved employee experience

Primary risks:

* Inference of sensitive attributes
* Unequal visibility into opportunities
* Perceived or actual employment decisions by algorithm
* Use of performance or manager data without a clear purpose

### Prioritization decision

The first production slice should be Candidate A, with a narrow scope:

> Provide cited, country-aware answers to a defined set of employee HR policy questions and offer a controlled handoff to HR Service Delivery when confidence, authorization, or policy coverage is insufficient.

This is not a claim that Candidate A is risk-free. It is selected because:

* The user value is easy to measure.
* A human and existing service process remain available.
* The first version can prohibit employment decisions and write actions.
* The knowledge boundary can be constrained.
* A production pilot can be launched for one country, one employee population, and a curated policy corpus.

The other candidates remain opportunities. They do not enter implementation until their own problem, risk, and evaluation evidence is ready.

## Phase 2: Business requirements document

The business requirements document must be approved before architecture work is considered complete.

### Product objective

Reduce avoidable HR policy and process questions reaching HR service agents while improving the speed and consistency of answers for employees and managers.

### In-scope users

* Employees in the pilot country
* People managers in the pilot country
* HR service agents receiving escalated cases

### Out of scope

* Hiring, promotion, performance, compensation, discipline, termination, or eligibility decisions
* Personalized legal advice
* Interpretation of an employee's private case history
* Automatic updates to Workday or ServiceNow
* Answers outside the approved policy corpus
* Cross-country answers when the user's country is unknown or unsupported

### User journey

1. An authenticated employee asks a question in Teams.
2. The service determines the user's country, worker population, and authorization context.
3. The service retrieves approved policy passages from the country-scoped corpus.
4. The model produces a concise answer with citations, effective dates, and a clear disclaimer.
5. The user can provide feedback or request HR assistance.
6. A handoff creates a ServiceNow HR case only after the user confirms the handoff and the allowed fields are shown.
7. The system records the interaction, result, safety signals, and handoff outcome without retaining unnecessary message content.

### Business acceptance measures

Baseline these measures before the pilot:

* Median time to answer for in-scope questions
* Percentage of cases in the selected categories
* First-contact resolution rate
* Employee answer usefulness score
* Citation correctness rate
* Unsupported-answer or escalation rate
* Policy freshness failures
* Cost per resolved interaction

Initial pilot targets must be approved by HR and operations. Example thresholds:

* At least 85% citation correctness on the golden evaluation set
* At least 95% of unsupported or ambiguous questions abstain or route to a human
* Zero critical privacy or unauthorized-disclosure incidents
* No material regression in HR case handling time
* At least 70% positive usefulness ratings from pilot users

The thresholds are examples, not defaults. The product owner must set them using the baseline and risk assessment.

## Phase 3: Architecture and delivery requirements

### Reference architecture

```text
Teams
  -> Entra ID and conditional access
  -> HR AI gateway
       -> policy and authorization enforcement
       -> country and worker-population resolver
       -> retrieval service
       -> approved model endpoint
       -> response validator and citation checker
       -> feedback and handoff workflow
  -> ServiceNow HRSD, explicit user confirmation required

Approved policy sources
  -> ingestion pipeline
  -> classification, redaction, effective-date checks
  -> country-scoped indexes

All components
  -> audit events, quality telemetry, cost telemetry, alerts, kill switch
```

### Architectural requirements

* The model must not access Workday or ServiceNow directly.
* All tool access must pass through an internal gateway with identity, authorization, rate limiting, schema validation, and audit logging.
* Retrieval must enforce country, worker population, audience, effective date, and document approval status.
* The answer must contain citations to retrieved passages or abstain.
* Prompt injection in source documents must be treated as untrusted content.
* The system must not infer protected attributes or use employee case history for general policy answers.
* ServiceNow case creation must require explicit confirmation and write only an approved minimum field set.
* The service must support an immediate kill switch and a deterministic fallback to the existing HR channel.
* Every model, prompt, policy index, retrieval configuration, validator, and tool schema must be versioned.
* Logs must be privacy-reviewed, access-controlled, redacted, retained for a defined period, and exportable for incident investigation.

### Delivery requirements

* Infrastructure must be deployed through the enterprise CI/CD path.
* All changes require code review, automated tests, evaluation regression checks, and provenance.
* The pilot must use a real production tenant and approved production data boundaries, not a mock environment.
* The pilot must have an on-call owner, support runbook, incident severity definitions, and rollback procedure.
* Accessibility, localization, records management, legal, privacy, and works council requirements must be assessed for the selected country.

## Phase 4: Research and solution validation

The research agent does not ask, "Which model should we use?" first. It answers decision-critical questions.

### Research questions

* Which approved HR content sources are authoritative?
* How are policy versions, effective dates, and country applicability represented?
* What Teams, Entra ID, Workday, and ServiceNow integration patterns are already approved?
* Which Azure model endpoints and data-processing regions are allowed?
* What existing HR virtual-agent capabilities can be configured instead of custom-built?
* What are the latency, throughput, and cost requirements?
* What audit, retention, and deletion obligations apply?
* Which employee populations and countries require additional consultation?
* What accessibility and language requirements apply?
* What are the safe failure modes when the user asks for a personal employment decision?

### Solution options

| Option | Decision |
| --- | --- |
| Configure existing HR service knowledge experience | Evaluate first. Prefer if it satisfies authorization, citation, evaluation, and telemetry requirements. |
| Build a custom retrieval and response service | Select only if the approved platform cannot meet the requirements. |
| Fine-tune a model | Do not select for the first slice. It adds governance and update complexity without demonstrated need. |
| Autonomous HR agent | Reject for the first slice. No autonomous external side effects are required. |
| Deterministic search with answer templates | Use as fallback and comparison baseline. |

The research record must cite vendor documentation, internal platform owners, security standards, integration constraints, and observed evidence. It must distinguish verified facts from assumptions.

## Phase 5: MCP servers, skills, plugins, and agents

### MCP policy

MCP is an adapter mechanism, not an authorization boundary. The model must never receive unrestricted access to internal systems.

Use only internally approved, version-pinned MCP servers behind the enterprise gateway:

| MCP capability | Access | Purpose | Restrictions |
| --- | --- | --- | --- |
| HR knowledge search | Read | Search approved policy passages | Country, audience, effective date, and approval filters are mandatory |
| Identity context | Read | Resolve user identity and pilot eligibility | Return only minimum claims needed for authorization |
| ServiceNow HRSD | Read and controlled create | Show case status and create confirmed handoff | No arbitrary query or update; schema allowlist and human confirmation |
| Telemetry | Write | Record metrics and audit events | No raw sensitive prompt content by default |
| Feature flag and kill switch | Read, controlled write | Control pilot exposure and disablement | Writes restricted to release operators |

Do not enable:

* Direct database MCP access
* Generic HTTP or shell MCP servers in production
* Workday write operations
* Tools that can execute arbitrary code
* Unreviewed community MCP servers

### Skills and plugins

Create an internal, pinned skill bundle with one canonical workflow per action:

* `hr-discovery`: facilitates stakeholder interviews and evidence capture.
* `hr-context`: maintains the shared HR vocabulary and system context.
* `hr-solution-design`: compares non-AI and AI options against constraints.
* `hr-research`: performs evidence-based platform and integration research.
* `hr-requirements`: writes business and delivery requirements.
* `hr-architecture`: creates architecture decisions and threat models.
* `hr-slice-plan`: produces executable work items and acceptance tests.
* `hr-evaluation`: manages golden sets, adversarial cases, rubrics, and regression results.
* `hr-security-review`: routes security, privacy, responsible AI, and legal checks.
* `hr-implement`: executes approved tasks with TDD and least privilege.
* `hr-release`: validates rollout, telemetry, support, rollback, and approvals.

Source patterns:

* HVE-Core supplies the evidence and lifecycle contracts.
* Superpowers supplies design, planning, TDD, isolated execution, and verification.
* Matt Pocock’s skills supply grilling, shared context, domain modeling, and specification.
* ECC supplies hooks, memory, audit concepts, and multi-harness installation patterns.
* agent-skills supplies role, review, security, testing, and ship patterns.

Every plugin and skill must be treated as software supply chain input:

* Pin the source revision.
* Scan content and dependencies.
* Review tool permissions.
* Test prompt behavior against known adversarial inputs.
* Assign an owner and support level.
* Promote changes through the same review process as application code.

### Agents and human accountability

Agents can prepare artifacts, analyze evidence, generate code, run bounded tests, and identify risks. They cannot approve their own work or waive enterprise controls.

Required human roles:

* Head of HR: outcome sponsor and business risk owner
* Product manager: scope, value, and acceptance owner
* HR domain lead: policy and workflow authority
* Enterprise architect: architecture and platform fit
* Security architect: threat model and control approval
* Privacy and legal: data use and employment-law review
* Responsible AI lead: impact assessment and fairness review
* Engineering lead: implementation and operational ownership
* HR operations lead: rollout, support, and process adoption

## Phase 6: Production implementation slices

The pilot is divided into production-shaped vertical slices.

### Slice 1: One-country cited policy answers

Deliver:

* Teams authentication and pilot allowlist
* One country and one employee population
* Curated policy corpus with owners and effective dates
* Retrieval with authorization filters
* Cited answer generation
* Abstention for unsupported questions
* Feedback capture and telemetry
* Kill switch and fallback link to the existing HR channel

This slice is eligible for a limited production pilot after all gates pass.

### Slice 2: Human-confirmed HR case handoff

Deliver:

* Handoff intent detection
* User-visible case summary for confirmation
* Minimum-field ServiceNow HRSD case creation
* Confirmation and audit event
* Failure recovery without duplicate case creation
* HR agent feedback loop

This slice requires a new release decision. Passing Slice 1 does not authorize it automatically.

### Slice 3: Additional countries and populations

Deliver one country or population at a time. Each expansion requires:

* Local policy corpus review
* Localization and accessibility review
* Privacy, legal, and works council assessment
* Country-specific evaluation set
* Separate rollout approval

## Phase 7: Enterprise gates

The slice cannot enter production because a demo looks good. It enters only when the evidence package is complete.

### Gate A: Problem and value

Approvers confirm:

* The problem is evidenced.
* The baseline is measured.
* The target and owner are explicit.
* The slice is smaller than the transformation ambition.
* The fallback preserves the current service.

### Gate B: Architecture and delivery

Approvers confirm:

* The selected solution is justified against alternatives.
* Integration contracts and ownership are explicit.
* Data flows and trust boundaries are documented.
* The stack can be operated by the named team.
* CI/CD, environments, observability, and rollback are ready.

### Gate C: Security, privacy, and responsible AI

Approvers confirm:

* Threat model and abuse cases are addressed.
* Data minimization, retention, access, and residency are approved.
* Prompt injection and untrusted document risks are tested.
* The system does not make prohibited employment decisions.
* Human oversight and escalation are real operational processes.
* Fairness, accessibility, localization, and user notice are addressed.

### Gate D: Evaluation

Approvers confirm:

* Golden and adversarial datasets are versioned.
* Retrieval, citation, abstention, and leakage thresholds pass.
* Tests cover country, worker population, policy version, and ambiguity boundaries.
* Human reviewers assessed usefulness and harm scenarios.
* Load, latency, cost, resilience, and degradation tests pass.

### Gate E: Production readiness

Approvers confirm:

* Feature flag and kill switch have been exercised.
* On-call, support, incident, and rollback procedures are staffed.
* Audit and telemetry dashboards work with production-like events.
* The pilot cohort and communication plan are approved.
* The product owner accepts only documented residual risk.

## Production pilot plan

The pilot is a real production deployment with a constrained cohort, not a prototype environment.

### Pilot shape

* One country
* One employee population
* One approved policy domain, such as leave and time off
* 500 to 1,000 invited users
* Existing HR service channel available at all times
* Four-week initial measurement period
* Daily operational review during the first week
* Weekly product, risk, and outcome review thereafter

### Rollout sequence

1. Deploy to the production tenant behind a disabled feature flag.
2. Run smoke, authorization, telemetry, and kill-switch checks.
3. Enable for internal HR users with approved test cases.
4. Enable for the first employee cohort.
5. Monitor quality, safety, adoption, and support signals.
6. Pause or roll back on defined triggers.
7. Expand only after the product and risk owners sign the next decision.

### Automatic pause triggers

* Any confirmed unauthorized disclosure
* Any critical citation or policy-version error
* Any evidence of prohibited employment decision support
* Repeated failure of authorization filters
* Material increase in HR service harm or case backlog
* Cost or latency exceeding the approved operating envelope
* Loss of auditability or monitoring

## Evidence package for the release decision

The release record links:

* Approved opportunity and business requirements
* Discovery and platform research
* Domain context and architecture decisions
* Data-flow diagram and threat model
* Privacy, legal, responsible AI, and security approvals
* Slice plan, code review, and CI results
* Evaluation datasets, rubrics, results, and regression comparison
* Production readiness checklist
* Rollout, support, rollback, and incident plans
* Named owners and residual risk acceptance

The release decision must state one of:

* Approve limited pilot
* Approve with conditions
* Hold for specific evidence
* Reject and return to discovery or design

## What this example proves

The harness is doing more than selecting a prompt or building an assistant:

* It converts an executive ambition into a measurable problem.
* It prevents three attractive use cases from becoming an uncontrolled program.
* It selects a bounded slice without pretending the risk is zero.
* It produces business, architecture, research, security, responsible AI, evaluation, and delivery artifacts.
* It maps tools and agents to explicit permissions and responsibilities.
* It delivers through the enterprise stack and operating model.
* It creates a production pilot that can be rolled back, supported, measured, and expanded.

The next real step is not implementation. It is to run the discovery interview with the actual Head of HR, HR operations, security, privacy, and employee representatives, then replace every assumption in this example with evidence.
