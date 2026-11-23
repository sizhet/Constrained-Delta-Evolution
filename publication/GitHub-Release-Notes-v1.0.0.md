# Constrained Delta Evolution (CDE) — v1.0.0

**Initial Research Framework Release — Proposed**

**Full title:** *Constrained Delta Evolution (CDE): A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence*  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Release tag:** `v1.0.0` (to be created upon publication)  
**Publication date:** To be confirmed at release  
**DOI:** Pending; do not insert an unassigned identifier  
**Release type:** Research papers, specifications, illustrative cases, and validation plans; **not** a validated software release

> **Publication note:** This file is the proposed text for a future GitHub Release. Its presence in the repository does not mean that the tag has been created, the release has been published, or a DOI has been assigned. Replace the pending fields only after those events are confirmed.

## Overview

Constrained Delta Evolution (CDE) is a **specifications-first research framework** for generating, evaluating, constraining, and evolving transitions across structured state spaces. It proposes a common *State–Delta–Constraint* abstraction that can host different search strategies while retaining explicit path semantics, validation boundaries, and provenance.

CDE develops ideas originating in **Bucket Tree of Permutation (BTP)** and **Dynamic Bucket Tree of Permutation (D-BTP)**. Rather than treating a fixed bucket-tree structure as mandatory, it allows a state to be any appropriately specified logical container and a Delta to be a typed, constrained proposal for changing that state. The resulting exploration may take the form of a tree, a directed acyclic graph, or a more general graph, depending on state equivalence and revisit rules.

The framework studies adaptive branching, directional and multi-scale Delta generation, integration with existing optimization methods, co-evolution of solution and metric spaces, distributed exploration, typed Structural Relay, and controlled recursive evolution of generators and policies.

**Research position:** CDE is a proposed organizing architecture and a set of testable mechanisms. This release does **not** establish that CDE universally outperforms existing solvers, guarantees convergence, discovers better metrics autonomously, or achieves recursive self-improvement or AGI.

## 1. Principal research contributions

### 1.1 State–Delta–Constraint as a reusable exploration contract

A CDE task can be described abstractly by

$$
\mathcal{P}=(S,\Delta,T,C,G,L), \qquad S_{t+1}=T(S_t,\delta_t), \quad \delta_t\in G(S_t).
$$

where the meaning and validity of the state, Delta, transition, constraints, generation mechanism, and path/learning context must be specified for the task. A proposed successor is not automatically a committed valid state: hard constraints, policy versions, and applicable validation requirements govern acceptance.

### 1.2 Adaptive M-to-N Delta branching

CDE separates the generation of candidate Deltas from their execution and the resulting graph topology. In particular, the framework distinguishes:

| Symbol | Meaning |
|---|---|
| **M** | Delta proposals generated |
| **Q** | Proposals selected for an attempted transition |
| **N** | Realized transition edges, including outcomes not committed to the active frontier where applicable |
| **Nc** | Committed successor edges |
| **K** | Distinct successor states after a valid equivalence test |

These counts are not interchangeable. Multiple Deltas can lead to the same state; rejected proposals and failed attempts do not imply committed successors. This distinction supports auditable branching statistics and fair comparisons.

### 1.3 Path memory, exclusion, and loop control

The research distinguishes *decision identity*, *Delta identity*, *state identity*, and *future equivalence*. Fixed-context constructive **NoSubtree** exclusions, deferred candidates, infeasible transitions, and proven pruning are different dispositions. A no-repeat decision policy does **not** by itself prove that a general state graph has no cycles. Loop Back is permitted only under explicitly specified revisit and progress policies.

### 1.4 Integration with established solver families

CDE is designed to orchestrate or complement general search, dynamic programming, branch-and-bound, local and population-based methods, and appropriate external optimization solvers. It does **not** replace the assumptions needed for their guarantees: dynamic programming requires sound future-equivalence and reuse conditions; branch-and-bound requires valid bounds; claims of completeness and optimality depend on the actual search space, transitions, pruning, and termination conditions.

### 1.5 Directional, multi-scale, and representation-aware exploration

The framework proposes local, regional, long-range, and Relay-informed Delta generation, guided by task-specific directional evidence and exploration policies. Directional scores are heuristics or evidence-bearing estimates unless separately proved. They cannot substitute for hard feasibility checks.

### 1.6 Solution–metric co-evolution with independent governance

CDE treats solution changes and metric changes as distinct typed operations. Metrics, constraints, and policies are versioned; a candidate metric improvement must be assessed against independent validation criteria, not merely the same objective it was optimized to satisfy. Cross-version comparisons, Goodhart-style failures, and rollback are explicit research concerns.

