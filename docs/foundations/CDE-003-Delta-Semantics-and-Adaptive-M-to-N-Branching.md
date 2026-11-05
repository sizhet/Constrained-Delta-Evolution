# CDE-003 — Delta Semantics and Adaptive M-to-N Branching

**Series:** Constrained Delta Evolution (CDE) — Foundations  
**Status:** Foundational research proposal, v1.0.0 draft  
**Date:** 2026-11-04  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Companions:** [CDE-001 — From BTP and D-BTP to Constrained Delta Evolution](CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md); [CDE-002 — State Containers and Constraint-Governed Transitions](CDE-002-State-Containers-and-Constraint-Governed-Transitions.md)

## Abstract

Constrained Delta Evolution (CDE) generalizes a binary, decision-oriented branching structure into **adaptive M-to-N branching** over typed, constraint-governed state transformations. A state may produce M candidate Deltas, but filtering, validation, expansion budgets, nondeterministic effects, and state coalescence mean that the number N of realized outgoing transition edges need not equal M. Each realized edge retains its originating Delta and sufficient transition provenance; path-level decision memory is distinct from the edge record. This paper specifies Delta identity, candidate generation, eligibility, acceptance and exclusion, branch semantics, adaptive fan-out, multi-scale exploration, and the relationship between trees, directed acyclic graphs, and cyclic state graphs. It defines conditions under which deduplication, pruning, and merging are sound, and outlines contracts for distributed exploration and structural relay. The proposal is a unifying framework, not a claim that arbitrary branching ensures termination, completeness, or better optimization performance.

**Keywords:** Constrained Delta Evolution; Delta semantics; adaptive branching; M-to-N expansion; candidate generation; constraint propagation; path memory; state graphs; search policy; structural localization; distributed exploration

## 1. Motivation: The Branching Structure Is Not the Problem

The original Bucket Tree of Permutation (BTP) made an important computational choice explicit: a candidate assignment or structural decision could be accepted (Yes) or excluded (No). Dynamic BTP (D-BTP) enlarged the space of possible decisions through dynamically generated, directionally informed transformations. Yet neither a bucket nor a binary node is required by the more general computation. In a scheduling system, one state may offer dozens of reassignment moves. In scientific discovery, one hypothesis may suggest many distinct experiments. In metric learning, one metric may admit alternative feature additions, weight updates, and representation changes. In distributed search, different workers may explore different parts of the same state space using heterogeneous methods.

CDE separates three concerns:

1. **What changes are representable?** This is the Delta language and its transition semantics.
2. **What changes are admissible?** This is governed by hard constraints, contextual validity, and decision history.
3. **Which changes should be explored now?** This is governed by a policy, evidence, budgets, and available computational resources.

The distinction is essential. An unexpanded candidate is not necessarily impossible, and a low-priority candidate is not necessarily invalid. CDE allows a node to expose many candidate Deltas while a policy realizes only a selected subset as outgoing branches.

## 2. Formal Context

Following CDE-002, let a problem instance be

\[
\mathcal P=(\mathcal S,\mathcal D,\mathcal K,G,T,I,\mathcal O),
\]

where \(\mathcal S\) is a space of state representations, \(\mathcal D\) is a universe of Delta descriptions, \(\mathcal K\) contains scoped constraints, \(G(s,c)\) proposes candidate Deltas, \(T\subseteq\mathcal S\times\mathcal D\times\mathcal S\) is a transition relation, \(I\) is the initial-state set, and \(\mathcal O\) provides optional objectives and evaluation protocols.

A **search configuration** should be distinguished from a domain state:

\[
q=(s,h,c,v,b),
\]

where \(s\) is the domain state, \(h\) is relevant path or decision history, \(c\) is the constraint and observation context, \(v\) identifies applicable schema/policy versions, and \(b\) records any budget or resource information that can change future behavior. These fields are conceptual: implementations may store references, hashes, immutable snapshots, or compact sufficient statistics. Fields that do not affect future legality, reward, or behavior need not be included in the equivalence key.

A transition edge is represented as

\[
e=(q_i,\delta,a,q_j,\pi),
\]

