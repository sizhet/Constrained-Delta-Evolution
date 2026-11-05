# CDE-010 — Structural Relay and Collective Learning

**Series:** Constrained Delta Evolution (CDE) — Foundations  
**Status:** Foundational research proposal, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Companion papers:** CDE-001 through CDE-009  
**Related planned specifications:** SPEC-003 Path Memory and Loop Control; SPEC-005 Distributed Exploration and Relay; SPEC-006 Metric and Recursive Evolution  
**Related planned case:** CASE-005 Distributed CDE and Method Relay

## Abstract

Constrained Delta Evolution (CDE) represents exploration as constraint-governed state transitions generated through typed Deltas. Distributed execution can accelerate exploration, but parallel computation alone does not create collective intelligence. To learn collectively, an exploration system must preserve, evaluate, transfer, adapt, and revalidate knowledge acquired by individual branches and workers. This paper proposes **Structural Relay** as the mechanism connecting CDE exploration episodes to a reusable, evidence-bearing knowledge ecosystem. It distinguishes result, evidence, constraint, method, policy, metric, and leftover relay; specifies provenance and validity boundaries; develops transfer and assimilation protocols; and examines the role of negative results, repeated execution, and heterogeneous worker methods. We propose a relay artifact contract, a receiving-side conformance process, a lifecycle for candidate knowledge, and an evaluation framework for transfer benefit and negative transfer. The resulting architecture positions CDE as a potential contributor to collective structural learning within the Structural Intelligence Framework (SIF), while separating demonstrated local results from unverified generalizations. Structural Relay is not assumed to guarantee reuse, improvement, convergence, or safe recursive self-improvement.

**Keywords:** Constrained Delta Evolution; Structural Relay; Collective Learning; Delta Exploration; distributed intelligence; method transfer; evidence provenance; negative knowledge; transfer validation; structural intelligence; SIF; recursive improvement.

---

## 1. Motivation: From Parallel Work to Collective Learning

An exploration worker may discover a feasible solution, a useful heuristic, a counterexample, a tighter bound, or a previously unknown constraint. A second worker can benefit only if the discovery is expressed in a form it can interpret and if its relevance to the second worker's task can be established. Merely transmitting a score or a final answer is often insufficient.

Consider two branches solving related vehicle-routing problems. One branch learns that a particular exchange operator consistently improves routes with geographically clustered stops. The other branch uses a different objective, capacity regime, and geographical distribution. Copying the first branch's best route would be meaningless. Copying its operator might be productive—but only after checking preconditions, adapting parameters, and testing whether it improves outcomes under the second branch's objective. This distinction is central to Structural Relay.

A CDE system can therefore be viewed as having two distinct feedback loops:

1. **Exploration loop:** generate, validate, execute, and evaluate Deltas within a problem context.
2. **Collective learning loop:** extract reusable structural knowledge from exploration, transfer it to a receiving context, and test whether it retains value.

The second loop is neither automatically implied by the first nor guaranteed by distributed execution. It requires explicit contracts.

## 2. Scope, Contributions, and Non-Claims

This paper proposes a framework for:

- Typing and packaging relayable exploration artifacts.
- Representing provenance, evidence, assumptions, scope, and uncertainty.
- Distinguishing **transfer** from **validated assimilation**.
- Supporting positive, negative, partial, and inconclusive outcomes.
- Managing versioning, retractions, supersession, and revalidation.
- Coordinating method relay among heterogeneous solvers.
- Defining how local experience can inform generator and policy evolution.
- Testing the net value of collective learning against isolated exploration.

The proposal does not assert that all knowledge is structurally transferable, that an artifact's author can determine its validity in another context, or that collective learning always improves performance. It also does not establish priority over existing work on transfer learning, case-based reasoning, knowledge graphs, distributed optimization, experience replay, or multi-agent learning. Its specific contribution is a candidate **CDE-native contract architecture** for those functions.

## 3. Formal Foundations

Let a CDE problem be described by

\[
\mathcal P=(\mathcal S,\mathcal D,\mathcal K,G,T,I,\mathcal O),
\]

where \(\mathcal S\) is the state universe, \(\mathcal D\) the typed Delta universe, \(\mathcal K\) the scoped constraint family, \(G\) the candidate generator, \(T\) the transition relation, \(I\) the initial states, and \(\mathcal O\) the objective and observation regime.

An exploration configuration can be represented as

\[
q=(s,h,c,v,b),
\]

where \(s\) is domain state, \(h\) is future-relevant history, \(c\) is evidence and constraint context, \(v\) is the version vector, and \(b\) is the relevant resource budget. A transition is recorded as

\[
e=(q_i,\delta,a,q_j,\pi),
\]

with Delta \(\delta\), decision semantics \(a\), and provenance \(\pi\).

A **relay** is not identical to a transition. It is a typed transformation from one or more exploration records into an artifact potentially usable by another context:

\[
R=\operatorname{Extract}(E_{1:n},\mathcal A,\Pi),
\]

where \(E_{1:n}\) are evidence-bearing exploration events, \(\mathcal A\) is the artifact schema, and \(\Pi\) is the extraction policy. The receiving side performs

\[
\operatorname{Assimilate}(R,\mathcal P',V')
\rightarrow\{\text{accepted},\text{rejected},\text{deferred},\text{quarantined}\}.
\]

**Relay publication is not assimilation.** The receiver must decide whether the artifact applies, whether its assumptions hold, and what level of evidence is sufficient for its intended use.

