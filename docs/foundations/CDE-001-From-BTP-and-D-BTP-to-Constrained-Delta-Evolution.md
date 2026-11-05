# CDE-001 — From BTP and D-BTP to Constrained Delta Evolution

**Series:** Constrained Delta Evolution (CDE) — Foundations  
**Status:** Foundational research proposal, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0

## Abstract

Bucket Tree of Permutation (BTP) and Dynamic Bucket Tree of Permutation (D-BTP) motivate a broader computational abstraction: **Constrained Delta Evolution (CDE)**. Rather than requiring bucket-shaped states, permutation-specific operations, or binary Yes/No branching, CDE represents a problem through logical state containers, candidate transformations (Deltas), constraints, transition semantics, and exploration policies. A state may produce multiple candidate Deltas and a dynamically selected set of successor transitions. The resulting search structure may be a tree, a directed acyclic graph, or a graph with revisitation and cycles, provided the relevant semantics and controls are explicit. CDE is proposed both as a minimal computational core and as an optional orchestration layer coordinating heterogeneous solvers, metric evolution, distributed exploration, structural relay, and controlled recursive improvement. This paper establishes the motivation, conceptual model, inheritance from BTP and D-BTP, architectural boundaries, relationship to Structural Intelligence Framework (SIF) localization, and a falsifiable research agenda. It does not claim that CDE is a new universal optimizer or that it improves on established methods without empirical validation.

**Keywords:** constrained delta evolution; structural intelligence; state-space search; constraint programming; dynamic branching; metric evolution; localization; distributed optimization; structural relay

## 1. Motivation: From a Specific Search Structure to a General Computational Model

BTP offers a useful way to express constructive decisions, including assignment and matching, through structured branching. In its familiar binary form, a candidate decision may produce a **Yes** branch that accepts it and a **No** branch that excludes it. D-BTP broadens this setting by introducing dynamically generated candidate transformations, directional guidance, structural feedback, and evolving exploration strategies.

These developments invite a question: which properties belong to the particular BTP implementation, and which belong to a more general family of constrained computational processes?

Several restrictions need not be fundamental:

1. A state need not be a *bucket*; it can be any logical container with well-defined semantics.
2. A transformation need not be a permutation or assignment; it can change a solution, a hypothesis, a metric, a representation, or a search policy.
3. A node need not have exactly two branches; a generator may propose **M** candidate Deltas, of which **N** successor transitions are realized.
4. The exploration structure need not remain a tree; distinct paths may reach equivalent states, and some problems legitimately revisit states.
5. The executor need not be a single algorithm or machine; different workers may explore different regions using different methods under shared contracts.

CDE is the proposed abstraction obtained by separating these general mechanisms from BTP-specific choices.

## 2. Intellectual Lineage and Scope

The conceptual progression is:

\[
\mathrm{BTP}\;\longrightarrow\;\mathrm{D\!\text{-}\!BTP}\;\longrightarrow\;\mathrm{CDE}.
\]

This arrow denotes **abstraction and inheritance**, not a proof that every implementation is a strict superset of every preceding implementation.

| Dimension | BTP (motivating form) | D-BTP (motivating extension) | CDE (proposed abstraction) |
|---|---|---|---|
| State representation | Bucket/permutation-oriented | Dynamically evolving structural states | Contract-defined logical state containers |
| Delta | Candidate constructive decision | Dynamically generated structural transformation | General typed, context-sensitive transformation |
| Branching | Typically Yes/No | Dynamically guided branching | Adaptive M-candidate-to-N-successor expansion |
| Topology | Primarily tree | Tree and richer search behavior | Tree, DAG, or cyclic state graph |
| Evaluation | Feasibility and objective | Directional/structural scoring | Pluggable objectives, metrics, evidence, validation |
| Execution | Search traversal | Adaptive exploration | Core transitions plus optional heterogeneous orchestration |
| Learning | Not essential | Feedback and relay opportunities | Optional metric, generator, policy, and method evolution |

**Scope discipline.** CDE is not defined as an algorithm that automatically solves all constrained problems. It is a framework for expressing and coordinating constrained transitions. A particular CDE solver must still specify its search strategy, objective, resources, admissible pruning rules, and stopping criteria.

