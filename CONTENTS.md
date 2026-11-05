# CONTENTS — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Repository:** `Constrained-Delta-Evolution/`  
**Authors:** Sizhe Tan and GPT-Obot  
**Version:** v1.0.0 research draft · **Date:** 2026-11-04  
**License:** Apache-2.0  
**Document role:** Canonical publication table of contents, reading map, and cross-reference index  
**Research status:** Proposed architecture, specifications, and validation protocols; no universal performance or AGI/RSI claim is established.

> **Navigation principle:** Read the Foundations to understand *why* CDE is proposed, the Specifications to understand *what must be true*, the Cases to understand *what can be tested*, and the Extensions to understand *what remains open*.

---

## 1. Quick navigation

| Entry | Purpose |
|---|---|
| [README.md](README.md) | Research motivation, framework overview, major claims and limitations |
| [START-HERE.md](START-HERE.md) | Audience-specific reading paths and first experiments |
| **CONTENTS.md** | Complete canonical index and dependency map |
| [GLOSSARY.md](GLOSSARY.md) | Vocabulary and formal distinctions |
| [FIGURE-INDEX.md](FIGURE-INDEX.md) | Ten-figure captions and placement guidance |
| [FUTURE-DIRECTIONS.md](FUTURE-DIRECTIONS.md) | Proposed next-stage research |
| [OPEN-QUESTIONS.md](OPEN-QUESTIONS.md) | Unresolved issues and counterexamples |
| [TESTABLE-HYPOTHESES.md](TESTABLE-HYPOTHESES.md) | Falsifiable claims, baselines, and controls |
| [VALIDATION-ROADMAP.md](VALIDATION-ROADMAP.md) | Conformance-first implementation and experiment sequence |
| [CHANGELOG.md](CHANGELOG.md) | Publication and contract history |
| [CITATION.cff](CITATION.cff) | Machine-readable citation metadata |
| [.zenodo.json](.zenodo.json) | Zenodo deposit metadata |
| [LICENSE](LICENSE) | Apache-2.0 license text |

The links above describe the intended repository structure. Publication assets must be present and their links verified before release.

## 2. Research corpus at a glance

| Series | Number | Role | Directory |
|---|---:|---|---|
| Foundations (`CDE`) | 12 | Core definitions, algorithms, limits, and architecture | `docs/foundations/` |
| Specifications (`SPEC`) | 6 | Normative-style proposed interfaces and conformance contracts | `docs/specifications/` |
| Cases (`CASE`) | 5 | Testbeds, experimental protocols, and negative controls | `docs/cases/` |
| Extensions (`EXT`) | 4 | Open-ended research directions and integration studies | `docs/extensions/` |
| **Research papers total** | **27** | **Research design corpus, not 27 validated experiments** | `docs/` |
| Figures (`Fig`) | 10 | Visual guide to the main architecture and mechanisms | `figures/` |

## 3. Foundations — 12 papers

Read these approximately in order for a coherent progression from the BTP lineage to collective structural intelligence.

| ID | Document | Central subject |
|---|---|---|
| CDE-001 | [From BTP and D-BTP to Constrained Delta Evolution](docs/foundations/CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md) | Lineage, motivation, and generalization from bucket-tree search |
| CDE-002 | [State Containers and Constraint-Governed Transitions](docs/foundations/CDE-002-State-Containers-and-Constraint-Governed-Transitions.md) | State meaning, transition validity, and hard/soft constraints |
| CDE-003 | [Delta Semantics and Adaptive M-to-N Branching](docs/foundations/CDE-003-Delta-Semantics-and-Adaptive-M-to-N-Branching.md) | Typed Delta semantics, proposal/attempt/realization accounting |
| CDE-004 | [Path Memory, No-Subtree Semantics, and Loop-Back Control](docs/foundations/CDE-004-Path-Memory-No-Subtree-Semantics-and-Loop-Back-Control.md) | Decision identity, path history, exclusion, pruning, revisitation |
| CDE-005 | [Search, Dynamic Programming, and Branch-and-Bound Integration](docs/foundations/CDE-005-Search-Dynamic-Programming-and-Branch-and-Bound-Integration.md) | Solver portfolio and conditions for valid proof and pruning |
| CDE-006 | [Directional Fields and Multi-Scale Delta Generation](docs/foundations/CDE-006-Directional-Fields-and-Multi-Scale-Delta-Generation.md) | Local/regional/long-range exploration and weak evidence |
| CDE-007 | [Solution-Space and Metric-Space Co-Evolution](docs/foundations/CDE-007-Solution-Space-and-Metric-Space-Co-Evolution.md) | Separate solution and metric evolution; Goodhart safeguards |
| CDE-008 | [CDE as a General Exploration and Optimization Orchestrator](docs/foundations/CDE-008-CDE-as-a-General-Exploration-and-Optimization-Orchestrator.md) | Cross-solver coordination without replacing established methods |
| CDE-009 | [Distributed CDE and Delta Exploration Contracts](docs/foundations/CDE-009-Distributed-CDE-and-Delta-Exploration-Contracts.md) | Distributed tasks, coherence, validation, and failure semantics |
| CDE-010 | [Structural Relay and Collective Learning](docs/foundations/CDE-010-Structural-Relay-and-Collective-Learning.md) | Typed knowledge transfer, Leftovers, and target-side evidence |
| CDE-011 | [Recursive Generator and Policy Evolution](docs/foundations/CDE-011-Recursive-Generator-and-Policy-Evolution.md) | Controlled meta-Deltas, promotion, and rollback |
| CDE-012 | [Generalization Boundaries, Guarantees, and Open Research](docs/foundations/CDE-012-Generalization-Boundaries-Guarantees-and-Open-Research.md) | Conditional guarantees, failure modes, and research agenda |