## 4. Structural Relay: A Working Definition

**Structural Relay** is the controlled transfer of typed, provenance-bearing knowledge from an exploration episode or worker to another episode, worker, domain, or future version of the same system, where the recipient can inspect its assumptions, evaluate applicability, and decide whether to reuse, adapt, or reject it.

Four properties distinguish this definition from unstructured message passing:

1. **Structure:** the artifact has machine- and human-interpretable semantics.
2. **Evidence:** claims are linked to observations, proofs, experiments, or explicit uncertainty.
3. **Context:** the source scope and intended applicability are declared.
4. **Receiving-side control:** reuse requires an explicit acceptance or validation step.

The word *structural* does not require one universal data representation. A typed JSON contract, proof certificate, executable operator, differential tree, constraint schema, or human-reviewed research note can all be relay artifacts if the receiving system understands the relevant semantics.

## 5. Seven Relay Families

### 5.1 Result Relay

Transfers a candidate solution, objective value, feasibility certificate, or search frontier. A result is useful only under a compatible problem definition, constraints, and metric version. A route optimized for one school district is not a solution for another district merely because both use buses.

### 5.2 Evidence Relay

Transfers observations, counterexamples, uncertainty estimates, benchmark outcomes, failed hypotheses, and supporting data references. Evidence can improve the receiver's decisions without providing an executable method.

### 5.3 Constraint Relay

Transfers discovered infeasibility conditions, domain restrictions, conflicts, or safe dominance conditions. Constraint relay is especially powerful but risky: a constraint valid for one domain may wrongly exclude feasible solutions in another. Each constraint requires an applicability scope and, where claimed, a soundness justification.

### 5.4 Method Relay

Transfers Delta operators, search heuristics, decomposition methods, solver configurations, or generator strategies. A method may be adapted to a different domain if its preconditions and evaluation protocol are satisfied. Method relay is a primary route to collective learning beyond simple answer sharing.

### 5.5 Policy Relay

Transfers exploration policies: budget allocation, branching priorities, solver selection, stopping criteria, or escalation rules. A policy proven useful under one workload may fail under another. Its claimed performance should be recorded with workload characteristics and validation results.

### 5.6 Metric Relay

Transfers scoring structures, feature transformations, comparison rules, or metric-validation findings. As established in CDE-007, metric quality cannot be established by self-scoring. Metric versions and independent validation regimes must accompany metric relay.

### 5.7 Leftover Relay

Transfers unresolved subproblems, rejected or deferred candidates, frontier regions, resource-limited branches, and outstanding validation obligations. A leftover is not waste; it is a precisely scoped opportunity for future exploration. However, a rejected candidate must retain its rejection reason and context so that it is not silently reintroduced as if it were unexplored.

## 6. Repeated Execution as Relay

Repeated execution can be understood as a special form of relay across time. A system repeatedly applying the same operator may accumulate observations about success rates, costs, failures, and contextual conditions. The useful relay artifact is not the repeated action itself, but the **updated evidence state** derived from its executions.

For example, a local-search operator applied one hundred times can yield a distribution of improvements conditional on route density. That distribution may help a future worker choose operators. The update is meaningful only if repeated trials are recorded with their context and dependence structure. One hundred correlated executions on nearly identical states are not equivalent to one hundred independent tests.

A minimal temporal relay chain is:

\[
\text{Execution}_t\rightarrow\text{Observation}_t
\rightarrow\text{Evidence Update}_{t+1}
\rightarrow\text{Policy Candidate}_{t+1}.
\]

No automatic policy promotion follows from evidence accumulation; promotion requires its own validation contract.

## 7. The Relay Artifact Contract

A canonical relay artifact may be represented as

\[
\mathcal R=(id,\tau,payload,scope,pre,claim,evidence,
versions,prov,rights,life),
\]

where:

| Field | Meaning |
|---|---|
| `id` | Stable artifact identity and content digest |
| `type` | Relay family and schema version |
| `payload` | Result, operator, constraint, evidence, or unresolved task |
| `scope` | Source problem, domain, state region, and intended target |
| `preconditions` | Requirements for interpretation and execution |
| `claim` | Precisely scoped proposition about utility or correctness |
| `evidence` | Supporting records, certificates, uncertainty, negative tests |
| `versions` | Solver, metric, constraint, dataset, and schema versions |
| `provenance` | Originating worker, task, lineage, and transformations |
| `rights` | Authorization, access, privacy, and permitted-use conditions |
| `lifecycle` | Draft, tested, accepted, superseded, revoked, or archived |

The artifact should be immutable after publication. Corrections, retractions, and additional validation are represented by linked events or new versions rather than silent overwrites.

### 7.1 Claim typing

Claims should identify their epistemic status:

- `observed`: measured in the source experiment.
- `proven`: established under stated formal assumptions.
- `estimated`: supported by statistical inference with uncertainty.
- `hypothesized`: proposed but not validated.
- `refuted`: contradicted under the recorded test conditions.

These labels are not interchangeable. In particular, a source-domain observation is not a target-domain proof.

### 7.2 Dependency closure

A relay artifact that depends on an unavailable feature extractor, dataset, model, or schema cannot be executed faithfully. The contract should expose dependencies and indicate whether the receiver may substitute them. Substitution requires fresh validation when it changes semantics.

## 8. Relay Lifecycle

A proposed lifecycle is:

```text
Discovery / Execution
    -> Extraction
    -> Local Validation
    -> Publication (Candidate Relay)
    -> Recipient Discovery
    -> Applicability Check
    -> Adaptation, if needed
    -> Target Validation
    -> Assimilation or Rejection
    -> Monitoring
    -> Renewal, Supersession, or Revocation
```

A receiving system may quarantine artifacts with missing provenance, incompatible licenses, unverifiable code, or unresolved safety conditions. Quarantine means **not trusted for execution**, not necessarily scientifically worthless.

The receiving-side decision must be recorded separately from the source's confidence. This preserves local autonomy and prevents a single worker from imposing its own findings as global policy.

## 9. Applicability and Transfer Contracts

Let \(R\) be a relay artifact from source context \(C_s\), and let \(C_t\) be the receiving context. Applicability is a predicate

\[
A(R,C_t)\in\{\text{yes},\text{no},\text{unknown}\}.
\]

The three-valued form matters: insufficient evidence is not equivalent to incompatibility.

A transfer mapping \(\phi\) may translate source entities, features, operators, or constraints into target-domain terms. The receiver must check:

1. **Type compatibility:** can the payload be interpreted?
2. **Semantic compatibility:** do translated operations mean the same thing?
3. **Constraint compatibility:** does the method preserve target hard constraints?
4. **Metric compatibility:** are claimed improvements evaluated under the target objective?
5. **Distribution compatibility:** does the source evidence cover relevant target conditions?
6. **Operational compatibility:** are required resources, permissions, and dependencies available?

Transfer is **negative** when adopting an artifact worsens target outcomes relative to an appropriate baseline. A relay system should record negative transfer explicitly rather than hiding it in aggregate success statistics.

## 10. Structural Similarity and Directional Weak Evidence

CDE-006 proposes directional fields for selecting promising Delta regions. Similarity between source and target structures can likewise guide relay discovery. A source artifact may be prioritized because the target shares constraint patterns, graph topology, feature semantics, or search behavior.

However:

\[
\text{Structural Similarity}\not\Rightarrow\text{Transfer Validity}.
\]

A directional score is a **prospecting signal**. It may justify a low-cost test, not automatic deployment. This distinction is especially important when source and target appear superficially similar but have different causal mechanisms, stakeholder priorities, or failure costs.

A relay candidate ranking might use

\[
P(R,C_t)=w_1\,S_{struct}+w_2\,E_{source}
-w_3\,Risk_{transfer}-w_4\,Cost_{validation},
\]

with explicit units, normalization, and governance for the weights. This is an illustrative heuristic, not a universal formula. Its quality must be measured by downstream target-domain performance.

## 11. Collective Learning as an Evidence-Bearing Update

A collective system can maintain a shared registry of artifacts, but a registry alone does not imply learning. A stronger operational definition is:

> **Collective structural learning occurs when experience from one or more participants changes another participant's future exploration or decision process, and the effect can be evaluated against an appropriate baseline.**

A collective update may alter:

- The Delta generator's operator library.
- The probability of proposing certain branches.
- The solver-selection policy.
- The constraint or infeasibility knowledge base.
- The metric candidate portfolio.
- The allocation of exploration budgets.
- The location of promising or unresolved structural regions.

The effect may be beneficial, neutral, or harmful. Recording that outcome is part of learning.

### 11.1 Local learning versus collective learning

A worker tuning its own operator weights is local learning. Publishing a validated operator-performance profile for others to test is relay. A second worker incorporating that profile and demonstrating a measurable effect is collective learning.

### 11.2 Shared memory versus shared authority

A shared artifact registry need not be a global command system. Workers may retain independent validation policies and local constraints. Collective memory and centralized decision authority are separate architectural choices.

## 12. Path Memory, NoSubtree, and Relay

CDE-004 distinguishes path-local decision uniqueness from global state uniqueness. The same distinction governs relay.

A branch may reject a Delta under its current context, recording a `NoSubtree` decision. That rejection should not automatically become a universal ban. Relay must preserve whether the rejection means:

- Syntactically invalid.
- Proven infeasible under a stated constraint set.
- Dominated under a fixed objective and valid bound.
- Heuristically deprioritized.
- Rejected by a branch-local Yes/No decision.
- Deferred for insufficient budget or evidence.

Only some categories justify target-side pruning, and even then only under compatible assumptions. A receiving worker must not treat every `NoSubtree` artifact as a globally valid constraint.

Similarly, Loop Back control requires distinguishing repeated decision keys, repeated domain states, and revisits with new evidence. A previously seen method may deserve re-evaluation after a genuine change in target context, without allowing arbitrary version increments to bypass deduplication.

## 13. Negative Knowledge and Counterexample Relay

Negative results can be more transferable than successful answers when they reveal a general failure mode. Examples include:

- A local search operator that violates capacity under a particular topology.
- A metric feature that leaks future information.
- A B&B lower bound invalidated by an objective change.
- A DP cache key that omits future-relevant path history.
- A proposed constraint that wrongly prunes feasible states.

A negative artifact should specify the exact claim refuted, counterexample input, execution or proof trace, and the narrowest justified scope. It should avoid claiming universal impossibility from a single unsuccessful search.

**Failure to find a solution is not proof of infeasibility** unless the solver's completeness and termination conditions support that conclusion.

## 14. Method Relay between Heterogeneous Solvers

Different workers may use DFS, DP, B&B, local search, evolutionary algorithms, or domain-specific procedures. They need not exchange internal implementation details to benefit from each other. A solver-neutral relay interface can transmit:

- Feasible incumbents under a fixed metric.
- Sound bounds and their proof obligations.
- Reusable neighborhood operators.
- Problem decompositions.
- Learned variable-ordering heuristics.
- Counterexample sets.
- Validated feature or metric candidates.

The receiving solver determines whether and how to use each artifact. For example, a B&B solver may adopt a feasible incumbent from a heuristic worker, but it cannot adopt an unproven heuristic estimate as a safe bound. A DP solver may reuse a decomposition only if its state equivalence conditions remain sound.

### 14.1 Relay is not solver homogenization

The purpose is to allow different methods to coexist and exchange selected knowledge. Requiring every worker to implement an identical solver would sacrifice much of the benefit of heterogeneous exploration.

## 15. Distributed Relay and Global Reconciliation

CDE-009 introduces distributed Delta Exploration Contracts. Relay extends those contracts beyond task completion. A worker may return a solution, a method, evidence, or a leftover task even when its primary objective was not achieved.

For globally coupled problems, independently feasible local results may be mutually incompatible. A bus cannot serve two overlapping routes merely because both local plans are individually feasible. The orchestrator must perform global reconciliation under the shared resource and constraint model.

A relay registry should distinguish:

1. **Locally valid artifact** under source assumptions.
2. **Globally admissible artifact** under the receiving problem's shared constraints.
3. **Validated improvement** under the target metric and evidence protocol.

The three statuses should never be collapsed into one `success` flag.

### 15.1 Asynchrony and stale knowledge

Workers may finish after a metric, constraint, or state version has changed. Late artifacts can remain historically valuable, but their applicability must be rechecked. The system should use immutable baselines and explicit version reconciliation rather than silently rewriting the context of a completed task.

### 15.2 Idempotency and duplicate relay

Repeated delivery should not cause repeated side effects. Artifact identity, event identity, and assimilation identity may differ. A worker may legitimately revalidate the same artifact under new evidence, but duplicate transport messages should not create duplicate promotions.

## 16. Knowledge Registry and Retrieval

A practical collective memory may maintain several indices:

| Index | Purpose |
|---|---|
| Artifact identity | Exact retrieval and deduplication |
| Domain/structural signature | Find related problems and methods |
| Preconditions | Filter incompatible artifacts |
| Evidence strength | Prioritize credible candidates |
| Failure conditions | Avoid known negative transfer |
| Metric/constraint versions | Guard comparability |
| Lineage | Track derivation, supersession, and attribution |
| Open leftovers | Resume unfinished exploration |

Retrieval can combine symbolic matching, vector search, graph traversal, and directional scoring. None of these retrieval techniques should be equated with validation.

### 16.1 Memory growth and forgetting

A registry that never retires artifacts may become expensive and misleading. Archival, decay of confidence under distribution shift, and supersession are legitimate operations. But deletion of evidence should not destroy auditability for decisions already made. A tiered storage model can retain compact immutable provenance while removing costly execution payloads when permitted.

## 17. Structural Relay as a Learning Unit

A reusable **Relay Unit** can be modeled as

\[
U=(\text{Problem Signature},\text{Method},\text{Preconditions},
\text{Evidence},\text{Failure Modes},\text{Adaptation Rule}).
\]

The unit is more informative than a naked method. It captures where the method works, where it fails, and how to test it elsewhere. This aligns with the broader SI preference for rich per-node intelligence and composable structural units.

A receiving agent may translate the unit into a local operator, retain it as a hypothesis, or use it only to generate new questions. The relay contract should allow these different assimilation modes.

## 18. Relation to SIF Localization

SIF Localization can be understood as finding relevant structures, methods, capabilities, and exploration regions. Structural Relay contributes a second route to localization: rather than searching from scratch, a worker can retrieve candidate knowledge from earlier exploration.

The combined process is:

\[
\text{Problem Signature}
\rightarrow\text{Relay Prospecting}
\rightarrow\text{Applicability Testing}
\rightarrow\text{Local Delta Generation}
\rightarrow\text{Validated Outcome}.
\]

CDE-006's multi-scale generators may explore local and distant state regions; relay may introduce **nonlocal structural jumps** by importing a method or decomposition discovered elsewhere. This is a potentially valuable form of exploratory localization, but its benefit depends on the quality of transfer mapping and validation.

The claim that CDE is a candidate Localization pillar is architectural. It does not imply that all SIF localization problems reduce to CDE search.

## 19. Structural Relay and Metric Co-Evolution

CDE-007 introduces a metric space distinct from the solution space. Relay across metric versions requires particular care.

A metric artifact must include its feature definitions, comparison semantics, validation evidence, data lineage, and governance context. A candidate that improves an internal benchmark may be unsuitable for a new domain or stakeholder perspective.

When a metric changes, old solution scores, B&B bounds, dominance relations, and search priorities may become invalid. A relay unit containing any of those claims must name the metric version on which it depends.

**Metric Relay is a hypothesis transfer until the receiving domain validates it.** An attractive metric structure can be reused as a starting point without inheriting the source domain's claims of validity.

## 20. Controlled Recursive Generator and Policy Evolution

Structural Relay can supply training evidence for improving the generators and policies that produce future Deltas:

\[
G_{t+1}=U(G_t,\mathcal E_t),\qquad
P_{t+1}=V(P_t,\mathcal E_t),
\]

where \(\mathcal E_t\) contains validated evidence and negative outcomes. The update operators \(U,V\) may themselves be learned, but recursive improvement must be bounded by explicit evaluation and rollback contracts.

