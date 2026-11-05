# CASE-004 — Metric Discovery and Co-Evolution

**Project:** Constrained Delta Evolution (CDE)  
**Series:** Cases / Research and Validation Designs  
**Status:** Proposed reference case and experimental protocol, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Related foundations:** CDE-001–012, particularly CDE-006, CDE-007, CDE-008, CDE-010, CDE-011, CDE-012  
**Related specifications:** SPEC-001–006, particularly SPEC-004 and SPEC-006  
**Suggested path:** `docs/cases/CASE-004-Metric-Discovery-and-Co-Evolution.md`

## Abstract

Constrained Delta Evolution (CDE) treats search states, candidate-generating operators, constraints, and evaluation metrics as distinct but interacting objects. This case studies a challenging extension: **the metric used to guide exploration can itself become an object of constrained exploration**. A solver may improve its score under a fixed metric while the metric remains incomplete, miscalibrated, vulnerable to proxy exploitation, or misaligned with the actual task. Conversely, a newly proposed metric may appear better simply because the solution generator adapts to exploit it.

CASE-004 defines a controlled laboratory for **Metric Discovery and Solution–Metric Co-Evolution (SMCE)**. It provides a typed Metric Container, Metric Delta taxonomy, multi-timescale search loops, independent validation protocols, anti-Goodhart controls, cross-version comparison rules, and a benchmark suite spanning synthetic ranking, constrained allocation, structural matching, and evolving preference tasks. It distinguishes candidate metric generation, internal score improvement, predictive validation, decision utility, and authorized promotion. A metric that assigns higher scores to its own preferred solutions is not thereby a better metric.

The proposed framework combines classical multiobjective optimization, surrogate evaluation, active learning, bilevel optimization, preference learning, and model selection within CDE's versioned State–Delta–Constraint–Branch architecture. The novelty claim is restricted to the proposed *contractual integration and relay semantics*, not to the invention of these constituent techniques. No experiments have been executed for this manuscript, and no empirical performance improvement is asserted.

**Research objective:** Determine when explicit, governed exploration of metric hypotheses improves out-of-sample decision quality and search efficiency relative to fixed, tuned, and jointly learned baselines, while preserving hard constraints and auditability.

---

## 1. Why Metrics Must Sometimes Be Explored

Optimization is typically described as finding:

\[
x^\star\in\arg\max_{x\in\mathcal F}m(x),
\]

where \(\mathcal F\) is the feasible solution space and \(m\) is an evaluation metric. But the formula assumes that \(m\) is sufficiently appropriate for the actual objective. In practice, this assumption may fail.

Examples include:

- A school-bus scheduler minimizes route count but overlooks actual physical fleet count.
- A structural matcher rewards superficial similarity while missing relational contradictions.
- A search portfolio maximizes short-term improvement and suppresses long-range exploration.
- A recommendation or allocation policy maximizes a proxy while worsening the independently measured outcome.
- A metric appears calibrated on training data but shifts under deployment or adversarial adaptation.

These failures motivate two separate questions:

1. **Solution search:** Given metric \(m\), which feasible solution \(x\) should be selected?
2. **Metric search:** Given independent evidence and governance, which metric \(m\) is useful, valid, and safe to employ?

CDE proposes that both questions can be represented with typed Deltas, but they require **different authority and validation contracts**.

## 2. Research Questions

**RQ-1:** Can a portfolio of Metric Deltas discover metrics with better held-out decision utility than a fixed hand-designed metric?

**RQ-2:** How can the system detect metric self-confirmation, reward hacking, and Goodhart-type failure when a solution generator adapts to the metric?

**RQ-3:** When does co-evolution outperform a sequential pipeline that first chooses a metric and then optimizes solutions?

**RQ-4:** What information is needed to compare results across metric versions without conflating score scales?

**RQ-5:** Can independent validators, counterexample search, and negative controls reduce the promotion of misleading metrics?

**RQ-6:** How do hard constraints, preference uncertainty, fairness considerations, and perspective changes interact with metric exploration?

**RQ-7:** Can metric knowledge and negative evidence transfer across tasks through Structural Relay without creating negative transfer?

**RQ-8:** Under what conditions can recursive generator or policy updates improve metric discovery while remaining auditable and reversible?

## 3. Falsifiable Hypotheses

**H1 — Metric exploration:** When the original proxy omits predictive features, a governed Metric Delta portfolio may improve held-out decision utility compared with an unchanged proxy.

**H2 — Goodhart vulnerability:** Unrestricted co-adaptation of solution generator and metric may increase internal scores without improving independent utility.

**H3 — Negative control:** On tasks with a known, sufficient, stable metric, metric evolution may add overhead or overfit and thus underperform a fixed-metric baseline.

**H4 — Independent validation:** Promotion gates using held-out evaluation and adversarial probes will reduce false metric promotions compared with self-scoring promotion.

**H5 — Portfolio diversity:** Preserving multiple metric hypotheses may improve robustness under perspective changes, but can increase compute and governance cost.

**H6 — Relay:** Source-validated Metric Relay may improve target-task learning if assumptions transfer; unvalidated metric reuse may produce negative transfer.

These are experimental hypotheses, not conclusions.

## 4. Core Separation: Solution, Metric, and Validation

Define three logically distinct spaces:

\[
\mathcal X = \text{candidate solutions},\quad
\mathcal M = \text{candidate metrics},\quad
\mathcal V = \text{independent validation evidence}.
\]

A solution score is \(m(x)\), while the quality of a metric is evaluated through a separate validation function:

\[
J(m;\mathcal D_{\mathrm{val}},\mathcal P,\mathcal G),
\]

where \(\mathcal D_{\mathrm{val}}\) is validation evidence, \(\mathcal P\) is a declared decision protocol, and \(\mathcal G\) is governance context.

The key separation is:

\[
\boxed{\text{High }m(x)\ \not\Rightarrow\ \text{High }J(m)}
\]

A candidate metric MUST NOT be promoted merely because solutions selected by that metric receive high values *under the same metric*.

### 4.1 The evaluation hierarchy

| Level | Object evaluated | Typical evidence | Permissible conclusion |
|---|---|---|---|
| E0 | Metric schema | Type and range checks | Metric is well formed |
| E1 | Metric computation | Deterministic fixture tests | Implementation behaves as specified |
| E2 | Predictive validity | Held-out labels/preferences | Metric predicts declared target |
| E3 | Decision utility | Outcomes of decisions selected by metric | Metric supports useful decisions |
| E4 | Robustness | Shift, adversarial and subgroup tests | Conditional robustness |
| E5 | Governance | Authorization, constraints, audit | Metric is eligible for deployment |

Passing E0–E2 does not automatically establish E3–E5.

## 5. Two-Timescale and Three-Loop Model

The minimal co-evolution model contains two nested search loops.

**Inner loop — Solution evolution:** Under a pinned metric version \(m_v\), explore feasible \(x\) using solution Deltas.