### Foundation dependency sketch

```text
BTP / D-BTP
     |
  CDE-001
     |
  CDE-002 ----> CDE-003 ----> CDE-004
     |             |               |
     +----------> CDE-005 <---------+
                    |
                 CDE-006
                    |
                 CDE-007
                    |
                 CDE-008
                    |
                 CDE-009 ----> CDE-010 ----> CDE-011
                                              |
                                           CDE-012
```

This is a recommended conceptual reading graph, not a formal theorem of logical dependence.

## 4. Specifications — six proposed contracts

Specifications are the recommended starting point for an executable implementation. They define conformance expectations but do not themselves constitute a validated runtime.

| ID | Document | Contract boundary |
|---|---|---|
| SPEC-001 | [State Container and Constraint Contract](docs/specifications/SPEC-001-State-Container-and-Constraint-Contract.md) | Typed state, validity, versioning, atomic transitions |
| SPEC-002 | [Delta Generation and Branch Contract](docs/specifications/SPEC-002-Delta-Generation-and-Branch-Contract.md) | Typed proposals, M/Q/N/K, branching and evidence |
| SPEC-003 | [Path Memory and Loop Control Contract](docs/specifications/SPEC-003-Path-Memory-and-Loop-Control-Contract.md) | Decision identity, NoSubtree, loops, cache equivalence |
| SPEC-004 | [Solver and Optimization Contract](docs/specifications/SPEC-004-Solver-and-Optimization-Contract.md) | Search/DP/B&B solver integration and proof status |
| SPEC-005 | [Distributed Exploration and Relay Contract](docs/specifications/SPEC-005-Distributed-Exploration-and-Relay-Contract.md) | Task dispatch, distributed validation, structural relay |
| SPEC-006 | [Metric and Recursive Evolution Contract](docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md) | Metric versions, meta-evolution, independent governance |

### Contract-first implementation order

`SPEC-001 → SPEC-002 → SPEC-003 → SPEC-004 → SPEC-005 → SPEC-006`

A minimal single-worker research prototype may implement only a declared subset, but it must not claim conformance to interfaces it does not support. `UNKNOWN` and `ERROR` are not equivalent to `PASS`. A heuristic is not a proof of infeasibility or optimality.

## 5. Cases — five research testbeds

| ID | Document | Main evaluation focus |
|---|---|---|
| CASE-001 | [Multi-Scale Peak Search](docs/cases/CASE-001-Multi-Scale-Peak-Search.md) | Multi-scale Delta search, adaptive branching, controlled numerical benchmarks |
| CASE-002 | [Constrained School-Bus Assignment and Routing](docs/cases/CASE-002-Constrained-School-Bus-Assignment-and-Routing.md) | Fleet minimization, routing, student burden, hard constraints |
| CASE-003 | [Dynamic Structural Matching](docs/cases/CASE-003-Dynamic-Structural-Matching.md) | Uncertain matching, competing hypotheses, counterevidence, abstention |
| CASE-004 | [Metric Discovery and Co-Evolution](docs/cases/CASE-004-Metric-Discovery-and-Co-Evolution.md) | Metric governance, independent decision utility, Goodhart risks |
| CASE-005 | [Distributed CDE and Method Relay](docs/cases/CASE-005-Distributed-CDE-and-Method-Relay.md) | Multi-agent search, global reconciliation, method transfer, collective learning |

### Suggested empirical progression