A controlled promotion sequence is:

1. Propose generator or policy Delta.
2. Verify schema, permissions, and hard constraints.
3. Test on isolated tasks or shadow workloads.
4. Compare with an unchanged baseline using independent evaluation.
5. Inspect failures, regressions, and resource costs.
6. Promote only under an authorized governance rule.
7. Monitor performance and retain rollback capability.

A policy that raises its own success score is not evidence of improvement if the scoring protocol changed simultaneously. A new generator's ability to produce more Deltas is not automatically better than generating fewer, more useful Deltas.

## 21. Relay Portfolio Selection and Resource Allocation

A collective system may have far more relay candidates than it can test. Selection can be formulated as a budgeted portfolio problem:

\[
\max_{A\subseteq\mathcal R}\operatorname{ExpectedUtility}(A)
\quad\text{subject to}\quad
\sum_{r\in A}\operatorname{ValidationCost}(r)\le B.
\]

The expected utility may include anticipated improvement, information gain, diversity, transfer risk, and downstream reuse. It is a decision model, not a proof of transfer success. Estimates must be calibrated and periodically checked against realized outcomes.

Diversity can matter: selecting ten nearly identical heuristics may offer less value than testing several structurally distinct methods. Conversely, diversity for its own sake can waste resources when evidence strongly favors a narrow family. CDE-Orchestrator should allow the exploration policy to make that tradeoff explicitly.

## 22. Worked Example A: School-Bus Scheduling

Suppose several workers explore a school district's vehicle-assignment and routing problem, where the primary objective is reducing fleet size while also limiting student ride times and respecting capacity and service constraints.

**Worker A** finds a route-exchange operator that improves total travel time in dense neighborhoods. **Worker B** discovers a counterexample where the same exchange causes an unacceptable maximum ride time. **Worker C** identifies a decomposition that groups geographically separated stops while preserving a global vehicle-availability constraint.

A useful collective relay package includes:

- A's operator, its preconditions, and observed improvement distribution.
- B's counterexample and the precise failure condition.
- C's decomposition with a proof or test of its constraint assumptions.
- The shared metric and constraint versions used for evaluation.

A new district can test the operator and decomposition on its own data. It should not import A's routes or C's vehicle allocation as globally valid solutions. The value of the relay lies in transferring **methods and evidence**, not copying answers.

**Testable prediction:** under comparable compute budgets, validated method relay may reduce time to a high-quality feasible schedule relative to isolated workers. Negative transfer must be counted, not hidden.

## 23. Worked Example B: Chess Strategy Search

A chess-search worker can relay an opening pattern, a tactical motif, an evaluation counterexample, or a move-ordering heuristic. The receiver must distinguish:

- A concrete move sequence valid in a particular position.
- A tactical pattern with structural preconditions.
- A search heuristic that may improve efficiency across positions.
- An evaluation claim that depends on engine settings and search depth.

Copying a move from a different board position is invalid; importing a validated move-ordering heuristic may be useful. This illustrates the difference between result relay and method relay in a domain with clear rules and reproducible tests.

## 24. Worked Example C: Scientific Question Generation

An autonomous questioning system may generate hypotheses and research questions. A question's usefulness can be relayed along with its assumptions, tested implications, negative evidence, and unanswered subquestions.

One agent may find a counterexample that invalidates a broad hypothesis. Another can use the counterexample to formulate a narrower, more testable question. The relay is productive even though the original hypothesis failed.

The appropriate outcome metric is not merely the number of new questions. More relevant measures may include testability, independent experimental yield, coverage of unresolved issues, and the cost of validation. These metrics themselves require explicit governance and independent evaluation.

## 25. Worked Example D: Method Transfer with Negative Transfer

Consider two optimization domains sharing a superficially similar graph representation. Domain A rewards short paths; Domain B rewards robustness against edge failures. A greedy shortest-path heuristic performs well in A and poorly in B.

The source worker publishes:

```text
Artifact: GreedyPathOrdering-v1
Type: MethodRelay
Source objective: minimize path length
Precondition: nonnegative edge weights
Evidence: 40 source benchmark instances
Claim: improves source-domain search time under specified settings
Known limitations: no robustness objective evaluated
```

The receiving worker should classify the artifact as a *candidate*, not a validated target-domain method. If tests reveal worse robustness or search efficiency, it records a negative-transfer artifact linked to the original method. Future retrieval can use that evidence to avoid repeating the same mistake.

## 26. Minimal Protocol and Pseudocode

The following pseudocode illustrates a solver-neutral receiving process.

```text
function RECEIVE_RELAY(artifact, target_context):
    verify_schema_and_integrity(artifact)
    if not authorized_for_target(artifact, target_context):
        return QUARANTINE("rights_or_policy")
    if not dependencies_available(artifact):
        return DEFER("missing_dependencies")

    applicability = check_applicability(artifact, target_context)
    if applicability == NO:
        return REJECT("incompatible_scope")
    if applicability == UNKNOWN:
        return DEFER("insufficient_evidence")

    candidate = adapt_without_promoting(artifact, target_context)
    if not hard_constraints_hold(candidate, target_context):
        return REJECT("constraint_violation")

    report = validate_on_target(candidate, target_context)
    record_relay_outcome(artifact.id, target_context.id, report)

    if report.has_unresolved_safety_or_correctness_issue:
        return QUARANTINE("unresolved_validation")
    if not report.meets_promotion_contract:
        return REJECT_OR_DEFER(report)
    if not governance_authorizes(report):
        return DEFER("awaiting_approval")

    return ASSIMILATE_VERSIONED(candidate, report)
```

