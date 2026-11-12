# Validation Roadmap — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**Status:** v1.0.0 proposed validation protocol; **no CDE experiments or outcomes are asserted here**  
**Date:** 2026-11-04  
**License:** Apache-2.0  
**Repository location:** `VALIDATION-ROADMAP.md`  
**Companion documents:** [README](README.md) · [START-HERE](START-HERE.md) · [CONTENTS](CONTENTS.md) · [OPEN-QUESTIONS](OPEN-QUESTIONS.md) · [TESTABLE-HYPOTHESES](TESTABLE-HYPOTHESES.md) · [FUTURE-DIRECTIONS](FUTURE-DIRECTIONS.md)

> **Research-integrity notice.** This is a forward-looking protocol and a roadmap for independent, reproducible research. Proposed test counts, schedules, thresholds, datasets, schemas, and milestone artifacts are **plans**, not evidence of tests run, benchmark results, or validated improvements. All outcomes, including regressions and null findings, must be preserved. A GitHub/Zenodo DOI certifies a deposited version, not scientific correctness or measured performance.

## 1. Executive Purpose

CDE generalizes BTP and D-BTP into a contract-governed architecture for exploring states using typed Deltas, adaptive branching, explicit path semantics, established solvers, directional weak evidence, evolving metrics and representations, and distributed Structural Relay. Its potential value lies not in merely wrapping existing optimization algorithms but in whether its structural interfaces produce independently measurable improvements in correctness, auditability, search efficiency, transfer, and collective learning under declared conditions.

This roadmap establishes **what must be built, tested, compared, recorded, and independently checked before each level of claim can be made**. It organizes the 54 testable hypotheses TH-001–TH-054, traced one-to-one to OQ-001–OQ-054, into gates that can be passed, failed, deferred, or explicitly left unresolved. It separates the repository's *theoretical publication readiness* from *executable conformance*, *experimental evidence*, and *generalization*. These are not interchangeable.

The primary objective is to produce credible evidence for or against CDE components, with matched compute budgets and mature solver baselines. The secondary objective is to make negative results and failed transfers reusable via versioned Structural Relay.

## 2. Scope, Artifacts, and Claim Levels

The research corpus consists of **12 Foundations (CDE-001–012), 6 Specifications (SPEC-001–006), 5 Cases (CASE-001–005), and 4 Extensions (EXT-001–004)**. Ten planned figures, repository guide documents, and publication metadata are communication assets, not empirical validation.

| Level | Claim category | Minimum evidence | Not implied |
|---|---|---|---|
| E0 | Proposed architecture | Coherent, versioned research text | Runtime exists |
| E1 | Contract conformance | Executable schemas and passing fixture tests | Algorithm is effective |
| E2 | Correctness within assumptions | Sound validator, proof where claimed, counterexamples | Global optimality |
| E3 | Empirical utility on named benchmarks | Fair baselines, preregistration, repeated independent runs | Transfer to other domains |
| E4 | Validated transfer/generalization | Held-out domains, negative transfer accounting, replication | Open-ended intelligence |
| E5 | Controlled recursive improvement | Independent multigeneration evidence and governance | Guaranteed monotonic RSI or AGI |

A statement such as “CDE can express a branch-and-bound solver” is architectural; “CDE is faster than branch-and-bound” is an empirical assertion requiring direct comparison and accounting for overhead. A proof of an admissible bound is conditional on its mathematical assumptions and the implemented encoding.

## 3. Core Formal and Measurement Contracts

For a pinned task version, define

$$\mathcal P_v=(S_v,\Delta_v,T_v,C_v,G_v,L_v),\qquad s_{t+1}=T_v(s_t,\delta_t),\quad \delta_t\in G_v(s_t).$$

Hard constraints govern state commits; soft scores and directional fields prioritize proposals but cannot certify feasibility. In any exploration scope distinguish **M** proposed Delta instances, **Q** attempted candidates, **N** realized edges, **N_c** committed edges, and **K** distinct successor states. Track Delta definitions **D** separately from Delta instances, and distinguish search depth from count of distinct decisions. All reported rates must declare numerator, denominator, deduplication scope, and whether aborted/retried attempts are included.

Each result must pin `state_schema_version`, `operator_manifest_version`, `constraint_bundle_version`, `metric_version`, `solver_version`, `generator_version`, `policy_version`, `validation_protocol_version`, `seed`, `task_instance_id`, and `budget`. State equivalence and memoization keys require declared future-equivalence assumptions. NoSubtree/YesSubtree semantics apply only in their specified constructive context; Loop Back may be valid in other search contexts. `EXCLUDE`, `INFEASIBLE`, `DEFER`, and `PRUNE` are distinct dispositions.

A solver claim MUST use typed statuses such as `CANDIDATE`, `FEASIBLE`, `BEST_KNOWN`, `BOUNDED_GAP`, `OPTIMAL`, `INFEASIBLE_PROVEN`, `EXHAUSTIVE`, and `UNKNOWN`; heuristic search failures do not prove infeasibility. Constraint-validation `UNKNOWN` or `ERROR` MUST NOT be promoted to `PASS`.