where \(\delta\) is the originating Delta, \(a\) is its transition or decision semantics, and \(\pi\) is provenance (e.g., generator identity, validation evidence, source state/version, and execution result). For ordinary applied transformations, \(a=\mathrm{Apply}\). For constructive Yes/No decision trees, \(a\) can denote \(\mathrm{Include}\) or \(\mathrm{Exclude}\), with explicit rules for what each decision changes. A deferred candidate ordinarily does not create a state-transition edge.

## 3. What Is a Delta?

A Delta is a **typed proposal for change**, not merely a numerical difference. It may be an assignment, an exclusion decision, a graph rewrite, a parameter update, a schedule move, a measurement proposal, or a new search strategy. Its meaning is established by a contract, not by its name.

A practical Delta descriptor can include:

| Field | Role | Requirement |
|---|---|---|
| `delta_type` | Identifies the operator family | Required |
| `payload` | Typed parameters for the proposed change | Required |
| `preconditions` | Conditions for applying or deciding the Delta | Required by contract, possibly empty |
| `effect_semantics` | Deterministic or relational outcome specification | Required |
| `scope` | State/context components that the Delta may read or modify | Required for safe composition |
| `identity_key` | Canonical comparison under a declared identity policy | Required when deduplicating |
| `provenance` | Generator, evidence, and derivation | Recommended; required for audited relay |
| `version` | Operator/schema semantics | Required when semantics may evolve |

The descriptor should not conflate a **Delta proposal** with its **execution event**. The same operator and payload may be proposed many times, but an execution event additionally has a particular source state, outcome, timestamp or sequence number, and evidence. A nondeterministic operator may produce several legitimate outcomes from one proposal.

### 3.1 Three distinct identities

**Syntactic identity:** two descriptors serialize to the same canonical representation under the same schema.  
**Semantic identity:** two descriptors have equivalent effects under the relevant state/context and equivalence relation.  
**Decision identity:** two proposals correspond to the same question already resolved on a constructive search path.

These identities can disagree. `Swap(A,B)` and `Swap(B,A)` may be semantically equivalent in a particular representation while syntactically distinct. `Assign(A,Job1)` in one time window may differ from the same payload in another window. An operator version bump does not by itself justify treating an already decided logical question as new.

Define a contextual key, when needed, as

\[
K_\delta=\operatorname{Canonicalize}(\delta,\operatorname{RelevantContext}(q),\nu),
\]

where \(\nu\) is the **semantic identity policy**, not simply the latest software revision. A safe key must retain any context needed to distinguish future-relevant decisions. Overly coarse keys risk suppressing valid exploration; overly fine keys weaken deduplication and may permit repeated equivalent work.

## 4. From M Candidates to N Realized Branches

Let

\[
D_t=G(q_t)=\{\delta_1,\ldots,\delta_M\}
\]

be the finite candidate batch generated at a particular expansion step. The generator may represent a bounded sample from a much larger or even infinite Delta universe; thus \(M\) refers to the **materialized batch**, not necessarily all possible Deltas.

A generic expansion pipeline is:

\[
D_t\xrightarrow{\text{normalize/deduplicate}}D_t^0
\xrightarrow{\text{eligibility}}D_t^1
\xrightarrow{\text{selection/budget}}D_t^2
\xrightarrow{\text{transition}}E_t.
\]

The resulting edge multiset is

\[
E_t=\{(q_t,\delta,a,q',\pi):\delta\in D_t^2,\; (q_t,\delta,a,q')\text{ is valid}\}.
\]

Let \(N=|E_t|\). **There is no universal requirement that \(N=M\), or even \(N\le M\)**: one nondeterministic Delta can yield multiple successor edges. In the special case of deterministic, one-successor-per-selected-Delta execution, \(N\le M\) after filtering, assuming no additional branches are introduced. If equivalent successor states are merged, the number of distinct successor nodes can be smaller still:

\[
N_{\mathrm{nodes}}=|\{[q']_\sim:(q,\delta,a,q',\pi)\in E_t\}|\le N.
\]

The distinction between **candidate count**, **transition-edge count**, and **unique successor-node count** prevents an otherwise subtle ambiguity in “M-to-N branching.”

