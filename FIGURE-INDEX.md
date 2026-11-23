# Figure Index — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**Version:** v1.0.0 (publication preparation)  
**Date:** 2026-11-23  
**License:** Apache-2.0  
**Repository location:** `FIGURE-INDEX.md`

## 1. Purpose and status

This index is the authoritative *figure map* for the ten CDE illustrations. It specifies figure identifiers, exact filenames, captions, conceptual content, recommended manuscript placement, and editorial validation checks. The figures are **conceptual research diagrams**, not empirical evidence, proof of algorithmic guarantees, or reports of measured performance.

## 2. Canonical figure inventory

| Figure | Exact filename | Title | Primary manuscript |
|---|---|---|---|
| Fig-001 | [Fig-001-CDE-Grand-Architecture.png](figures/Fig-001-CDE-Grand-Architecture.png) | CDE Grand Architecture | `docs/foundations/CDE-008-CDE-as-a-General-Exploration-and-Optimization-Orchestrator.md` |
| Fig-002 | [Fig-002-From-BTP-and-D-BTP-to-CDE.png](figures/Fig-002-From-BTP-and-D-BTP-to-CDE.png) | From BTP and D-BTP to CDE | `docs/foundations/CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md` |
| Fig-003 | [Fig-003-State-Container-and-Constraint-Governed-Delta.png](figures/Fig-003-State-Container-and-Constraint-Governed-Delta.png) | State Container and Constraint-Governed Delta | `docs/foundations/CDE-002-State-Containers-and-Constraint-Governed-Transitions.md` |
| Fig-004 | [Fig-004-Adaptive-M-to-N-Delta-Branching.png](figures/Fig-004-Adaptive-M-to-N-Delta-Branching.png) | Adaptive M-to-N Delta Branching | `docs/foundations/CDE-003-Delta-Semantics-and-Adaptive-M-to-N-Branching.md` |
| Fig-005 | [Fig-005-Path-Memory-No-Subtree-and-Loop-Back.png](figures/Fig-005-Path-Memory-No-Subtree-and-Loop-Back.png) | Path Memory, NoSubtree, and Loop Back | `docs/foundations/CDE-004-Path-Memory-No-Subtree-Semantics-and-Loop-Back-Control.md` |
| Fig-006 | [Fig-006-Search-DP-and-Branch-and-Bound-Integration.png](figures/Fig-006-Search-DP-and-Branch-and-Bound-Integration.png) | Search, DP, and Branch-and-Bound Integration | `docs/foundations/CDE-005-Search-Dynamic-Programming-and-Branch-and-Bound-Integration.md` |
| Fig-007 | [Fig-007-Multi-Scale-Delta-Generation-and-Directional-Exploration.png](figures/Fig-007-Multi-Scale-Delta-Generation-and-Directional-Exploration.png) | Multi-Scale Delta Generation and Directional Exploration | `docs/foundations/CDE-006-Directional-Fields-and-Multi-Scale-Delta-Generation.md` |
| Fig-008 | [Fig-008-Solution-and-Metric-Co-Evolution.png](figures/Fig-008-Solution-and-Metric-Co-Evolution.png) | Solution and Metric Co-Evolution | `docs/foundations/CDE-007-Solution-Space-and-Metric-Space-Co-Evolution.md` |
| Fig-009 | [Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png](figures/Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png) | Distributed CDE Orchestrator and Structural Relay | `docs/foundations/CDE-009-Distributed-CDE-and-Delta-Exploration-Contracts.md` |
| Fig-010 | [Fig-010-Recursive-Generator-Evolution-and-Collective-Learning.png](figures/Fig-010-Recursive-Generator-Evolution-and-Collective-Learning.png) | Recursive Generator Evolution and Collective Learning | `docs/foundations/CDE-011-Recursive-Generator-and-Policy-Evolution.md` |

## 3. Figure-by-figure captions and placement

### Fig-001 — CDE Grand Architecture

**File:** `figures/Fig-001-CDE-Grand-Architecture.png`  
**Primary placement:** CDE-008, immediately after the overview of the orchestrator's principal functional layers.  
**Secondary references:** README; START-HERE; CDE-001; CDE-012; EXT-004.

