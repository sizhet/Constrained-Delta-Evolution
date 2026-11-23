# GLOSSARY — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**Version:** v1.0.0 research release · **Date:** 2026-11-23  
**License:** Apache-2.0  
**Role:** Cross-document vocabulary, notation, semantic distinctions, and conformance-oriented reading reference  
**Scope:** 12 Foundations, 6 Specifications, 5 Cases, and 4 Extensions (27 research papers)

> **Research-status note:** A defined concept or proposed contract is not evidence that it has been implemented, empirically validated, or proven universally correct. Formal guarantees are conditional on stated assumptions. For implementation-specific conflicts, consult the corresponding SPEC document and record the discrepancy rather than silently changing semantics.

## 1. Quick orientation

CDE generalizes BTP and D-BTP into a State–Delta–Constraint framework with adaptive branching, multiple solver families, versioned metrics, Structural Relay, and independently governed meta-evolution.

```text
Task / State Container (S)
       |
       v
Delta Generator (G) --> M proposals --> Q attempts
       |                                |
       v                                v
Constraint Validation (C) -----> N realized edges
       |                                |
       v                                v
Committed Transitions (N_c) --> K unique successor states
       |
       +--> Path Memory / Solver Portfolio / Evidence
       |
       +--> Structural Relay --> Independent Target Validation
       |
       +--> Candidate Generator / Policy / Representation Evolution
                                  |
                                  v
                         Governance / Promotion / Rollback
```

The counts `M`, `Q`, `N`, `N_c`, and `K` are deliberately not interchangeable. CDE distinguishes proposals, attempts, edges, committed edges, and unique successors.

## 2. Reading conventions

**Definition:** a term given a scoped meaning in the CDE research model. **Proposed contract:** a normative-style interface intended for future implementation. **Hypothesis:** a testable claim not yet established. **Evidence:** a result with provenance and a declared validity scope. Terms such as *MUST* and *SHOULD* describe proposed conformance behavior, not a claim that a runtime exists.

For the formal specification series, see [SPEC-001](docs/specifications/SPEC-001-State-Container-and-Constraint-Contract.md), [SPEC-002](docs/specifications/SPEC-002-Delta-Generation-and-Branch-Contract.md), [SPEC-003](docs/specifications/SPEC-003-Path-Memory-and-Loop-Control-Contract.md), [SPEC-004](docs/specifications/SPEC-004-Solver-and-Optimization-Contract.md), [SPEC-005](docs/specifications/SPEC-005-Distributed-Exploration-and-Relay-Contract.md), and [SPEC-006](docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md).


## 3. Foundations and formal objects


**CDE — Constrained Delta Evolution.** A proposed contract-driven framework for exploring and evolving structured states through typed Deltas, hard constraints, adaptive branching, solver orchestration, and validated learning. CDE is not a universal optimization guarantee.


**BTP — Bucket Tree of Permutation.** Precursor framework using bucket-based constructive decisions and Yes/No branching; its fixed-context path-decision discipline informs CDE.


**D-BTP — Dynamic Bucket Tree of Permutation.** Intermediate framework emphasizing dynamically generated structural candidates, directional weak evidence, and controlled structural evolution.


**Problem Instance.** The declared domain, inputs, assumptions, objective/metric, admissible actions, hard constraints, and validation rules for a particular task.


**State Container (S).** Typed representation of a candidate solution or exploratory situation, including relevant domain data, context, derived information, and provenance; not necessarily a tree node.


**State Schema.** Versioned contract specifying permitted fields, types, invariants, and semantic interpretation of a State Container.


**State Identity.** Identifier for a particular encoded state under a specific representation and version; distinct from semantic or future equivalence.


**Delta (Δ).** Typed proposal to transform a state; a Delta definition, Delta instance, attempted transition, and committed transition edge are different objects.


**Delta Operator.** A versioned transformation family with input types, preconditions, effects, constraint dependencies, and validation obligations.


**Transition Function (T).** Mechanism mapping a state and applicable Delta to a proposed successor or failure outcome, subject to the declared transition contract.


