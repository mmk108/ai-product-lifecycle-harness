---
title: AI Product Development Lifecycle Harness
description: Enterprise-oriented methodology for discovering, designing, building, evaluating, and operating AI products in releasable slices
author: AI Product Engineering
ms.date: 2026-09-20
ms.topic: concept
keywords:
  - AI product development
  - AI engineering
  - product lifecycle
  - evaluation
  - enterprise delivery
estimated_reading_time: 15
---

# AI Product Development Lifecycle Harness

## Executive direction

Build a governed delivery system, not a larger prompt library. The harness should make every AI product slice traceable from a verified business problem to measurable user value, tested behavior, security approval, production telemetry, and a decision to expand, change, or stop.

Use the cloned repositories as source material:

* [agent-skills](../agent-skills) contributes composable roles, commands, persona patterns, and evaluation ideas.
* [hve-core](../hve-core) contributes evidence-led Research, Plan, Implement, Review, and Follow-up workflows, artifact taxonomy, and governance.
* [ECC](../ECC) contributes multi-harness installation, lifecycle hooks, security guidance, memory, and harness audits.
* [superpowers](../superpowers) contributes mandatory design-before-code, executable plans, subagent isolation, TDD, and verification-before-completion.
* [mattpocock-skills](../mattpocock-skills) contributes customer alignment, grilling, shared domain language, domain modeling, specification, ticket slicing, and codebase reasoning.

Do not make any of these repositories an unreviewed production dependency. Pin selected artifacts, adapt them to your controls, and maintain an internal compatibility and ownership policy.

## Design principles

* Start with an outcome and a measurable business constraint, not a model or agent.
* Separate discovery evidence, product decisions, implementation plans, code changes, evaluations, and operational evidence.
* Require human approval at problem framing, solution selection, risk acceptance, and release readiness.
* Ship thin vertical slices that exercise the real user journey, data path, model behavior, controls, and telemetry.
* Treat prompts, tools, retrieval, policies, models, and workflows as versioned product components.
* Make unsafe, ungrounded, low-quality, or unobservable behavior fail closed.
* Prefer reversible decisions and short feedback loops over speculative architecture.
* Keep the harness provider-neutral at its core, with adapters for VS Code, Copilot, Claude, Codex, and other runtimes.

## The lifecycle

### 0. Intake and evidence boundary

Capture the request as an opportunity, not an implementation ticket.

Required outputs:

* Problem statement with affected users, current workflow, frequency, cost, and business consequence.
* Evidence record containing interviews, workflow observations, support data, process metrics, and representative examples.
* Explicit non-goals and assumptions.
* Data classification, regulatory context, and affected business owners.
* Initial success measures and a baseline measurement plan.

Exit gate:

* A product owner and domain expert agree that the problem is real, material, and worth investigating.

Recommended source patterns:

* Use `grill-me` and `grill-with-docs` from [mattpocock-skills](../mattpocock-skills) to expose ambiguity.
* Use an HVE-Core research artifact to record evidence and unresolved questions.
* Use ECC security guidance to identify prompt injection, sensitive data, and external tool risks before solution design.

### 1. Problem framing and opportunity contract

Turn evidence into an explicit contract for the slice.

Required outputs:

* One-sentence outcome statement.
* User personas and job-to-be-done.
* Current-state and target-state workflow.
* Quantified baseline, target, and measurement owner.
* Constraints, dependencies, and decision log.
* Failure impact and acceptable fallback behavior.

Exit gate:

* The team can explain who benefits, what changes, how success is measured, and what happens when the AI is wrong.

### 2. Solution space design

Explore alternatives before selecting an AI design. Include a non-AI option and a simpler automation option.

Evaluate at least:

* Process change without AI.
* Deterministic software or rules.
* Retrieval-augmented generation.
* Structured extraction or classification.
* Human-in-the-loop decision support.
* Agentic orchestration, only where tool use creates measurable value.

Required outputs:

* Options matrix covering value, feasibility, risk, latency, cost, data access, and reversibility.
* Context map and shared domain language.
* Domain model and key invariants.
* Architecture decision records for material choices.
* Threat model, misuse cases, and control strategy.
* Evaluation plan with representative, adversarial, and boundary cases.

Exit gate:

* A review group approves one bounded slice, its architecture, its evaluation criteria, and its risk posture.

Recommended source patterns:

* Use `CONTEXT.md` and domain modeling from [mattpocock-skills](../mattpocock-skills).
* Use brainstorming and plan-writing from [superpowers](../superpowers).
* Use HVE-Core architecture, security, and responsible AI artifacts.

### 3. Slice definition and executable plan

Define the smallest production-shaped increment that can prove or disprove the value hypothesis.

A slice must include:

* One primary user journey.
* Realistic data contracts and authorization boundaries.
* A deterministic fallback or human escalation path.
* Instrumentation for quality, safety, latency, cost, and adoption.
* Automated tests and evaluation fixtures.
* Rollback or feature-flag behavior.
* A named owner and explicit release decision.

