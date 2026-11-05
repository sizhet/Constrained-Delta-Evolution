# SPEC-006 — Metric and Recursive Evolution Contract

**Project:** Constrained Delta Evolution (CDE)  
**Series:** Specifications  
**Status:** Proposed normative contract, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Normative dependencies:** SPEC-001 through SPEC-005  
**Principal foundations:** CDE-007, CDE-008, CDE-009, CDE-010, CDE-011, CDE-012

## Abstract

This specification defines versioned contracts for **metric evolution**, **Delta-generator evolution**, **exploration-policy evolution**, and **controlled recursive improvement** within Constrained Delta Evolution (CDE). It establishes separate identities and validation regimes for task solutions, evaluation metrics, generators, policies, and governance protocols. It defines candidate creation, sandbox evaluation, independent verification, authorization, promotion, monitoring, rollback, and Structural Relay. Particular attention is given to Goodhart effects, benchmark leakage, cross-version score comparability, changing optimization objectives, solver-certificate invalidation, negative transfer, distributed consistency, and the distinction between internal score gains and independently demonstrated improvements. The contract supports research into structural recursive self-improvement (RSI) without claiming that recursion guarantees progress, safety, convergence, or general intelligence. It concludes the initial six-specification CDE series with explicit invariants, reference schemas, pseudocode, conformance tests, and a validation roadmap.

**Keywords:** CDE; metric evolution; generator evolution; policy evolution; recursive self-improvement; independent validation; governance; rollback; Structural Relay; conformance.

---

## 1. Normative Language

The terms **MUST**, **MUST NOT**, **REQUIRED**, **SHOULD**, **SHOULD NOT**, and **MAY** are normative within the declared conformance profile. MUST identifies an indispensable condition; SHOULD identifies a recommendation whose exception requires a documented rationale; MAY identifies an optional feature.

This is a **proposed research interoperability specification**, not an adopted industry standard or a claim of independently verified implementation.

## 2. Scope and Non-Goals

This contract governs:

1. versioned metric candidates and their validation;
2. generator and policy candidates as meta-states;
3. typed meta-Deltas and their applicability;
4. evaluation protocols and independent evidence;
5. controlled promotion, monitoring, and rollback;
6. compatibility with state, branch, path, solver, and distributed contracts;
7. Relay of method, metric, policy, evidence, and Leftover artifacts;
8. recursive experimentation with explicit resource and governance limits.

It does **not** define a universally correct objective function, guarantee that a new metric is better, certify that an arbitrary generator is complete, guarantee monotonic RSI, or grant an evolving agent authority to rewrite its own validation requirements.

## 3. Dependencies and Separation of Responsibilities

| Specification | Authority retained |
|---|---|
| SPEC-001 | State schemas, hard constraints, authorization, validated commit |
| SPEC-002 | Delta generation, proposals, branching, coverage declarations |
| SPEC-003 | Path memory, NoSubtree, Loop Back, state revisitation |
| SPEC-004 | Solver adapters, bounds, optimality and certificate semantics |
| SPEC-005 | Distributed tasks, global reconciliation, Relay provenance |
| SPEC-006 | Metric, generator, policy, and governed meta-evolution |

A conforming SPEC-006 implementation **MUST NOT** override the hard-constraint or commit semantics of SPEC-001. An evolved generator MUST still produce SPEC-002-conforming proposals. An evolved policy MUST respect SPEC-003 and SPEC-004 assumptions. Distributed promotion MUST respect SPEC-005 version and reconciliation rules.

## 4. Conceptual Model: Four Evolution Layers

We distinguish:

- **L0 — Task state:** a solution, partial solution, or environment state.
- **L1 — Generator state:** Delta operator portfolio, sampling distribution, granularity, representation, and generation contracts.
- **L2 — Policy state:** solver selection, branching, budgets, scheduling, exploration/exploitation, and Relay use.
- **L3 — Validation and governance:** independent evaluation protocols, constraint authorities, release decisions, audit, and rollback.

L0, L1, and L2 MAY be candidates for change. L3 MAY also evolve, but changes to the validation/governance regime **MUST be separately authorized and independently assessed**. An L1 or L2 candidate MUST NOT approve changes to its own L3 evaluation rules.

A conceptual transition is

\[
X_{k+1}=T_X(X_k,\delta_X),\quad X\in\{m,G,P\},
\]

where \(m\) is a metric, \(G\) a generator, and \(P\) a policy. This equation denotes candidate construction, not automatic promotion.

## 5. Identity, Versioning, and Artifact Classes

A conforming implementation MUST distinguish:

1. **Artifact identity** — immutable versioned metric, generator, policy, or validation protocol.
2. **Candidate identity** — proposed successor artifact not yet approved.
3. **Meta-Delta identity** — typed operation that produced the candidate.
4. **Evaluation-run identity** — one execution of a frozen protocol on declared data/tasks.
5. **Decision identity** — one governance action: promote, reject, quarantine, or rollback.
6. **Deployment identity** — which version is active for which population, task, or scope.

A content digest MAY supplement identifiers, but equality of artifact bytes does not prove equality of runtime behavior when external dependencies differ.

## 6. Artifact Registry Contract

Every metric, generator, policy, and validation protocol MUST have a registry record:

```yaml
EvolutionArtifact:
  artifact_id: string
  artifact_kind: metric | generator | policy | validation_protocol
  artifact_version: string
  artifact_digest: string
  schema_id: string
  schema_version: string
  immutable_definition_ref: string
  dependency_versions: [string]
  constraint_bundle_id: string
  creator_ref: string
  provenance_ref: string
  lifecycle_status: candidate | validated | approved | active | retired | quarantined
```