**Generator (G).** Mechanism proposing candidate Delta instances for a state, task, and context; proposal is not authorization to commit.


**Constraint Bundle (C).** Versioned collection of hard requirements and possibly soft preferences, with explicit evaluation semantics and provenance.


**Metric / Evaluation (M).** Versioned function, vector, partial order, or evaluation contract used to compare candidates under a declared perspective; not automatically a hard constraint.


**Path / Lineage Context (L).** Record of relevant decisions, transition history, versions, evidence, and path-local exclusions.


**CDE Problem Tuple.** Convenient notation P=(S,Δ,T,C,G,L), sometimes augmented with metrics and policies; components must be interpreted through their contracts.


**Candidate State.** A proposed successor not yet established as feasible, committed, or globally valid.


**Committed State.** A state accepted by the appropriate transition and hard-constraint checks under coherent versions.


**State Graph.** Graph of states and transition edges; may be a tree, DAG, or cyclic graph depending on equivalence, revisitation, and Loop Back policies.


## 4. Delta generation and branching


**M — Proposed Delta Count.** Number of candidate Delta instances proposed at a given expansion scope; not the number of accepted branches.


**Q — Selected Attempt Count.** Number of selected candidate attempts actually submitted to evaluation or transition processing.


**N — Realized Edge Count.** Number of realized transition edges, including separate edges that may lead to the same successor, as defined by the reporting contract.


**N_c — Committed Edge Count.** Number of realized edges admitted as committed transitions after required validation.


**K — Unique Successor Count.** Number of distinct successor states under a declared identity/equivalence policy; K can be less than N.


**D — Distinct Delta Definition Count.** Number of distinct operator definitions in scope; not interchangeable with the M count of instantiated proposals.


**Adaptive M-to-N Branching.** Dynamic selection of how many Delta candidates to generate, attempt, and realize based on constraints, evidence, budgets, and policy.


**Branch Fan-out.** Number of outgoing realized or committed edges at a node, with the counting convention explicitly stated.


**Delta Identity.** Stable, version-aware identity for a particular Delta proposal or operator instance; distinct from the identity of its effect.


**Decision Identity.** Identity of a logical choice that may be accepted, excluded, deferred, or reconsidered under explicit context rules.


**Transition Edge.** Provenance-bearing record of parent, Delta, disposition, outcome, and successor; Delta semantics attach to transitions, not automatically to nodes.


**Local Delta.** Small-scale change near the current solution or structural neighborhood.


**Regional Delta.** Medium-scale change spanning a neighborhood or cluster.


**Long-Range Delta.** Large-scale or discontinuous exploration proposal that may escape local search basins.


**Directional Delta.** Proposal biased by a directional field, evidence, gradients, heuristics, or other signals without assuming those signals are correct.


**Composite Delta.** Combination or macro of multiple operations with declared atomicity, preconditions, and validation requirements.


**Delta Portfolio.** Collection of generators or operator families with allocation rules, coverage accounting, and evidence about applicability.


**Coverage Claim.** Explicit statement about which candidates, operators, states, or decisions are covered; requires a bounded scope and supporting argument.


## 5. Constraints, paths, and proof discipline


**Hard Constraint.** Requirement that must pass for a relevant transition to be committed; UNKNOWN and ERROR are not equivalent to PASS.


**Soft Constraint / Preference.** Preference or penalty that guides ranking without overriding hard validity unless the policy explicitly reclassifies it.


**PASS / FAIL / UNKNOWN / ERROR.** Four distinct validator outcomes: verified pass, verified failure, unresolved status, and evaluation malfunction.


**EXCLUDE.** Explicit path-level decision to disallow a candidate in the declared constructive context; not necessarily a global proof of infeasibility.


**INFEASIBLE.** Failure of an applicable hard constraint, ideally with a scope-aware certificate or diagnostic.


**DEFER.** Postpone a decision or evaluation; does not imply rejection.