**Stage A:** CASE-001 as the smallest reproducible numerical testbed.  
**Stage B:** CASE-002 for meaningful combinatorial constraints and optimization tradeoffs.  
**Stage C:** CASE-003 for uncertainty, negative evidence, and structural hypotheses.  
**Stage D:** CASE-004 for independently validated metric evolution.  
**Stage E:** CASE-005 for coordination, transfer, and distributed fault handling.

The cases provide experimental designs and illustrative mechanisms. They are not claims of completed benchmarks, measured speedups, or established superiority.

## 6. Extensions — four research frontiers

| ID | Document | Proposed frontier |
|---|---|---|
| EXT-001 | [Biomimetic and Population-Based CDE](docs/extensions/EXT-001-Biomimetic-and-Population-Based-CDE.md) | Population search, island models, diversity, and biomimetic hypotheses |
| EXT-002 | [Open-Ended Delta and Representation Evolution](docs/extensions/EXT-002-Open-Ended-Delta-and-Representation-Evolution.md) | Operator genesis, state-representation change, migration contracts |
| EXT-003 | [CDE for Scientific Hypothesis and Metric Discovery](docs/extensions/EXT-003-CDE-for-Scientific-Hypothesis-and-Metric-Discovery.md) | Hypothesis generation, experiments, evidence, scientific metrics |
| EXT-004 | [CDE Collective Intelligence and Structural RSI](docs/extensions/EXT-004-CDE-Collective-Intelligence-and-Structural-RSI.md) | Multi-agent learning, Structural Relay, controlled recursive improvement |

The extension sequence moves from **population exploration**, to **evolving what can be represented**, to **scientific discovery**, to **collective structural learning**. These are open research directions, not guaranteed outcomes.

## 7. Figures — ten visual references

| Figure | File | Recommended primary placement |
|---|---|---|
| Fig-001 | [CDE Grand Architecture](figures/Fig-001-CDE-Grand-Architecture.png) | README; CDE-008 |
| Fig-002 | [From BTP and D-BTP to CDE](figures/Fig-002-From-BTP-and-D-BTP-to-CDE.png) | CDE-001 |
| Fig-003 | [State Container and Constraint-Governed Delta](figures/Fig-003-State-Container-and-Constraint-Governed-Delta.png) | CDE-002; SPEC-001 |
| Fig-004 | [Adaptive M-to-N Delta Branching](figures/Fig-004-Adaptive-M-to-N-Delta-Branching.png) | CDE-003; SPEC-002 |
| Fig-005 | [Path Memory, No-Subtree and Loop Back](figures/Fig-005-Path-Memory-No-Subtree-and-Loop-Back.png) | CDE-004; SPEC-003 |
| Fig-006 | [Search, DP and Branch-and-Bound Integration](figures/Fig-006-Search-DP-and-Branch-and-Bound-Integration.png) | CDE-005; SPEC-004 |
| Fig-007 | [Multi-Scale Delta Generation and Directional Exploration](figures/Fig-007-Multi-Scale-Delta-Generation-and-Directional-Exploration.png) | CDE-006; CASE-001 |
| Fig-008 | [Solution and Metric Co-Evolution](figures/Fig-008-Solution-and-Metric-Co-Evolution.png) | CDE-007; CASE-004 |
| Fig-009 | [Distributed CDE Orchestrator and Structural Relay](figures/Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png) | CDE-009; CDE-010; CASE-005 |
| Fig-010 | [Recursive Generator Evolution and Collective Learning](figures/Fig-010-Recursive-Generator-Evolution-and-Collective-Learning.png) | CDE-011; EXT-004 |

**Figure integrity note:** The table identifies ten *intended individual files*. If artwork was generated as a composite sheet, split and inspect the individual panels before publishing under these names. A planned link does not establish that the corresponding PNG exists.

## 8. Reading paths by research interest

| Interest | Suggested sequence |
|---|---|
| **Core CDE model** | README → CDE-001 → CDE-002 → CDE-003 → CDE-004 |
| **Algorithms and optimization** | CDE-005 → CDE-006 → SPEC-004 → CASE-001 → CASE-002 |
| **Software architecture** | SPEC-001 → SPEC-002 → SPEC-003 → SPEC-004 → CASE-001 |
| **Search semantics and guarantees** | CDE-003 → CDE-004 → CDE-005 → CDE-012 |
| **Dynamic structural matching** | CDE-006 → CASE-003 → EXT-002 |
| **Metric discovery** | CDE-007 → SPEC-006 → CASE-004 → EXT-003 |
| **Distributed systems** | CDE-008 → CDE-009 → SPEC-005 → CASE-005 |
| **Structural Relay** | CDE-010 → CASE-005 → EXT-004 |
| **Open-ended representation** | CDE-011 → EXT-002 → EXT-003 |
| **AI/AGI and controlled RSI** | CDE-010 → CDE-011 → CDE-012 → EXT-004 |
| **Critical evaluation** | CDE-012 → TESTABLE-HYPOTHESES → VALIDATION-ROADMAP → relevant CASE |