The registry MUST preserve historical versions. A correction MUST create a new revision or append-only correction event; it MUST NOT silently overwrite evidence attached to a prior version.

## 7. Metric Definition Contract

A metric is a versioned rule for measuring, comparing, predicting, or evaluating something. It is not necessarily a single scalar.

```yaml
MetricDefinition:
  metric_id: string
  metric_version: string
  objective_domain: string
  input_schema_refs: [string]
  output_schema_ref: string
  feature_registry_refs: [string]
  measurement_protocol_ref: string
  aggregation_rule_ref: optional
  direction: maximize | minimize | partial_order | descriptive
  uncertainty_contract_ref: optional
  stakeholder_perspective_refs: [string]
  known_limitations: [string]
  provenance_ref: string
```

A metric MUST declare whether its output is a utility, cost, risk estimate, feasibility signal, calibrated probability, ordinal ranking, or descriptive observation. An arbitrary score MUST NOT be interpreted as a probability or proof without a valid calibration or verification contract.

### 7.1 Hard constraints versus metrics

A metric MAY rank feasible candidates. It MUST NOT override hard constraints. If a metric becomes part of a hard decision rule, the rule MUST be separately authorized and versioned under SPEC-001.

### 7.2 Normative objectives

Metric discovery cannot by itself establish which stakeholder values are legitimate. A change in the normative objective or stakeholder weighting MUST be explicitly approved under the relevant governance authority. Better predictive accuracy is not equivalent to better values.

## 8. Metric Meta-Delta Types

A metric-evolution engine MAY support:

- `ADJUST_WEIGHT` — modify an authorized scalarization weight;
- `ADD_FEATURE` / `REMOVE_FEATURE` — change input features;
- `CHANGE_NORMALIZATION` — modify scales or reference populations;
- `CHANGE_AGGREGATION` — change objective composition;
- `ADD_PERSPECTIVE` — introduce stakeholder-specific views;
- `CHANGE_UNCERTAINTY_MODEL` — alter calibration or intervals;
- `REFINE_EVIDENCE_SOURCE` — replace or augment measurement data;
- `SPLIT_METRIC` / `MERGE_METRICS` — change evaluation structure;
- `REVISE_VALIDATION_TARGET` — governance-controlled protocol change.

Each type MUST specify preconditions, affected dependencies, comparability impact, and whether prior solver bounds or cached scores become invalid.

## 9. Cross-Version Comparability

Raw scores under distinct metrics MUST NOT be assumed comparable.

If \(m_1(x)=f(x)\) and \(m_2(x)=100f(x)\), a score increase is not evidence of improved solution quality. If weights change, rankings may reverse.

A comparison across versions MUST use at least one declared method:

1. re-evaluate both candidates under a common frozen metric;
2. apply a validated equivalence or calibration mapping;
3. report separate metrics without asserting a common ordering;
4. use an authorized external outcome measure.

The comparison record MUST identify the method and its limitations.

## 10. Metric Evaluation Record

```yaml
MetricEvaluation:
  evaluation_id: string
  metric_id: string
  metric_version: string
  evaluated_artifact_ref: string
  task_or_dataset_version: string
  protocol_version: string
  result_ref: string
  uncertainty_ref: optional
  evaluator_id: string
  execution_environment_ref: string
  cost_ref: string
  provenance_ref: string
```

An evaluation MUST bind the exact metric, dataset/task, and protocol versions. A changed dataset or measurement pipeline MUST produce a new evaluation context.

## 11. Generator Evolution Contract

A generator candidate MAY change operator sets, sampling distributions, granularity, neighborhood topology, proposal ordering, representation, directional fields, or generator composition.

```yaml
GeneratorCandidate:
  candidate_id: string
  parent_generator_id: string
  parent_generator_version: string
  candidate_generator_version: string
  meta_delta_refs: [string]
  operator_manifest_ref: string
  applicable_state_schemas: [string]
  coverage_claim: none | partial | exhaustive_under_conditions
  coverage_evidence_refs: [string]
  resource_contract_ref: string
  compatibility_impact_refs: [string]
  provenance_ref: string
```

A new generator MUST receive a new immutable version. Coverage certificates, canonicalization assumptions, and completeness claims MUST be revalidated when their dependencies change. An improved internal yield does not prove improved task performance.

## 12. Policy Evolution Contract

A policy candidate MAY change branch prioritization, solver dispatch, budget allocation, stopping conditions, checkpointing, Relay selection, or exploration/exploitation tradeoffs.

```yaml
PolicyCandidate:
  candidate_id: string
  parent_policy_ref: string
  candidate_policy_ref: string
  meta_delta_refs: [string]
  solver_capability_requirements: [string]
  constraint_and_authority_refs: [string]
  resource_limits_ref: string
  fairness_or_coverage_claim_ref: optional
  termination_policy_ref: string
  compatibility_impact_refs: [string]
  provenance_ref: string
```

A policy MUST NOT claim certified pruning merely because a learned model predicts low value. If a policy modifies a solver assumption, the affected proof or certificate MUST be revalidated under SPEC-004.

## 13. Meta-Delta Contract

```yaml
MetaDelta:
  meta_delta_id: string
  target_kind: metric | generator | policy | validation_protocol
  target_artifact_ref: string
  operation_type: string
  operation_parameters: typed
  precondition_contract_ref: string
  expected_effects: [string]
  affected_contract_refs: [string]
  authority_scope_ref: string
  proposer_ref: string
  provenance_ref: string
```

Meta-Deltas are proposals. Their application to a candidate artifact does not confer deployment authority. The system MUST check types, preconditions, permissions, and hard constraints before constructing or registering the candidate.

## 14. Candidate Lifecycle