**PRUNE.** Remove a branch using a stated rule; proof-backed pruning and heuristic pruning must be distinguished.


**YesSubtree.** Constructive BTP-style continuation where a candidate decision is accepted under fixed-context semantics.


**NoSubtree.** Constructive BTP-style continuation where a candidate decision is excluded; not a claim that an arbitrary state graph has no subtree.


**Accepted Set (A).** Set of accepted constructive decision identities along a path.


**Excluded Set (R).** Set of explicitly excluded constructive decision identities along a path.


**Resolved Set (U).** U=A∪R for a fixed constructive decision universe; fresh decisions must not be in U under the corresponding NoSubtree discipline.


**Path Memory.** Version-aware history of decisions and states relevant to loop prevention, explanation, and admissible future actions.


**Loop Back.** Controlled revisitation of an earlier state or region when representation, evidence, policy, or search context makes revisitation legitimate.


**State Deduplication.** Coalescing states under a declared identity/equivalence relation; incorrect equivalence may destroy valid future possibilities.


**Future Equivalence.** Relation under which two states have equivalent admissible continuations and objective implications for a declared task context.


**Dominance.** Relation allowing one candidate to supersede another under defined constraints, objective, and continuation assumptions.


**Admissible Bound.** Bound guaranteed not to exclude a better feasible solution when used under its declared assumptions.


**Sound Pruning.** Branch elimination justified by valid constraints, dominance, or admissible bounds.


**Heuristic Pruning.** Branch elimination based on estimated promise; may lose completeness or optimality.


**Atomic Commit.** Transition is recorded as committed only after all required checks pass against coherent versions.


**Proof Certificate.** Evidence that supports a precise formal claim, such as an admissible bound or infeasibility result, within its declared assumptions.


**BEST_KNOWN.** Best feasible solution currently found, without a claim of global optimality.


**BOUNDED_GAP.** Optimization status supported by a valid bound and a stated remaining gap.


**OPTIMAL.** Global optimality established under the encoded model and valid proof assumptions.


**INFEASIBLE_PROVEN.** No feasible solution exists for the specified model under a sound proof.


**EXHAUSTIVE.** All candidates in a declared finite or otherwise provably covered search universe have been examined.


**UNKNOWN (solver status).** No stronger result has been established; not evidence of infeasibility.


## 6. Search, optimization, and direction


**Search.** Exploration of candidate states or paths under a chosen strategy; may be exact, approximate, heuristic, or hybrid.


**Dynamic Programming (DP).** Reuse of subproblem results under a sound subproblem identity and future-equivalence relation.


**Branch-and-Bound (B&B).** Optimization technique using valid bounds to eliminate branches that cannot improve the incumbent.


**Solver Portfolio.** Set of alternative solvers or strategies selected or coordinated by the CDE control plane.


**Orchestrator.** Logical control plane that allocates tasks, budgets, validation, solver choice, and result integration; not necessarily a single centralized process.


**Incumbent.** Current best feasible candidate under a pinned objective and constraint context.


**Directional Field.** Signal over candidate transformations or regions indicating promising directions; may be weak, noisy, perspective-dependent, or misleading.


**Weak Evidence.** Incomplete, uncertain, or indirect signal that may guide prospecting but cannot by itself establish a hard conclusion.


**Prospecting.** Deliberate exploratory probes designed to discover promising directions or reduce uncertainty.


**Exploration–Exploitation Tradeoff.** Allocation between searching less-known possibilities and refining already promising candidates.


**Multi-Scale Exploration.** Combination of local, regional, long-range, and potentially directional operators.


**Localization.** Finding relevant structural positions, capabilities, methods, or evidence; in SIF, may include exploratory localization of solutions.


**Pareto Dominance.** Multiobjective comparison in which one candidate is no worse on all specified objectives and better on at least one.


**Lexicographic Objective.** Priority ordering of objectives where a lower-priority objective cannot compensate for a worse higher-priority objective.


## 7. Metric and representation evolution


**Solution Space.** Set of possible solution candidates under a representation and task definition.