## 9. Canonical vocabulary and distinctions

**State Container:** A typed, versioned representation of a problem or candidate solution state.

**Delta:** A typed proposed change with declared preconditions, effects, and provenance. A Delta is a transition proposal, not necessarily a committed state change.

**Hard Constraint:** A condition that must be satisfied before a transition may be committed under the relevant contract.

**Soft Metric / Directional Field:** A preference, estimate, or weak signal used for prioritization; not a substitute for hard feasibility or proof.

**Adaptive M-to-N:** Separates `M` generated Delta proposals, `Q` selected attempts, `N` realized transition edges, `N_c` committed edges, and `K` distinct successor states. Counts must not be silently conflated.

**Path Memory:** A versioned record of decisions, visited states, and transition provenance. Fixed-context NoSubtree rules are not universal cycle-prevention theorems.

**Structural Relay:** Typed transfer of Result, Evidence, Constraint, Method, Policy, Metric, and Leftover artifacts, with source and target applicability checks.

**Collective Learning:** Measured improvement in target tasks attributable to transferred, validated knowledge; not merely shared messages or parallel execution.

**Structural RSI:** Candidate evolution of generators, representations, metrics, or policies, subject to independent validation and authorization. It is not guaranteed monotonic improvement.

### Essential logical boundaries

```text
Representable ≠ Generable ≠ Reachable under budget
Reachable ≠ Complete ≠ Optimal ≠ Efficient
Higher score ≠ Better metric ≠ Better decision utility
Distributed execution ≠ Validated transfer ≠ Collective learning
New representation ≠ Proven improvement ≠ Generalized intelligence
```

## 10. Publication assets and release preparation

| Asset | Location | Purpose |
|---|---|---|
| GitHub release notes | [publication/GitHub-Release-Notes-v1.0.0.md](publication/GitHub-Release-Notes-v1.0.0.md) | Release summary, scope, limitations |
| ResearchGate description | [publication/ResearchGate-Description.txt](publication/ResearchGate-Description.txt) | Plain-text public project summary |
| ResearchGate overview | [publication/CDE-v1.0.0-ResearchGate-Overview.pdf](publication/CDE-v1.0.0-ResearchGate-Overview.pdf) | One-page introduction and repository links |
| Citation metadata | [CITATION.cff](CITATION.cff) | Author and software/research citation |
| Zenodo metadata | [.zenodo.json](.zenodo.json) | DOI deposit metadata |
| Changelog | [CHANGELOG.md](CHANGELOG.md) | Release and contract history |

Before creating a release or DOI deposit, verify that all intended files exist; relative Markdown links resolve; figure filenames match the actual files; the metadata names, title, version, and authors are consistent; and no empirical claim exceeds the available evidence.

## 11. Research boundaries and responsible interpretation

CDE is a **proposed research framework**, not a validated universal algorithm. Its integration of Search, DP, Branch-and-Bound, local/evolutionary search, and distributed solvers does not replace the assumptions under which those methods are correct. The framework does not establish unconditional completeness, optimality, efficiency, open-ended growth, AGI, or RSI.

Examples in the cases and extensions are illustrative until executable implementations and independent results are published. Negative controls, unsuccessful transfers, partial migrations, unresolved Leftovers, and failed replications are meaningful research outputs and should remain visible.

## 12. Contribution and maintenance conventions

1. Preserve canonical document IDs and filenames. Update this index when adding, moving, or renaming a paper.
2. Keep normative contract language in Specifications distinct from proposals and examples in Foundations, Cases, and Extensions.
3. Maintain versioned State, Delta, Constraint, Metric, Solver, and Relay identifiers across compatible documents.
4. Preserve provenance and counterexamples; never silently promote heuristic observations into proofs.
5. Require conformance checks before claiming runtime support and independent validation before claiming generalization or transfer.
6. Record explicit Leftovers, including failed migrations, unresolved hypotheses, and negative results.
7. Recheck all relative links and release metadata after repository assembly.
8. Welcome critical replication and alternative methods under an open, low-key, collective-learning-friendly research posture.

---

**Canonical corpus:** 12 Foundations + 6 Specifications + 5 Cases + 4 Extensions = **27 research papers**, supported by 10 planned figures and repository publication assets.  
**Guiding principle:** *Specifications and contracts first; conformance before runtime; explicit leftovers; independent validation before promotion.*