The procedure must not interpret `UNKNOWN` applicability as `YES`. Target validation should measure the relevant outcome under a fixed metric version, with a baseline and a reproducible test protocol.

### 26.1 Publishing a relay artifact

```text
function PUBLISH_RELAY(events, claim, relay_type, scope):
    artifact = extract_typed_artifact(events, relay_type)
    attach_claim_and_scope(artifact, claim, scope)
    attach_immutable_provenance(artifact, events)
    attach_evidence_and_counterexamples(artifact, events)
    attach_constraint_metric_dataset_versions(artifact)
    check_privacy_rights_and_dependencies(artifact)
    validate_schema(artifact)
    return publish_as_candidate(artifact)
```

Publication creates a discoverable candidate. It does not assert global validity or authorize execution by recipients.

## 27. Conformance Requirements

The following are proposed draft tests, not a claim of an existing standard.

| ID | Requirement | Negative or positive test |
|---|---|---|
| REL-CONF-01 | Every artifact has a typed relay family | Unknown type rejected |
| REL-CONF-02 | Every claim declares its epistemic status | Hypothesis cannot be presented as proof |
| REL-CONF-03 | Source provenance is immutable | Altered lineage detected |
| REL-CONF-04 | Source and target contexts are distinguished | Source score not silently reused as target score |
| REL-CONF-05 | Metric and constraint versions are recorded | Missing versions block dependent comparison |
| REL-CONF-06 | Preconditions are machine- or review-checkable | Violated precondition prevents execution |
| REL-CONF-07 | `UNKNOWN` applicability is distinct from `NO` | Unknown produces deferral, not permanent ban |
| REL-CONF-08 | Publication is distinct from assimilation | Published artifact not automatically activated |
| REL-CONF-09 | Hard constraints remain authoritative | Attractive but infeasible method rejected |
| REL-CONF-10 | Counterexamples remain linked to claims | Known failure not hidden by promotion |
| REL-CONF-11 | Negative transfer is recorded | Failed target test remains queryable |
| REL-CONF-12 | Duplicate delivery is idempotent | Replayed message causes no duplicate promotion |
| REL-CONF-13 | Stale artifacts are revalidated | Obsolete metric-bound result flagged |
| REL-CONF-14 | Local rejection is not global infeasibility | Branch-local NoSubtree not propagated as theorem |
| REL-CONF-15 | Sound pruning requires justification | Heuristic estimate cannot masquerade as bound |
| REL-CONF-16 | Solver-specific artifacts declare dependencies | Missing solver feature blocks unsafe reuse |
| REL-CONF-17 | Repeated execution tracks dependence | Correlated trials not counted as independent |
| REL-CONF-18 | Rights and privacy are enforced | Unauthorized data transfer blocked |
| REL-CONF-19 | Revocation and supersession are auditable | Revoked artifact not silently active |
| REL-CONF-20 | Metric relay requires external validation | Self-scoring-only promotion blocked |
| REL-CONF-21 | Global coupling triggers reconciliation | Conflicting local schedules cannot both commit |
| REL-CONF-22 | Policy evolution supports rollback | Failed policy promotion reversible |
| REL-CONF-23 | Leftovers retain reason and scope | Deferred task not mislabeled impossible |
| REL-CONF-24 | Target-side effect is measurable | Assimilation records baseline comparison |

## 28. Testable Hypotheses

**H-010-01 — Method Relay Efficiency.** Under equal aggregate compute budgets, validated method relay reduces time-to-target solution quality compared with isolated workers on selected related tasks.

**H-010-02 — Negative Evidence Value.** Counterexample relay reduces repeated invalid candidate generation and constraint violations.

**H-010-03 — Transfer Scope.** Explicit preconditions and applicability checks reduce negative transfer relative to similarity-only retrieval.

**H-010-04 — Leftover Reuse.** Structured leftover relay increases the proportion of previously unresolved search regions productively revisited under new resources or evidence.

**H-010-05 — Heterogeneous Portfolio.** Method relay across different solver families provides benefits not captured by sharing incumbents alone.

**H-010-06 — Temporal Relay.** Evidence accumulated from repeated execution improves subsequent operator selection when dependence and context are modeled correctly.

**H-010-07 — Metric Safety.** Metric-version and validation guards reduce false improvement claims in distributed experiments.

**H-010-08 — Distributed Coordination Cost.** Relay improves sample efficiency only above a task-relatedness threshold sufficient to offset transfer and validation overhead.

**H-010-09 — Generator Evolution.** Generator updates based on validated relay evidence improve held-out task performance compared with updates based solely on source-domain scores.

**H-010-10 — SIF Localization.** Relay-assisted method prospecting improves discovery of relevant solution regions on selected structural problems versus local-only Delta generation.

**H-010-11 — Evidence Diversity.** Retaining counterexamples and competing methods improves robustness under distribution shift relative to storing only successful artifacts.

**H-010-12 — Collective Learning Boundary.** On unrelated tasks, strict transfer validation prevents most negative transfer but may impose a measurable net overhead; this should be reported rather than suppressed.

Each hypothesis requires preregistered benchmarks, fixed budgets, appropriate baselines, uncertainty intervals, and explicit criteria for failure.

## 29. Validation Roadmap

### Phase I — Contract Conformance