## 4. Twelve Milestones and Their Gates

The milestones are ordered by dependency, not calendar promises. Each has entry criteria, deliverables, pass/fail rules, and an explicit evidence class.

### M01 — Corpus and Contract Freeze

**Entry/work:** Pin the 27 research documents, terminology, versioned contracts, links, and claims register.

**Required artifacts:** Machine-readable document inventory; normative-term audit; unresolved contradictions list.

**Gate:** Every referenced document and contract resolves; no silently conflicting definitions of Delta, M/Q/N/N_c/K, state equivalence or metric versions.

**Evidence level:** E0. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M02 — State/Delta Reference Kernel

**Entry/work:** Implement SPEC-001 and SPEC-002 with deterministic state transitions and typed hard constraints.

**Required artifacts:** Reference transition engine; schemas; fixtures; mutation and rollback tests.

**Gate:** Invalid transitions cannot commit; unknown validation is not pass; full provenance is recorded.

**Evidence level:** E1–E2. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M03 — Path and Search Semantics

**Entry/work:** Implement SPEC-003 including path-local decisions, Loop Back, equivalence, deduplication and explicit dispositions.

**Required artifacts:** Finite fixtures with ground-truth state graph; exhaustive checker; cycle tests.

**Gate:** Constructive no-repeat assumptions are enforced only where applicable; cache keys and cycle handling are sound for tested fixtures.

**Evidence level:** E1–E2. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M04 — Solver Interoperability

**Entry/work:** Implement SPEC-004 and test Search, DP, B&B, and at least one mature exact solver adapter.

**Required artifacts:** Solver portfolio; bound certificates; oracle instances; reproducible harness.

**Gate:** No heuristic result is mislabeled proof; exact claims match oracle on solvable small instances.

**Evidence level:** E1–E3. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M05 — CASE-001 Search Benchmarks

**Entry/work:** Test multi-scale peak search with local/regional/long-range/directional Deltas and adaptive branching.

**Required artifacts:** Synthetic landscape generator; fixed-budget baselines; held-out landscapes; negative controls.

**Gate:** Preregistered quality and cost endpoints reported with confidence intervals; regressions retained.

**Evidence level:** E3. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M06 — CASE-002 Routing and Scheduling

**Entry/work:** Test constrained school-bus assignment/routing with fleet-first objectives and service safeguards.

**Required artifacts:** Synthetic instances; capacity/time/ride-time validators; CP-SAT/MIP/LNS baselines.

**Gate:** Every reported feasible solution passes independent global validation; solver cost and objective tradeoffs are disclosed.

**Evidence level:** E2–E3. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M07 — CASE-003 Dynamic Matching

**Entry/work:** Test structured correspondence hypotheses, directional weak evidence, abstention and global consistency.

**Required artifacts:** Synthetic/consented graph fixtures; ground-truth variants; calibration and counterexample reports.

**Gate:** No attribution or identity inference claim exceeds available evidence; constraints and abstention are measured.

**Evidence level:** E3. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M08 — CASE-004 Metric Co-Evolution

**Entry/work:** Evaluate proposed metrics separately from solutions under pinned versions and independent decision-utility tests.

**Required artifacts:** Metric registry; Goodhart fixtures; holdout preferences; re-scoring and rollback tests.

**Gate:** Internal score improvements alone never trigger metric promotion; metric versions and tradeoffs are explicit.

**Evidence level:** E2–E3. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M09 — CASE-005 Distributed and Relay

**Entry/work:** Test distributed task contracts, global reconciliation, idempotency, stale workers and typed Relay.

**Required artifacts:** Multi-worker harness; fault injection; source/target transfer benchmark; cost ledger.

**Gate:** No local result is promoted as globally valid without reconciliation; retries are idempotent; transfer requires target validation.

**Evidence level:** E2–E4. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M10 — EXT-001/002 Population and Open-Ended Representation

**Entry/work:** Test population search, operator genesis, migration, novelty and semantic preservation.

**Required artifacts:** Island-model baselines; representation manifests; migration fixtures; held-out task families.

**Gate:** Novelty is not mistaken for utility; migrations disclose information loss and invalidate unsound caches.

**Evidence level:** E2–E4. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M11 — EXT-003 Scientific Discovery

**Entry/work:** Test hypothesis generation, experiment discrimination, evidence governance and metric discovery.

**Required artifacts:** Synthetic causal/physical-law tasks; experiment design comparators; independent evidence ledger.

**Gate:** Predictive accuracy is not called causal proof; metric gaming and invalid evidence are caught by controls.

**Evidence level:** E3–E4. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

### M12 — EXT-004 Collective Learning and Controlled RSI

**Entry/work:** Evaluate cross-agent transfer, generator/policy evolution, independent governance, and broad falsification.

**Required artifacts:** Multi-generation sealed evaluations; governance logs; negative-transfer and rollback records; independent replication package.

**Gate:** Any improvement claim is restricted to measured conditions; no guaranteed RSI or AGI claim is inferred.