### 4.1 Illustrative example

A routing state generates eight candidate Deltas: three route swaps, two reassignment moves, one vehicle addition, one time-window adjustment, and one redundant swap. After canonicalization, seven remain. Two violate hard capacity or time constraints. A budget allows three of the five eligible candidates to be executed now. Under deterministic transition semantics, the expansion produces three outgoing edges. The two unselected eligible candidates are **postponed**, not proven infeasible. If the generator or transition is nondeterministic, edge counts must be reported separately.

### 4.2 A branch remembers its Delta

A minimal reproducible branch record includes the parent configuration reference, Delta descriptor and identity policy, decision/action semantics, successor reference or outcome, and provenance. The Delta should logically label the **edge**. A node may have multiple incoming edges with different Deltas, particularly after state merging. Storing only a single “incoming Delta” on a shared node would erase valid history.

## 5. Candidate Lifecycle and Decision Semantics

A useful lifecycle is:

`Proposed → Normalized → Checked → Eligible → Selected → Executed → Validated → Recorded`.

Alternative terminal or suspended outcomes include `Duplicate`, `Hard-Infeasible`, `Excluded`, `Deferred`, `Execution-Failed`, and `Superseded`. These labels should not be used interchangeably.

- **Hard-infeasible** means a declared hard constraint rejects the candidate under a specified context and sound checker.
- **Excluded** means the current search branch commits to a negative decision, when the problem's decision semantics allow such a branch.
- **Deferred** means exploration has been postponed; it does not imply permanent exclusion.
- **Low-scoring** means a heuristic or metric ranks the candidate poorly; it is not a proof of infeasibility.
- **Superseded** means the candidate has been replaced or made irrelevant under a documented policy; any completeness consequence must be stated.

The original BTP Yes/No mechanism is a **specialized decision semantics**, not the only possible CDE branch form. A constructive decision `Include(δ)` and `Exclude(δ)` may both create meaningful child configurations, whereas a reconfiguration solver might generate only applied moves and never create explicit No branches.

## 6. Path-Local Memory: Yes and No Both Count

For a fixed-context constructive decision problem, let \(A_t\) be accepted decisions and \(R_t\) be excluded decisions on the current path. Define

\[
U_t=A_t\cup R_t.
\]

A fresh binary decision with canonical key \(k\) requires \(k\notin U_t\); either branch then adds \(k\) to \(U_{t+1}\). This prevents an already rejected question from reappearing later on the same path, just as an accepted question should not reappear.

This rule is **not universal CDE law**. A reconfiguration problem may legitimately revisit the same move after other moves change the context; a cyclic controller may repeat actions; and an evidence-driven search may reopen an earlier decision when its semantics explicitly permit revision. Each solver must declare its own path-memory and redecision policy.

Also distinguish **decision uniqueness** from resource uniqueness. Preventing `Assign(A,Job1)` from being decided twice does not automatically prevent `Assign(A,Job2)` when actor A is already occupied. The latter is a separate assignment or capacity constraint.

CDE-004 develops No-subtree semantics, loop-back, and the conditions under which these policies guarantee termination.

## 7. Tree, DAG, and Cyclic State Graph Semantics

The expansion process can be **unfolded** as a tree of search histories, even when the underlying state space is a graph. But practical implementations may merge equivalent configurations into a directed acyclic graph or permit cycles.

**Tree:** each search-history node has one incoming edge; easy to retain path-local decision memory.  
**DAG:** multiple paths share a successor only when their future-relevant configurations are equivalent.  
**Cyclic graph:** repeated states or reconfiguration loops are permitted; loop-control and termination policies become explicit.

For state merging, equality of domain payload alone is generally insufficient. A sound equivalence \(q_i\sim q_j\) must preserve whatever is relevant to future admissible actions, transition outcomes, objective accumulation, and required outputs. A safe merge can be defined through an appropriate continuation equivalence or solver-specific dominance relation. When path constraints or accumulated costs matter, their sufficient statistics must be retained.