**Suggested caption:** *Figure 1. Conceptual architecture of Constrained Delta Evolution. A versioned State–Delta–Constraint core supports adaptive proposal generation and solver portfolios; optional orchestration, independent validation, Structural Relay, and controlled meta-evolution extend the system without weakening hard-constraint governance.*

**Required visual elements:** problem/task inputs; State Container; Delta Generator; constraint validation; transition/commit; solver portfolio; metric and directional scoring; path memory; evidence ledger; distributed orchestrator; Structural Relay; independent validation/governance. Clearly separate mandatory logical contracts from optional implementations.

**Avoid:** depicting RSI as an automatically successful feedback loop or claiming that all components must reside in a single runtime.

### Fig-002 — From BTP and D-BTP to CDE

**File:** `figures/Fig-002-From-BTP-and-D-BTP-to-CDE.png`  
**Primary placement:** CDE-001, following the lineage and motivation discussion.  
**Secondary references:** README; START-HERE; CDE-003; CDE-012.

**Suggested caption:** *Figure 2. Research lineage from BTP's binary constructive decisions, through D-BTP's dynamic and directional Delta generation, to CDE's representation-independent State–Delta–Constraint framework. The diagram expresses a conceptual generalization, not a claim of strict algorithmic superiority.*

**Required visual elements:** BTP's Yes/No decision; D-BTP dynamic Delta and directional weak evidence; CDE general State Container and typed transitions; an arrow indicating expanded modeling scope.

**Avoid:** treating a binary BTP tree as the only possible CDE topology or suggesting all BTP properties transfer unconditionally.

### Fig-003 — State Container and Constraint-Governed Delta

**File:** `figures/Fig-003-State-Container-and-Constraint-Governed-Delta.png`  
**Primary placement:** CDE-002, after the formal transition definition.  
**Secondary references:** SPEC-001; SPEC-002; CDE-003; EXT-002.

**Suggested caption:** *Figure 3. A typed Delta proposes a transition from a versioned State Container to a candidate successor. Hard-constraint checks and coherent-version validation determine whether the transition may be committed; directional scores may prioritize attempts but cannot substitute for feasibility.*

**Required visual elements:** source state; typed Delta; candidate state; constraint validator; PASS / FAIL / UNKNOWN / ERROR dispositions; atomic commit only after PASS; provenance and versions.

**Avoid:** equating `UNKNOWN` with success, or showing infeasible proposals as committed states.

### Fig-004 — Adaptive M-to-N Delta Branching

**File:** `figures/Fig-004-Adaptive-M-to-N-Delta-Branching.png`  
**Primary placement:** CDE-003, alongside the proposal/selection/realization accounting.  
**Secondary references:** SPEC-002; CASE-001; CDE-006; EXT-001.

**Suggested caption:** *Figure 4. Adaptive Delta branching distinguishes M proposals, Q selected attempts, N realized transition edges, N_c committed edges, and K distinct successor states. Deduplication, invalid attempts, and uncommitted edges prevent these counts from being treated as interchangeable.*

**Required visual elements:** visibly separate stages labeled `M`, `Q`, `N`, `N_c`, `K`; validation and deduplication branches; example counts explicitly labeled **illustrative only** if used.

**Avoid:** assuming `M = N = K`, or implying every proposal produces a committed successor.

### Fig-005 — Path Memory, NoSubtree, and Loop Back

**File:** `figures/Fig-005-Path-Memory-No-Subtree-and-Loop-Back.png`  
**Primary placement:** CDE-004, following the explanation of path-local decision memory.  
**Secondary references:** SPEC-003; CDE-005; CDE-012; EXT-002.

**Suggested caption:** *Figure 5. Path-local accepted and excluded decision sets support constructive NoSubtree semantics under fixed-context assumptions. Deferred, infeasible, and pruned branches remain distinct; controlled Loop Back may be legitimate in reconfiguration, cyclic, or representation-evolving searches.*

**Required visual elements:** accepted set `A`; excluded set `R`; used decisions `U = A ∪ R`; Yes/No decision branches; separate `DEFER`, `INFEASIBLE`, and `PRUNE`; version-aware Loop Back.

**Avoid:** suggesting that NoSubtree globally prohibits all revisits or proves termination in open-ended settings.

### Fig-006 — Search, DP, and Branch-and-Bound Integration