**Evidence level:** E4–E5. **Disposition:** `NOT_STARTED` initially; then `PASS`, `FAIL`, `INCONCLUSIVE`, `BLOCKED`, or `DEFERRED` with recorded rationale. No milestone is marked passed by this roadmap.

## 5. Hypothesis Traceability Register (TH-001–TH-054)

This register references the canonical detailed tests in [`TESTABLE-HYPOTHESES.md`](TESTABLE-HYPOTHESES.md) and their source questions in [`OPEN-QUESTIONS.md`](OPEN-QUESTIONS.md). The descriptions below are navigation labels, not new or modified hypothesis claims. **All statuses begin `UNTESTED`.** The proposed milestone assignments are implementation priorities, not assertions of test coverage.

| Hypothesis | Open question | Research topic | Proposed first gate | Status |
|---|---|---|---|---|

| [TH-001](TESTABLE-HYPOTHESES.md#th-001) | OQ-001 | What is the smallest useful CDE State Container contract? | M02–M03 | `UNTESTED` |

| [TH-002](TESTABLE-HYPOTHESES.md#th-002) | OQ-002 | Which constraints must be checked before, during, and after a Delta? | M02–M03 | `UNTESTED` |

| [TH-003](TESTABLE-HYPOTHESES.md#th-003) | OQ-003 | Can Delta identity be made stable across contexts? | M02–M03 | `UNTESTED` |

| [TH-004](TESTABLE-HYPOTHESES.md#th-004) | OQ-004 | What are sound state-equivalence rules for memoization? | M02–M03 | `UNTESTED` |

| [TH-005](TESTABLE-HYPOTHESES.md#th-005) | OQ-005 | How should constraint versions evolve without invalidating historical evidence? | M02–M03 | `UNTESTED` |

| [TH-006](TESTABLE-HYPOTHESES.md#th-006) | OQ-006 | How should CDE represent incomplete, probabilistic, or contested evidence? | M02–M03 | `UNTESTED` |

| [TH-007](TESTABLE-HYPOTHESES.md#th-007) | OQ-007 | When does adaptive branching beat fixed branching? | M02–M03 | `UNTESTED` |

| [TH-008](TESTABLE-HYPOTHESES.md#th-008) | OQ-008 | Which branching counts are necessary for honest reporting? | M03–M05 | `UNTESTED` |

| [TH-009](TESTABLE-HYPOTHESES.md#th-009) | OQ-009 | How can local, regional, and long-range Deltas be allocated adaptively? | M03–M05 | `UNTESTED` |

| [TH-010](TESTABLE-HYPOTHESES.md#th-010) | OQ-010 | When is a directional field genuinely informative? | M03–M05 | `UNTESTED` |

| [TH-011](TESTABLE-HYPOTHESES.md#th-011) | OQ-011 | How should candidate diversity be measured? | M03–M05 | `UNTESTED` |

| [TH-012](TESTABLE-HYPOTHESES.md#th-012) | OQ-012 | Can a generator learn when *not* to generate? | M03–M05 | `UNTESTED` |

| [TH-013](TESTABLE-HYPOTHESES.md#th-013) | OQ-013 | Exactly when is path-local NoSubtree sound? | M03–M05 | `UNTESTED` |

| [TH-014](TESTABLE-HYPOTHESES.md#th-014) | OQ-014 | How can Loop Back be controlled without forbidding useful revisitation? | M03–M05 | `UNTESTED` |

| [TH-015](TESTABLE-HYPOTHESES.md#th-015) | OQ-015 | Which path histories are sufficient for future equivalence? | M03–M05 | `UNTESTED` |

| [TH-016](TESTABLE-HYPOTHESES.md#th-016) | OQ-016 | Can Leftovers become a reliable research memory? | M04–M06 | `UNTESTED` |

| [TH-017](TESTABLE-HYPOTHESES.md#th-017) | OQ-017 | What is CDE's incremental value over mature solvers? | M04–M06 | `UNTESTED` |

| [TH-018](TESTABLE-HYPOTHESES.md#th-018) | OQ-018 | When can DP memoization be safely shared across Delta paths? | M04–M06 | `UNTESTED` |

| [TH-019](TESTABLE-HYPOTHESES.md#th-019) | OQ-019 | How should admissible bounds be represented and verified? | M04–M06 | `UNTESTED` |

| [TH-020](TESTABLE-HYPOTHESES.md#th-020) | OQ-020 | Can a solver portfolio improve robustness across problem families? | M04–M06 | `UNTESTED` |

| [TH-021](TESTABLE-HYPOTHESES.md#th-021) | OQ-021 | How should optimality and infeasibility claims survive dynamic changes? | M04–M06 | `UNTESTED` |

| [TH-022](TESTABLE-HYPOTHESES.md#th-022) | OQ-022 | How can metric quality be evaluated independently of score increases? | M04–M06 | `UNTESTED` |

| [TH-023](TESTABLE-HYPOTHESES.md#th-023) | OQ-023 | When should metrics be scalar, lexicographic, vector-valued, or partial orders? | M08 | `UNTESTED` |

| [TH-024](TESTABLE-HYPOTHESES.md#th-024) | OQ-024 | Can metric drift be detected before it corrupts search? | M08 | `UNTESTED` |

| [TH-025](TESTABLE-HYPOTHESES.md#th-025) | OQ-025 | How should conflicting perspectives be reconciled? | M08 | `UNTESTED` |

| [TH-026](TESTABLE-HYPOTHESES.md#th-026) | OQ-026 | Can a system discover useful measurement features rather than merely reweight them? | M08 | `UNTESTED` |

| [TH-027](TESTABLE-HYPOTHESES.md#th-027) | OQ-027 | How can distributed CDE guarantee coherent commits? | M08 | `UNTESTED` |

| [TH-028](TESTABLE-HYPOTHESES.md#th-028) | OQ-028 | What is the right lease, epoch, and idempotency model? | M08 | `UNTESTED` |

| [TH-029](TESTABLE-HYPOTHESES.md#th-029) | OQ-029 | How should search overlap and shared incumbents be managed? | M08 | `UNTESTED` |

| [TH-030](TESTABLE-HYPOTHESES.md#th-030) | OQ-030 | What constitutes a valid Method Relay artifact? | M09 | `UNTESTED` |

| [TH-031](TESTABLE-HYPOTHESES.md#th-031) | OQ-031 | When does repeated execution become Temporal Relay? | M09 | `UNTESTED` |

| [TH-032](TESTABLE-HYPOTHESES.md#th-032) | OQ-032 | Can negative transfer be predicted and contained? | M09 | `UNTESTED` |

| [TH-033](TESTABLE-HYPOTHESES.md#th-033) | OQ-033 | What evidence distinguishes sharing from Collective Learning? | M09 | `UNTESTED` |

| [TH-034](TESTABLE-HYPOTHESES.md#th-034) | OQ-034 | How can representational bottlenecks be diagnosed? | M09 | `UNTESTED` |

| [TH-035](TESTABLE-HYPOTHESES.md#th-035) | OQ-035 | Can new operators expand reachable solutions safely? | M09 | `UNTESTED` |

| [TH-036](TESTABLE-HYPOTHESES.md#th-036) | OQ-036 | Which representation migrations preserve task meaning? | M09 | `UNTESTED` |

| [TH-037](TESTABLE-HYPOTHESES.md#th-037) | OQ-037 | How can novelty be measured without rewarding meaningless complexity? | M10 | `UNTESTED` |

| [TH-038](TESTABLE-HYPOTHESES.md#th-038) | OQ-038 | Can multiple representations coexist coherently? | M10 | `UNTESTED` |

| [TH-039](TESTABLE-HYPOTHESES.md#th-039) | OQ-039 | Can CDE generate falsifiable rather than merely plausible hypotheses? | M10 | `UNTESTED` |

| [TH-040](TESTABLE-HYPOTHESES.md#th-040) | OQ-040 | Can experiment selection maximize information gain under cost constraints? | M10 | `UNTESTED` |

| [TH-041](TESTABLE-HYPOTHESES.md#th-041) | OQ-041 | How can correlation, prediction, and causation remain distinct? | M10 | `UNTESTED` |

| [TH-042](TESTABLE-HYPOTHESES.md#th-042) | OQ-042 | Can metric discovery improve scientific measurement rather than reward gaming? | M10 | `UNTESTED` |

| [TH-043](TESTABLE-HYPOTHESES.md#th-043) | OQ-043 | Can unresolved Leftovers drive useful autonomous questions? | M10 | `UNTESTED` |

| [TH-044](TESTABLE-HYPOTHESES.md#th-044) | OQ-044 | What is the smallest measurable unit of collective learning? | M11 | `UNTESTED` |

| [TH-045](TESTABLE-HYPOTHESES.md#th-045) | OQ-045 | Can competing agents improve evidence quality? | M11 | `UNTESTED` |

| [TH-046](TESTABLE-HYPOTHESES.md#th-046) | OQ-046 | How can generator and policy evolution be validated independently? | M11 | `UNTESTED` |

| [TH-047](TESTABLE-HYPOTHESES.md#th-047) | OQ-047 | Under what conditions does recursive improvement persist? | M11 | `UNTESTED` |

| [TH-048](TESTABLE-HYPOTHESES.md#th-048) | OQ-048 | What is the appropriate boundary between agent autonomy and governance? | M11 | `UNTESTED` |

| [TH-049](TESTABLE-HYPOTHESES.md#th-049) | OQ-049 | How can the system preserve plural perspectives without forced convergence? | M12 | `UNTESTED` |

| [TH-050](TESTABLE-HYPOTHESES.md#th-050) | OQ-050 | Which termination guarantees survive dynamic operator creation? | M12 | `UNTESTED` |

| [TH-051](TESTABLE-HYPOTHESES.md#th-051) | OQ-051 | What is the strongest defensible completeness claim? | M12 | `UNTESTED` |

| [TH-052](TESTABLE-HYPOTHESES.md#th-052) | OQ-052 | Can transferability be predicted from structural similarity? | M12 | `UNTESTED` |

| [TH-053](TESTABLE-HYPOTHESES.md#th-053) | OQ-053 | When does CDE add explanatory value beyond a generic orchestrator? | M12 | `UNTESTED` |

| [TH-054](TESTABLE-HYPOTHESES.md#th-054) | OQ-054 | What would count as evidence *against* the overall CDE thesis? | M12 | `UNTESTED` |

## 6. Experimental Profiles and Benchmark Families

### P1 — Finite exact-oracle semantics

Use small, fully enumerated state graphs and fixed finite Delta universes. Generate deliberately malformed constraints, duplicate states, unsound equivalence keys, cycles, and incorrect pruning certificates. Compare CDE transition logs with an independent oracle enumerator. **Primary endpoints:** false commits, false proof claims, state coverage under declared assumptions, and replay consistency. **Pass criterion:** zero false commits or unsupported proof statuses in mandatory fixtures; this is not a statistical claim of zero real-world defect probability.

### P2 — Continuous multi-peak optimization (CASE-001)

Use seeded synthetic functions with known or high-confidence optima, constrained regions, narrow ridges, deceptive local peaks, and shifted coordinate systems. Compare random search, multistart local search, CMA-ES or an appropriate evolutionary baseline, Bayesian optimization where evaluation budgets justify it, and fixed/adaptive CDE variants. Report best feasible objective at equal evaluation budget, time-to-target, wall-clock, seed variability, and overhead. Distinguish changes in reachability from improvements in sampling efficiency.

### P3 — School-bus assignment and routing (CASE-002)

Use synthetic districts with explicit school schedules, stops, students, capacities, accessibility, travel-time matrices, and maximum ride-time limits. Optimize **physical vehicle count** as the primary objective while treating safety, capacity, and scheduling as hard constraints and service burden as an explicitly weighted or lexicographic secondary objective. Compare direct CP-SAT/MIP, routing heuristics, large-neighborhood search, and CDE with identical input information. Report fleet size, ride-time distribution, route length, infeasibility, compute cost, and optimality gaps where certified. Do not equate an unverified route plan with a feasible schedule.

### P4 — Dynamic structural matching (CASE-003)

Generate graph/ontology matching tasks with one-to-one and one-to-many correspondences, missing attributes, contradictory evidence, structural drift, and abstention options. Compare rule-based, graph matching, probabilistic, and appropriate learned baselines. Report precision/recall, calibration, abstention, global consistency violations, false positives, and provenance completeness. Avoid claims about identifying real persons unless independent authorization and rigorous safeguards exist; synthetic benchmarks are the default.

### P5 — Metric discovery and Goodhart stress (CASE-004)

Construct tasks where an easily optimized proxy conflicts with independent decision utility. Include versioned metric proposals, held-out decision labels or stakeholder constraints, and re-scoring rules. Compare fixed metrics, weight tuning, multiobjective/lexicographic methods, and metric co-evolution. Report utility under the **independent evaluation standard**, proxy–utility divergence, promotion error, fairness diagnostics, and metric instability. Candidate metrics must not choose their own acceptance criterion.

### P6 — Distributed orchestration and Method Relay (CASE-005)

Run single-worker, embarrassingly parallel, distributed island-model, distributed LNS, and CDE orchestrator variants. Inject worker crashes, stale leases, duplicate retries, delayed messages, overlapping partitions, and inconsistent local constraints. Evaluate global feasibility, idempotency, throughput, wall-clock, communication overhead, coordination costs, and source-to-target method transfer. A source-worker certificate is not automatically a global certificate.

### P7 — Population, representation, and operator evolution (EXT-001/002)

Test population diversity, migration between islands, operator composition, new primitive synthesis, and representation adapters. Include synthetic tasks with **known expressivity bottlenecks** and negative controls where a fixed representation is already adequate. Compare hand-designed operators, genetic programming, program synthesis, and fixed portfolios. Report semantic preservation, reachability under a bounded grammar, held-out utility, novelty–utility divergence, migration cost, and negative transfer.

### P8 — Scientific hypothesis and experiment discovery (EXT-003)

Use synthetic causal systems and simulated scientific environments where ground truth is accessible to evaluators but hidden from agents. Compare random/greedy experiment selection, Bayesian experimental design, active learning, and structured CDE hypothesis exploration. Report discrimination among competing hypotheses, evidence calibration, intervention utility, false causal conclusions, and experimental cost. Predictive fit alone does not establish causal identification.

### P9 — Collective learning and controlled RSI (EXT-004)

Use multiple source and target task families with deliberately compatible and incompatible methods. Compare isolated agents, shared raw logs, shared validated methods, and CDE Relay with governance. Test successive generator/policy versions against a sealed, refreshed test distribution and frozen baselines. Report independent target gains, negative transfer, cost-adjusted collective benefit, regression rate, authorization failures, and rollback quality. A single successful generation does not establish sustained recursive improvement.

## 7. Comparator and Fairness Protocol

For each task family, preregister at least one **mature domain-specific baseline** and one **minimal CDE ablation**. Match task information, hardware where feasible, random seeds or instance pairing, wall-clock or evaluation budgets, tuning opportunities, and feasibility rules. Report both fixed-evaluation and fixed-wall-clock comparisons when orchestration overhead is material. Separate solver performance from CDE wrapper overhead by running the same solver directly and under the CDE adapter. For distributed tests, include communication and idle-worker cost, not only CPU time spent on successful subproblems.

**Do not cherry-pick best runs.** Report medians, dispersion, failures, timeouts, infeasible returns, and missing runs. A performance claim must state the task distribution, comparator, metric, budget, hardware, version, sample size, confidence interval, and relevant limitations.

## 8. Pre-Registration and Statistical Decision Rules

A confirmatory experiment must freeze the following **before** inspecting final test outcomes: primary endpoint, primary comparison, minimum practically important effect (or equivalence margin), randomization/seed plan, sample-size or precision rationale, stopping rule, multiple-comparison treatment, exclusion criteria, and the independence unit (task instance, district, graph, or independent run). The evaluator must retain sealed final-test tasks, or use an external independent evaluator.

Use paired comparisons where both methods run on the same instances. Report effect sizes and uncertainty intervals. If the protocol has no defensible effect threshold, label the study **exploratory**. Correctness gates may use deterministic adversarial fixtures; statistical utility claims require replicated samples. Multiple generations tested on the same holdout can overfit it, so rotate sealed evaluation sets or use an independent final test.

### 8.1 Claim disposition

- `SUPPORTED_WITH_SCOPE`: primary result and robustness checks meet preregistered criteria; precise scope disclosed.
- `NOT_SUPPORTED`: data do not establish the proposed effect; avoid treating nonsignificance as proof of no effect.
- `CONTRADICTED`: evidence conflicts with the directional prediction or mandatory invariant.
- `INCONCLUSIVE`: insufficient precision, execution failure, or conflicting results.
- `UNTESTED`: no completed study.

A hypothesis may be supported in one domain and contradicted in another. Keep both records.

## 9. Cross-Cutting Negative Controls

| ID | Injected failure or misleading signal | Required detection |
|---|---|---|
| NC-01 | Constraint validator returns UNKNOWN | No commit as PASS |
| NC-02 | Unsound state-equivalence key merges distinct futures | Cache claim rejected |
| NC-03 | Heuristic pruning labeled exact proof | Proof status rejected |
| NC-04 | Repeated Delta decision on constructive NoSubtree path | Path contract violation |
| NC-05 | Legitimate reconfiguration revisits a state | Not incorrectly forbidden |
| NC-06 | M proposals counted as N committed edges | Accounting mismatch |
| NC-07 | Soft directional score conflicts with hard constraint | Hard constraint wins |
| NC-08 | Weak evidence mistaken for proof | Evidence claim rejected |
| NC-09 | Proxy metric improves while external utility worsens | Metric not promoted |
| NC-10 | Cross-version scores compared without re-scoring | Comparison blocked |
| NC-11 | Local distributed solutions jointly violate global capacity | Reconciliation fails |
| NC-12 | Duplicate retry commits twice | Idempotency preserved |
| NC-13 | Stale worker reports under old constraint version | Quarantined/revalidated |
| NC-14 | Source method transferred to incompatible target | Adoption rejected |
| NC-15 | Repeated evidence samples are perfectly correlated | No false independence claim |
| NC-16 | Novel operator is semantic duplicate | Novelty challenged |
| NC-17 | Representation migration loses required observable | Migration rejected or scoped |
| NC-18 | Candidate generator modifies own evaluator | Governance boundary enforced |
| NC-19 | New RSI policy overfits repeated holdout | Independent test reveals regression |
| NC-20 | Unresolved Leftover silently dropped | Traceability failure |
| NC-21 | Rollback attempted after irreversible external action | Limitation explicitly recorded |
| NC-22 | Strong direct solver outperforms CDE wrapper | Negative result retained |
| NC-23 | Wrong proof certificate accepted by an adapter | Independent checker rejects |
| NC-24 | CDE makes universal optimality/AGI claim from finite tests | Claim-governance failure |

## 10. Ablation and Factorial Design

| ID | Remove or freeze | Contribution isolated |
|---|---|---|
| AB-01 | Adaptive M-to-N | Dynamic branch control |
| AB-02 | Directional field | Guidance vs unguided proposals |
| AB-03 | Regional Deltas | Mid-scale exploration |
| AB-04 | Long-range Deltas | Escape from local regions |
| AB-05 | Path memory | Redundant search prevention |
| AB-06 | Future-equivalence memoization | DP benefit |
| AB-07 | Bound-based pruning | B&B contribution |
| AB-08 | Solver portfolio | Orchestrator vs fixed solver |
| AB-09 | Constraint precheck | Early rejection efficiency |
| AB-10 | Independent constraint revalidation | Safety contribution |
| AB-11 | Metric evolution | Metric discovery contribution |
| AB-12 | Independent metric holdout | Goodhart control |
| AB-13 | Multi-worker dispatch | Distribution contribution |
| AB-14 | Global reconciliation | Distributed feasibility |
| AB-15 | Structural Relay | Cross-task transfer |
| AB-16 | Negative-transfer quarantine | Transfer safety |
| AB-17 | Population diversity | Premature convergence |
| AB-18 | Operator genesis | New search operators |
| AB-19 | Representation evolution | New state schemas |
| AB-20 | Migration validation | Semantic integrity |
| AB-21 | Counterexample agents | Adversarial testing |
| AB-22 | Generator meta-evolution | RSI-specific contribution |
| AB-23 | Policy meta-evolution | Control-policy contribution |
| AB-24 | Governance and rollback | Unauthorized regressions |

Use targeted factorial studies for interactions (e.g., directional field × long-range Deltas; representation × solver; Relay × independent validation). A large ablation grid may be prohibitively expensive: preregister a prioritized subset, then treat additional comparisons as exploratory.

## 11. Conformance Gates by Specification

| Specification | Mandatory contract family | Representative fixtures | Acceptance |
|---|---|---|---|
| SPEC-001 | State Container and Constraint | schema mismatch, atomic commit, UNKNOWN/ERROR, provenance | Zero unauthorized commits in mandatory tests |
| SPEC-002 | Delta Generation and Branch | type checks, M/Q/N/N_c/K ledger, invalid Delta, deduplication | Exact ledger reconciliation |
| SPEC-003 | Path Memory and Loop Control | NoSubtree, YesSubtree, Loop Back, cache keys, cycles | No unsound equivalence/pruning in oracle fixtures |
| SPEC-004 | Solver and Optimization | bounds, statuses, certificates, solver adapters | Independent certificate checking; no unsupported optimality |
| SPEC-005 | Distributed Exploration and Relay | leases, fencing, retries, global constraints, typed Relay | Idempotent commits and version-coherent reconciliation |
| SPEC-006 | Metric and Recursive Evolution | versioning, heldout evaluation, promotion, rollback | No self-certification or unauthorized policy promotion |

Case and Extension conformance suites should be mapped into a machine-readable test manifest with `test_id`, `source_document`, `requirement`, `fixture`, `oracle`, `result`, and `artifact_hash`. A count of written test descriptions is not a count of executed tests. Required `FAIL` or `UNKNOWN` results cannot be averaged away by unrelated passing tests.

## 12. Independent Validation and Governance

Validation must be separated from proposal generation in authority, sealed evaluation data, and promotion controls. An evaluator may use the same organization or software infrastructure, but independence claims must accurately describe any shared models, datasets, or incentives. The candidate cannot rewrite its success metric, its held-out test set, or the gate that authorizes itself.

**Promotion lifecycle:** `PROPOSED → TYPE_CHECKED → SANDBOXED → CONSTRAINT_VALIDATED → HELDOUT_EVALUATED → GOVERNANCE_REVIEWED → STAGED → ACTIVE_UNDER_SCOPE → MONITORED`; branches include `REJECTED`, `DEFERRED`, `QUARANTINED`, `ROLLED_BACK`, and `RETIRED`.

Every promotion record should identify approved scope, signer/authority, source and target versions, independent evaluation artifact, known limitations, monitoring signals, and rollback checkpoint. Rollback may be impossible for irreversible real-world actions; experiments should use synthetic or sandboxed environments unless independently authorized and safe.

## 13. Reproducibility and Evidence Manifest

```yaml
experiment_id: "cde-case001-paired-study-001"
status: "PLANNED"  # not an observed result
hypothesis_ids: ["TH-001"]  # replace with actual tested hypotheses
benchmark_family: "synthetic-multipeak"
preregistration_ref: "protocols/cde-case001-paired-study-001.yaml"
source_documents: ["CASE-001", "SPEC-001", "SPEC-002", "SPEC-004"]
task_instance_manifest_ref: "instances/manifest.json"
train_dev_test_split_ref: "splits/manifest.json"
independent_evaluator_ref: "evaluators/evaluator-v1"
state_schema_version: "v1"
operator_manifest_version: "v1"
constraint_bundle_version: "v1"
metric_version: "v1"
solver_version: "v1"
generator_version: "v1"
policy_version: "v1"
validation_protocol_version: "v1"
seed_list: []  # fill before execution
hardware_ref: null
budget:
  objective_evaluations: null
  wall_clock_seconds: null
  tuning_cost_included: true
  validation_cost_included: true
  communication_cost_included: true
primary_endpoint: null
minimum_practical_effect: null
sample_size_rationale: null
comparison_method: null
multiple_comparison_policy: null
stopping_rule: null
run_artifacts: []
results: null
limitations: []
```

The manifest deliberately leaves unknown quantities `null`; do not fabricate measurements. Store exact code commit hashes, dependency lockfiles, dataset licensing, benchmark generator seeds, validation code, raw logs, failed runs, and a machine-readable status for each claim. Reports should be independently reproducible from raw artifacts, not only summary charts.

## 14. Proposed Repository Validation Workspace

```text
validation/
├── README.md
├── contracts/
│   ├── state-container.schema.json
│   ├── delta-contract.schema.json
│   ├── constraint-result.schema.json
│   ├── solver-result.schema.json
│   ├── relay-artifact.schema.json
│   └── metric-promotion.schema.json
├── preregistrations/
│   ├── case-001.yaml
│   ├── case-002.yaml
│   ├── case-003.yaml
│   ├── case-004.yaml
│   └── case-005.yaml
├── benchmarks/
│   ├── multipeak/
│   ├── school-bus/
│   ├── structural-matching/
│   ├── metric-discovery/
│   └── distributed-relay/
├── tests/
│   ├── conformance/
│   ├── property-based/
│   ├── counterexamples/
│   ├── fault-injection/
│   └── migration/
├── baselines/
├── runs/
├── reports/
│   ├── hypothesis-status.csv
│   ├── negative-results.md
│   ├── limitations.md
│   └── replication-notes.md
└── governance/
    ├── promotion-log.yaml
    ├── rollback-log.yaml
    └── independent-review.md
```

This tree is a **proposal**. No listed executable files or empirical reports are asserted to exist.

## 15. Claim Review and Falsification

Before publication of any empirical result, reviewers should answer:

1. Is the claim an architectural property, formal conditional theorem, benchmark observation, or extrapolation?
2. Is its reference baseline strong, current, and given fair tuning and information?
3. Are hard constraints independently validated and violations reported?
4. Are runtime, synthesis, validation, and communication costs all included?
5. Are version changes and metric rescoring explicit?
6. Are task splits and independent units appropriate?
7. Were negative controls and expected failure cases actually executed?
8. Can an independent reviewer replay the result from raw artifacts?
9. Are negative transfer, regressions, and unsuccessful runs disclosed?
10. What specific result would refute or substantially narrow the claim?

**Framework-level falsification (TH-054):** Compare integrated CDE against direct mature solvers and simpler orchestrators across multiple predeclared task families. If CDE adds overhead without net benefit, violates core invariants, or fails to transfer beyond narrow examples, explicitly restrict or reject the corresponding generality claims. A modular architecture may still be useful for traceability even if it is not a performance improvement; these contributions must be reported separately.

## 16. Research Release Gates

| Release stage | Required readiness | Permitted description |
|---|---|---|
| R0 — Theory repository | Coherent 27-paper corpus, accurate links, metadata, scope/limitations | Proposed framework and research program |
| R1 — Contract reference | Executable schemas, conformance reports, reproducible fixtures | Reference implementation with scoped conformance |
| R2 — Benchmarked MVP | CASE-001 results and fair baselines, independent validation | Measured outcomes on named synthetic tasks |
| R3 — Multi-domain study | CASE-002–004 outcomes with strong comparators | Scoped cross-domain findings |
| R4 — Distributed/Relay | CASE-005 fault and transfer experiments | Validated distributed and transfer behaviors |
| R5 — Collective RSI | Sealed multigeneration tests and independent replication | Limited evidence of controlled recursive improvements, if observed |

A **v1.0.0 DOI release of the theoretical repository may proceed at R0**, provided it is clearly labeled as a research framework, the metadata and references are accurate, and no unperformed tests are claimed to have passed. Do not delay a theory publication merely because later experiments are not complete; equally, do not imply R2–R5 evidence from an R0 DOI.

## 17. Collaboration and Structural Relay

External contributors may submit new Deltas, solvers, metrics, counterexamples, negative results, benchmark instances, migration adapters, and replication reports. Contributions should include an applicability contract, source versions, reproducible evidence, license, and independent validation scope. A Method Relay artifact is not approved for target use merely because it worked on the source task. Preserve rejected proposals and Leftovers as traceable research artifacts; repeated executions count as evidence Relay only when their dependence and provenance are recorded.

## 18. Decision Register and Maintenance

Maintain a public or appropriately access-controlled decision log with: `decision_id`, `date`, `milestone`, `hypothesis_ids`, `evidence_refs`, `baseline_versions`, `reviewer`, `decision`, `scope`, `limitations`, and `next_action`. Update the roadmap when a protocol changes, with a changelog explaining whether the change was made before or after outcome inspection. Avoid retroactive relabeling of exploratory findings as confirmatory evidence.

**Current declared state:** all milestones `NOT_STARTED`; TH-001–TH-054 `UNTESTED`; executable conformance and benchmark results `NOT REPORTED`. This is a statement about this roadmap's evidence register, not a claim that no external researcher has independently studied related methods.

## 19. Final Principle

CDE's validation program should be **contracts first, conformance before runtime claims, fair baselines before performance claims, independent evaluation before promotion, and explicit Leftovers before claims of completeness**. The research objective is to discover *where* CDE mechanisms work, *why* they work, *what they cost*, and *where they fail*. Credible limits are a research result, not an embarrassment.

---

**Research draft:** v1.0.0  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0