Required outputs:

* Slice specification with acceptance criteria.
* Work items sized for one short delivery cycle.
* Executable implementation plan with files, commands, dependencies, and verification steps.
* Test and evaluation cases written before implementation.
* Release checklist and evidence locations.

Exit gate:

* Every work item has a reason, acceptance test, owner, dependency, and verification command.

Recommended source patterns:

* Use HVE-Core RPI planning and tracking.
* Use `to-spec` and `to-tickets` from [mattpocock-skills](../mattpocock-skills).
* Use Superpowers executable plans and isolated worktrees.

### 4. Build with feedback loops

Implement in small increments with a red, green, refactor loop.

The default execution loop is:

1. Select one plan task.
2. Write or update a failing unit, contract, integration, or evaluation test.
3. Implement the smallest change.
4. Run targeted checks.
5. Review the diff and evidence.
6. Record the result in the slice ledger.
7. Continue only when the task is green.

The harness should expose specialized roles for:

* Product and domain clarification.
* Architecture and threat modeling.
* Retrieval and data quality.
* Prompt and tool design.
* Test and evaluation authoring.
* Implementation.
* Security and privacy review.
* Independent product acceptance.
* Release and operations.

Recommended source patterns:

* Use Superpowers TDD, subagent-driven development, and verification-before-completion.
* Use ECC hooks for pre-tool, post-tool, session, compaction, and stop checks.
* Use agent-skills review, test, security, and ship command patterns.

### 5. Evaluation and acceptance

Do not equate passing software tests with a useful AI product. Use layered evidence.

Evaluation layers:

* Unit tests for deterministic logic.
* Contract tests for model, tool, retrieval, and schema boundaries.
* Golden-set evaluations for representative user cases.
* Adversarial evaluations for injection, data leakage, unsafe actions, and instruction conflicts.
* Human review for usefulness, correctness, tone, and workflow fit.
* Operational tests for latency, rate limits, cost, resilience, and degradation.
* Outcome measurement against the baseline from intake.

Every evaluation must record:

* Dataset or fixture version.
* Model, prompt, tool, and retrieval versions.
* Rubric and thresholds.
* Pass, fail, and abstain results.
* Reviewer identity or automated evaluator version.
* Known limitations and unresolved failures.

Exit gate:

* The slice meets product, quality, safety, security, and operational thresholds, or an accountable owner accepts documented residual risk.

Important adaptation:

* ECC's evaluation material is a useful framework, but its repository documentation identifies execution and containment limitations. Add real sandboxing, least-privilege credentials, network policy, and controlled test data before treating agent execution as enterprise-safe.

### 6. Release and controlled rollout

Release the smallest safe population and expand only on evidence.

Required controls:

* Feature flag or tenant allowlist.
* Versioned prompt, model, retrieval index, tool schema, and policy bundle.
* Audit logging with privacy review.
* Kill switch and rollback procedure.
* On-call owner and support playbook.
* Monitoring for quality proxies, safety events, latency, cost, and adoption.
* User feedback and correction path.

Rollout sequence:

1. Internal test users with synthetic or approved data.
2. One representative business team.
3. Limited production cohort.
4. Measured expansion by cohort.
5. General availability only after stable evidence.

Exit gate:

* Production evidence supports the next rollout decision, and the organization can detect, contain, and recover from failure.

### 7. Operate, learn, and follow up

Treat production as part of the product lifecycle.

Review on a regular cadence:

* User and business outcomes.
* Error and abstention patterns.
* Drift in data, prompts, policies, and model behavior.
* Security events and near misses.
* Cost per successful outcome.
* Support burden and user trust.
* New opportunities and residual gaps.

Route follow-up work to the correct phase:

* Unclear need returns to discovery.
* Weak value returns to problem framing.
* Unsafe design returns to solution design.
* Implementation defects return to the plan and test loop.
* Poor production behavior returns to evaluation, rollout, or operations.

## Harness architecture

Use a layered internal architecture:

| Layer | Responsibility | Source pattern |
| --- | --- | --- |
| Bootstrap | Loads policy, role, current lifecycle stage, and allowed tools | Superpowers session bootstrap, ECC hooks |
| Intent | Clarifies the request and selects the workflow | agent-skills commands, Matt Pocock grilling |
| Orchestration | Runs gated lifecycle stages and handoffs | HVE-Core RPI |
| Knowledge | Stores context, decisions, plans, evaluations, and ledgers | HVE-Core artifact taxonomy, Matt Pocock `CONTEXT.md` |
| Execution | Uses tools through least-privilege adapters | ECC hooks and multi-harness installers |
| Verification | Runs tests, evaluations, reviews, and evidence checks | Superpowers verification, agent-skills eval patterns |
| Governance | Enforces approvals, provenance, security, and auditability | HVE-Core governance and security model |
| Operations | Manages rollout, telemetry, incidents, and learning | New internal capability required |

Keep the core lifecycle and artifact schemas independent from any specific agent runtime. Add adapters for each supported host instead of duplicating the methodology.

