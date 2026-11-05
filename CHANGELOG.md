# Changelog

**Project:** Constrained Delta Evolution (CDE)  
**Subtitle:** A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0  
**Document date:** 2026-11-04  
**Publication status:** v1.0.0 research-release candidate; **not yet confirmed as a tagged GitHub release or DOI deposit**.

This document records changes to the **CDE research corpus and publication artifacts**, not to an implemented or empirically validated software product. It follows the general conventions of [Keep a Changelog](https://keepachangelog.com/en/1.1.0/), adapted to a specifications-first research repository.

> **Release discipline:** A document being drafted, a repository being made public, a GitHub release being tagged, and a DOI being minted are separate events. No release or DOI is claimed here until independently confirmed.

## [Unreleased]

### Publication preparation

- Verify the canonical repository name, URL, version tag, author attribution, licensing, and cross-document terminology.
- Audit all internal Markdown links against the actual repository tree, including references to the 12 foundations, 6 specifications, 5 cases, and 4 extensions.
- **Export and verify ten individual PNG figures** with the exact names specified by `FIGURE-INDEX.md`. Earlier artwork was assembled as a composite; a composite is not equivalent to ten independently checked image assets.
- Confirm that each paper's embedded figure links resolve and that captions describe the corresponding artwork accurately.
- Prepare and validate `CITATION.cff`, `.zenodo.json`, `GitHub-Release-Notes-v1.0.0.md`, `ResearchGate-Description.txt`, and `CDE-v1.0.0-ResearchGate-Overview.pdf`.
- Check publication metadata consistency, including the license identifier `Apache-2.0`, contributors, repository URL, and release date when known.
- Perform a final research-claim audit: no benchmark performance, solver optimality, empirical generalization, autonomous discovery, or structural RSI achievement may be presented as measured unless supported by reproducible evidence.
- Keep unresolved matters visible as **Explicit Leftovers**, rather than silently upgrading them to completed deliverables.

### Research and validation still to be performed

- Implement a minimal executable State–Delta–Constraint core with pinned schema and policy versions.
- Create executable conformance fixtures for SPEC-001 through SPEC-006; distinguish **proposed test cases** from **executed and passed tests**.
- Implement traceable accounting for proposed Deltas, selected attempts, realized edges, committed edges, and distinct successors (M/Q/N/Nc/K).
- Test Path Memory, NoSubtree, and Loop Back semantics under constructive and cyclic scenarios.
- Compare CDE configurations with appropriate established baselines (search, dynamic programming, branch-and-bound, CP-SAT/MIP where applicable) under matched budgets.
- Run reproducible experiments for CASE-001 through CASE-005, including negative controls and ablations.
- Validate cross-version metric comparisons, distributed reconciliation, transfer applicability, and recursive promotion/rollback independently.
- Report null and negative results, failure cases, and evidence limitations alongside any improvements.

## [1.0.0] — Proposed initial research release (date pending)

**Status:** Contents prepared for the first archival research release. This heading describes the **intended release scope**; it does **not** assert that `v1.0.0` has already been published, tagged, or assigned a DOI.

### Added — Core research framework

- Defined **Constrained Delta Evolution (CDE)** as a versioned structural exploration framework centered on State Containers, typed Deltas, transitions, hard constraints, generators, and path/learning memory.
- Explained the conceptual lineage from **Bucket Tree of Permutation (BTP)** and **Dynamic Bucket Tree of Permutation (D-BTP)** without treating a particular tree topology as a requirement of CDE.
- Introduced **Adaptive M-to-N Delta Branching** with separate accounting for proposal generation, attempts, realized transitions, committed transitions, and unique successors.
- Specified Path Memory, NoSubtree, Loop Back, identity and equivalence distinctions, and the difference between exclusion, deferral, pruning, and proven infeasibility.
- Positioned CDE as a possible orchestration layer for established search, dynamic programming, branch-and-bound, constraint programming, and heuristic optimization, subject to the assumptions of each method.
- Developed multi-scale Delta generation and directional guidance with explicit uncertainty and provenance; soft evidence does not override hard constraints.
- Proposed **Solution–Metric Co-Evolution** with pinned metric versions, independent validation, Goodhart-risk controls, and governance for metric changes.
- Developed **Distributed CDE** exploration contracts, including snapshots, budgets, leases, fencing, idempotency, stale-worker handling, and global reconciliation.
- Defined seven canonical **Structural Relay** artifact classes: Result, Evidence, Constraint, Method, Policy, Metric, and Leftover.
- Developed bounded, governance-aware proposals for generator, policy, operator, and representation evolution, including rollback and independent evaluation.
- Distinguished formal definitions, conditional guarantees, hypotheses, illustrative cases, and experimental findings.

### Added — Foundations (12 papers)

1. `docs/foundations/CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md`
2. `docs/foundations/CDE-002-State-Containers-and-Constraint-Governed-Transitions.md`
3. `docs/foundations/CDE-003-Delta-Semantics-and-Adaptive-M-to-N-Branching.md`
4. `docs/foundations/CDE-004-Path-Memory-No-Subtree-Semantics-and-Loop-Back-Control.md`
5. `docs/foundations/CDE-005-Search-Dynamic-Programming-and-Branch-and-Bound-Integration.md`
6. `docs/foundations/CDE-006-Directional-Fields-and-Multi-Scale-Delta-Generation.md`
7. `docs/foundations/CDE-007-Solution-Space-and-Metric-Space-Co-Evolution.md`
8. `docs/foundations/CDE-008-CDE-as-a-General-Exploration-and-Optimization-Orchestrator.md`
9. `docs/foundations/CDE-009-Distributed-CDE-and-Delta-Exploration-Contracts.md`
10. `docs/foundations/CDE-010-Structural-Relay-and-Collective-Learning.md`
11. `docs/foundations/CDE-011-Recursive-Generator-and-Policy-Evolution.md`
12. `docs/foundations/CDE-012-Generalization-Boundaries-Guarantees-and-Open-Research.md`

### Added — Specifications (6 contracts)

- `docs/specifications/SPEC-001-State-Container-and-Constraint-Contract.md` — state, transition, constraint and version semantics.
- `docs/specifications/SPEC-002-Delta-Generation-and-Branch-Contract.md` — typed Delta generation, branch accounting and selection.
- `docs/specifications/SPEC-003-Path-Memory-and-Loop-Control-Contract.md` — path-local memory, revisit and pruning semantics.
- `docs/specifications/SPEC-004-Solver-and-Optimization-Contract.md` — optimization claims, bounds, memoization and proof statuses.
- `docs/specifications/SPEC-005-Distributed-Exploration-and-Relay-Contract.md` — distributed work and transferable artifacts.
- `docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md` — metric, operator, representation and policy evolution controls.

**Specification status:** Proposed contracts and conformance designs; not an assertion that a conformant runtime has been built or certified.

### Added — Cases (5 research designs)

- `docs/cases/CASE-001-Multi-Scale-Peak-Search.md` — local-to-global search and directional Delta benchmarking.
- `docs/cases/CASE-002-Constrained-School-Bus-Assignment-and-Routing.md` — physical fleet minimization subject to hard student and routing constraints.
- `docs/cases/CASE-003-Dynamic-Structural-Matching.md` — uncertainty-aware structural matching and evidence provenance.
- `docs/cases/CASE-004-Metric-Discovery-and-Co-Evolution.md` — independently validated metric discovery and anti-Goodhart controls.
- `docs/cases/CASE-005-Distributed-CDE-and-Method-Relay.md` — fault-aware distributed exploration and method transfer.

**Case status:** Illustrative architectures and proposed tests, not completed experimental results.

### Added — Extensions (4 exploratory studies)

- `docs/extensions/EXT-001-Biomimetic-and-Population-Based-CDE.md` — populations, islands, diversity and exploration.
- `docs/extensions/EXT-002-Open-Ended-Delta-and-Representation-Evolution.md` — typed operator genesis and representation migration.
- `docs/extensions/EXT-003-CDE-for-Scientific-Hypothesis-and-Metric-Discovery.md` — hypothesis, experiment, evidence and metric cycles.
- `docs/extensions/EXT-004-CDE-Collective-Intelligence-and-Structural-RSI.md` — collective learning and controlled recursive structural improvement.

**Extension status:** Research hypotheses and validation proposals; no claim of realized open-ended intelligence or AGI.

### Added — Navigation and research governance

- `README.md` — project thesis, research boundaries, repository map and entry points.
- `START-HERE.md` — reading routes, terminology, contracts and scope qualifications.
- `CONTENTS.md` — full 27-paper research corpus map and dependency paths.
- `GLOSSARY.md` — canonical terminology, mathematical notation, identity and evidence distinctions.
- `FIGURE-INDEX.md` — intended ten-figure index, captions and placement guidance.
- `FUTURE-DIRECTIONS.md` — prioritized engineering and theoretical research directions.
- `OPEN-QUESTIONS.md` — OQ-001 through OQ-054, including disconfirmation questions.
- `TESTABLE-HYPOTHESES.md` — TH-001 through TH-054 with proposed baselines and falsification criteria.
- `VALIDATION-ROADMAP.md` — research phases, conformance gates, benchmarks, ablations, negative controls and release evidence levels.
- `CHANGELOG.md` — versioned record of the proposed research-release scope and unresolved publication work.

### Planned — Figure set

The research corpus specifies the following ten figures under `figures/`. **Their presence in this list does not establish that ten separate PNG files have been exported and verified.**

1. `Fig-001-CDE-Grand-Architecture.png`
2. `Fig-002-From-BTP-and-D-BTP-to-CDE.png`
3. `Fig-003-State-Container-and-Constraint-Governed-Delta.png`
4. `Fig-004-Adaptive-M-to-N-Delta-Branching.png`
5. `Fig-005-Path-Memory-No-Subtree-and-Loop-Back.png`
6. `Fig-006-Search-DP-and-Branch-and-Bound-Integration.png`
7. `Fig-007-Multi-Scale-Delta-Generation-and-Directional-Exploration.png`
8. `Fig-008-Solution-and-Metric-Co-Evolution.png`
9. `Fig-009-Distributed-CDE-Orchestrator-and-Structural-Relay.png`
10. `Fig-010-Recursive-Generator-Evolution-and-Collective-Learning.png`

### Research-claim limitations

- **No universal optimizer claim.** A shared abstraction does not imply superior performance on every problem.
- **No unconditional completeness or optimality claim.** Guarantees require task-specific assumptions and sound methods.
- **No empirical benchmark claim.** The cases and test matrices are designs until results are executed and published.
- **No automatic metric improvement claim.** Metric revisions require held-out validation and governance.
- **No automatic transfer or collective learning claim.** Shared artifacts must be applicable and independently validated.
- **No demonstrated recursive self-improvement or AGI claim.** The RSI extensions remain proposed research directions.

## Versioning and evidence conventions

| Label | Meaning | Evidence required |
|---|---|---|
| Draft | Authored research material under review | Document available for inspection |
| Proposed | Specification, mechanism or hypothesis described | Clear definition and assumptions |
| Conformance-designed | Tests specified, not necessarily run | Test contracts and expected outcomes |
| Conformance-passed | Tests actually executed and passed | Reproducible logs and versioned fixtures |
| Empirically evaluated | Benchmarks actually executed | Datasets, baselines, metrics and methodology |
| Validated | Claim supported within a stated scope | Independent checks and limitations |
| Released | Public version tag and assets exist | Verifiable release URL and immutable tag |
| DOI archived | Archival record and identifier issued | Resolvable DOI and matching metadata |

The project may use `v1.0.0` for the **first archived research framework**, not as a software stability or benchmark certification statement. Later revisions should distinguish changes to theory, schemas, empirical evidence, and publication assets.

### Future changelog entry template

```markdown
## [X.Y.Z] — YYYY-MM-DD

### Added
- New research, contracts, tests, figures or data (with links).

### Changed
- Revised definitions, assumptions, schemas or interpretations.

### Fixed
- Corrected formulas, cross-references, errors or misleading claims.

### Validated
- Executed tests and results, with reproducible evidence links.

### Limitations
- Unresolved issues, regressions, negative results and applicability boundaries.
```

## Related documents

- [Project overview](README.md)
- [Reading guide](START-HERE.md)
- [Complete contents](CONTENTS.md)
- [Terminology](GLOSSARY.md)
- [Figure index](FIGURE-INDEX.md)
- [Open research questions](OPEN-QUESTIONS.md)
- [Testable hypotheses](TESTABLE-HYPOTHESES.md)
- [Validation roadmap](VALIDATION-ROADMAP.md)

---

*Maintained as part of the CDE research publication record. A proposed release scope is not proof of publication, implementation, or experimental success.*