## 3. A Minimal CDE Problem Model

Let a CDE problem instance be described by:

\[
\mathcal P=(\mathcal S,\mathcal D,G,T,C,E,L),
\]

where:

- \(\mathcal S\) is the space of logically meaningful states;
- \(\mathcal D\) is the space of possible Delta descriptions;
- \(G(s,h,b)\) generates or proposes candidate Deltas given state \(s\), relevant history \(h\), and resource budget \(b\);
- \(T(s,\delta,\omega)\) describes a successor transition, with \(\omega\) optionally representing stochastic outcomes or observations;
- \(C(s,\delta,s')\) specifies hard feasibility and transition constraints;
- \(E(s,m,e)\) evaluates a state or outcome under metric definition \(m\) and evidence \(e\);
- \(L\) specifies terminal conditions, convergence criteria, or a stopping policy.

An individual candidate transition is:

\[
 e_i=(s,\delta_i,a_i,s'_i,\kappa_i),
\]

where \(a_i\) identifies the transition or decision action and \(\kappa_i\) carries necessary provenance, context, or version information. In deterministic settings, \(s'=T(s,\delta)\); in stochastic settings, a Delta may admit multiple possible successors.

The core invariants are:

1. **Interpretability:** the state and Delta have specified meaning in their problem domain.
2. **Transition validity:** a realized successor satisfies the applicable transition contract and hard constraints.
3. **Traceability:** a realized branch records the Delta and context sufficient to explain its origin.
4. **Explicit control:** repetition, revisitation, pruning, and stopping are governed by stated policies rather than hidden assumptions.

These are design requirements for an implementation, not claims that all implementations satisfy them automatically.

### 3.1 State as a Logical Container

A state may include assignments, routes, graphs, symbolic hypotheses, partial programs, belief states, or learned metrics. The term **container** is intentionally semantic rather than prescriptive: CDE does not require a particular memory layout or data structure.

A useful implementation view is:

\[
 s=(x,c,e,h,\mu),
\]

with domain configuration \(x\), applicable constraint context \(c\), evidence \(e\), relevant history \(h\), and metadata \(\mu\). These fields need not all be stored inside the state object; the contract must only make their effects explicit.

### 3.2 Adaptive M-to-N Delta Branching

For a given state, let:

\[
 G(s)=\{\delta_1,\ldots,\delta_M\}.
\]

After feasibility checking, selection, deduplication, and budget allocation, the solver realizes a set of \(N\) successor **transitions**:

\[
 \operatorname{Expand}(s)=\{e_1,\ldots,e_N\}.
\]

Neither \(N=M\) nor binary branching is required. A single Delta can produce more than one outcome in nondeterministic models; several Deltas can lead to the same canonical state. Thus **M counts proposed Delta descriptions; N counts realized transitions**, not necessarily unique successor states. An implementation must specify finite expansion budgets or a productive enumeration policy when the candidate space is unbounded.

### 3.3 Delta Identity Is Not State Identity

The following are distinct questions:

- Is the **same Delta** being proposed again?
- Is the **same decision** being made again along a path?
- Is the **same state** being revisited?

Canonical Delta identity may depend on the operation, operands, and relevant context. Canonical state identity must preserve all information that affects future legal transitions and evaluation. Conflating these identities can cause duplicate work, invalid pruning, or incorrect loop handling.

## 4. Path Memory, Yes/No Decisions, and Loop Back

A constructive BTP subfamily may impose **path-local decision uniqueness**. If a candidate assignment \(\delta\) has already been accepted or permanently rejected along a path, that same decision should not be made again on that path:

\[
 U_t=A_t\cup R_t,\qquad \delta_{t+1}\notin U_t.
\]

Here \(A_t\) and \(R_t\) are accepted and permanently rejected decisions. This explicitly includes **No-subtree** decisions; otherwise a rejected assignment could be proposed again indefinitely.

However, the rule is **not universal to CDE**. A temporary failure, deferred candidate, or context-dependent infeasibility may legitimately be reconsidered after the state or evidence changes. Likewise, reconfiguration and iterative-improvement problems may require undoing an assignment or revisiting a state.

CDE therefore separates:

- **Decision-memory policy:** which decisions may recur within a path;
- **State-revisit policy:** whether an equivalent state may be explored again;
- **Progress/termination policy:** how cycles, budgets, ranking functions, and convergence are handled.

A finite Delta vocabulary alone does not prove termination if operations can repeat, if context changes, or if cycles are allowed. Termination and completeness require separate assumptions and, where possible, proofs.

## 5. CDE as a Solver-Neutral Computational Core

CDE should not collapse established algorithms into one label. Their operational distinctions remain important:

- **State-space search** specifies how alternatives are traversed.
- **Dynamic programming** exploits equivalent subproblems and reusable results under a sound state-equivalence relation.
- **Branch-and-bound** prunes only when mathematically valid bounds justify exclusion.
- **Constraint propagation** reduces domains or rules out infeasible transitions using sound inferences.
- **Heuristic and population-based search** prioritize or sample candidates but generally do not guarantee global optimality without additional conditions.

CDE provides a common interface for these mechanisms. A solver may combine them, but a high directional score is **not** a proof of safe pruning, and caching is **not** automatically sound unless the cached state key includes all future-relevant information.

A useful separation is:

\[
 \text{CDE-Core} + \text{Solver Policy} + \text{Validation Contract}.
\]

The core defines transitions and constraints; the solver policy determines exploration; validation establishes which claims are justified.

## 6. Two Evolution Spaces: Solutions and Metrics

In conventional optimization, a metric \(m\) is often fixed while solutions \(x\) vary:

\[
 x^*=\arg\max_{x\in\mathcal X}m(x).
\]

Many practical systems also need to **discover or improve the metric itself**. CDE can represent metric changes as Deltas:

\[
 m_{t+1}=T_m(m_t,\delta^m_t),
\]

including changes to feature sets, weights, aggregation rules, structural scoring trees, or perspective-dependent evaluation.

This introduces a coupled exploration space:

\[
 (x_{t+1},m_{t+1})=T_{xm}((x_t,m_t),\delta_t).
\]

**Critical caveat:** scores produced under different metrics are not automatically comparable. Every result should retain its metric version, constraint version, evaluation context, and evidence. Metric quality must be assessed against an external criterion—such as held-out predictive performance, decision utility, robustness, or independently specified domain requirements—to avoid circular self-validation and reward hacking.

Metric evolution is an optional CDE application, not a mandatory feature of every core transition system.

## 7. CDE-Orchestrator: Distributed Exploration and Structural Relay

The **CDE-Orchestrator** is a proposed layer above CDE-Core. It can allocate exploration tasks across heterogeneous workers, each potentially using a different solver or Delta generator.

A minimal task contract is:

\[
 W=(s,\mathcal D_{\mathrm{scope}},C,P,B,V),
\]

where \(s\) is a starting state, \(\mathcal D_{\mathrm{scope}}\) the authorized Delta scope, \(C\) the constraint context, \(P\) the exploration policy, \(B\) the budget, and \(V\) the validation protocol.

Workers may return several different kinds of artifacts:

1. **Result Relay:** candidate states and their validated evaluations;
2. **Evidence Relay:** observations, counterexamples, and failure information;
3. **Constraint Relay:** discovered conflicts or justified restrictions;
4. **Method Relay:** reusable search operators, heuristics, or Delta generators;
5. **Policy Relay:** proposed resource-allocation or exploration-policy changes.

Independent best-result aggregation may be simple when all results share a comparable objective. Combining local solutions under global resource or coupling constraints may be difficult and require a separate reconciliation optimization. Asynchronous execution also requires provenance, version management, duplicate-work controls, and termination detection.

### 7.1 From Feedback to Controlled Recursive Improvement

A generator or policy may adapt using validated feedback:

\[
 G_{t+1}=U(G_t,F_t),\qquad P_{t+1}=V_P(P_t,F_t).
\]

Parameter adaptation is distinct from **structural recursive self-improvement**, which proposes changes to representations, operators, or evaluation mechanisms. Such changes should be tested independently and remain reversible. The framework does not imply autonomous improvement is guaranteed.

## 8. CDE as a Candidate Localization Pillar within SIF

A further research hypothesis is that CDE can serve as an important **Localization** method within the **Structural Intelligence Framework (SIF)**.

Localization need not mean merely retrieving a known address. In many problems, the relevant target, capability, structural component, or useful search region is not yet known. It must be **constructed, discovered, or progressively approached** through constrained transformations.

This suggests a distinction:

- **Direct localization:** identify or retrieve a known or indexed structure.
- **Exploratory localization:** generate and evaluate candidate structural paths or regions until an acceptable target is located.

CDE is a candidate mechanism for the second form, and may work alongside existing SIF problem–capability maps, addressing mechanisms, directional scoring, and relay infrastructure.

A conceptual interface is:

\[
 \text{SIF Localization Request}
 \rightarrow \text{CDE Exploration Contract}
 \rightarrow \text{Validated Candidate Locations}
 \rightarrow \text{SIF Structural Relay}.
\]

This is a **proposed architectural integration**, not a demonstrated replacement for indexing, retrieval, graph search, or other localization methods. Future studies should identify where exploratory Delta localization adds value, when direct lookup is superior, and how the two can cooperate.

## 9. Illustrative Problem Families

### 9.1 Structural Matching and Assignment

Deltas accept, exclude, revise, or propagate candidate assignments. Hard constraints enforce compatibility and uniqueness. This family provides a direct bridge from BTP and D-BTP and a useful testbed for No-subtree semantics.

### 9.2 School-Bus Scheduling and Routing

Deltas assign students to routes, alter stops, reallocate vehicles, or modify schedules. The objective may balance fleet size, ride duration, capacity, and fairness. Local solutions may conflict over shared vehicles, making global reconciliation essential.

### 9.3 Multi-Scale Peak Exploration

A metaphorical population of search workers explores local neighborhoods and distant regions using different Delta generators. It illustrates the distinction between **generation reachability** and **selection quality**: a perfect evaluator cannot select an optimum that no generator can reach.

### 9.4 Metric Discovery

Deltas alter the features, weights, or structure of an evaluation metric. Independent validation assesses whether the changed metric improves the intended downstream outcome rather than merely increasing its own score.

### 9.5 Distributed Method Relay

Heterogeneous workers contribute candidate solutions, counterexamples, and reusable operators. Common contracts permit exchange, while conformance and empirical tests determine whether a method transfers successfully.

## 10. Relationship to Existing Research Traditions

CDE overlaps substantially with established ideas in transition systems, graph search, constraint satisfaction, mathematical optimization, automated planning, hyper-heuristics, black-box optimization, population-based search, and distributed computation. The general ideas of states, transitions, branching, feedback, and meta-level adaptation are not claimed as new.

The research contribution proposed here is the **explicit, modular integration** of:

1. generalized state and Delta contracts;
2. adaptive M-to-N branching with traceable transition semantics;
3. separate path-decision, state-revisit, and termination policies;
4. heterogeneous solver interfaces;
5. metric and generator evolution under independent validation;
6. distributed exploration and evidence/method relay;
7. exploratory localization within a broader structural intelligence architecture.

Whether this integration yields practical benefits remains an empirical question. A literature review and head-to-head comparisons are necessary before making stronger novelty or performance claims.

## 11. Research Questions and Testable Hypotheses

**RQ1 — Expressiveness.** Which constrained problem families can be encoded without losing essential transition semantics? What representation overhead does CDE introduce?

**RQ2 — Branching.** Under what conditions does adaptive M-to-N Delta generation improve solution quality per unit of computation over fixed branching or established baselines?

**RQ3 — Correctness.** Which combinations of decision memory, state equivalence, revisitation policy, and admissible pruning preserve soundness, completeness, or termination?

**RQ4 — Metric evolution.** Can externally validated metric Deltas improve downstream decisions without overfitting or metric gaming?

**RQ5 — Distributed orchestration.** When does distributed task decomposition provide net speedup after communication, reconciliation, and validation costs?

**RQ6 — Method relay.** Can validated Delta generators or search methods transfer across domains better than independently tuned baselines?

**RQ7 — SIF localization.** Does exploratory CDE localization improve coverage or discovery of non-indexed targets relative to retrieval and conventional graph-search alternatives?

Candidate hypotheses for later preregistered tests:

- **H1:** On selected constrained benchmarks, adaptive branching improves best feasible objective per fixed budget relative to a specified fixed-branching baseline.
- **H2:** Explicit No-decision memory reduces duplicate constructive search without removing valid solutions under permanent-exclusion semantics.
- **H3:** Validated method relay improves median time-to-target on a specified family of related tasks after accounting for transfer overhead.
- **H4:** Metric evolution improves held-out decision utility relative to fixed-metric and standard metric-learning baselines.

These are hypotheses, not established results.

## 12. Boundaries, Failure Modes, and Non-Claims

CDE does **not** guarantee:

- that every problem has a tractable Delta representation;
- that a generated search graph is finite or converges;
- that heuristic pruning preserves optimality or completeness;
- that adaptive generators can reach previously unreachable optima;
- that distributed workers produce linear speedup;
- that independently optimized local solutions can be merged cheaply;
- that evolving a metric necessarily makes it more valid;
- that recursive feedback produces reliable self-improvement.

Key failure modes include unsound state canonicalization, inconsistent constraint versions, cyclic exploration without progress controls, metric self-confirmation, biased candidate generation, unsafe bound sharing, and relay artifacts lacking provenance.

The framework should be judged by the quality of its contracts, its ability to expose these failure modes, and measurable results on controlled tasks—not by breadth of terminology alone.

## 13. Roadmap and Companion Documents

This paper establishes the opening position of the CDE research series. The planned foundations continue with:

- **CDE-002:** State Containers and Constraint-Governed Transitions.
- **CDE-003:** Delta Semantics and Adaptive M-to-N Branching.
- **CDE-004:** Path Memory, No-Subtree Semantics, and Loop-Back Control.
- **CDE-005:** Search, Dynamic Programming, and Branch-and-Bound Integration.
- **CDE-006:** Directional Fields and Multi-Scale Delta Generation.
- **CDE-007:** Solution-Space and Metric-Space Co-Evolution.
- **CDE-008:** CDE as a General Exploration and Optimization Orchestrator.
- **CDE-009:** Distributed CDE and Delta Exploration Contracts.
- **CDE-010:** Structural Relay and Collective Learning.
- **CDE-011:** Recursive Generator and Policy Evolution.
- **CDE-012:** Generalization Boundaries, Guarantees, and Open Research.

Separate specifications will define state, Delta, path-memory, solver, distributed-relay, and metric-evolution contracts. Cases and validation documents will provide falsifiable examples and benchmark plans.

## 14. Conclusion

Constrained Delta Evolution emerges from a simple but consequential generalization: a constrained computational process can be described through meaningful states, candidate transformations, valid transitions, and explicit exploration policies without requiring bucket-shaped representations or binary decision trees. This abstraction accommodates adaptive branching and, under additional contracts, state merging, revisitation, solver interoperability, metric evolution, distributed task execution, and structural relay.

The most promising long-term role of CDE may be as a **common language and coordination layer** through which domain experts contribute problems, solver methods, evaluation mechanisms, and validated evidence. Its potential contribution to SIF Localization is particularly relevant where a target must be discovered through exploration rather than retrieved from a known index.

The immediate priority is not to claim universal optimization capability. It is to formalize the smallest useful core, establish conformance conditions, and test whether the proposed modular integration produces reliable, transferable advantages.

---

## Suggested Figures

- `figures/Fig-001-CDE-Grand-Architecture.png` — insert after Section 2 or at the beginning of Section 3.
- `figures/Fig-002-From-BTP-and-D-BTP-to-CDE.png` — insert after the lineage table in Section 2.
- `figures/Fig-004-Adaptive-M-to-N-Delta-Branching.png` — insert after Section 3.2.
- `figures/Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png` — insert after Section 7.

*Figure paths are placeholders until the corresponding assets are created.*

## Research Context and Attribution

This document consolidates the CDE conceptual discussion of November 4, 2026 and extends the earlier BTP/D-BTP and Structural Intelligence research program. For related work, consult the associated research repositories and subsequent CDE bibliography as it is developed. This draft does not assert priority over pre-existing transition-system, optimization, or distributed-search literature.

**Related repository:** [Dynamic Bucket Tree of Permutation and Directional Structural Evolution](https://github.com/sizhet/Dynamic-Bucket-Tree-of-Permutation-and-Directional-Structural-Evolution).