The normative candidate lifecycle is:

```text
PROPOSED
   -> SCHEMA_CHECKED
   -> SANDBOXED
   -> EVALUATED
   -> INDEPENDENTLY_VALIDATED
   -> GOVERNANCE_REVIEWED
   -> APPROVED
   -> STAGED
   -> ACTIVE
   -> MONITORED
   -> RETIRED
```

Alternative dispositions include `REJECTED`, `INCONCLUSIVE`, `QUARANTINED`, `ROLLED_BACK`, `EXPIRED`, `CANCELLED`, and `VALIDATION_ERROR`.

An implementation MAY combine safe internal steps, but MUST preserve evidence of each required gate. `EVALUATED` MUST NOT be treated as equivalent to `APPROVED` or `ACTIVE`.

## 15. Candidate Proposal Record

```yaml
EvolutionProposal:
  proposal_id: string
  target_artifact_ref: string
  proposed_candidate_ref: string
  meta_delta_refs: [string]
  rationale_ref: string
  expected_improvement_claims: [string]
  anticipated_risks: [string]
  dependency_impact_refs: [string]
  proposed_validation_plan_ref: string
  proposer_identity: string
  authorization_ref: string
  provenance_ref: string
```

The proposer MUST identify the expected benefit and the failure conditions to test. Assertions of improvement MUST be labeled hypotheses until independent evaluation supports them.

## 16. Evaluation Protocol Contract

```yaml
EvolutionValidationProtocol:
  protocol_id: string
  protocol_version: string
  target_artifact_kind: string
  task_distribution_ref: string
  training_data_refs: [string]
  development_data_refs: [string]
  held_out_data_refs: [string]
  baseline_artifact_refs: [string]
  primary_outcome_metrics: [metric_ref]
  secondary_outcome_metrics: [metric_ref]
  hard_safety_constraints: [constraint_ref]
  compute_budget_ref: string
  statistical_analysis_plan_ref: string
  stopping_rule_ref: string
  evaluator_independence_rule_ref: string
  leakage_controls_ref: string
  approval_threshold_ref: string
  provenance_ref: string
```

The protocol MUST be versioned and frozen before the decisive evaluation, except for separately approved changes recorded as a new protocol version. A candidate MUST NOT unilaterally redefine its own success criterion after observing results.

## 17. Independent Validation and Separation of Duties

An evaluation is independent when its success criteria and decisive evidence are not controlled solely by the candidate being assessed.

A conforming deployment MUST declare who can:

- propose a candidate;
- choose evaluation tasks;
- alter the metric or protocol;
- execute tests;
- approve promotion;
- deploy and roll back versions.

The same human or organization MAY perform multiple roles in a small research prototype, but the lack of organizational separation MUST be disclosed. Logical independence still requires frozen protocols, immutable evidence, and an explicit approval gate.

## 18. Validation Evidence Levels

A report SHOULD distinguish:

- **structural validity:** schema, typing, and contract checks;
- **functional validity:** the candidate runs and produces permitted outputs;
- **safety validity:** hard constraints remain satisfied;
- **statistical validity:** evidence supports a performance difference on the tested distribution;
- **transfer validity:** performance survives a receiving environment or held-out domain;
- **governance validity:** required authorities approve deployment.

Passing one level does not imply passing the others.

## 19. Goodhart and Proxy-Optimization Controls

A candidate may improve a proxy while degrading the intended outcome. Metric evolution can make this failure self-reinforcing.

A conforming evaluation protocol MUST include at least one outcome or criterion not freely rewritable by the candidate under test. It SHOULD include stress tests for distribution shift, subgroup effects, adversarial gaming, and calibration where relevant.

A metric candidate MUST NOT validate itself solely by reporting a higher value under its own new scoring rule.

## 20. Benchmark Leakage and Adaptive Overfitting

Repeatedly tuning a generator or policy on the same benchmark can produce apparent gains without generalization.

An implementation SHOULD:

1. separate development and held-out evaluations;
2. limit or account for repeated access to held-out results;
3. version benchmark suites;
4. preserve failed trials, not just promoted candidates;
5. use fresh or independent tasks when feasible;
6. disclose the number and cost of candidate evaluations.

A benchmark that has become part of the optimization loop MUST NOT be described as untouched independent evidence.

## 21. Resource-Normalized Comparison

A candidate's benefit MUST be evaluated with an explicitly declared cost model. Relevant costs include generation, solver execution, validation, orchestration, failed trials, distributed communication, and monitoring.

Comparisons SHOULD report both outcome quality and resource consumption. A faster task solver that requires much more meta-search may not reduce end-to-end cost.

A policy MUST NOT be called more efficient solely because it shifts work to uncounted workers or external services.

## 22. Promotion Decision Contract

```yaml
PromotionDecision:
  decision_id: string
  candidate_artifact_ref: string
  baseline_artifact_ref: string
  validation_protocol_ref: string
  evaluation_report_refs: [string]
  safety_report_refs: [string]
  compatibility_report_refs: [string]
  decision: approve | reject | defer | quarantine
  deployment_scope_ref: optional
  rollback_plan_ref: optional
  approver_identity: string
  authority_ref: string
  decision_timestamp: string
  rationale_ref: string
  provenance_ref: string
```

A promotion MUST NOT occur without the required successful checks and authorized decision. The decision MUST state its scope: one task family, one environment, one worker group, or a broader deployment. Approval for a narrow scope MUST NOT be silently generalized.

## 23. Staged Deployment and Monitoring

A candidate MAY be promoted through shadow mode, canary deployment, or limited cohorts. The deployment MUST pin the active artifact versions and governing constraints.

Monitoring SHOULD track:

- task outcome quality;
- hard-constraint violations;
- resource use and latency;
- coverage and fairness regressions;
- solver certificate failures;
- unexpected distribution shifts;
- Relay applicability failures;
- operational incidents and rollback triggers.

Monitoring evidence MUST be versioned. An observed regression SHOULD trigger the declared quarantine or rollback policy.

## 24. Rollback Contract

```yaml
RollbackPlan:
  rollback_plan_id: string
  promoted_artifact_ref: string
  prior_approved_artifact_ref: string
  activation_scope_ref: string
  trigger_conditions: [string]
  compatibility_preconditions: [string]
  state_migration_or_compensation_ref: optional
  dependent_cache_invalidation_refs: [string]
  authorization_ref: string
  audit_record_ref: string
```

Rollback MUST restore a compatible approved configuration or enter a safe fallback state. It MUST NOT claim to reverse irreversible external effects without an explicit compensation mechanism. Historical evidence MUST remain intact.

## 25. Constraint and Authorization Evolution

Changing hard constraints or the authorization model is a governance action, not an ordinary optimization step. Such changes MUST use SPEC-001 versioned migration and separately authorized review.

A generator or policy candidate MUST NOT relax safety constraints, expand its own authority, or disable its validator merely to improve measured performance.

If the governance protocol itself changes, the change MUST have a higher-level approval path and an explicit account of which previous validations remain applicable.

## 26. Solver Proof and Cache Invalidation

A metric, constraint, generator, representation, or policy change may invalidate:

- DP memoization keys;
- B&B bounds and incumbents;
- admissible heuristic assumptions;
- completeness claims;
- stopping criteria;
- state equivalence relations;
- cached scores and rankings.

Each candidate MUST declare affected dependencies. An implementation MUST invalidate or reverify any certificate whose assumptions are no longer satisfied. An exact-optimal result under metric \(m_1\) MUST NOT be relabeled optimal under \(m_2\) without a valid proof.

## 27. Recursive Evolution Control

Recursive improvement MAY propose new generators, policies, and evaluation methods. Each recursive layer MUST retain:

1. a finite or enforceable resource budget;
2. a defined authority boundary;
3. a frozen validation protocol for decisive tests;
4. immutable candidate and evaluation records;
5. explicit promotion and rollback decisions;
6. a stopping or escalation condition.

Unbounded recursion, unlimited self-modification, and unrestricted modification of governance are not required for conformance.

### 27.1 No monotonic improvement guarantee

Even a correctly governed process may produce many rejected candidates, plateaus, or negative transfer. Promotion thresholds may reduce risk but do not prove performance improves on every future task.

### 27.2 No automatic acceleration guarantee

Recursive depth does not imply increasing improvement rate. The costs of meta-search, validation, and coordination may dominate the benefit.

## 28. Meta-Evolution Branching and Path Memory

A meta-state is still a State Container under SPEC-001. A meta-Delta is still a typed Delta under SPEC-002. Its exploration history is governed by SPEC-003.

An implementation MUST distinguish:

- candidate versions from approved versions;
- repeated testing from new candidate creation;
- path-local exclusion from global rejection;
- failed evaluation from proof of universal uselessness.

A rejected meta-Delta MAY be reconsidered under changed context, but the old rejection and its evidence MUST remain traceable.

## 29. Distributed Meta-Evolution

Multiple workers MAY independently propose or test candidates. SPEC-005 governs dispatch, lease/retry semantics, and reconciliation.

Two locally successful evaluations MUST NOT be merged into a global improvement claim without compatible tasks, metrics, versions, and statistical interpretation. Promotion authority MUST be explicit and cannot be inferred from worker autonomy.

A candidate evaluated against stale constraints or an obsolete protocol MUST be revalidated or marked stale before promotion.

## 30. Structural Relay Contract

SPEC-006 uses the SPEC-005 Relay artifact envelope to exchange:

- **Metric Relay:** definitions, calibration, known limitations, and external validation evidence;
- **Method Relay:** generator operators and applicability conditions;
- **Policy Relay:** scheduling or solver-selection policies and tested scope;
- **Evidence Relay:** successful, failed, and inconclusive evaluation results;
- **Constraint Relay:** newly discovered restrictions or counterexamples, subject to governance;
- **Leftover Relay:** unfinished tasks, unresolved regressions, and missing proof obligations.

A receiving system MUST check compatibility and applicability before promoting a transferred artifact. Relay transmission alone is not validation or Collective Learning.

## 31. Repeated Execution as Temporal Relay

Repeated execution MAY accumulate evidence about stability, drift, stochastic outcomes, failure modes, and resource costs.

To qualify as evidence-bearing temporal Relay, the implementation SHOULD preserve:

1. a stable task or method reference;
2. comparable execution contexts;
3. observed outcomes and uncertainty;
4. a rule for updating beliefs or decisions;
5. provenance of changes caused by accumulated evidence.

Repetition without retained information or changed future behavior MUST NOT be described as demonstrated learning.

## 32. Collective Learning Claim Contract

A claim of Collective Learning MUST identify:

- the source and receiving participants;
- transferred artifacts and versions;
- receiving-side validation;
- an appropriate no-transfer or alternative-transfer baseline;
- a measurable outcome;
- the distribution of tasks tested;
- resource costs and uncertainty;
- negative-transfer cases.

The number of messages, shared artifacts, or agent interactions is not itself evidence of learning.

## 33. SIF Localization and Meta-Localization

Within the Structural Intelligence Framework (SIF), CDE may act as an exploratory Localization mechanism. Metric and generator evolution may additionally search for better ways to locate promising problem regions, representations, or solver capabilities.