### 1.7 Distributed exploration and typed Structural Relay

A logical CDE Orchestrator can distribute bounded exploration tasks to multiple workers and reconcile results under shared contracts. Proposed Relay artifacts include **Result, Evidence, Constraint, Method, Policy, Metric, and Leftover**. Transfer requires provenance, applicability checks, version compatibility, and target-domain validation. Local feasibility does not automatically imply global feasibility, and a worker report is not itself a proof.

### 1.8 Controlled recursive generator and policy evolution

CDE explores changes not only to candidate solutions but also to generators, representations, evaluation policies, and orchestration methods. The proposed governance model separates **candidate proposal**, **measurement**, **independent validation**, and **authorized promotion or rollback**. Recursive improvement remains a hypothesis to test, not an assumed monotonic process.

## 2. Research corpus included in the proposed v1.0.0 release

The planned corpus contains **27 research documents**: 12 Foundations, 6 Specifications, 5 Cases, and 4 Extensions. These are research texts and designs, not evidence that the described systems have been implemented or benchmarked.

### Foundations — 12 papers

1. `docs/foundations/CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md` — conceptual lineage and generalization.
2. `docs/foundations/CDE-002-State-Containers-and-Constraint-Governed-Transitions.md` — state containers, transition validity, and constraint governance.
3. `docs/foundations/CDE-003-Delta-Semantics-and-Adaptive-M-to-N-Branching.md` — Delta types, branch accounting, and successor semantics.
4. `docs/foundations/CDE-004-Path-Memory-No-Subtree-Semantics-and-Loop-Back-Control.md` — exclusion, revisit, and cycle semantics.
5. `docs/foundations/CDE-005-Search-Dynamic-Programming-and-Branch-and-Bound-Integration.md` — solver integration and conditional guarantees.
6. `docs/foundations/CDE-006-Directional-Fields-and-Multi-Scale-Delta-Generation.md` — directional and multi-scale exploration.
7. `docs/foundations/CDE-007-Solution-Space-and-Metric-Space-Co-Evolution.md` — versioned solution and metric evolution.
8. `docs/foundations/CDE-008-CDE-as-a-General-Exploration-and-Optimization-Orchestrator.md` — solver portfolio and orchestration.
9. `docs/foundations/CDE-009-Distributed-CDE-and-Delta-Exploration-Contracts.md` — distributed tasks, budgets, and reconciliation.
10. `docs/foundations/CDE-010-Structural-Relay-and-Collective-Learning.md` — reusable evidence, methods, and explicit leftovers.
11. `docs/foundations/CDE-011-Recursive-Generator-and-Policy-Evolution.md` — controlled meta-level evolution.
12. `docs/foundations/CDE-012-Generalization-Boundaries-Guarantees-and-Open-Research.md` — theoretical limits and falsifiability.

### Specifications — 6 contracts

1. `docs/specifications/SPEC-001-State-Container-and-Constraint-Contract.md`
2. `docs/specifications/SPEC-002-Delta-Generation-and-Branch-Contract.md`
3. `docs/specifications/SPEC-003-Path-Memory-and-Loop-Control-Contract.md`
4. `docs/specifications/SPEC-004-Solver-and-Optimization-Contract.md`
5. `docs/specifications/SPEC-005-Distributed-Exploration-and-Relay-Contract.md`
6. `docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md`

Specifications describe proposed interfaces, invariants, conformance expectations, and failure handling. **Specification publication is not evidence that conformance tests have been executed.**

### Illustrative cases — 5 studies

1. `docs/cases/CASE-001-Multi-Scale-Peak-Search.md` — multimodal search and exploration-scale comparisons.
2. `docs/cases/CASE-002-Constrained-School-Bus-Assignment-and-Routing.md` — fleet minimization under service, ride-time, and routing constraints.
3. `docs/cases/CASE-003-Dynamic-Structural-Matching.md` — constrained and revisable structural matching.
4. `docs/cases/CASE-004-Metric-Discovery-and-Co-Evolution.md` — metric candidates, holdout evaluation, and governance.
5. `docs/cases/CASE-005-Distributed-CDE-and-Method-Relay.md` — distributed tasks, reusable methods, and transfer validation.

Cases are proposed designs and validation scenarios; **do not interpret them as completed experiments** without separately published data and reproducible results.

### Research extensions — 4 papers