**Important:** the graph edge can retain its Delta even if its successor node is shared. Conversely, a merged node may need multiple predecessor/provenance records. Deduplicating states is not equivalent to deduplicating Deltas.

## 8. Adaptive Branching Policy

Let the selection policy depend on state, evidence, direction, available resources, and current search objectives:

\[
D_t^2=\Pi(q_t,D_t^1,\mathcal E_t,\mathcal R_t,\mathcal O_t).
\]

The branch budget may be adaptive:

\[
B_t=\operatorname{BudgetPolicy}(q_t,\mathcal E_t,\mathcal R_t),
\qquad |D_t^2|\le B_t.
\]

A policy might allocate more branches near promising regions, reserve a fraction for long-range exploration, or spend additional effort where uncertainty is high. The policy should distinguish:

1. **Feasibility screening:** reject candidates shown to violate hard constraints.
2. **Sound pruning:** discard candidates only when a solver-specific proof shows they cannot contribute to the required solution set or improve the incumbent.
3. **Heuristic prioritization:** rank or schedule candidates using uncertain scores.
4. **Budget truncation:** explore only a subset under finite resources, potentially sacrificing completeness.

Adaptive branching can improve resource allocation but cannot create a guarantee of global optimality by itself. In particular, a fixed beam width may permanently exclude the path to the optimum.

### 8.1 A minimal algorithm sketch

```text
EXPAND(q, generator, constraints, policy, budget):
    candidates = generator.propose(q, budget.candidate_limit)
    normalized = canonicalize_and_deduplicate(candidates, q.identity_policy)
    eligible = []
    for delta in normalized:
        result = constraints.check_preconditions(q, delta)
        if result == PROVEN_INFEASIBLE:
            record_rejection(q, delta, result)
        elif result == ELIGIBLE:
            eligible.append(delta)
        else:
            retain_for_review(q, delta, result)  # unknown is not infeasible
    selected = policy.select(q, eligible, budget.branch_limit)
    edges = []
    for delta in selected:
        for outcome in transition_outcomes(q, delta):
            if constraints.validate_successor(q, delta, outcome):
                edge = record_edge(q, delta, outcome, provenance=True)
                edges.append(edge)
    return edges
```

This pseudocode is a **contract illustration**, not a complete solver. It omits concurrency, cost propagation, exceptions, and explicit constructive No branches; those must be added by the relevant solver profile.

## 9. Multi-Scale Delta Generators

A single generator need not monopolize exploration. Let a portfolio of typed generators be

\[
\mathbb G=\{G_{\mathrm{local}},G_{\mathrm{regional}},G_{\mathrm{long-range}},G_{\mathrm{relay}}\}.
\]

Local operators refine a nearby solution; regional operators alter larger components; long-range operators jump between distant representations or basins; relay operators use discoveries transferred from other workers or domains. A generator portfolio policy chooses which generators receive resources:

\[
\alpha_t=\operatorname{Allocate}(q_t,\mathcal E_t,\mathcal R_t),
\qquad \sum_k\alpha_{t,k}\le1.
\]

The “bee swarms searching for mountain peaks” example illustrates exploration/exploitation, but CDE is not limited to spatial search, scalar maxima, or bee-inspired algorithms. In many applications the object being improved is a **metric**, an evaluation procedure, a constraint representation, or an exploration strategy. Long-range Delta generation expands the *potentially reachable* region only if it actually introduces valid transitions outside the previous generator's support.

### 9.1 Generator evolution

Feedback may update a generator's parameters or structure:

\[
G_{t+1}=U(G_t,F_t).
\]

Here \(F_t\) should include externally checked evidence when available, not only self-reported scores. The updated generator must retain a versioned contract. Improvements in training performance do not imply generalization; independent validation and rollback should be specified. CDE-006 and CDE-011 study these questions in depth.

## 10. Directional Fields and Exploratory Localization

Within the Structural Intelligence Framework (SIF), localization can mean identifying relevant structures, capabilities, candidate solutions, or regions worth exploring. CDE offers a complementary **exploratory localization** mechanism: rather than merely addressing an already indexed target, it can generate and test successive Deltas that approach or discover useful target regions.

A directional score can guide candidate ordering:

\[
\rho(q,\delta\mid p)=\operatorname{DirectionalScore}(q,\delta,p),
\]

where \(p\) is an explicitly declared perspective or objective. Directional scoring is not necessarily a distance metric, a feasibility proof, or a reliable gradient. It can be uncertain, misleading, and context-dependent. An exploratory localization system should retain uncertainty estimates, counterevidence, and fallback exploration to avoid confusing directional preference with proven reachability.

**Research proposition:** CDE may serve as an additional localization pillar within SIF by providing a constrained, Delta-driven mechanism for searching beyond pre-existing addresses. The scope, comparative effectiveness, and integration costs of this proposition require experimental validation.

## 11. Distributed M-to-N Expansion

An outgoing branch can become a distributable exploration contract:

\[
W_i=(q_t,\delta_i,\mathcal K_i,\Pi_i,B_i,V_i),
\]

where \(\mathcal K_i\) specifies the relevant constraints, \(\Pi_i\) the local search policy, \(B_i\) a resource budget, and \(V_i\) the validation and versioning rules. Workers may run different algorithms while returning results under a common evidence and transition contract.

Distribution creates obligations beyond ordinary branching:

- **Snapshot consistency:** workers must know which state and constraint versions they received.
- **Idempotent task handling:** retries must not silently duplicate side effects.
- **Result provenance:** outputs must identify the originating branch and applicable evaluation rules.
- **Global constraint reconciliation:** independently feasible local solutions may conflict when combined.
- **Stale-result policy:** outdated results may be useful evidence even when no longer executable.
- **Termination detection:** the controller must account for queued, running, deferred, and recursively spawned tasks.

For independent peak-search tasks, aggregation may be as simple as selecting the best validated result under one fixed metric. For coupled scheduling or assignment, reconciling locally optimal results can itself be a hard optimization problem. Thus **task dispatch is often straightforward; correct global result merging is problem-dependent**.

## 12. Delta Exploration and Structural Relay

The branch label is a natural carrier of structural provenance, but it is not the entire relay artifact. A useful relay record includes:

\[
R=(\mathrm{TaskID},\mathrm{StateRef},\delta,\mathrm{Outcome},\mathrm{MetricRef},
\mathrm{Evidence},\mathrm{MethodRef},\mathrm{Versions}).
\]

Relay can transfer at least four distinct products:

- **Result Relay:** candidate solutions, scores, and feasibility certificates.
- **Evidence Relay:** observations, counterexamples, failure modes, and uncertainty.
- **Method Relay:** reusable Delta generators, pruning rules, and search procedures.
- **Policy Relay:** validated resource-allocation and exploration strategies.

A recipient should verify contract compatibility before reuse. A method that works under one objective, constraint set, or domain representation may not transfer to another without adaptation. Collective Learning is a hypothesis about *validated reusable transfer*, not a guarantee that pooled information improves performance.

## 13. Interaction with Search, DP, and Branch-and-Bound

CDE's M-to-N expansion is compatible with multiple solver families, but does not replace their correctness conditions.

| Solver family | Use of Delta branches | Additional obligations |
|---|---|---|
| Backtracking search | Enumerate admissible choices or Yes/No decisions | Decision coverage; duplicate handling; termination |
| Dynamic programming | Generate transitions among equivalent subproblems | Sound state equivalence; recurrence; cost/history sufficiency |
| Branch-and-Bound | Expand promising candidates and prune by bounds | Valid bounds; incumbent consistency; admissible pruning |
| Beam or heuristic search | Select a bounded subset of candidate branches | State clearly when completeness is lost |
| Local search | Apply neighborhood Deltas, possibly revisiting states | Cycling policy; stopping rule; neighborhood coverage |
| Population search | Explore many states and generator portfolios | Diversity, evaluation comparability, resource allocation |

For a minimization problem, a branch may be safely pruned against incumbent \(U\) only if a sound lower bound \(L(q)\) satisfies \(L(q)\ge U\), subject to the objective and constraint assumptions. A directional score or predicted quality estimate is not automatically such a bound.

## 14. Metric Evolution as a Special Delta Domain