**Metric Space.** Set of candidate scoring rules or evaluation structures, each with explicit versions and validation requirements.


**Solution–Metric Co-Evolution (SMCE).** Coupled but separately governed search over solutions and evaluation metrics; a higher score under a new metric does not establish a better metric.


**Metric Container.** Typed, versioned artifact defining objectives, features, aggregation, perspectives, missingness, uncertainty, and comparison semantics.


**Metric Delta.** Typed proposed change to a metric, such as feature addition, reweighting, calibration, aggregation change, or perspective change.


**Metric Version Pinning.** Fixing metric identity during comparisons, optimization claims, and proof/certificate evaluation.


**Metric Re-scoring.** Re-evaluating comparable candidates under a common metric version when meaningful cross-version comparison is required.


**Goodhart Effect.** Failure mode in which optimizing a proxy score undermines the underlying goal it was intended to represent.


**Proxy Misalignment.** Mismatch between an operational metric and the intended decision utility or scientific objective.


**Independent Metric Validation.** Assessment of candidate metrics using evidence or criteria not controlled by the candidate metric optimizer.


**Perspective.** Explicit stakeholder, objective, or interpretive viewpoint used to evaluate solutions and evidence.


**Representation.** Encoding and semantic interpretation of domain states, entities, relations, and distinctions.


**Representation Delta.** Meta-level proposal to change the state schema, abstraction, granularity, decomposition, or semantic interface.


**Operator Genesis.** Generation of a new Delta operator type or macro, subject to typed contracts and independent validation.


**Migration Contract.** Versioned map between representations with declared preserved observables, information loss, constraint checks, and cache policy.


**Semantic Preservation.** Retention of specified domain observables across a representation transformation, under declared assumptions.


**Lossy Abstraction.** Representation change that discards distinctions; permissible only when the loss is disclosed and appropriate validation remains possible.


**Representability.** Whether a target structure can be expressed in the current State Representation.


**Generability.** Whether available Delta operators can produce a target from an admissible starting state.


**Reachability.** Whether a target can be reached through a sequence of valid transitions under specified operators and constraints.


**Discoverability Under Budget.** Whether a search procedure can actually find and validate a target with bounded resources.


**Open-Ended Delta Evolution.** Research direction allowing proposal of new operator and representation families rather than only tuning a fixed vocabulary.


## 8. Distributed execution and Structural Relay


**Distributed CDE.** CDE execution across multiple workers or solvers with versioned tasks, reconciliation, failure semantics, and global validation.


**Task Envelope.** Versioned package describing a worker assignment, source snapshot, Delta scope, constraints, metric, budget, authority, and provenance.


**Lease.** Time- or epoch-bounded authority for a worker to process an assigned task.


**Epoch / Fencing Token.** Monotonic version marker used to reject stale worker results or commits after reassignment.


**Idempotency.** Property that replaying the same authorized operation does not duplicate its committed effect.


**Global Reconciliation.** Process verifying that local results remain mutually compatible with shared constraints, resources, and metric versions.


**Distributed Termination.** Claim that the distributed search has finished a declared scope, requiring accounting for in-flight tasks, leases, queues, and failures.


**Structural Relay.** Versioned transfer of typed results, evidence, constraints, methods, policies, metrics, and unresolved work across tasks or agents.


**Result Relay.** Transfer of candidate solutions, states, and appropriately scoped certificates.


**Evidence Relay.** Transfer of observations, counterexamples, uncertainty, and provenance.


**Constraint Relay.** Transfer of constraint definitions, bounds, validity assumptions, and validation requirements.


**Method Relay.** Transfer of operators, search strategies, solvers, or procedural knowledge with applicability contracts.


**Policy Relay.** Transfer of validated selection, allocation, control, or governance policies.


**Metric Relay.** Transfer of evaluation criteria, metric definitions, calibration evidence, and comparison semantics.


**Leftover Relay.** Transfer of unresolved tasks, failed attempts, missing evidence, and open research questions.