Implement artifact schemas, versioning, provenance, three-valued applicability, idempotent delivery, and revocation. Construct adversarial tests for stale metric versions, missing dependencies, false global constraints, and duplicate messages.

### Phase II — Single-Domain Relay

Use a reproducible search domain, such as routing or chess positions, to compare isolated workers against workers sharing results, methods, and negative evidence. Measure quality, runtime, transfer overhead, and invalid transfers.

### Phase III — Cross-Solver Relay

Combine heuristic search, DP, and B&B workers. Test incumbent sharing, decomposition transfer, heuristic ordering, and valid bound certificates. Confirm that heuristic evidence cannot trigger unsafe pruning.

### Phase IV — Cross-Domain Transfer

Transfer selected operators across related but nonidentical domains. Evaluate applicability prediction, adaptation cost, target-side quality, and negative transfer. Include deliberately unrelated tasks to expose overgeneralization.

### Phase V — Controlled Collective Evolution

Allow validated relay outcomes to update generator and orchestration policies in a sandbox. Compare against frozen policies on held-out tasks. Require rollback, auditability, and explicit governance before promotion.

## 30. Metrics for Evaluating Collective Learning

A credible experiment should report more than the best final solution. Useful measures include:

| Measure | Interpretation |
|---|---|
| Time-to-feasible | How quickly a valid candidate is found |
| Time-to-target-quality | How quickly a specified quality threshold is reached |
| Independent target quality | Improvement under fixed external evaluation |
| Transfer success rate | Fraction of tested artifacts that improve target outcomes |
| Negative transfer rate | Fraction that worsen target outcomes |
| Validation overhead | Compute and review cost of receiving artifacts |
| Redundant exploration | Work repeated without new information |
| Counterexample reuse | Avoided recurrence of known failure modes |
| Knowledge retention | Useful artifacts remaining applicable over time |
| Cross-domain robustness | Performance under changed target conditions |
| Policy regression rate | Frequency of harmful promoted updates |
| Audit completeness | Fraction of decisions with reproducible lineage |

Statistical analysis should account for correlated tasks, shared data, and repeated use of the same benchmarks. Claims of speedup must specify whether communication and validation costs are included.

## 31. Safety, Security, and Governance

Relay creates a trust boundary. Artifacts may contain executable code, learned policies, private data, or malicious instructions. A receiving system should not execute untrusted artifacts merely because they originated from another agent in the collective.

Controls may include schema validation, signed provenance where appropriate, least-privilege execution, sandboxing, dependency verification, privacy review, rate limits, and independent evaluation. These measures reduce risk but do not guarantee safety.

Scientific or operational claims should also preserve authorship and evidence lineage. A relay system should not inflate the credibility of a finding by counting multiple downstream copies as independent confirmations.

For high-stakes decisions, human or institutional authorization may be required. Collective learning does not imply collective authority over values, constraints, or deployment.

## 32. Failure Modes and Counterarguments

**F1 — Message Passing Masquerading as Learning.** Workers exchange artifacts but never adapt or improve their decisions. Remedy: measure target-side effects.

**F2 — Source-Context Leakage.** A method is applied without its original assumptions. Remedy: typed preconditions and target validation.

**F3 — False Generalization.** Similar-looking problems have different causal or constraint structures. Remedy: transfer testing and explicit negative evidence.

**F4 — Invalid Global Pruning.** A local NoSubtree rejection becomes a universal exclusion. Remedy: preserve decision scope and proof status.

**F5 — Metric Drift.** Source and target scores use incompatible objectives. Remedy: immutable metric versions and common evaluation.

**F6 — Evidence Laundering.** Repeated copies of one experiment appear to be independent confirmations. Remedy: lineage-aware evidence aggregation.

**F7 — Registry Bloat.** Storage and retrieval costs exceed reuse benefits. Remedy: tiered retention, indexing, and measured utility.

**F8 — Over-Centralization.** One registry or coordinator suppresses local diversity. Remedy: separate shared memory from local validation authority.

**F9 — Recursive Self-Confirmation.** A generator promotes changes using a metric it also changed. Remedy: frozen independent validation and rollback.

**F10 — Unsafe Artifact Execution.** Relay payloads cross trust boundaries without controls. Remedy: authorization, sandboxing, and least privilege.

**F11 — Unbounded Validation Costs.** Every transfer requires expensive retesting. Remedy: tiered evidence requirements, risk-based validation, and explicit budgets.

**F12 — Universal-Framework Inflation.** Every exchange of information is labeled Structural Relay. Remedy: require typed artifacts, applicability, provenance, receiving-side decisions, and measurable effects.

## 33. Conditional Properties and Proof Obligations

### 33.1 Idempotent assimilation

If every assimilation event is keyed by a stable artifact identity, target context, and promotion version, and the commit operation is atomic, repeated delivery can be made idempotent. This property does not guarantee that the artifact is correct.

### 33.2 Feasibility preservation

If an imported Delta operator is executed only after sound target constraint checking, then committed target states can preserve the declared hard constraints. The proof depends on the correctness of the checker and the completeness of the modeled constraints.

### 33.3 Sound negative knowledge

A source-domain infeasibility certificate may be reused only if the receiving problem satisfies the certificate's assumptions and the mapping preserves the relevant semantics. Otherwise it remains evidence or a hypothesis, not a valid pruning rule.

### 33.4 Termination

A finite relay queue with bounded validation attempts and no automatic unbounded resubmission terminates under fair scheduling and terminating validators. Open-ended recursive relay does not inherit this guarantee.