**File:** `figures/Fig-006-Search-DP-and-Branch-and-Bound-Integration.png`  
**Primary placement:** CDE-005, after the solver-integration overview.  
**Secondary references:** SPEC-004; CDE-008; CASE-001; CASE-002.

**Suggested caption:** *Figure 6. CDE acts as a contract-driven exploration interface over solver families. General search proposes candidates; dynamic programming requires valid future-equivalence keys; branch-and-bound requires admissible bounds and sound pruning. Solver-specific certificates remain distinct from heuristic best-known solutions.*

**Required visual elements:** common CDE state/transition interface; search branch; DP/memoization branch; B&B/incumbent/bounds branch; proof/certificate distinction; return of validated results.

**Avoid:** depicting heuristic pruning as a mathematical proof or promising universal optimality.

### Fig-007 — Multi-Scale Delta Generation and Directional Exploration

**File:** `figures/Fig-007-Multi-Scale-Delta-Generation-and-Directional-Exploration.png`  
**Primary placement:** CDE-006, after multi-granularity operator families are introduced.  
**Secondary references:** CASE-001; CASE-003; EXT-001; EXT-002.

**Suggested caption:** *Figure 7. Local, regional, long-range, and Relay-guided Delta generators explore complementary neighborhoods. Directional fields and weak evidence guide proposal ranking while maintaining uncertainty, diversity, counterexample probes, and hard-constraint checks.*

**Required visual elements:** four Delta scales; direction vectors or fields; uncertainty/weak-evidence annotation; diverse candidate trajectories; constraint boundary.

**Avoid:** representing directional weak evidence as proof or assuming every long-range jump is useful.

### Fig-008 — Solution and Metric Co-Evolution

**File:** `figures/Fig-008-Solution-and-Metric-Co-Evolution.png`  
**Primary placement:** CDE-007, following the two-space formalism.  
**Secondary references:** CASE-004; SPEC-006; EXT-003.

**Suggested caption:** *Figure 8. Solutions and metrics evolve through separate, versioned candidate loops. Improved solution scores under a metric do not certify that the metric itself is better; metric changes require independent decision-utility validation, held-out evidence, and governance before promotion.*

**Required visual elements:** solution loop; metric loop; pinned metric versions; independent evaluator; Goodhart/proxy-risk warning; governance gate; optional recursive generator loop shown separately.

**Avoid:** comparing scores across incompatible metric versions without re-scoring.

### Fig-009 — Distributed CDE Orchestrator and Structural Relay

**File:** `figures/Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png`  
**Primary placement:** CDE-009, after the distributed control-plane introduction.  
**Secondary references:** CDE-010; SPEC-005; CASE-005; EXT-004.

**Suggested caption:** *Figure 9. A logical CDE orchestrator dispatches version-pinned exploration contracts to heterogeneous workers, reconciles global constraints and evidence, and publishes typed Structural Relay artifacts. Shared results and methods require target-side validation before being counted as transferable capabilities.*

**Required visual elements:** orchestrator; worker/solver portfolio; versioned task contracts; lease/retry/fencing concepts; global reconciliation; seven Relay types—Result, Evidence, Constraint, Method, Policy, Metric, Leftover; independent target validation.

**Avoid:** assuming local feasibility implies global feasibility, or worker consensus implies correctness.

### Fig-010 — Recursive Generator Evolution and Collective Learning

**File:** `figures/Fig-010-Recursive-Generator-Evolution-and-Collective-Learning.png`  
**Primary placement:** CDE-011, after the recursive evolution levels are defined.  
**Secondary references:** CDE-010; SPEC-006; EXT-002; EXT-004.

**Suggested caption:** *Figure 10. Controlled recursive evolution separates task exploration (L0), generator/operator evolution (L1), policy/representation adaptation (L2), and independent validation and governance (L3). Structural Relay can transmit validated methods and evidence across tasks, but recursive improvement and collective learning must be established empirically.*

**Required visual elements:** four levels `L0–L3`; candidate meta-Deltas; independent held-out evaluation; authorized promotion/rollback; validated Relay and target learning; negative-transfer branch.

**Avoid:** a closed self-approval loop, guaranteed monotonic progress, or an unsupported AGI endpoint.

## 4. Recommended Markdown embedding convention