A claim that this improves Localization MUST be evaluated against appropriate static, heuristic, active-learning, and other search baselines. A successful architectural integration does not by itself establish superior discovery performance.

## 34. Evidence and Evaluation Report Contract

```yaml
EvolutionEvaluationReport:
  report_id: string
  candidate_ref: string
  baseline_refs: [string]
  protocol_ref: string
  task_distribution_ref: string
  dataset_and_seed_refs: [string]
  outcome_metric_refs: [string]
  measured_results_ref: string
  uncertainty_analysis_ref: string
  safety_checks_ref: string
  resource_accounting_ref: string
  failures_and_regressions_ref: string
  evaluator_identity: string
  evaluator_independence_statement: string
  conclusion: improved | equivalent | regressed | inconclusive
  limitations: [string]
  provenance_ref: string
```

The report MUST distinguish statistical evidence from normative approval. `improved` means improvement under the declared protocol and task distribution, not universal dominance.

## 35. Formal Safety Invariants

**INV-MRE-01 — Artifact immutability.** Historical artifact versions and their validation evidence are not silently rewritten.

**INV-MRE-02 — Typed meta-transition.** Every candidate has a valid target type and authorized meta-Delta.

**INV-MRE-03 — Constraint supremacy.** Metric or policy changes do not bypass hard constraints.

**INV-MRE-04 — Version coherence.** Evaluation and promotion refer to compatible artifact, task, metric, protocol, and constraint versions.

**INV-MRE-05 — No self-certification.** A candidate cannot unilaterally approve its own promotion or rewrite decisive evaluation criteria.

**INV-MRE-06 — Independent evidence.** Promotion requires the declared independent validation evidence.

**INV-MRE-07 — Scope-bound approval.** Deployment does not exceed approved scope without new authorization.

**INV-MRE-08 — Comparability discipline.** Cross-version raw scores are not treated as comparable without a valid bridge.

**INV-MRE-09 — Certificate invalidation.** Changed assumptions invalidate or trigger revalidation of affected solver certificates.

**INV-MRE-10 — Budget accounting.** Meta-search and validation costs are included in reported end-to-end resource claims.

**INV-MRE-11 — Rollback traceability.** Every rollback preserves the original promotion and incident records.

**INV-MRE-12 — No unsupported RSI claim.** Recursive modification is not labeled proven improvement without evaluation.

**INV-MRE-13 — Relay applicability.** Transferred artifacts are checked in the receiving context.

**INV-MRE-14 — Failure preservation.** Failed and inconclusive evaluations remain available for authorized audit and negative Relay.

**INV-MRE-15 — Governance versioning.** Changes to validation or authority rules use a separate approved versioned path.

**INV-MRE-16 — Stochastic honesty.** Uncertainty and run-to-run variability are not concealed by selective reporting.

## 36. Conditional Propositions and Proof Obligations

### 36.1 Proposition A — Safe promotion under declared checks

If a candidate passes every required check of a fixed, sound protocol, receives valid authorization, and is deployed only within its approved scope, then its promotion conforms to that protocol.

*Proof sketch:* each gate is a necessary condition for the `APPROVED` transition; an atomic, version-coherent decision ensures all are satisfied. This proves procedural conformance, **not** future performance or absence of unknown hazards.

### 36.2 Proposition B — Version-safe comparison

If two candidate outcomes are evaluated under the same fixed metric, task distribution, and protocol, their reported measurements are comparable according to that protocol's measurement semantics, subject to sampling uncertainty.

This does not imply that the metric captures all legitimate goals.

### 36.3 Proposition C — Conditional rollback safety

If the previous version is compatible with current state and external dependencies, all required compensations succeed, and the rollback transaction respects its consistency boundary, then restoring the previous approved configuration preserves the declared rollback invariants.

The proposition does not apply to irreversible side effects lacking valid compensation.

### 36.4 Proposition D — No general RSI monotonicity

From the existence of a meta-Delta operator and a recursive evaluation loop alone, monotonic improvement does not follow. A counterexample is a generator update that overfits the development benchmark and performs worse on held-out tasks.

### 36.5 Proof obligations

Implementations claiming stronger properties MUST provide the relevant evidence for: validator soundness, safe metric transformation, coverage preservation, bound validity, statistical reliability, authority separation, distributed consistency, and rollback compatibility.

## 37. Reference Algorithm: Candidate Evaluation and Promotion

```text
function EVALUATE_AND_PROMOTE(proposal):
    parent = registry.resolve_pinned(proposal.target_artifact_ref)
    if not authorized_to_propose(proposal):
        return UNAUTHORIZED

    candidate = apply_typed_meta_delta(parent, proposal.meta_delta_refs)
    if not schema_valid(candidate) or not hard_constraints_valid(candidate):
        return REJECTED

    candidate_ref = registry.store_immutable(candidate)
    protocol = governance.resolve_frozen_protocol(proposal)
    if not protocol.is_independent_enough_for_declared_profile:
        return VALIDATION_ERROR

    sandbox_result = sandbox.run(candidate_ref, protocol)
    if sandbox_result.has_safety_failure:
        registry.record(candidate_ref, QUARANTINED, sandbox_result)
        return QUARANTINED

    report = independent_evaluator.compare(
        candidate_ref, protocol.baselines, protocol
    )
    registry.record_immutable(report)

    if report.is_inconclusive or not report.meets_approval_threshold:
        return REJECTED_OR_DEFERRED

    impact = dependency_analyzer.check_invalidation(candidate_ref)
    if not impact.safe_for_staging:
        return DEFERRED

    decision = governance.request_approval(
        candidate_ref, report, impact, protocol
    )
    if decision != APPROVED:
        return decision

    deployment = deploy_staged_atomically(candidate_ref, decision.scope)
    monitoring.attach(deployment, decision.rollback_plan)
    return STAGED
```