A metric definition can itself be a state component. Let \(m\in\mathcal M\) be a versioned metric, and define metric Deltas such as `AddFeature`, `RemoveFeature`, `AdjustWeight`, `ChangeAggregation`, and `ReplaceMetric`:

\[
m_{t+1}=T_m(m_t,\delta_m).
\]

A CDE orchestrator may therefore explore both solutions \(x\in\mathcal X\) and metrics \(m\in\mathcal M\). The joint configuration can be represented as \(q=(x,m,\ldots)\), with separate constraints and validation protocols for each component. This does **not** mean scores computed under different metrics are automatically comparable. A candidate result should carry its metric identity, metric version, validation set, and external evaluation criteria. Optimizing a metric against its own score invites circular validation and reward hacking.

CDE-007 develops solution–metric co-evolution and the requirements for meaningful cross-metric comparison.

## 15. Guarantees, Non-Guarantees, and Failure Modes

### 15.1 Termination under a restricted constructive profile

If (i) there is a finite set of \(K\) canonical decision keys, (ii) each path commits to a previously undecided key at each step, (iii) no step removes a committed key, and (iv) expansion is finitely branching, then every decision path has depth at most \(K\). If the search exhaustively expands this finite tree, it terminates. This result is **profile-specific**, not a property of all CDE systems.

### 15.2 Reachability and completeness

A solution may be expressible in \(\mathcal S\) but unreachable under the available generator support and transition relation. Completeness additionally depends on fair or exhaustive exploration, sound pruning, and appropriate treatment of cycles. Adaptive policies may improve practical coverage without guaranteeing it.

### 15.3 Common failure modes

1. **False deduplication:** canonical keys collapse contextually different Deltas.
2. **False exclusion:** a deferred or low-scoring candidate is treated as permanently impossible.
3. **Unsound state merging:** histories with different future admissibility are merged.
4. **Branch explosion:** M grows faster than resources can support.
5. **Generator collapse:** feedback drives all workers toward the same misleading region.
6. **Looping without progress:** repeated states or reconfigurations consume resources indefinitely.
7. **Inconsistent distributed evaluation:** results computed under different metric or constraint versions are ranked together.
8. **Provenance loss:** merged nodes retain states but lose the Deltas and evidence that produced them.
9. **Incorrect pruning:** heuristic scores are mistaken for certified bounds.
10. **Overclaiming:** a flexible modeling language is presented as a universal performance breakthrough.

Each failure mode should motivate a corresponding conformance test or counterexample.

## 16. Testable Hypotheses and Validation Design

The following are **hypotheses**, not established empirical findings.

| ID | Hypothesis | Proposed test | Primary measurements |
|---|---|---|---|
| H-003-01 | Adaptive M-to-N expansion can improve solution quality per compute budget in selected problem families | Compare fixed branching, adaptive branching, and exhaustive baselines | Quality, wall time, evaluations, memory |
| H-003-02 | Context-aware Delta identity reduces redundant exploration without excluding valid solutions | Inject equivalent and near-equivalent operators across contexts | Duplicate rate, false suppression, coverage |
| H-003-03 | Separating hard rejection from heuristic deferral improves auditability and search correctness | Controlled search traces with ambiguous candidates | Incorrect exclusions, recovered solutions |
| H-003-04 | Multi-scale generators improve coverage on selected multimodal landscapes | Local-only versus mixed local/long-range operators | Coverage, best score, diversity, variance |
| H-003-05 | Delta-labeled edges preserve useful provenance under sound state merging | DAG and cyclic-graph benchmarks | Reconstruction fidelity, merge correctness |
| H-003-06 | Distributed branch contracts enable heterogeneous workers to exchange reusable methods | Compare isolated and relay-enabled workers | Transfer success, overhead, net performance |
| H-003-07 | Exploratory localization helps discover useful structures not directly addressed by a static index | SIF-inspired localization tasks with hidden targets | Recall, localization cost, false discoveries |

**Baseline discipline:** hold compute budgets, evaluation protocols, random seeds where applicable, and hardware conditions as comparable as possible. Report negative results, uncertainty intervals, and sensitivity to problem families. Compare against established constraint-programming, graph-search, planning, and metaheuristic methods rather than only against a deliberately weak baseline.