Place each image near its first substantive discussion, preferably after the paragraph that introduces the mechanism and before detailed caveats. From a document under `docs/foundations/`, use the relative path `../../figures/<filename>`.

Example for CDE-003:

```markdown
![Fig-004. Adaptive M-to-N Delta Branching: proposals, attempts, realized edges, committed edges, and unique successors.](../../figures/Fig-004-Adaptive-M-to-N-Delta-Branching.png)

*Figure 4. Adaptive M-to-N Delta Branching. M, Q, N, N_c, and K are distinct accounting quantities; counts shown in the illustration, if any, are schematic.*
```

For the root `README.md`, use `figures/<filename>` instead. For a document under `docs/cases/`, `docs/specifications/`, or `docs/extensions/`, use `../../figures/<filename>`.

Do not embed all ten large figures in every paper; use the primary placement and link to this index for other references.

## 5. Cross-reference matrix

| Topic | Main figure(s) | Relevant documents |
|---|---|---|
| CDE scope and architecture | Fig-001, Fig-002 | CDE-001, CDE-008, README |
| State and transition contracts | Fig-003 | CDE-002, SPEC-001 |
| Delta accounting and branching | Fig-004 | CDE-003, SPEC-002 |
| Path memory and revisit semantics | Fig-005 | CDE-004, SPEC-003 |
| Solver integration and proofs | Fig-006 | CDE-005, SPEC-004 |
| Directional, multiscale exploration | Fig-007 | CDE-006, CASE-001, EXT-001 |
| Metric evolution and Goodhart | Fig-008 | CDE-007, CASE-004, SPEC-006 |
| Distributed exploration and Relay | Fig-009 | CDE-009, CDE-010, CASE-005, SPEC-005 |
| Generator evolution and RSI | Fig-010 | CDE-011, EXT-002, EXT-004 |
| Scientific hypothesis discovery | Fig-007, Fig-008, Fig-010 | EXT-003 |
| Generalization boundaries | Fig-001, Fig-006, Fig-010 | CDE-012 |

## 6. Publication quality checklist

- [ ] Ten individual PNG files exist at their exact canonical `figures/` paths.
- [ ] Each file has the correct title, identifier, and subject; no panels are swapped.
- [ ] Diagrams are legible at ordinary GitHub document width, including mobile viewing.
- [ ] Labels and arrows remain readable without relying only on color.
- [ ] Figure identifiers, filenames, captions, and manuscript references agree.
- [ ] Mathematical symbols use consistent notation (`M`, `Q`, `N`, `N_c`, `K`; `L0–L3`).
- [ ] Hard-constraint validation is visually distinct from soft scoring and directional guidance.
- [ ] Solver proof status is not conflated with heuristic performance.
- [ ] Metric-version boundaries and independent validation are clear.
- [ ] Relay transfer includes target applicability and validation, not mere publication.
- [ ] All diagrams are labeled conceptual where appropriate; illustrative counts are not presented as experimental results.
- [ ] PNG metadata, licensing, and accessibility text are reviewed before release.
- [ ] GitHub renders all relative links without broken images.

## 7. Figure generation and revision policy

Use one canonical identifier per figure. Keep the filename stable across non-semantic cosmetic revisions; record substantive conceptual changes in the repository changelog or release notes. Avoid adding unverified performance numbers or experimental results to illustrations. If a figure introduces a new concept not defined in the manuscripts, either document that concept in the relevant paper or remove the unsupported visual claim.

A composite contact sheet can be useful as an overview or editorial preview, but it must not be used as evidence that all ten standalone image assets exist. For DOI release, verify each individual file.

## 8. Related repository assets

- [`README.md`](README.md) — project introduction.
- [`START-HERE.md`](START-HERE.md) — reader pathways.
- [`CONTENTS.md`](CONTENTS.md) — complete document index.
- [`GLOSSARY.md`](GLOSSARY.md) — terminology and notation.
- [`VALIDATION-ROADMAP.md`](VALIDATION-ROADMAP.md) — planned empirical verification.
- [`CHANGELOG.md`](CHANGELOG.md) — revision history.

**Research integrity note:** This figure index documents intended conceptual illustrations and their placement. It does not certify that all ten standalone image files have been generated, that any software has been implemented, or that any benchmark has been run.