**Outer loop — Metric evolution:** Propose candidate metrics \(m_{v'}\), evaluate their effect on independently validated decisions, and accept or reject promotion.

A third, slower loop may revise the generator and policy used in either loop, subject to SPEC-006:

\[
\begin{aligned}
x_{t+1}&=T_X(x_t,\delta^X_t; m_v,C_v),\\
m_{k+1}&=T_M(m_k,\delta^M_k; G_M),\\
(G,P)_{r+1}&=T_{\mathrm{meta}}((G,P)_r,\delta^{\mathrm{meta}}_r).
\end{aligned}
\]

Here \(t,k,r\) denote different logical timescales; no fixed ratio is assumed. Metric candidates may be evaluated in isolated sandboxes without replacing the production metric.

### 5.1 Version isolation

An inner-loop result MUST retain the metric, constraint, generator, and policy versions under which it was obtained. Outer-loop promotion MUST NOT retroactively rewrite historical scores.

### 5.2 Non-monotonicity

Neither the inner nor the outer loop is guaranteed to improve monotonically. Even independently validated improvements may fail under distribution shift.

## 6. Canonical State Containers

Two State Container profiles are needed.

### 6.1 Solution State

\[
S^X=(x,\mathcal H_X,C_v,M_v,P_v,B_X,\Gamma_X).
\]

It includes the current solution, relevant search history, constraint and metric versions, policy, budget, and provenance.

### 6.2 Metric State

\[
S^M=(m,\mathcal H_M,D_{\mathrm{train}},D_{\mathrm{val}}^{\mathrm{ref}},G_v,B_M,\Gamma_M).
\]

It includes a candidate metric, its change history, references to training and validation partitions, governance version, budget, and provenance. The validation partition SHOULD be access-controlled; the generator must not be able to inspect hidden validation labels.

### 6.3 Metric Container example

```yaml
metric_id: "metric-allocation-v3"
metric_schema: "cde-metric-container@1.0.0"
version: "3.0.0-candidate"
task_profile: "constrained-allocation"
input_feature_schema_ref: "allocation-features-v2"
metric_form:
  family: "weighted-linear"
  parameters_ref: "metric-weights-candidate-03"
  output_range: "real"
  direction: "maximize"
perspective_id: "service-quality-and-efficiency"
normalization_contract_ref: "normalization-v2"
missingness_policy_ref: "missingness-v1"
uncertainty_model_ref: "uncertainty-v1"
hard_constraint_bundle_ref: "allocation-hard-constraints-v4"
training_data_ref: "train-split-v2"
validation_protocol_ref: "heldout-utility-v3"
promotion_status: "SANDBOX_CANDIDATE"
parent_metric_ref: "metric-allocation-v2"
provenance_ref: "metric-proposal-013"
```

A metric can be a vector, partial order, constraint-aware function, or learned model; scalar weighted sums are only one profile.

## 7. Metric Delta Taxonomy

Metric Deltas are typed transformations of a metric definition or its operational context.

| Delta | Operation | Primary risk |
|---|---|---|
| `REWEIGHT` | Adjust feature weights | Overfitting to observed outcomes |
| `ADD_FEATURE` | Introduce a feature | Leakage, spurious correlation |
| `REMOVE_FEATURE` | Remove a feature | Loss of important signal |
| `TRANSFORM_FEATURE` | Change scaling or nonlinear transform | Score discontinuity |
| `CHANGE_AGGREGATION` | Weighted sum to lexicographic/Pareto rule | Incomparability |
| `CHANGE_PERSPECTIVE` | Shift stakeholder/task viewpoint | Authority mismatch |
| `ADD_UNCERTAINTY` | Include uncertainty estimate | Miscalibration |
| `CALIBRATE` | Fit score-to-probability mapping | Dataset shift |
| `REVISE_MISSINGNESS` | Change treatment of missing values | Selection bias |
| `REVISE_TEMPORAL_SCOPE` | Change evidence horizon | Stale or leaking evidence |
| `ADD_ROBUSTNESS_TERM` | Penalize sensitivity to shift | Wrong robustness proxy |
| `ADD_FAIRNESS_DIAGNOSTIC` | Add evaluation criterion | Poorly defined groups or tradeoffs |
| `COMPOSE_METRICS` | Build composite metric | Double counting |
| `SPLIT_METRIC` | Separate conflicting objectives | Loss of simple ordering |
| `PROPOSE_NEW_FORM` | Change metric family | Excessive search capacity |
| `REVISE_VALIDATION` | Propose validator changes | Validator capture |

The final operation is **not** an ordinary metric update. Changes to independent validation require separate governance approval and a new validation-contract version.

### 7.1 Metric Delta descriptor

```yaml
metric_delta_id: "delta-metric-009"
delta_type: "ADD_FEATURE"
type_version: "1.0.0"
source_metric_ref: "metric-allocation-v2"
feature_ref: "student-ride-time-p95-v1"
precondition_contract_ref: "feature-availability-v1"
expected_effects:
  - "new-input-dimension"
  - "new-score-version"
required_validation:
  - "schema"
  - "heldout-predictive"
  - "decision-utility"
  - "shift-and-subgroup"
authorization_scope: "sandbox-only"
provenance_ref: "proposal-009"
```

The descriptor does not imply that the feature is appropriate or beneficial.

## 8. Solution Delta Taxonomy

The inner solution loop uses task-specific operators, including:

- local parameter edits;
- route or assignment changes;
- structural graph rewrites;
- regional neighborhood search;
- long-range restarts;
- cross-domain method proposals.

A Metric Delta MUST NOT silently change hard feasibility rules. The constraint bundle has separate authority and versioning.

### 8.1 Distinguishing improvement types

A candidate can:

1. improve its score under a fixed metric;
2. improve independently measured task utility;
3. remain feasible under hard constraints;
4. improve robustness across contexts.

These are separate predicates. A solution promotion policy may require all four, depending on the task.

## 9. Benchmark Family A — Synthetic Proxy Misalignment

Construct candidates \(x\) with observable features \(z(x)\), a proxy metric \(m_0(x)\), and a hidden reference utility \(u^\star(x)\).

For example:

\[
u^\star(x)=2z_1(x)+z_2(x)-3\max(0,z_3(x)-\tau)^2,
\]

\[
m_0(x)=2z_1(x)+z_2(x).
\]

The proxy omits a nonlinear penalty. A solution generator that maximizes \(m_0\) may exploit high \(z_3\) values if correlated with \(z_1\). A Metric Delta adding the omitted penalty may improve decisions.

**Caution:** In a real discovery experiment, the candidate metric generator must not receive the hidden formula. The benchmark oracle retains it solely for independent evaluation.

### 9.1 Variants

- A0: proxy exactly equals reference utility (negative control);
- A1: omitted feature;
- A2: wrong feature sign;
- A3: nonlinear threshold omitted;
- A4: correlated spurious feature;
- A5: delayed outcome;
- A6: distribution shift;
- A7: adversarially optimized proxy;
- A8: conflicting stakeholder perspectives.

### 9.2 Primary metrics

Measure regret relative to a trusted oracle or numerical reference, held-out decision utility, metric calibration where applicable, and promotion error rate.

## 10. Benchmark Family B — School-Bus Objective Design

Use a simplified school-bus assignment and routing instance to illustrate the difference between optimizing a valid objective and optimizing the right objective.

A plausible *declared* operational hierarchy is:

1. satisfy safety, capacity, and timing constraints;
2. minimize physical fleet count;
3. among equal-fleet solutions, reduce student ride-time burden;
4. then consider mileage, robustness, or operational costs.

A weighted sum that trades one additional vehicle against a small ride-time reduction may violate the intended fleet-first priority. A lexicographic metric can represent the policy more faithfully:

\[
\min_{\mathrm{lex}}\left(
N_{\mathrm{vehicles}},
T_{\mathrm{ride},95},
T_{\mathrm{ride,total}},
C_{\mathrm{operations}}
\right).
\]

The ordering is a **case assumption** and must be authorized by the relevant stakeholder; it is not universally correct.

### 10.1 Metric discovery challenge

Compare:

- route count as proxy;
- physical fleet count;
- fleet count plus average ride time;
- fleet count plus worst-case or 95th-percentile ride time;
- lexicographic multiobjective policy.

A candidate metric that reduces route count but increases required physical vehicles demonstrates a semantic metric error.

### 10.2 Hard-constraint boundary

Student safety, vehicle capacity, and mandatory time windows are not merely weighted penalties. A Metric Delta cannot authorize violating them.

## 11. Benchmark Family C — Dynamic Structural Matching

Use synthetic graph matching from CASE-003.

Candidate metrics include:

\[
m_{\mathrm{attr}},\quad
m_{\mathrm{rel}},\quad
m_{\mathrm{topo}},\quad
m_{\mathrm{context}},\quad
m_{\mathrm{combined}}.
\]

An attribute-heavy metric may perform well on clean labels and fail on corrupted labels. A relation-heavy metric may fail on symmetric or noisy graphs. Co-evolution may adapt feature weights or metric structure, but must be tested on held-out graphs with different corruption patterns.

### 11.1 Critical separation

Improved internal matching score does not imply improved ground-truth F1, calibration, or attribution reliability.

### 11.2 Abstention

A metric that increases coverage by forcing uncertain matches may worsen false-positive risk. Evaluate selective risk as a function of coverage.

## 12. Benchmark Family D — Preference and Perspective Evolution

Generate synthetic decision tasks with multiple stakeholder perspectives. Each perspective has a declared latent utility function; the learner observes limited pairwise preferences or ranking judgments.

The metric generator must infer a useful representation from training feedback and generalize to held-out preferences.

### 12.1 Perspective identity

A change in perspective is not a routine weight update if it changes who is entitled to define success. The metric must carry a `perspective_id` and an authorization record.

### 12.2 Disagreement

Where stakeholders genuinely disagree, there may be no single universally correct scalar metric. Report a Pareto set, a family of metrics, or an explicit governance choice instead of fabricating consensus.

## 13. Benchmark Family E — Metric Drift and Feedback Loops

Model a deployment environment in which the data distribution changes as the policy optimized by the metric changes.

\[
D_{t+1}=\Phi(D_t,\pi_{m_t},\xi_t).
\]

The metric affects the policy, which affects future observations. This creates feedback loops and possible distribution shift.

A naive evaluation on historical, policy-dependent data may overestimate performance. The benchmark should compare static holdout evaluation with a simulated intervention or randomized policy evaluation when feasible.

### 13.1 Feedback contamination

A metric may look more predictive after it influences which outcomes are observed. The experiment must track selection mechanisms and missing counterfactual outcomes.

### 13.2 Off-policy evaluation

Importance weighting or doubly robust estimators may be useful under appropriate assumptions. They are not universally unbiased or stable; overlap and propensity quality must be checked.

## 14. Benchmark Family F — Metric Discovery Without a Known Oracle

Real scientific discovery often lacks a ground-truth utility formula. The case therefore includes a restricted profile with:

- independent expert judgments;
- predeclared observable outcomes;
- inter-rater disagreement;
- confidence intervals;
- delayed replication;
- explicit `UNRESOLVED` status.

The laboratory MUST NOT claim a discovered metric is objectively true merely because experts agree on a small sample. It may claim conditional predictive or decision utility within the validated scope.

## 15. Adaptive M-to-N Metric Branching

At each outer-loop invocation:

\[
M=\#\text{Metric Delta proposals},
\quad Q=\#\text{validation attempts},
\quad N=\#\text{realized candidate evaluations},
\quad K=\#\text{distinct metric successors}.
\]

Some candidates may be invalid, redundant, deferred, or rejected. The system MUST record the full accounting, including unsuccessful proposals and validation costs.

### 15.1 Proposal diversity

The metric generator SHOULD preserve alternatives spanning:

- feature selection;
- functional form;
- aggregation rule;
- robustness assumptions;
- perspective;
- calibration method.

A purely greedy metric search may prematurely converge on a spurious proxy.

### 15.2 Coverage claims

A finite search over a predefined weight grid can be exhaustive for that grid, but not for all possible metrics. The generator must state the scope of any coverage claim.

### 15.3 Cross-scale proposals

Local Metric Deltas may perturb weights; regional Deltas may change feature groups; long-range Deltas may propose a different metric family. Their costs and validation burdens differ.

## 16. Metric Candidate Lifecycle

The normative lifecycle is:

```text
PROPOSED
  -> TYPE_VALIDATED
  -> SANDBOX_READY
  -> TRAINED_OR_INSTANTIATED
  -> INDEPENDENTLY_EVALUATED
  -> {REJECTED, DEFERRED, ELIGIBLE_FOR_PROMOTION}
  -> AUTHORIZED_FOR_LIMITED_USE
  -> MONITORED
  -> {RETAINED, ROLLED_BACK, RETIRED}
```

A candidate may fail at any stage. `ELIGIBLE_FOR_PROMOTION` is not equivalent to `AUTHORIZED_FOR_LIMITED_USE`.

### 16.1 Promotion record

```yaml
promotion_decision_id: "promotion-024"
candidate_metric_ref: "metric-allocation-v3-candidate"
baseline_metric_ref: "metric-allocation-v2"
evaluation_protocol_ref: "heldout-utility-v3"
evidence_refs:
  - "eval-summary-024"
  - "shift-report-024"
  - "constraint-audit-024"
decision: "DEFERRED"
reason_codes:
  - "INSUFFICIENT_SHIFT_EVIDENCE"
authorized_scope: null
rollback_checkpoint_ref: "metric-allocation-v2"
reviewer_or_authority_ref: "governance-board-v1"
provenance_ref: "promotion-event-024"
```

This is a schema example, not an actual promotion decision.

## 17. Hard Constraints and Governance

Metric evolution must preserve the distinction between:

- **hard constraints:** feasibility and authorization requirements;
- **soft preferences:** optimizable tradeoffs;
- **evaluation criteria:** evidence used to judge metric quality;
- **governance rules:** authority to deploy or change metrics.

A Metric Delta that attempts to weaken a hard constraint MUST be rejected or routed to a separate authorized constraint-change process.

### 17.1 Authorization

The metric generator may propose changes, but it should not have unilateral authority to change the production objective or independent validator.

### 17.2 Safety and fairness

When metrics affect people, the validation plan should include appropriate impact analysis, subgroup performance, contestability, and human oversight. A fairness diagnostic is not a universal fairness guarantee.

### 17.3 Data rights

Metric learning must respect data access restrictions, privacy, retention rules, and provenance. A feature that predicts outcomes well may still be prohibited or inappropriate to use.

## 18. Independent Validation and Leakage Control

The metric-generation loop should not optimize directly against the final held-out evaluation set.

Recommended separation:

1. **Training partition:** used to fit or generate candidate metrics.
2. **Development partition:** used for internal tuning and preliminary comparisons.
3. **Promotion validation partition:** controlled independent evaluation.
4. **Final test partition:** reserved for pre-registered claims.
5. **Deployment monitoring:** prospective evidence after authorization.

Repeated adaptive access to a validation set can overfit it. Promotion decisions should account for the number of candidates tested, selection bias, and uncertainty.

### 18.1 Independent evaluator

The evaluator must use criteria that are not trivially identical to the candidate metric's own output. It may evaluate true task outcomes, trusted labels, prospective utility, constraint compliance, or calibrated preferences.

### 18.2 Metric self-confirmation test

For each candidate \(m'\), compare:

\[
\Delta_{\mathrm{self}}
=
m'(x_{m'})-m'(x_{\mathrm{baseline}})
\]

with:

\[
\Delta_{\mathrm{ind}}
=
U_{\mathrm{ind}}(x_{m'})-U_{\mathrm{ind}}(x_{\mathrm{baseline}}).
\]

A positive \(\Delta_{\mathrm{self}}\) with nonpositive \(\Delta_{\mathrm{ind}}\) is a warning sign. The independent utility estimator may itself be uncertain and must report that uncertainty.

## 19. Goodhart and Reward-Hacking Stress Tests

Construct adversarial or adaptive optimizers that deliberately exploit metric weaknesses.

### 19.1 Omitted penalty

The proxy ignores an important nonlinear cost. Optimize the proxy aggressively and measure hidden true utility.

### 19.2 Feature manipulation

Permit the solver to increase a measured feature without improving the underlying task outcome.

### 19.3 Dataset shift

Change correlations between training and test environments.

### 19.4 Validator overfitting

Allow many candidate metrics to query the same development set, then compare with a sealed final test.

### 19.5 Proxy laundering

A candidate metric incorporates another learned proxy and appears to add independent evidence while actually double counting the same source.

### 19.6 Governance bypass

A generator proposes changing the evaluation or constraint contract to make its own performance appear better. This must trigger separate review, not automatic acceptance.

### 19.7 Reward inversion

Create a benchmark where optimizing the proxy above a threshold worsens true utility. A robust evaluation should detect this non-monotonicity.

## 20. Metric Comparability and Version Semantics

Scores from different metric versions are not automatically comparable.

For example, `82` under a 0–100 scale and `0.82` under a normalized scale may represent equivalent rankings, but only if a validated transformation exists. A change from weighted sum to lexicographic ordering may eliminate scalar comparability entirely.

### 20.1 Required metadata

Every score SHOULD carry:

```yaml
score_record:
  solution_ref: "solution-028"
  metric_id: "metric-allocation"
  metric_version: "3.0.0"
  perspective_id: "fleet-first"
  feature_schema_version: "2.0.0"
  constraint_bundle_id: "school-bus-C4"
  score_value: 0.82
  score_type: "scalar"
  uncertainty_ref: null
  evaluation_context_ref: "context-028"
  provenance_ref: "score-event-028"
```

### 20.2 Re-scoring

When comparing solution candidates across metrics, the system SHOULD re-score the same candidate set under a common pinned metric, or use a validated cross-version comparison contract. A direct comparison of incompatible scores is prohibited.

### 20.3 Pareto and lexicographic cases

Vector-valued metrics require a declared dominance or selection rule. A Pareto frontier cannot be reduced to a single best score without an additional authorized preference.

## 21. Solver Portfolio

The metric-discovery problem can be approached by multiple solver types.

| Method | Primary role | Typical limitation |
|---|---|---|
| Grid/random search | Simple parameter exploration | Expensive in high dimensions |
| Bayesian optimization | Sample-efficient hyperparameter search | Surrogate assumptions |
| Evolutionary search | Diverse metric structures | Cost and stochastic variance |
| Gradient-based learning | Differentiable metric fitting | Local minima and proxy dependence |
| Bilevel optimization | Optimize metric for downstream decisions | Computationally demanding |
| Preference learning | Infer utility from comparisons | Preference noise and disagreement |
| Multiobjective optimization | Preserve tradeoffs | Selection requires governance |
| CDE Metric Delta portfolio | Versioned orchestration of alternatives | No intrinsic optimality guarantee |

CDE MAY orchestrate these methods but MUST preserve solver-specific assumptions and proof claims.

### 21.1 Bound-based pruning

A metric candidate may be pruned only with a sound bound under a pinned objective, validation protocol, and constraint version. Heuristic low ranking is not certified pruning.

### 21.2 Approximation claims

If a solver reports an approximation gap, the report must specify its bound, reference objective, assumptions, and validity scope. A learned validation score does not create a certified gap by itself.

## 22. Reference Algorithm — Governed Co-Evolution

```text
function GOVERNED_SOLUTION_METRIC_COEVOLUTION(task, configuration):
    active_metric = pin_authorized_metric(task)
    active_constraints = pin_hard_constraints(task)
    solution_ledger = immutable_ledger()
    metric_ledger = immutable_ledger()

    for outer_round in configured_outer_budget:
        # Inner loop: solution search under a fixed metric.
        solution_result = run_solver_portfolio(
            task=task,
            metric=active_metric,
            constraints=active_constraints,
            budget=configuration.solution_budget
        )
        validate_and_record_solution_result(solution_result, solution_ledger)

        # Outer loop: propose metrics; do not activate them.
        proposals = generate_typed_metric_deltas(
            active_metric,
            evidence=authorized_training_evidence(),
            budget=configuration.metric_proposal_budget
        )
        record_proposals(proposals, metric_ledger)

        selected = select_metric_validation_attempts(
            proposals,
            diversity_policy=configuration.diversity_policy
        )

        for candidate in selected:
            if not type_check_and_authorize(candidate):
                record_rejection(candidate)
                continue

            sandbox_metric = instantiate_metric_in_sandbox(candidate)
            sandbox_result = run_comparable_solution_search(
                task,
                metric=sandbox_metric,
                constraints=active_constraints,
                fixed_budget=configuration.comparison_budget
            )

            independent_report = evaluate_without_metric_self_scoring(
                sandbox_result,
                protocol=sealed_validation_protocol(),
                include_shift_and_adversarial_tests=True
            )

            record_candidate_and_evidence(
                candidate,
                sandbox_result,
                independent_report,
                metric_ledger
            )

            if independently_eligible(independent_report):
                submit_to_authorized_promotion_gate(candidate)
            else:
                preserve_negative_evidence_and_leftovers(candidate)

        reconcile_branch_accounting_and_budgets()

        promotion = read_authorized_promotion_decision()
        if promotion.approved:
            active_metric = activate_versioned_metric(promotion)
            retain_rollback_checkpoint()

        monitor_active_metric_for_regression_and_shift()
        if rollback_conditions_met():
            active_metric = rollback_to_authorized_checkpoint()

    return audited_report(
        active_metric,
        solution_ledger,
        metric_ledger,
        unresolved_metric_candidates(),
        resource_costs(),
        versioned_provenance()
    )
```

This is a conceptual reference algorithm. A real implementation must ensure that the validation evaluator is independent of candidate-generation authority and that sealed validation data are not exposed through side channels or repeated adaptive queries.

## 23. Reference Algorithm — Metric Comparison

```text
function COMPARE_METRICS(metric_A, metric_B, task_suite, protocol):
    assert compatible_task_scope(metric_A, metric_B)
    assert same_hard_constraint_bundle(protocol)

    results = []
    for task in protocol.heldout_tasks:
        seed = protocol.paired_seed(task)
        run_A = solve_under_metric(metric_A, task, seed, protocol.budget)
        run_B = solve_under_metric(metric_B, task, seed, protocol.budget)

        verify_feasibility(run_A)
        verify_feasibility(run_B)

        outcome_A = independent_outcome_evaluation(run_A, protocol)
        outcome_B = independent_outcome_evaluation(run_B, protocol)

        results.append({
            "task": task.id,
            "outcome_A": outcome_A,
            "outcome_B": outcome_B,
            "cost_A": run_A.total_cost,
            "cost_B": run_B.total_cost
        })

    return paired_statistical_analysis(
        results,
        primary_metric=protocol.primary_metric,
        uncertainty_method=protocol.uncertainty_method
    )
```

Matching seeds and budgets does not eliminate all variance. Randomization, solver initialization, and candidate set differences should be documented.

## 24. Worked Example A — Proxy Improvement Versus True Utility

Suppose a candidate \(x\) has features:

\[
z_1=4,\quad z_2=2,\quad z_3=5,\quad \tau=2.
\]

Under the synthetic formulas in Section 9:

\[
m_0(x)=2(4)+2=10,
\]

\[
u^\star(x)=10-3(5-2)^2=-17.
\]

A different candidate \(y\) with \(z_1=3,z_2=2,z_3=2\) has:

\[
m_0(y)=8,\qquad u^\star(y)=8.
\]

Thus the proxy ranks \(x\) above \(y\), while the reference utility ranks \(y\) above \(x\). This is a **constructed arithmetic counterexample**, not an experimental observation.

A Metric Delta introducing the missing penalty may repair this instance, but independent tests are needed to establish generalization.

## 25. Worked Example B — Fleet-First Metric Revision

A school-bus optimizer compares:

- Plan A: 12 physical vehicles, 35-minute median ride;
- Plan B: 11 physical vehicles, 38-minute median ride.

If the authorized policy is strictly fleet-first, B ranks ahead of A, assuming both satisfy all hard ride-time and safety constraints. A metric heavily weighting ride time may reverse the ranking.

The research issue is not that one metric is universally correct. It is that the metric must faithfully represent the **declared decision policy**, including priority order, rather than substituting an arbitrary scalarization.

## 26. Worked Example C — Matching Metric Under Schema Drift

A structural matcher initially uses labels and neighbor topology. The target graph later adopts a new naming convention. Label similarity declines while relational consistency remains informative.

The metric generator proposes reducing label weight and increasing relation weight. Independent held-out graphs test whether the change improves correspondence F1 and calibration. If it only improves training performance, the candidate is not promoted.

## 27. Worked Example D — Competing Perspectives

Two stakeholders value operational cost and service reliability differently. The system maintains two perspective-specific metrics rather than averaging them into an unapproved compromise.

A decision report can show a Pareto comparison and request a legitimate policy choice. CDE's role is to preserve the structural alternatives and evidence, not to invent whose preferences should prevail.

## 28. Worked Example E — Recursive Generator Evolution

A metric generator repeatedly proposes small weight changes but rarely explores functional-form changes. A meta-Delta proposes adding nonlinear feature transformations to its operator library.

The new generator is evaluated on held-out metric-discovery tasks, including tasks where linear metrics are sufficient. Promotion requires evidence that its broader proposal capacity yields net benefit after compute cost and overfitting risk.

This is controlled generator evolution, not proof of autonomous self-improvement.

## 29. Negative Controls

**NC-01 — Correct fixed metric:** The original metric is sufficient; evolution should not be presumed helpful.

**NC-02 — Spurious training correlation:** A feature predicts training outcomes but reverses under shift.

**NC-03 — Metric self-scoring:** The candidate's internal score increases while independent utility decreases.

**NC-04 — Hidden validation leakage:** The generator obtains information from a supposedly sealed test set.

**NC-05 — Metric scale change:** Scores are numerically incomparable across versions.

**NC-06 — Incompatible perspectives:** The system tries to combine unauthorized stakeholder objectives.

**NC-07 — Constraint laundering:** A metric change attempts to convert a hard requirement into a soft penalty.

**NC-08 — Proxy manipulation:** The solution generator inflates a measurable proxy without improving the underlying outcome.

**NC-09 — Multiple-testing bias:** Many candidates are tried, and only the best validation result is reported.

**NC-10 — Stale evidence:** A metric trained on obsolete feature semantics is reused.

**NC-11 — Correlated evidence:** Multiple validators share the same underlying data source.

**NC-12 — Negative Relay transfer:** A metric successful in one domain fails in another.

**NC-13 — Recursive overfitting:** Generator evolution improves training discovery but worsens held-out outcomes.

**NC-14 — Coordination overhead:** Co-evolution consumes more resources than its utility gains justify.

## 30. Ablation Matrix

| ID | Ablation | Mechanism tested |
|---|---|---|
| A1 | Freeze metric | Value of metric evolution |
| A2 | Freeze solution generator | Co-adaptation effects |
| A3 | No independent validator | Self-confirmation risk |
| A4 | No shift tests | Robustness gate value |
| A5 | No negative controls | False promotion risk |
| A6 | No long-range Metric Deltas | Metric-family exploration |
| A7 | No diversity reserve | Premature convergence |
| A8 | No Relay | Cross-task transfer |
| A9 | No versioned score records | Comparability failure |
| A10 | No rollback | Recovery from regression |
| A11 | Fixed versus adaptive allocation | Branch selection value |
| A12 | No governance review | Authorization boundary |
| A13 | No uncertainty model | Calibration and selection risk |
| A14 | No counterexample search | Metric weakness discovery |
| A15 | No cost penalty | Over-complexity risk |
| A16 | No generator meta-evolution | Value of recursive adaptation |

Ablations MUST be evaluated under comparable budgets and reported with negative results.

## 31. Experimental Baselines

| ID | Baseline | Description |
|---|---|---|
| B0 | Fixed expert metric | No metric search |
| B1 | Random metric search | Simple search over allowed parameters |
| B2 | Grid search | Exhaustive within declared finite grid |
| B3 | Bayesian hyperparameter optimization | Surrogate-guided metric tuning |
| B4 | Gradient-based metric learning | Differentiable fitting |
| B5 | Bilevel optimization | Metric tuned for downstream decisions |
| B6 | Preference learning | Learn from pairwise choices |
| B7 | Evolutionary metric search | Population-based metric generation |
| B8 | Multiobjective/Pareto selection | Preserve tradeoffs |
| C1 | CDE metric portfolio | Typed Metric Deltas and validation |
| C2 | CDE co-evolution | Joint solution and metric loops |
| C3 | CDE co-evolution + Relay | Target-validated transfer |
| C4 | CDE controlled recursive evolution | Governed generator/policy updates |

Comparisons must account for differences in information access, tuning budget, number of metric candidates, and downstream solver evaluations.

## 32. Primary Evaluation Metrics

### 32.1 Held-out decision utility

\[
U_{\mathrm{test}}(m)
=
\frac{1}{n}\sum_{i=1}^{n}
u_i^\star(x_i(m)),
\]

where \(x_i(m)\) is the solution chosen using \(m\) on task \(i\), and \(u_i^\star\) is an independent reference outcome available to the benchmark evaluator.

### 32.2 Decision regret

\[
R(m)=\frac{1}{n}\sum_i
\left[u_i^\star(x_i^\star)-u_i^\star(x_i(m))\right].
\]

If \(x_i^\star\) is only numerically approximated, label the result approximate regret, not certified optimality gap.

### 32.3 False promotion rate

Fraction of promoted metrics that fail a preregistered independent improvement or noninferiority criterion. The criterion and its uncertainty must be stated.

### 32.4 Robustness under shift

Measure change in decision utility across shifted environments, including worst-case performance over a declared finite stress-test family.

### 32.5 Metric stability

Track variation in decisions and scores under small input perturbations. Stability alone is not validity.

### 32.6 Cost-adjusted improvement

Report improvement per objective evaluation and per total compute cost. Do not hide validation or orchestration overhead.

### 32.7 Calibration

Where scores are probabilistic, evaluate calibration with proper scoring rules. Nonprobabilistic metrics should not be labeled calibrated probabilities.

### 32.8 Governance compliance

Count unauthorized changes, stale-version acceptances, constraint violations, rollback failures, and missing provenance events.

## 33. Experimental Protocol

1. Define the task family and independently measured target outcomes.
2. Fix hard constraints and decision authority.
3. Split training, development, promotion validation, and final test sets.
4. Freeze baseline methods and tuning budgets.
5. Generate Metric Delta candidates under versioned manifests.
6. Run inner-loop solution search with equal primary budgets.
7. Validate selected solutions independently.
8. Conduct shift, adversarial, and negative-control tests.
9. Apply promotion gates without leaking final test results.
10. Evaluate authorized metrics prospectively or on sealed final tests.
11. Report uncertainty, failure modes, and total cost.
12. Preserve artifacts, Leftovers, and rollback records.

### 33.1 Replication

Use multiple independent tasks and random seeds. A starting target of 30 independent runs per configuration is reasonable for exploratory planning, but statistical power must be assessed for the chosen effect size and variance.

### 33.2 Paired comparison

Prefer paired task instances and matched initial information. Report paired outcome differences and confidence intervals.

### 33.3 Multiple comparisons

Metric discovery is an adaptive multiple-testing problem. Predefine primary contrasts, account for candidate-selection bias, and reserve a sealed final test.

### 33.4 Negative findings

Publish failed Metric Deltas, harmful shifts, and unsuccessful promotions with appropriate provenance. Suppressing these would bias conclusions about metric evolution.

## 34. Distributed Metric Exploration

Metric candidates can be evaluated by heterogeneous workers, but a distributed result is not automatically trustworthy.

A worker task envelope SHOULD contain:

```yaml
task_id: "metric-eval-047"
candidate_metric_ref: "metric-v3-candidate"
parent_metric_ref: "metric-v2"
task_suite_ref: "heldout-suite-v2"
constraint_bundle_id: "constraints-v4"
evaluation_protocol_ref: "independent-utility-v3"
policy_version: "policy-v2"
budget:
  solver_evaluations: 100
  wall_time_seconds: 1800
validation_contract_ref: "metric-eval-contract-v2"
idempotency_key: "metric-eval-047-v1"
authorization_scope: "sandbox"
provenance_ref: "dispatch-047"
```

The coordinator MUST reconcile task epochs, metric versions, evidence provenance, and any global resource constraints before accepting results.

### 34.1 Leakage risks

Distributing sealed validation data to untrusted workers can defeat independence. Use restricted evaluation services, privacy-preserving protocols where appropriate, or independent evaluators with explicit access control.

### 34.2 Result merging

Two workers' score summaries are not directly mergeable if they used different validation datasets, metrics, constraints, or sampling procedures.

### 34.3 Failure injection

Test stale results, duplicate retries, inconsistent seeds, unauthorized metric changes, corrupted evidence, and partial completion. Preserve typed Leftover artifacts for unfinished evaluations.

## 35. Structural Relay for Metric Knowledge

Seven Relay artifact families are applicable:

- **Result:** validated metric-comparison outcomes;
- **Evidence:** held-out observations, failures, calibration diagnostics;
- **Constraint:** conditions under which a metric is invalid;
- **Method:** effective metric-generation or validation techniques;
- **Policy:** budget and selection rules;
- **Metric:** candidate metric definitions and scope;
- **Leftover:** unresolved metric hypotheses, untested shifts, missing evidence.

### 35.1 Relay contract

```yaml
relay_id: "metric-relay-021"
artifact_type: "Metric"
source_task_ref: "allocation-study-v1"
source_metric_ref: "metric-allocation-v3"
applicability_conditions:
  - "same-feature-semantics"
  - "compatible-hard-constraints"
  - "similar-outcome-horizon"
evidence_refs:
  - "validation-report-021"
known_failure_modes:
  - "degrades-under-high-demand-shift"
target_validation_status: "NOT_EVALUATED"
authorization_scope: "candidate-only"
provenance_ref: "relay-event-021"
```

### 35.2 Target assimilation

A target task MUST validate feature semantics, perspective, constraints, and decision utility before adopting a transferred metric. Source-task success is not target-task proof.

### 35.3 Negative transfer

A metric may be valid for one school district, graph family, or preference group and inappropriate for another. Negative transfer reports should be first-class Relay artifacts.

### 35.4 Repeated execution

Repeated evaluations under independent seeds or contexts can accumulate confidence or expose instability. Repetition should be purposeful and budgeted, not automatically counted as improvement.

## 36. Controlled Recursive Evolution

Metric discovery can motivate changes to its own generator or policy. This creates an outer meta-learning problem.

Candidate meta-Deltas may:

- introduce new Metric Delta operators;
- change proposal scale distributions;
- allocate more budget to counterexample search;
- alter solver portfolio selection;
- adjust validation scheduling;
- propose new uncertainty models.

The meta-loop MUST NOT rewrite the independent success criterion merely to validate itself. Candidate changes require sandbox evaluation, held-out tests, authorization, monitoring, and rollback.

### 36.1 Recursive risk

A system that selects future evaluators using its own preferred metric can create an escalating self-confirmation loop. Separate evaluator governance is therefore essential.

### 36.2 No monotonicity guarantee

A sequence of accepted meta-Deltas can still regress under new environments. Promotions are conditional decisions, not a proof of open-ended improvement.

## 37. Conformance Test Matrix

| ID | Test | Expected behavior |
|---|---|---|
| MCE-001 | Valid Metric Container | Schema accepted |
| MCE-002 | Missing metric version | Rejected |
| MCE-003 | Unknown perspective | Deferred/rejected |
| MCE-004 | Unauthorized feature | Blocked |
| MCE-005 | Invalid Metric Delta | Rejected |
| MCE-006 | Delta changes hard constraint | Routed to separate authority |
| MCE-007 | Proposal/attempt accounting | M/Q/N/K reconciled |
| MCE-008 | Partial metric generation | No exhaustive claim |
| MCE-009 | Duplicate metric proposal | Distinct provenance retained |
| MCE-010 | Invalid score comparison | Blocked |
| MCE-011 | Cross-version rescore | Common metric used |
| MCE-012 | Pareto metric | Dominance contract enforced |
| MCE-013 | Lexicographic metric | Priority order preserved |
| MCE-014 | Self-score increase only | No automatic promotion |
| MCE-015 | Independent utility regression | Candidate rejected/deferred |
| MCE-016 | Validation UNKNOWN | No unsupported approval |
| MCE-017 | Validation leakage | Flagged/blocked |
| MCE-018 | Multiple candidate selection | Bias accounted for |
| MCE-019 | Stale feature schema | Revalidation |
| MCE-020 | Changed normalization | New version |
| MCE-021 | Calibration claim | Proper evidence required |
| MCE-022 | Wrong constraint bundle | Rejected |
| MCE-023 | Hard feasibility failure | Solution not committed |
| MCE-024 | Metric candidate unauthorized | Sandbox only |
| MCE-025 | Governance approval absent | No activation |
| MCE-026 | Promotion record incomplete | Rejected |
| MCE-027 | Rollback checkpoint missing | Promotion blocked if required |
| MCE-028 | Deployment regression | Monitoring and rollback |
| MCE-029 | Correlated evidence | Dependency recorded |
| MCE-030 | Hidden oracle requested | Access denied |
| MCE-031 | Fixed-metric negative control | Reported |
| MCE-032 | Spurious feature shift | Evaluated |
| MCE-033 | Proxy exploitation | Independent outcome reported |
| MCE-034 | False optimality claim | Rejected |
| MCE-035 | Candidate not generated | No coverage claim |
| MCE-036 | Metric generator version changed | Coverage rechecked |
| MCE-037 | Policy meta-Delta | Independent promotion gate |
| MCE-038 | Validator meta-Delta | Separate authority |
| MCE-039 | Worker stale epoch | Result not blindly accepted |
| MCE-040 | Worker duplicate retry | Idempotent committed effects |
| MCE-041 | Different validation datasets | Results not naively merged |
| MCE-042 | Relay schema mismatch | Applicability check |
| MCE-043 | Relay target failure | Not promoted |
| MCE-044 | Leftover metric candidate | Preserved |
| MCE-045 | Negative evidence relay | Scoped correctly |
| MCE-046 | Repeated evaluation | Distinct evidence events |
| MCE-047 | Budget overrun | Reported and stopped |
| MCE-048 | No feasible solution | Typed outcome |
| MCE-049 | Missing cost ledger | Conformance failure |
| MCE-050 | Privacy-restricted feature | Access control |
| MCE-051 | Conflicting perspectives | No fabricated consensus |
| MCE-052 | Undefined score | Explicit missingness handling |
| MCE-053 | Selection-induced feedback | Evaluation caveat |
| MCE-054 | Approximate regret | Not labeled certified gap |
| MCE-055 | Claimed universal superiority | Unsupported |
| MCE-056 | No experiments executed | No empirical results claimed |

These tests are a proposed suite, not executed test results.

## 38. Formal Safety and Integrity Invariants

**INV-MCE-01 — Typed metrics:** Every metric has a schema, version, output semantics, and perspective.

**INV-MCE-02 — Constraint supremacy:** A metric cannot override applicable hard constraints.

**INV-MCE-03 — Source isolation:** Metric candidate generation cannot access sealed validation outcomes without authorization.

**INV-MCE-04 — Version fidelity:** Every score records the metric and relevant context versions.

**INV-MCE-05 — Comparison soundness:** Incompatible scores are not directly compared as though they share semantics.

**INV-MCE-06 — Promotion separation:** Candidate generation and independent promotion authority remain distinct.

**INV-MCE-07 — Validation evidence:** Promotion requires declared, traceable independent evidence.

**INV-MCE-08 — Historical integrity:** Metric changes do not rewrite historical evaluations.

**INV-MCE-09 — Reversibility:** Where rollback is required, a valid checkpoint is retained.

**INV-MCE-10 — Uncertainty preservation:** Unknown validation outcomes are not treated as success.

**INV-MCE-11 — Budget accountability:** Proposal, training, validation, and orchestration costs are recorded.

**INV-MCE-12 — Relay scope:** Source metric validity does not imply target applicability.

**INV-MCE-13 — Governance boundary:** Validator and constraint changes require separate authorization.

**INV-MCE-14 — No self-proof:** A metric's own score cannot alone establish its quality.

**INV-MCE-15 — Negative evidence retention:** Failed candidates and adverse findings remain traceable.

**INV-MCE-16 — No false guarantee:** Conformance does not imply globally optimal metrics or monotonic recursive improvement.

These invariants rely on correctly implemented validators, coherent versioning, and enforcement of access-control boundaries.

## 39. Conditional Correctness Arguments

### 39.1 Constraint preservation

If every committed solution satisfies a sound hard-constraint validator under a coherent version snapshot, and metric changes cannot modify that validator, then metric evolution alone cannot authorize an infeasible committed solution.

This does not establish that the encoded constraints are complete or appropriate.

### 39.2 Version-consistent comparison

If all compared solutions are re-evaluated under the same deterministic metric version and input-feature semantics, their numerical scores are comparable within that metric's declared ordering.

This does not establish that the metric is valid for the external objective.

### 39.3 Promotion integrity

If production activation requires an unforgeable authorized promotion record and candidates cannot bypass the gate, then an unapproved candidate cannot be activated through the defined interface.

This depends on enforcement and does not exclude out-of-band administrative errors.

### 39.4 No general convergence theorem

The search space of possible metrics may be unbounded, the validation target may drift, and the metric generator may be incomplete. Consequently, no universal termination, completeness, global optimality, or monotonic co-evolution guarantee follows from the CDE abstraction.

## 40. Failure Modes and Counterexamples

**F1 — Self-scoring loop:** Candidate metric rewards its own selected solutions.

**F2 — Proxy gaming:** Solution search manipulates an observable feature.

**F3 — Hidden leakage:** Metric generator indirectly sees sealed outcomes.

**F4 — Incorrect perspective:** Metric optimizes an objective not authorized by stakeholders.

**F5 — Constraint softening:** Hard feasibility is converted into a weighted penalty.

**F6 — Cross-version confusion:** Scores from different versions are ranked directly.

**F7 — Selection bias:** Only successful candidate metrics are reported.

**F8 — Metric proliferation:** Search complexity overwhelms validation capacity.

**F9 — Overfit calibration:** Score probabilities look good in development but fail under shift.

**F10 — Data dependency:** Several evidence sources reflect the same observation.

**F11 — Nonstationary outcomes:** Historical evaluation does not predict deployed behavior.

**F12 — Invalid transfer:** A source metric is reused under different feature semantics.

**F13 — Generator overfitting:** Meta-evolution specializes to benchmark fixtures.

**F14 — Weak governance:** Candidate authorizes its own promotion.

**F15 — No rollback:** Regression persists after a metric update.

**F16 — Misleading Pareto reduction:** Incomparable tradeoffs are forced into a scalar.

**F17 — Hidden costs:** Additional validation or agent calls are excluded from comparisons.

**F18 — False discovery claim:** A fitted metric is called a discovered universal law.

Each failure should be represented in at least one negative-control or fault-injection experiment.

## 41. Reproducibility and Evidence Package

A future executable companion SHOULD include:

```text
case-004-metric-coevolution/
├── README.md
├── contracts/
│   ├── metric-container.schema.json
│   ├── metric-delta.schema.json
│   ├── promotion-record.schema.json
│   └── score-record.schema.json
├── benchmarks/
│   ├── proxy_misalignment.py
│   ├── school_bus_objectives.py
│   ├── structural_matching_metrics.py
│   ├── perspective_preferences.py
│   └── feedback_shift.py
├── solvers/
│   ├── fixed_metric.py
│   ├── metric_search.py
│   ├── bilevel_baseline.py
│   └── cde_coevolution.py
├── validators/
│   ├── independent_utility.py
│   ├── shift_tests.py
│   ├── adversarial_probes.py
│   └── governance_gate.py
├── experiments/
│   ├── preregistration.yaml
│   ├── train_manifest.yaml
│   ├── promotion_validation_manifest.yaml
│   ├── final_test_manifest.yaml
│   └── run.py
├── tests/
│   ├── test_contracts.py
│   ├── test_leakage.py
│   ├── test_promotion.py
│   ├── test_negative_controls.py
│   └── test_rollback.py
└── reports/
    ├── metric_candidates.csv
    ├── decision_utility.csv
    ├── uncertainty_intervals.csv
    ├── cost_accounting.csv
    └── limitations.md
```

This is a proposed implementation layout. The listed code, datasets, and results are not claimed to exist.

## 42. Run Reporting Schema

```yaml
run_id: string
benchmark_family: string
task_instance_id: string
seed: integer
active_metric_ref: string
candidate_metric_ref: optional_string
constraint_bundle_id: string
generator_version: string
policy_version: string
validation_protocol_ref: string
solution_evaluations: integer
metric_proposals: integer
metric_validation_attempts: integer
total_compute_cost: number
heldout_decision_utility: number_or_null
approximate_regret: number_or_null
shift_utility: number_or_null
false_promotion: boolean_or_null
constraint_violations: integer
promotion_decision: string
rollback_status: string
conformance_status: string
provenance_ref: string
```

Unknown and undefined values MUST be represented explicitly, not silently replaced with zero.

## 43. Validation Roadmap

**V1 — Contract fixtures:** Review Metric Container, Metric Delta, score, and promotion schemas.

**V2 — Deterministic counterexamples:** Verify proxy inversion, scale mismatch, and hard-constraint rejection.

**V3 — Baseline implementation:** Establish fixed-metric, random, grid, and learned-metric comparators.

**V4 — Inner/outer loop separation:** Verify version pinning and independent outcome evaluation.

**V5 — Goodhart stress tests:** Introduce proxy gaming, adversarial optimization, and distribution shift.

**V6 — Metric portfolio:** Compare local, regional, and long-range Metric Delta proposals.

**V7 — Cross-domain cases:** Evaluate school-bus objectives and structural matching metrics.

**V8 — Governance and rollback:** Test unauthorized activation, stale context, and regression recovery.

**V9 — Relay and negative transfer:** Transfer metric candidates across related and unrelated tasks.

**V10 — Recursive evolution:** Evaluate generator/policy meta-Deltas with sealed validation.

**V11 — External replication:** Publish manifests, code, uncertainty, negative results, and limitations.

Each phase has an evidence requirement. No phase is reported completed by this manuscript.

## 44. Relation to the CDE Architecture

CASE-004 connects directly to several CDE research layers:

- **CDE-007:** solution-space and metric-space co-evolution;
- **CDE-008:** orchestration across heterogeneous solvers and evaluators;
- **CDE-010:** Structural Relay of metrics, methods, evidence, and Leftovers;
- **CDE-011:** recursive generator and policy evolution;
- **SPEC-001:** versioned state and hard constraints;
- **SPEC-002:** typed Delta proposals and M-to-N accounting;
- **SPEC-003:** path memory and legitimate revisitation;
- **SPEC-004:** solver guarantees and certified pruning;
- **SPEC-005:** distributed evaluation and relay contracts;
- **SPEC-006:** independent validation, promotion, and rollback.

The case also supports the Structural Intelligence Framework (SIF) localization theme: the system is not only locating promising solutions but prospecting for useful *evaluation structures*. This is a research interface, not a proven performance advantage.

## 45. Generalization Boundaries

Metric discovery can mean very different things:

1. identifying a useful feature;
2. fitting a predictive score;
3. improving a decision policy;
4. representing stakeholder preferences;
5. discovering a stable causal relationship;
6. discovering a scientific measurement construct.

Evidence for one level does not automatically establish the next. A learned score may predict labels without representing causal structure; a decision metric may work locally without generalizing across populations; a scientifically meaningful metric requires additional construct validation.

The relevant hierarchy is:

\[
\boxed{
\text{Metric Representability}
\neq
\text{Metric Discovery}
\neq
\text{Predictive Validity}
\neq
\text{Decision Utility}
\neq
\text{Generalization}
}
\]

CDE supplies contracts for exploration and validation. It does not eliminate the epistemic difficulty of choosing what should be measured.

## 46. Open Research Questions

1. How can a system generate meaningful new metric structures rather than merely tune weights?
2. When does co-evolution outperform sequential metric selection?
3. Can metric candidate coverage be quantified under a restricted grammar?
4. How should Metric Deltas be composed while preserving semantic interpretability?
5. Which independent validation protocols resist adaptive overfitting?
6. Can counterexample agents identify proxy exploitation before deployment?
7. How can metrics be compared across perspectives without imposing unauthorized preferences?
8. When should a metric remain vector-valued or partially ordered?
9. How can metric uncertainty be propagated into branch allocation?
10. What are sound rules for revisiting previously rejected metric candidates after new evidence?
11. How should validation costs be allocated across many candidate metrics?
12. Can a solver portfolio discover better metrics while maintaining rigorous proof boundaries?
13. What constitutes an adequate rollback trigger for a deployed metric?
14. How should negative transfer be detected in Metric Relay?
15. Can recursive generator evolution improve held-out discovery without overfitting?
16. How can metric drift be separated from environment drift?
17. What role should causal evidence play in metric promotion?
18. How can an AI system represent irreducible stakeholder disagreement?
19. What are the limits of metric-native directional scoring?
20. How does metric discovery connect to autonomous questioning and structural localization?

## 47. Conclusion

Metric Discovery and Co-Evolution addresses a central weakness of optimization systems: **the best solution under the wrong metric may be a poor real-world decision**. CDE's proposed approach is to make the metric itself a typed, versioned, testable object of exploration while keeping hard constraints and independent validation outside the metric generator's unilateral control.

The essential research loop is:

\[
\boxed{
\begin{gathered}
\text{Generate Solution Deltas under }m_v\\
\downarrow\\
\text{Observe Outcomes and Counterexamples}\\
\downarrow\\
\text{Generate Metric Deltas in Sandbox}\\
\downarrow\\
\text{Independently Validate Decision Utility}\\
\downarrow\\
\text{Authorize, Monitor, or Reject}\\
\downarrow\\
\text{Relay Evidence, Methods, and Leftovers}
\end{gathered}
}
\]

A higher internal score is not sufficient evidence of a better metric. A better metric on one benchmark is not proof of cross-domain generalization. A successful recursive update is not a guarantee of sustained self-improvement.

The proposed contribution is a **governed, auditable co-evolution contract** that makes these distinctions explicit and experimentally testable.

---

## Appendix A — Minimal Benchmark Configuration

```yaml
benchmark_id: "CASE-004-SMCE"
version: "1.0.0"
families:
  - "synthetic-proxy-misalignment"
  - "school-bus-objective-design"
  - "structural-matching-metrics"
  - "perspective-preference-learning"
  - "metric-feedback-drift"
primary_outcome: "independent-heldout-decision-utility"
negative_controls:
  - "correct-fixed-metric"
  - "proxy-manipulation"
  - "spurious-feature-shift"
  - "validator-leakage"
  - "constraint-laundering"
candidate_metric_scales:
  - "local-weight"
  - "regional-feature"
  - "long-range-structure"
comparison_budgets:
  solution_evaluations: [50, 100, 250]
  metric_candidates: [10, 30, 100]
replicates_per_configuration_target: 30
require:
  independent_validation: true
  metric_version_pinning: true
  hard_constraint_preservation: true
  rollback_checkpoint: true
  negative_result_reporting: true
```

These are proposed configuration values, not completed runs.

## Appendix B — Example Metric Delta Ledger

```yaml
invocation_id: "metric-generation-001"
source_metric_ref: "metric-v2"
generated_proposals_M: 12
selected_attempts_Q: 7
realized_candidate_evaluations_N: 5
distinct_successor_metrics_K: 4
dispositions:
  deferred_proposals: 3
  excluded_by_policy_proposals: 2
  rejected_attempts: 2
  evaluated_attempts: 5
coverage_claim: "partial"
metric_context_version: "metric-v2"
validation_protocol_ref: "independent-utility-v3"
provenance_ref: "metric-invocation-001"
```

All counts are illustrative. This record distinguishes proposal dispositions from attempted validation outcomes.

## Appendix C — Claims and Evidence Registry

| Claim | Minimum supporting evidence | Insufficient evidence |
|---|---|---|
| Metric schema valid | Contract tests | Example YAML alone |
| Metric predicts target | Held-out predictive evaluation | Training fit |
| Metric improves decisions | Independent decision utility | Candidate's own score |
| Metric robust under shift | Declared shift experiments | In-distribution accuracy |
| Metric safe to activate | Constraint and governance checks | High score |
| Metric transferable | Target-task validation | Source-task success |
| Generator improved | Held-out meta-evaluation | Self-reported gain |
| Globally optimal metric | Valid optimization proof under defined class | Best sampled candidate |
| Collective learning | Measured improvement in target contexts | Shared artifacts |

## Appendix D — Figure Guidance

The following planned CDE repository figures are relevant:

- `Fig-004-Adaptive-M-to-N-Delta-Branching.png`
- `Fig-007-Multi-Scale-Delta-Generation-and-Directional-Exploration.png`
- `Fig-008-Solution-and-Metric-Co-Evolution.png`
- `Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png`
- `Fig-010-Recursive-Generator-Evolution-and-Collective-Learning.png`

No figures are embedded or claimed to have been generated by this document.

## Appendix E — Research Status and Attribution

This manuscript is a proposed research and validation design. It incorporates established concepts from multiobjective optimization, metric learning, model selection, bilevel optimization, preference learning, Goodhart/reward-hacking analysis, causal evaluation, and adaptive experimentation. Its proposed contribution is the explicit CDE State–Delta–Constraint–Branch orchestration and the integration of independent validation, versioned governance, and Structural Relay.

**No benchmark experiments have been run for this document.** Numerical examples are constructed demonstrations, and all performance hypotheses remain unverified. Future work should provide executable implementations, preregistered benchmarks, independent results, uncertainty intervals, limitations, and appropriate prior-art references.

**Version:** v1.0.0 draft  
**License:** Apache-2.0