**Explicit Leftover.** First-class artifact preserving unresolved or unprocessed work rather than silently treating it as solved or discarded.


**Temporal Relay.** Repeated execution viewed as knowledge accumulation when independently informative evidence is preserved across runs.


**Target-Side Validation.** Independent check that a transferred artifact is applicable and useful in the receiving task or domain.


**Negative Transfer.** Transfer that degrades target performance, violates constraints, or misleads inference.


**Provenance.** Traceable source, versions, assumptions, transformations, evidence, and authority of an artifact.


**Capability Registry.** Versioned catalog of validated methods, operators, representations, and their compatibility limits.


## 9. Collective intelligence, RSI, and scientific discovery


**Collective Learning.** Measurable improvement in subsequent tasks attributable to reusable shared knowledge, beyond merely running multiple agents.


**Multi-Agent RSI.** Research approach in which multiple agents explore, critique, and propose improvements to methods or policies under independent validation.


**Structural RSI.** Controlled recursive self-improvement through versioned changes to generators, representations, metrics, and policies, with independent promotion authority.


**Meta-Delta.** Proposed change to an exploration generator, policy, representation, or other meta-level mechanism rather than an ordinary task state.


**Generator Evolution.** Modification of how Delta candidates are proposed, including operator portfolios, granularity, sampling, and representation.


**Policy Evolution.** Modification of branching, solver selection, budgets, prioritization, or stopping rules.


**Promotion Gate.** Explicit independent requirements before a candidate capability becomes active in a defined scope.


**Rollback.** Controlled return to an authorized earlier configuration where feasible; irreversible external effects cannot necessarily be undone.


**Governance Layer.** Independent authority for validating and approving changes to capabilities, constraints, metrics, and deployment scope.


**Counterexample Agent.** Logical role dedicated to finding violations of proposed claims, operators, migrations, or transfer assumptions.


**Hypothesis Unit.** Structured scientific explanation candidate with assumptions, predictions, evidence requirements, and falsification conditions.


**Experimental Design Delta.** Proposed change to an experiment intended to improve discrimination between competing hypotheses.


**Replication.** Independent repetition of a procedure to assess robustness; correlated repetitions do not count as independent evidence.


**Causal Identification.** Support for a causal claim under a justified design or identification assumption; distinct from prediction accuracy.


**AQSI — Autonomous Questioning Structural Intelligence.** Related research direction for generating and relaying structured research questions and evidence gaps.


**SIF — Structural Intelligence Framework.** Related architecture for problem–capability mapping, localization, control, and node-level intelligence.


**UIERU — Universal Intelligence Evolution and Relay Unit.** Related framework for intelligence units, structural relay, and collective evolution.


## 10. Validation, evidence, and research claims


**Conformance.** Agreement of an implementation or artifact with a stated specification; conformance is not a proof of usefulness.


**Contract First.** Design discipline of declaring schemas, preconditions, effects, invariants, and evidence requirements before runtime behavior.


**Conformance before Runtime.** Preference for validating interfaces and semantics before scaling execution or accepting capability claims.


**Test Fixture.** Controlled input and expected outcome used to exercise a contract or failure condition.


**Negative Control.** Case designed to show no improvement, expose an invalid mechanism, or catch confounding.


**Ablation.** Experiment removing one mechanism to estimate its contribution while controlling other factors.


**Held-Out Validation.** Evaluation on data or tasks not used for candidate generation or tuning.


**Independent Evaluator.** Evaluation authority whose criteria and evidence are not freely modifiable by the candidate being assessed.


**Reproducibility Manifest.** Record of versions, seeds, tasks, budgets, metrics, results, provenance, and limitations needed to repeat an experiment.


**Feasibility.** Satisfaction of the encoded hard constraints; does not by itself imply real-world correctness or optimality.


**Completeness.** Property of finding a solution if one exists within a declared search universe, under specified assumptions.


**Optimality.** Proof that no feasible candidate in the declared problem has a better objective value.


**Efficiency.** Resource cost for obtaining a result; must include orchestration, migration, synthesis, and validation overhead when relevant.