1. `docs/extensions/EXT-001-Biomimetic-and-Population-Based-CDE.md`
2. `docs/extensions/EXT-002-Open-Ended-Delta-and-Representation-Evolution.md`
3. `docs/extensions/EXT-003-CDE-for-Scientific-Hypothesis-and-Metric-Discovery.md`
4. `docs/extensions/EXT-004-CDE-Collective-Intelligence-and-Structural-RSI.md`

These extensions explore longer-horizon research directions and explicitly retain uncertainty about feasibility, generalization, and recursive capability gains.

## 3. Navigation, terminology, and validation assets

| File | Role |
|---|---|
| [`README.md`](README.md) | Main project orientation and framework summary |
| [`START-HERE.md`](START-HERE.md) | Reading paths for different audiences |
| [`CONTENTS.md`](CONTENTS.md) | Canonical corpus index and cross-reference map |
| [`GLOSSARY.md`](GLOSSARY.md) | Formal terminology and easily confused distinctions |
| [`FIGURE-INDEX.md`](FIGURE-INDEX.md) | Ten planned figures, captions, and insertion guidance |
| [`FUTURE-DIRECTIONS.md`](FUTURE-DIRECTIONS.md) | Research directions and staged priorities |
| [`OPEN-QUESTIONS.md`](OPEN-QUESTIONS.md) | OQ-001 through OQ-054 |
| [`TESTABLE-HYPOTHESES.md`](TESTABLE-HYPOTHESES.md) | TH-001 through TH-054, with test and failure criteria |
| [`VALIDATION-ROADMAP.md`](VALIDATION-ROADMAP.md) | Proposed experiments, conformance gates, and release evidence levels |
| [`CHANGELOG.md`](CHANGELOG.md) | Version-scope history and explicit publication leftovers |
| [`CITATION.cff`](CITATION.cff) | GitHub citation metadata |
| [`.zenodo.json`](.zenodo.json) | Zenodo archival metadata |

**Figure status:** Ten canonical figure names are specified in `FIGURE-INDEX.md`. The earlier artwork was a composite containing ten panels. **Do not claim that ten independent PNG assets have been exported or checked** until each `figures/Fig-001-...png` through `figures/Fig-010-...png` exists, opens, and matches its caption.

## 4. Intended audience and suggested reading paths

- **Algorithm and optimization researchers:** `CDE-001` → `CDE-003` → `CDE-004` → `CDE-005` → `SPEC-002` → `SPEC-004` → `CASE-001` → `VALIDATION-ROADMAP.md`.
- **Constraint and scheduling researchers:** `CDE-002` → `CDE-005` → `SPEC-001` → `SPEC-004` → `CASE-002`.
- **Distributed and multi-agent systems researchers:** `CDE-009` → `CDE-010` → `SPEC-005` → `CASE-005`.
- **Metric and evaluation researchers:** `CDE-007` → `SPEC-006` → `CASE-004` → `TESTABLE-HYPOTHESES.md`.
- **Open-ended learning and RSI researchers:** `CDE-011` → `CDE-012` → `EXT-002` → `EXT-004` → `OPEN-QUESTIONS.md`.

For a full index, see [`CONTENTS.md`](CONTENTS.md).

## 5. Scope, guarantees, and important limitations

The following distinctions are intentional and central to this release:

1. **Expressibility is not reachability.** A representation capable of describing a solution does not establish that a chosen Delta generator can reach it.
2. **Reachability is not completeness.** A search may fail to enumerate reachable states due to budgets, heuristics, pruning, or termination rules.
3. **Completeness is not optimality.** Optimality requires an appropriate objective and proof conditions, not just broad exploration.
4. **Hard feasibility is not heuristic desirability.** Directional fields and soft scores prioritize candidates but do not prove admissibility.
5. **NoSubtree is not a universal no-cycle theorem.** Path-level decision exclusions require explicit context and do not eliminate all graph cycles.
6. **Memoization requires sound equivalence.** Incorrectly merging states can invalidate dynamic programming and solver claims.
7. **Pruning requires evidence.** A heuristic rejection must not be represented as a proof of infeasibility or dominance.
8. **Metric evolution is not self-validation.** Candidate evaluation changes require independent checks and version-aware comparisons.
9. **Distributed feasibility is not automatically global feasibility.** Cross-worker conflicts and shared resource constraints require reconciliation.
10. **Relay is not automatically collective learning.** Transferred artifacts must be applicable and independently validated in the receiving context.
11. **Repeated trials are not necessarily independent evidence.** Evidence accumulation depends on sampling, correlation, and provenance.
12. **Recursive modification is not proven recursive improvement.** Promotion requires measured, independently validated gains and rollback capability.