### 33.5 Non-guarantees

No general theorem follows that relay improves solution quality, that collective knowledge converges, that learned policies transfer safely, or that more agents produce greater intelligence. These are empirical questions subject to workload, communication, governance, and validation constraints.

## 34. Open Research Questions

1. What is the smallest interoperable Relay Unit that retains enough semantics for safe transfer?
2. How should artifact similarity be computed without mistaking superficial overlap for applicability?
3. Which classes of negative knowledge can be transferred with formal guarantees?
4. How can a receiver estimate transfer value before paying full validation cost?
5. When does method relay outperform result relay under equal budgets?
6. How should conflicting evidence from heterogeneous workers be reconciled?
7. Can a collective memory represent incomplete, competing, and perspective-dependent findings without premature consensus?
8. What state equivalence conditions support caching of relay-validation results?
9. How should distributed systems detect stale or revoked relay artifacts in long-running tasks?
10. What are effective privacy-preserving ways to relay methods and evidence without sharing sensitive source data?
11. Can Structural Relay improve SIF Localization on problems with no useful static address map?
12. How should repeated execution update confidence when observations are correlated?
13. What is the right balance between central validation and worker autonomy?
14. How can method-transfer experiments separate algorithmic gain from additional compute and data exposure?
15. Which generator or policy updates can be proven to preserve hard safety invariants?
16. How can Collective Learning remain open-ended without turning every speculative idea into an active policy?

## 35. Relationship to the CDE Series

- **CDE-001:** Establishes the transition from BTP and D-BTP to general CDE.
- **CDE-002:** Defines State Containers and constraint-governed transitions.
- **CDE-003:** Establishes typed Delta semantics and adaptive M-to-N branching.
- **CDE-004:** Separates Path Memory, NoSubtree, and Loop Back semantics.
- **CDE-005:** Defines solver integration and the boundary between heuristics and proofs.
- **CDE-006:** Provides directional and multi-scale candidate generation.
- **CDE-007:** Introduces solution and metric co-evolution, including independent validation.
- **CDE-008:** Proposes the CDE-Orchestrator and its control-plane responsibilities.
- **CDE-009:** Defines distributed exploration and task contracts.
- **CDE-010 (this paper):** Defines the transition from distributed exploration to evidence-bearing collective learning.
- **CDE-011 (planned):** Studies recursive generator and policy evolution.
- **CDE-012 (planned):** Examines generalization boundaries, guarantees, and open research.

The present paper is a conceptual foundation for **SPEC-005 Distributed Exploration and Relay Contract** and **CASE-005 Distributed CDE and Method Relay**.

## 36. Conclusion

Distributed exploration creates opportunities for collaboration, but it does not by itself establish collective intelligence. Structural Relay adds the missing discipline: discoveries must be packaged with their structure, evidence, assumptions, provenance, and applicability limits; recipients must validate and govern their use; and the effects of reuse must be measured.

The most important distinction is:

\[
\boxed{\text{Shared Results}\neq\text{Validated Transfer}\neq\text{Collective Learning}}
\]

A CDE system becomes collectively learning only when transferred experience changes future exploration or decisions in a way that can be evaluated. Result Relay, Method Relay, Negative Evidence, and Leftover Relay all contribute, but none should be promoted beyond its evidence. This contract-first, low-assumption approach offers a practical research direction for connecting CDE exploration to SIF Localization, heterogeneous solver portfolios, and controlled recursive evolution.

---

## Appendix A — Compact Canonical Relay Record

```yaml
relay_id: RELAY-EXAMPLE-001
schema_version: "0.1-draft"
relay_type: method
claim_status: observed
source:
  task_id: TASK-001
  problem_signature: source-domain-signature
  constraint_version: constraints-v3
  metric_version: metric-v2
payload:
  method_id: operator-v1
  method_type: delta-generator-operator
preconditions:
  - source-specific-structural-condition
claim:
  statement: "Improved source benchmark performance under the recorded protocol"
  scope: source-benchmark-only
evidence:
  benchmark_id: benchmark-v1
  validation_protocol: validation-v1
  negative_cases: []
provenance:
  origin_worker: worker-A
  parent_artifacts: []
rights:
  permitted_use: research-validation
lifecycle:
  status: candidate
  target_validation_required: true
```

This record is intentionally illustrative. A production specification must define canonical serialization, stable identifiers, cryptographic integrity where appropriate, schema evolution, and authorization semantics.

## Appendix B — Suggested Figure

**Fig-009 — Distributed CDE Orchestrator and Structural Relay** should distinguish the execution plane from the knowledge-relay plane. It should show heterogeneous workers producing result, method, evidence, constraint, and leftover artifacts; a versioned relay registry; receiving-side applicability and validation gates; and feedback to Delta generators and exploration policies. The figure should explicitly show that publication is not promotion.

## Appendix C — Research Context and Attribution

This proposal intersects with transfer learning, case-based reasoning, experience replay, distributed optimization, multi-agent systems, knowledge representation, proof-carrying artifacts, and scientific reproducibility. These established fields provide important baselines and comparison methods. The proposed CDE-specific synthesis is the combination of typed State–Delta–Constraint exploration with auditable relay artifacts and receiving-side validation. Empirical novelty and advantage remain to be demonstrated through the stated validation program.

**Research status:** Architectural proposal and draft conformance requirements; not an empirical performance claim.  
**Version:** v1.0.0 draft  
**License:** Apache-2.0