**Validated Generalization.** Measured performance beyond the development distribution, with clearly bounded evidence and uncertainty.


**Research Hypothesis.** Falsifiable proposed explanation or performance claim; not a confirmed result.


**Conditional Guarantee.** Guarantee valid only if its explicit assumptions, encodings, and proof obligations hold.


**Evidence Scope.** Set of tasks, distributions, versions, assumptions, and conditions actually supported by an observation or proof.


**Unknown versus False.** Absence of evidence is not evidence of infeasibility or invalidity; an unresolved validator outcome must not be silently collapsed into failure or success.


## 11. Mathematical notation and counting

| Symbol | Meaning | Important qualification |
|---|---|---|
| $\mathcal P$ | Declared problem | Includes scope, versions, and assumptions |
| $S$, $s$ | State space; state instance | Representation-dependent |
| $\Delta$, $\delta$ | Delta family; Delta instance | Not necessarily a committed edge |
| $T(s,\delta)$ | Proposed transition | Must be constraint-checked |
| $C(s)$ | Constraint evaluation | `UNKNOWN` is not `PASS` |
| $G(s)$ | Delta generator | Proposal does not imply reachability |
| $L$ | Path/lineage context | Version and scope dependent |
| $M$ | Proposed Delta instances | Counts proposals |
| $Q$ | Selected attempts | Counts attempted evaluations |
| $N$ | Realized transition edges | May include convergent successors |
| $N_c$ | Committed transition edges | Must pass applicable validation |
| $K$ | Unique successor states | Requires stated equivalence policy |
| $D$ | Distinct Delta definitions | Different from Delta instances |
| $A$ | Accepted constructive decisions | Path-local |
| $R$ | Excluded constructive decisions | Path-local; not a global proof |
| $U=A\cup R$ | Resolved constructive decisions | Assumes fixed decision universe |
| $\mathcal R_v$ | Representation version | Migration may invalidate caches |
| $\mathcal O_v$ | Operator vocabulary version | Operator changes require revalidation |
| $m_v$ | Metric version | Cross-version scores may not compare |
| $x$ | Candidate solution | Distinct from a metric or policy |

A basic task-state transition is:

$$
\mathcal P=(S,\Delta,T,C,G,L),\qquad
s_{t+1}=T(s_t,\delta_t),\quad \delta_t\in G(s_t).
$$

A generated successor becomes committed only if all required checks pass under coherent versions. A representation/operator meta-transition is a separate, governed candidate:

$$
(\mathcal R_{v+1},\mathcal O_{v+1})
=\mathcal E(\mathcal R_v,\mathcal O_v,\mu_v).
$$

The expression describes a proposed change; it does not authorize deployment.

## 12. Frequently confused distinctions

| Distinction | Why it matters |
|---|---|
| Delta definition vs Delta instance vs transition edge | An operator type can produce many proposals; several edges can reach one state |
| M vs Q vs N vs N_c vs K | Proposal, attempt, realization, commitment, and uniqueness are separate accounting units |
| State identity vs decision identity vs future equivalence | Different equality relations support different deduplication and pruning claims |
| EXCLUDE vs INFEASIBLE vs DEFER vs PRUNE | A search decision is not automatically a proof |
| Hard constraint vs soft score | A high score cannot override a failed hard constraint |
| Weak evidence vs certificate | Directional hints guide exploration but do not establish truth |
| NoSubtree vs no cycles | Constructive decision nonrepetition does not rule out revisitation in general state graphs |
| Search vs DP vs B&B | Reuse and bounds need solver-specific correctness assumptions |
| Representability vs generability vs reachability | Encoding a target does not ensure operators can produce it |
| Reachability vs completeness vs optimality | Finding a path does not prove exhaustive search or a best solution |
| Internal metric gain vs validated decision utility | Optimizing a proxy can make the proxy worse |
| Source method success vs target transfer | Target compatibility and independent validation are required |
| Parallel execution vs Collective Learning | More workers do not prove reusable learning |
| Candidate meta-evolution vs Structural RSI | A proposed change is not a validated recursive improvement |
| Contract conformance vs empirical effectiveness | Passing tests does not demonstrate performance superiority |
| Unknown vs false | Missing proof must not be converted into negative proof |