### 16.1 Minimal conformance examples

**Test A — binary constructive decision:** accepting or excluding a canonical assignment key marks it decided on that path; proposing the same question again is rejected by that profile's path-memory rule.

**Test B — M-to-N filtering:** six generated candidates, one syntactic duplicate, one certified infeasible, two selected by budget, produce two deterministic outgoing edges; the unselected eligible candidates remain deferred.

**Test C — nondeterministic transition:** one selected Delta has two valid outcomes; the graph records two outgoing edges with the same Delta descriptor and distinct outcomes.

**Test D — shared successor:** two different Delta sequences lead to continuation-equivalent configurations; the DAG merges the successor while retaining both incoming edge labels and provenance.

**Test E — context-dependent identity:** a Delta excluded in one fixed context may be reconsidered only under an explicitly allowed context-change/redecision policy.

**Test F — metric versioning:** two solution scores produced by incompatible metric versions cannot be directly ranked without a declared comparison protocol.

**Test G — distributed retry:** replaying the same task ID does not duplicate an external side effect, and the result retains the source snapshot and constraint version.

## 17. Open Questions

1. What is the smallest Delta identity contract sufficient for safe deduplication across heterogeneous solvers?
2. Which classes of context-dependent decisions admit compact path-memory representations?
3. How can a generator portfolio maintain exploration coverage under changing directional scores?
4. When does state merging preserve all future-relevant evidence and decision constraints?
5. How should infinite or continuously parameterized Delta spaces be sampled fairly?
6. Can learned feasibility classifiers be integrated without silently compromising completeness?
7. How can independent metric validation guide generator evolution without circular scoring?
8. What are the practical limits of asynchronous distributed M-to-N branching under globally coupled constraints?
9. Which SIF localization tasks benefit from Delta-driven discovery compared with direct indexing, retrieval, or graph search?
10. How should reusable Method Relay artifacts specify preconditions, transfer limits, and evidence quality?

## 18. Relationship to the CDE Series

- **CDE-001:** BTP → D-BTP → CDE lineage and research architecture.
- **CDE-002:** state containers, constraint scope, and transition semantics.
- **CDE-003 (this paper):** Delta identity, lifecycle, and adaptive M-to-N branching.
- **CDE-004:** detailed No-subtree, path memory, loop-back, and termination.
- **CDE-005:** Search, DP, and Branch-and-Bound solver integration.
- **CDE-006:** directional fields, multi-scale generation, and SIF exploratory localization.
- **CDE-007:** solution and metric co-evolution.
- **CDE-008–011:** orchestration, distributed exploration, structural relay, and controlled RSI.
- **CDE-012:** guarantees, limitations, counterexamples, and open research.

The planned **SPEC-002 — Delta Generation and Branch Contract** should convert the semantics here into machine-checkable interface requirements. **SPEC-003** should specify path memory and loop-control policies, while **SPEC-005** should govern distributed exploration and relay.

## 19. Conclusion

Adaptive M-to-N branching replaces an unnecessarily rigid binary-tree assumption with a more general and auditable mechanism: generate typed candidate Deltas, determine their admissibility, select branches under declared policies, execute transitions, and preserve Delta-labeled provenance. The number of candidates, realized edges, and unique successor states must be distinguished. Constructive Yes/No decisions, state reconfiguration, nondeterministic transitions, and cyclic exploration can all be represented, but each requires its own correctness and termination contracts. This separation makes CDE a candidate common language for structural search, optimization, metric discovery, exploratory localization, and distributed collective learning. Its value must ultimately be demonstrated through precise conformance tests and comparative experiments.

---

### Research and citation note

This document is a foundational research proposal intended for critical review, reproducible experiments, and collective learning. It does not claim that the generic state-transition model, multiway branching, graph search, adaptive search, or distributed optimization are newly invented. Potential contributions lie in the coherent contracts, explicit separation of decision/transition identities, cross-solver interoperability, and testable structural relay mechanisms. Add the repository DOI and finalized citation metadata only after publication.