The pseudocode assumes trustworthy external services. It is not a proof that the evaluation protocol is correct or that deployment will improve future outcomes.

## 38. Reference Algorithm: Monitoring and Rollback

```text
function MONITOR_ACTIVE(deployment):
    observations = monitor.collect_versioned_observations(deployment)
    incident = evaluate_triggers(observations, deployment.rollback_plan)

    if not incident:
        append_monitoring_evidence(observations)
        return ACTIVE

    freeze_further_promotion(deployment)
    record_immutable_incident(incident)

    if rollback_preconditions_hold(deployment.rollback_plan):
        result = execute_authorized_rollback(deployment.rollback_plan)
        invalidate_dependent_caches_and_certificates(result)
        record_rollback_evidence(result)
        return ROLLED_BACK

    enter_declared_safe_fallback_or_escalation(deployment)
    return QUARANTINED
```

## 39. Conformance Profiles

### 39.1 CDE-Metric-Evolution-v1

Requires versioned metric definitions, explicit comparison semantics, independent validation, governance-controlled promotion, and historical preservation.

### 39.2 CDE-Generator-Evolution-v1

Adds generator meta-Deltas, operator/coverage dependency analysis, held-out evaluation, and versioned promotion.

### 39.3 CDE-Policy-Evolution-v1

Adds policy meta-Deltas, solver/branch compatibility checks, resource accounting, and rollback.

### 39.4 CDE-Recursive-Evolution-v1

Adds bounded recursive proposal cycles, separate governance authority, promotion history, and failure/negative-transfer tracking.

### 39.5 CDE-Distributed-MetaEvolution-v1

Adds distributed evaluation provenance, version-coherent result aggregation, worker trust boundaries, and receiving-side Relay validation.

An implementation MUST declare supported profiles and MUST NOT infer recursive or distributed conformance merely from metric versioning.

## 40. Conformance Test Suite

The following **60 proposed tests** include both success and negative paths.

| ID | Scenario | Expected outcome |
|---|---|---|
| MRE-001 | Register valid metric definition | Accepted |
| MRE-002 | Missing metric version | Rejected |
| MRE-003 | Unknown input schema | Rejected or explicitly deferred |
| MRE-004 | Rewrite historical metric version | Rejected |
| MRE-005 | Adjust weight through typed meta-Delta | Candidate created |
| MRE-006 | Meta-Delta targets wrong artifact type | Rejected |
| MRE-007 | Metric change violates hard constraint | Rejected |
| MRE-008 | New metric reports larger raw scores | No automatic improvement claim |
| MRE-009 | Compare incompatible metric versions directly | Refused or qualified |
| MRE-010 | Common-metric re-evaluation | Comparison permitted |
| MRE-011 | Change metric feature registry | Dependency impact recorded |
| MRE-012 | Metric candidate self-scores as superior | Not sufficient for approval |
| MRE-013 | Uncalibrated score labeled probability | Rejected claim |
| MRE-014 | Stakeholder objective changed without authority | Rejected |
| MRE-015 | Independent validation report present | Eligible for review |
| MRE-016 | Missing decisive held-out evidence | Not independently validated |
| MRE-017 | Benchmark used repeatedly for tuning | Leakage disclosed; evidence downgraded |
| MRE-018 | Candidate changes success threshold post hoc | Rejected or new protocol required |
| MRE-019 | Protocol version changed during evaluation | New evaluation context required |
| MRE-020 | Promotion without authorization | Rejected |
| MRE-021 | Approval scoped to one task family | Cannot deploy outside scope |
| MRE-022 | Failed safety check despite high performance | Quarantined or rejected |
| MRE-023 | Inconclusive statistical result | No categorical improvement claim |
| MRE-024 | Regression on held-out task | Recorded and considered |
| MRE-025 | Missing compute-cost accounting | Efficiency claim nonconformant |
| MRE-026 | Meta-search cost omitted | End-to-end efficiency claim rejected |
| MRE-027 | Generator operator added | New generator version required |
| MRE-028 | Generator operator removed | Coverage claim revalidated |
| MRE-029 | Generator representation changed | Equivalence/cache impact checked |
| MRE-030 | Old completeness certificate reused blindly | Rejected |
| MRE-031 | Policy changes solver dispatch | Versioned policy candidate |
| MRE-032 | Policy uses learned score as B&B proof | Rejected |
| MRE-033 | Policy modifies stopping rule | Compatibility and termination review |
| MRE-034 | Policy changes resource budget | Authorized versioned change |
| MRE-035 | Policy disables hard validation | Rejected |
| MRE-036 | Meta-Delta changes own authority | Separate governance required |
| MRE-037 | Candidate approves itself | Rejected |
| MRE-038 | Validator changed without separate review | Rejected |
| MRE-039 | Candidate passes sandbox but fails independent test | Not promoted |
| MRE-040 | Candidate passes all gates and is authorized | Eligible for staged deployment |
| MRE-041 | Canary regression crosses rollback threshold | Rollback or quarantine triggered |
| MRE-042 | Rollback incompatible with current state | Safe fallback/escalation |
| MRE-043 | Irreversible external effect | No false rollback guarantee |
| MRE-044 | Historical promotion record after rollback | Preserved |
| MRE-045 | Changed metric invalidates incumbent | Revalidate or invalidate |
| MRE-046 | Changed constraint invalidates B&B bound | Revalidate or invalidate |
| MRE-047 | Changed generator invalidates DP key | Revalidate or invalidate |
| MRE-048 | Distributed worker uses stale protocol | Result marked stale |
| MRE-049 | Two workers report incompatible gains | No unqualified aggregation |
| MRE-050 | Remote method Relay received | Applicability checked |
| MRE-051 | Negative-transfer evidence available | Retained and reported |
| MRE-052 | Leftover Relay unresolved | Explicit open status |
| MRE-053 | Repeated runs without retained evidence | Not labeled learning |
| MRE-054 | Repeated runs update validated belief | Temporal Relay recorded |
| MRE-055 | Recursive loop exceeds budget | Stops or escalates |
| MRE-056 | Recursive update regresses | No monotonic improvement claim |
| MRE-057 | Candidate improves only development set | Generalization not established |
| MRE-058 | Metric and generator co-evolve | Independent evaluation boundary retained |
| MRE-059 | Full provenance replay | Candidate, evaluation, decision traceable |
| MRE-060 | Recursive system claims guaranteed AGI/ASI | Unsupported guarantee rejected |