## 13. Reading map by terminology cluster

| Topic | Primary papers |
|---|---|
| BTP lineage and CDE identity | [CDE-001](docs/foundations/CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md) |
| State, transitions, constraints | [CDE-002](docs/foundations/CDE-002-State-Containers-and-Constraint-Governed-Transitions.md), [SPEC-001](docs/specifications/SPEC-001-State-Container-and-Constraint-Contract.md) |
| Delta, M-to-N, operator types | [CDE-003](docs/foundations/CDE-003-Delta-Semantics-and-Adaptive-M-to-N-Branching.md), [SPEC-002](docs/specifications/SPEC-002-Delta-Generation-and-Branch-Contract.md) |
| NoSubtree, memory, Loop Back | [CDE-004](docs/foundations/CDE-004-Path-Memory-No-Subtree-Semantics-and-Loop-Back-Control.md), [SPEC-003](docs/specifications/SPEC-003-Path-Memory-and-Loop-Control-Contract.md) |
| Search, DP, B&B, proof status | [CDE-005](docs/foundations/CDE-005-Search-Dynamic-Programming-and-Branch-and-Bound-Integration.md), [SPEC-004](docs/specifications/SPEC-004-Solver-and-Optimization-Contract.md) |
| Directional and multi-scale exploration | [CDE-006](docs/foundations/CDE-006-Directional-Fields-and-Multi-Scale-Delta-Generation.md), [CASE-001](docs/cases/CASE-001-Multi-Scale-Peak-Search.md) |
| Metric evolution | [CDE-007](docs/foundations/CDE-007-Solution-Space-and-Metric-Space-Co-Evolution.md), [CASE-004](docs/cases/CASE-004-Metric-Discovery-and-Co-Evolution.md) |
| Distributed CDE and Relay | [CDE-009](docs/foundations/CDE-009-Distributed-CDE-and-Delta-Exploration-Contracts.md), [CDE-010](docs/foundations/CDE-010-Structural-Relay-and-Collective-Learning.md), [SPEC-005](docs/specifications/SPEC-005-Distributed-Exploration-and-Relay-Contract.md), [CASE-005](docs/cases/CASE-005-Distributed-CDE-and-Method-Relay.md) |
| Recursive evolution and governance | [CDE-011](docs/foundations/CDE-011-Recursive-Generator-and-Policy-Evolution.md), [SPEC-006](docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md), [EXT-004](docs/extensions/EXT-004-CDE-Collective-Intelligence-and-Structural-RSI.md) |
| Operator and representation invention | [EXT-002](docs/extensions/EXT-002-Open-Ended-Delta-and-Representation-Evolution.md) |
| Scientific discovery and questioning | [EXT-003](docs/extensions/EXT-003-CDE-for-Scientific-Hypothesis-and-Metric-Discovery.md) |
| Limits and conditional guarantees | [CDE-012](docs/foundations/CDE-012-Generalization-Boundaries-Guarantees-and-Open-Research.md) |

## 14. Terminology maintenance and change control

1. **Preserve stable meaning.** Changing a term's semantics requires a documented version change, not a silent edit.
2. **Prefer explicit qualifiers.** For example, write *committed edge count* instead of an unqualified *branch count*.
3. **Keep claim status visible.** Proposed mechanisms, conformance obligations, experimental hypotheses, and observed results must remain distinguishable.
4. **Resolve specification conflicts explicitly.** Record which SPEC and version governs an implementation; update the glossary and dependent files consistently.
5. **Protect reproducibility.** Preserve legacy aliases and mappings when terminology changes, especially for stored experiments and Relay artifacts.
6. **Welcome counterexamples.** Ambiguous or inconsistent definitions should be recorded as open issues and refined with examples and tests.

**End of glossary.**