No benchmark tables or numerical speedup claims are presented as completed results in this release. Any future empirical claim should cite reproducible experiments, matched baselines, and clearly stated statistical uncertainty.

## 6. Validation agenda and proposed first implementation

The recommended initial executable target is a minimal, auditable CDE core:

1. Implement a typed State Container, Delta proposal, transition evaluator, and constraint/policy snapshot.
2. Record M/Q/N/Nc/K counts and the provenance and disposition of every attempted transition.
3. Implement path memory and fixed-context exclusion with explicit loop/revisit policies.
4. Provide deterministic seeds, reproducible fixtures, and an independent feasibility checker.
5. Evaluate `CASE-001` against appropriate baseline searches under matched computational budgets.
6. Add an optimization case such as `CASE-002` with strong reference methods (e.g., CP-SAT or MIP where appropriate).
7. Introduce distributed exploration only after local contract conformance is established.
8. Evaluate metric and generator evolution with holdouts, promotion criteria, negative controls, and rollback.

The detailed plan appears in [`VALIDATION-ROADMAP.md`](VALIDATION-ROADMAP.md), with falsifiable propositions in [`TESTABLE-HYPOTHESES.md`](TESTABLE-HYPOTHESES.md). The central research question is whether CDE delivers **reproducible, task-conditional net value** relative to strong existing methods after accounting for orchestration and validation overhead. A negative answer in some or all task families is a valid scientific outcome and should narrow the claims accordingly.

## 7. Open collaboration and contribution priorities

The project welcomes **collective learning through explicit, reviewable artifacts**, especially:

- Counterexamples to Delta semantics, state equivalence, NoSubtree assumptions, and pruning logic.
- Minimal conformance tests for `SPEC-001`–`SPEC-006`, including invalid and adversarial cases.
- Independent baselines and fair, budget-matched benchmarking protocols.
- Small reproducible implementations with complete transition traces and independent constraint checks.
- Negative transfer and metric-gaming examples that test governance boundaries.
- Typed Relay artifacts that preserve assumptions, provenance, validation evidence, and unresolved leftovers.
- Corrections to ambiguous definitions, overbroad claims, cross-document links, and figure captions.

A contribution should distinguish **proposal**, **implemented behavior**, **observed result**, and **independently validated result**. Inconclusive and negative findings are welcome. Contributions should avoid presenting speculative capabilities as demonstrated facts.

## 8. Release and archival checklist

Complete these checks **before** publishing this text as the GitHub Release description:

- [ ] Confirm the canonical repository URL, public access, and Apache-2.0 license file.
- [ ] Confirm all 27 research documents exist in the specified `docs/` subdirectories.
- [ ] Verify root navigation files and every relative Markdown link.
- [ ] Export, inspect, and verify all ten **individual** figure PNGs; update or remove broken image references.
- [ ] Review formulas, symbol definitions, figure captions, and cross-paper terminology for consistency.
- [ ] Confirm the actual content of `CITATION.cff` and `.zenodo.json`, including author and version metadata.
- [ ] Confirm the final release date and the `v1.0.0` tag; do not backdate a release.
- [ ] Audit claims: distinguish research proposals, conformance specifications, case designs, and completed empirical evidence.
- [ ] Publish the GitHub Release only after the release assets and scope are checked.
- [ ] If Zenodo integration is used, verify the resulting deposit and DOI **after** archival completion; then update citations and links.
- [ ] Record remaining issues in `CHANGELOG.md` as explicit leftovers rather than silently marking them resolved.

## 9. Citation and related work

**Suggested citation before DOI assignment:**

> Tan, S., & GPT-Obot. (2026). *Constrained Delta Evolution (CDE): A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence* (Version 1.0.0, proposed research release). GitHub research repository. [Repository URL to be inserted after confirmation].

After an archival DOI is assigned, use the final DOI and published metadata in preference to this provisional citation. The canonical machine-readable records are `CITATION.cff` and `.zenodo.json`.

**Conceptual lineage:** BTP and D-BTP; see `CDE-001` for the formal research framing and distinctions. Related research includes structural intelligence, constraint-guided search, dynamic programming, branch-and-bound, multi-agent exploration, and evaluation governance. Similarity of vocabulary does not imply equivalence of algorithms or guarantees.

---

**Release summary:** CDE v1.0.0 is intended as an openly inspectable **research framework and validation agenda** for constrained structural exploration, solver orchestration, metric evolution, and distributed intelligence. Its contribution at this stage is the explicit architecture, contracts, hypotheses, and research program—not an assertion of established performance superiority or achieved structural RSI.

**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Status:** Proposed initial research release; GitHub publication and DOI pending independent confirmation.