### 40.1 Required fixtures

A conformance suite SHOULD include a versioned toy metric, a generator with removable operators, a solver-selection policy, a held-out dataset, a simulated stale worker, an invalid B&B bound, and a reversible staged deployment.

### 40.2 Fault injection

The suite SHOULD inject validator outages, mismatched versions, benchmark leakage, missing authorization, malicious score inflation, partial rollout failure, and rollback incompatibility.

## 41. Worked Example A — School-Bus Scheduling Metrics

Suppose the task minimizes fleet count while respecting hard vehicle capacity, student coverage, and maximum ride-time constraints. A candidate metric adds a soft preference for shorter average ride time.

The new metric may improve fairness or comfort, but its adoption is not automatic. The evaluation compares both metrics on the same held-out scheduling instances, measures fleet count, ride-time distribution, feasibility, and compute cost, and records stakeholder-approved tradeoffs. A higher score under the new metric is not evidence of improvement without a common external criterion.

If a policy update allocates more compute to globally coupled route exchanges, it must be tested against a fixed baseline and may not relax hard student-safety constraints.

## 42. Worked Example B — Multi-Scale Peak Search

A generator begins with local perturbations. A meta-Delta adds regional and long-range proposals. The candidate may discover separated peaks that the baseline misses, but may also waste computation on irrelevant distant probes.

A valid comparison measures best objective achieved at matched total budgets across multiple landscapes, including unfavorable cases. Generator reachability, proposal yield, and independent outcome quality are reported separately. A broader candidate set is not automatically a better generator.

## 43. Worked Example C — Dynamic Structural Matching

A metric candidate introduces a directional similarity feature. The system records feature provenance, uncertainty, calibration, and false-positive costs. Validation uses independent labeled or otherwise defensible evidence where available.

Weak correlations MUST NOT be converted into definitive identity assertions merely by increasing the similarity score. A receiving domain with different data-generating conditions must revalidate the transferred metric.

## 44. Worked Example D — Solver-Portfolio Policy Evolution

A baseline policy allocates equal budgets to DP, B&B, and heuristic search. A candidate policy uses task features to route more resources to the solver predicted to be effective.

The evaluation measures solution quality, proven gaps, latency, compute cost, and failures on held-out task families. If the policy directs a heuristic result into a proof-required workflow, SPEC-004's certificate requirements remain in force. A better average outcome may still conceal unacceptable regressions on a critical task class.

## 45. Worked Example E — Distributed Method Relay

Worker A discovers a generator operator that improves performance on dense graphs. Worker B receives the method for sparse graphs. The Relay artifact includes tested scope, operator version, constraints, and negative evidence.

Worker B must independently test applicability. If the method performs worse, the failure becomes negative Relay rather than being suppressed. Collective Learning is demonstrated only if the network's future outcomes improve under an appropriate baseline, not because the operator was shared.

## 46. Worked Example F — Controlled Recursive Generator Evolution

A meta-generator proposes successive versions \(G_0,G_1,G_2\). Each version is evaluated under a frozen protocol and approved only within a declared scope. \(G_1\) improves held-out performance; \(G_2\) overfits development cases and is rejected.

This is a successful *governed research process*, even though the second recursive update fails. The evidence does not support monotonic RSI. The rejected candidate and its counterexamples remain available for future Method and Leftover Relay.

## 47. Validation Roadmap

**Phase 1 — Contracts.** Implement versioned metric, generator, policy, and protocol registries; pass schema and authorization tests.

**Phase 2 — Independent evaluation.** Establish frozen baseline comparisons, held-out tasks, uncertainty reporting, and leakage controls.

**Phase 3 — Metric evolution.** Test feature, weight, and structural metric changes under external outcomes and stakeholder governance.

**Phase 4 — Generator evolution.** Evaluate operator additions, representation changes, coverage preservation, and resource-normalized results.

**Phase 5 — Policy evolution.** Benchmark solver dispatch, budget adaptation, and stopping-rule changes while preserving SPEC-004 guarantees.

**Phase 6 — Distributed Relay.** Validate transfer, stale-result handling, negative knowledge, and receiving-side performance.

**Phase 7 — Controlled RSI.** Run bounded recursive experiments with independent promotion and rollback; report plateaus and regressions.

**Phase 8 — Independent replication.** Invite external implementations and reproducible cross-domain benchmarks, including negative results.

## 48. Open Research Questions