## Minimum artifact set

Store artifacts in a repository or controlled workspace using stable identifiers:

* `opportunities/`: evidence, problem statement, baseline, and decision owner.
* `contexts/`: shared language, domain model, constraints, and system map.
* `decisions/`: architecture, model, data, safety, and rollout decisions.
* `slices/`: slice specification, acceptance criteria, and work breakdown.
* `plans/`: executable implementation plans and progress ledgers.
* `evals/`: datasets, rubrics, thresholds, results, and regression history.
* `reviews/`: independent engineering, security, privacy, and product reviews.
* `releases/`: rollout plan, approvals, monitoring, and rollback evidence.
* `operations/`: incidents, feedback, drift reviews, and follow-up decisions.

Use machine-readable frontmatter or schemas for identifiers, status, owner, classification, version, and related artifacts. Human-readable Markdown remains useful for review, but automation must validate required fields and links.

## Enterprise control plane

The harness needs controls that are not supplied by prompt content alone:

* Identity and access management for users, agents, tools, data, and environments.
* Secret isolation and short-lived credentials.
* Network egress policy and approved MCP or tool servers.
* Sandboxed execution for untrusted code and agent experiments.
* Data retention, redaction, residency, and privacy controls.
* Prompt, model, tool, dataset, and index provenance.
* Immutable audit events for consequential actions.
* Human approval for external side effects and high-impact decisions.
* Central policy enforcement that agents cannot override through instructions.
* Supply-chain scanning and pinning for skills, plugins, hooks, and dependencies.
* A support model for the internal harness, including versioning and deprecation.

## How to use the five repositories

Adopt selectively in this order:

1. Establish the lifecycle and evidence contracts from HVE-Core.
2. Add design, planning, TDD, isolated execution, and verification gates from Superpowers.
3. Add customer alignment, shared language, domain modeling, specifications, and ticket slicing from Matt Pocock’s skills.
4. Add the role, command, and evaluation patterns from agent-skills.
5. Add ECC hooks, installation adapters, memory, audits, and security guidance.
6. Remove duplicates and define one canonical command for each lifecycle action.
7. Reimplement enterprise controls in your own control plane instead of assuming a prompt or hook is a security boundary.

Avoid combining overlapping workflows verbatim. For example, maintain one canonical planning artifact and one canonical verification gate even if several source repositories provide similar skills.

## Delivery roadmap

### Phase A: Harness minimum viable product

Deliver:

* Artifact schemas and repository layout.
* Bootstrap policy and lifecycle router.
* Discovery, solution design, slice planning, implementation, review, and release commands.
* Evidence ledger and approval states.
* Local test and evaluation runner.
* One host adapter, preferably the team's primary IDE.

Success measure:

* A team can take one real internal problem from intake through a controlled pilot with complete evidence.

### Phase B: Production safety baseline

Deliver:

* Identity, secret, data, and tool policy enforcement.
* Sandboxed execution and network controls.
* Prompt/model/tool/index versioning.
* Golden-set and adversarial evaluation runner.
* Feature flags, kill switch, audit log, and rollback workflow.
* Security, privacy, and product review templates.

Success measure:

* A pilot can be operated safely by someone other than its original builder.

### Phase C: Repeatable enterprise delivery

Deliver:

* Reusable domain packs and approved patterns.
* CI checks for artifact schemas, links, policy, evaluation regressions, and provenance.
* Multi-harness adapters.
* Central metrics and portfolio reporting.
* Upgrade policy for upstream skills and plugins.
* Training, ownership, and support model.

Success measure:

* Multiple teams can ship slices using the same controls without copying the harness implementation.

## First pilot recommendation

Choose a low-to-moderate risk internal workflow where the outcome is measurable and human review is available. Avoid autonomous external actions, regulated decisions, and broad enterprise search for the first pilot.

The first slice should prove the whole system:

* One real user problem.
* One bounded data source.
* One model-backed capability.
* One deterministic fallback.
* One evaluation dataset.
* One controlled cohort.
* One rollback path.
* One post-release review.

The pilot is successful when the harness produces trustworthy delivery evidence, not when the agent generates a large amount of code.

See the complete walkthrough in [Example HR Transformation Delivery](./EXAMPLE-HR-TRANSFORMATION.md). It applies this lifecycle to an HR leadership request and shows how to move from three candidate use cases to a controlled, production-shaped pilot.

## Source review notes

The local clones were created for reference:

* `agent-skills` at `../agent-skills`
* `hve-core` at `../hve-core`
* `ECC` at `../ECC`
* `superpowers` at `../superpowers`
* `mattpocock-skills` at `../mattpocock-skills`

The requested `npx skills add mattpocock/skills` command could not run because Node.js and `npx` are not installed in this environment. The repository was cloned directly from GitHub instead, preserving the requested source for review. When Node.js is available, use the documented command `npx skills@latest add mattpocock/skills` only after deciding which skills and target agents should be installed.