1. Which external validation criteria remain trustworthy as metric search becomes more capable?
2. How can a system evolve representations without invalidating every useful cache or certificate?
3. Can generator coverage properties be preserved through restricted classes of meta-Deltas?
4. How should multi-objective stakeholder preferences be revised and governed?
5. What statistical procedures best handle adaptive repeated evaluation and benchmark reuse?
6. What is the minimum independent-evaluator separation needed for small research teams?
7. How can one measure net RSI benefit after counting meta-search and validation costs?
8. Which forms of negative Method Relay transfer reliably across domains?
9. When do distributed candidate evaluations introduce misleading selection bias?
10. How should governance protocols evolve without granting self-authorization?
11. Can failure patterns be compressed into reusable, scoped meta-constraints?
12. When does temporal repeated execution create genuine new information?
13. What evidence would support CDE as a useful SIF meta-localization method?
14. How should a system identify when further recursive optimization is not worth its cost?
15. Which claims about open-ended evolution can be tested without confusing novelty with progress?

## 49. Implementation Checklist

- [ ] Metric, generator, policy, and protocol identities are distinct.
- [ ] All artifact versions and dependencies are immutable and resolvable.
- [ ] Meta-Deltas have typed preconditions and authority scopes.
- [ ] Hard constraints remain authoritative under all candidate changes.
- [ ] Evaluation protocols are frozen for decisive comparisons.
- [ ] Independent validation and governance decisions are recorded.
- [ ] Cross-version score comparisons use an explicit common basis.
- [ ] Benchmark leakage and repeated candidate testing are disclosed.
- [ ] Meta-search and validation costs are included in efficiency claims.
- [ ] Solver bounds, DP caches, and coverage claims are invalidated as needed.
- [ ] Promotion is scoped and staged where appropriate.
- [ ] Monitoring and rollback plans are tested.
- [ ] Irreversible side effects are not falsely described as reversible.
- [ ] Distributed workers cannot self-authorize promotion.
- [ ] Relay artifacts carry applicability, provenance, and negative evidence.
- [ ] Recursive experiments have explicit budgets and stopping rules.
- [ ] Regressions and inconclusive outcomes are preserved.
- [ ] No unsupported monotonic RSI or AGI/ASI guarantee is asserted.

## 50. Conclusion

SPEC-006 completes the initial CDE specification series by defining how the **methods of exploration** may themselves become subjects of constrained Delta exploration. The central distinction is:

\[
\boxed{
\text{Candidate Change}
\neq
\text{Measured Improvement}
\neq
\text{Validated Generalization}
\neq
\text{Authorized Promotion}
}
\]

Metric evolution can change what a system values or measures. Generator evolution can change what it is capable of proposing. Policy evolution can change where it spends effort. Recursive combinations may create useful new exploration methods—but none of these changes is automatically beneficial or safe.

The governing principle is **Specifications and Contracts First; Conformance Before Runtime; Independent Validation Before Promotion; Explicit Leftover for Collective Learning**. A credible CDE evolution ecosystem should retain failed hypotheses and unresolved work alongside successful results, making both available for carefully scoped Structural Relay.

---

## Appendix A — End-to-End Evolution Contract Map

```text
                 CDE Task / State / Constraint Context
                                |
                   SPEC-001: Valid State + Commit
                                |
                   SPEC-002: Delta Proposals
                                |
                   SPEC-003: Path / Loop Control
                                |
                   SPEC-004: Solver + Certificates
                                |
                   SPEC-005: Distributed Relay
                                |
           +--------------------+--------------------+
           |                    |                    |
      Metric Candidate    Generator Candidate   Policy Candidate
           |                    |                    |
           +--------------------+--------------------+
                                |
                       Typed Meta-Deltas
                                |
                      Sandbox Evaluation
                                |
                    Independent Validation
                                |
                    Governance Authorization
                                |
                       Staged Promotion
                                |
                    Monitoring / Rollback
                                |
             Evidence + Method + Leftover Relay
                                |
                    Next Candidate Generation
```

This is a logical architecture. Implementations MAY omit optional evolution layers and MAY distribute governance physically while preserving authority separation.

## Appendix B — Minimal Example of a Candidate Promotion Record

```json
{
  "proposal_id": "meta-proposal-042",
  "target_artifact_ref": "generator-routing-v1",
  "proposed_candidate_ref": "generator-routing-v2-candidate",
  "meta_delta_refs": ["add-regional-swap-operator"],
  "validation_protocol_ref": "routing-heldout-v3",
  "evaluation_report_refs": ["eval-104", "eval-105"],
  "decision": "approve",
  "deployment_scope_ref": "routing-pilot-cohort-A",
  "rollback_plan_ref": "rollback-routing-v2-to-v1",
  "approver_identity": "authorized-reviewer",
  "provenance_ref": "audit-event-884"
}
```

This abbreviated example illustrates the relationships; it is not a complete serialized conformance fixture. A production record must also satisfy the full schemas and authority checks.

## Appendix C — Specification Series Completion Map

| Document | Contract focus |
|---|---|
| SPEC-001 | State Container and Constraint Contract |
| SPEC-002 | Delta Generation and Branch Contract |
| SPEC-003 | Path Memory and Loop Control Contract |
| SPEC-004 | Solver and Optimization Contract |
| SPEC-005 | Distributed Exploration and Relay Contract |
| **SPEC-006** | **Metric and Recursive Evolution Contract** |

Together, these six documents define a proposed modular contract stack. Full interoperability remains to be demonstrated through executable schemas, reference implementations, and independent conformance testing.

## Appendix D — Research Status, Attribution, and Placement

**Suggested repository location:**

`docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md`

This document synthesizes established ideas from optimization, meta-learning, algorithm configuration, evaluation design, distributed systems, provenance, and safety governance within the proposed CDE State–Delta–Constraint and Structural Relay framework. It does not claim that these underlying methods were invented by CDE. Formal literature review, executable reference code, and independent validation remain future work.

**Version:** v1.0.0 draft  
**License:** Apache-2.0
