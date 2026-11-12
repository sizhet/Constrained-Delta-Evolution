# Testable Hypotheses — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**Status:** v1.0.0 research protocol draft — hypotheses proposed, not empirically verified  
**Date:** 2026-11-04  
**License:** Apache-2.0  
**Related:** [README](README.md) · [OPEN-QUESTIONS](OPEN-QUESTIONS.md) · [FUTURE-DIRECTIONS](FUTURE-DIRECTIONS.md) · [VALIDATION-ROADMAP](VALIDATION-ROADMAP.md)

> **Research integrity.** Every result in this document is a *proposed test*, not a completed experiment. No performance gains, significance levels, or empirical guarantees are asserted. Negative results and evidence against the framework are publishable outcomes.

## 1. Purpose

This document turns the 54 questions in `OPEN-QUESTIONS.md` into an indexed, falsifiable hypothesis register **TH-001–TH-054**, with a one-to-one question mapping. The goal is to make CDE testable against mature optimization algorithms and credible alternative architectures, not to establish CDE by definition. Each entry states an intervention, baseline, measurement, and an outcome that would limit or contradict the hypothesis. The research program spans 12 Foundations, six Specifications, five Cases, and four Extensions.

## 2. Core Model and Evidence Discipline

For a pinned version of a task, CDE describes a state and transition system:

$$\mathcal P_v=(S_v,\Delta_v,T_v,C_v,G_v,L_v),\qquad s_{t+1}=T_v(s_t,\delta_t),\quad\delta_t\in G_v(s_t).$$

Hard constraints are validated independently of directional/soft scores. Adaptive branching must distinguish **M** proposals, **Q** attempted candidates, **N** realized edges, **N_c** committed edges, and **K** distinct successor states. Every claim is relative to pinned state, operator, constraint, metric, solver, and validation versions. A locally successful candidate is not necessarily globally feasible; a higher internal score is not proof of a better metric; sharing an artifact is not proof of collective learning.

### 2.1 Required hypothesis fields

- **Hypothesis ID / question link:** stable TH and OQ identifiers.
- **Claim:** directional and conditional prediction, not a universal assertion.
- **Test:** intervention, fixtures, task distribution, and comparator.
- **Primary endpoint:** observable measurement, preferably preregistered.
- **Falsifier or limiting result:** evidence against the prediction or its useful scope.
- **Evidence state:** `UNTESTED`, `IN_PROGRESS`, `SUPPORTED_WITH_SCOPE`, `NOT_SUPPORTED`, `CONTRADICTED`, or `INCONCLUSIVE`.

### 2.2 Statistical commitments

For each confirmatory study, pre-register the primary endpoint, minimum practically important effect, sample size/power or precision rationale, stopping rule, and treatment of multiple comparisons. Use paired task instances and independent random seeds where appropriate. Report confidence intervals and distributions, not only best runs. Treat repeated checkpoints from one run as correlated. If no clear effect threshold is known, designate the study exploratory and avoid post-hoc confirmatory claims.

### 2.3 Comparison integrity

Account for all search evaluations, solver tuning, synthesis, validation, migration, orchestration, communication, and retry costs. Give baseline methods comparable information, hardware, wall-clock limits, and tuning opportunity. Include strong direct CP-SAT/MIP/DP/B&B and domain-specialized solvers when applicable. If CDE is merely wrapping the same solver, isolate the orchestration/contract contribution in a controlled comparison.

## 3. Hypothesis Register

The register preserves the OQ ordering and priorities. `UNTESTED` means the document contains no recorded empirical evaluation.


### Core Semantics and State Contracts


#### TH-001 — What is the smallest useful CDE State Container contract?

**Source:** OQ-001 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Minimal typed-state contract versus ad hoc state dictionaries.  
**Test design:** Cross-domain schema conformance and adapter implementation on peak search, routing, and matching.  
**Comparator:** Unstructured dictionaries with equivalent functionality.  
**Primary endpoint(s):** Integration defects, adapter effort, valid transition throughput.  
**Evidence against / limitation:** No reduction in defects or effort after overhead.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-002 — Which constraints must be checked before, during, and after a Delta?

**Source:** OQ-002 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Atomic constraint validation prevents invalid committed states.  
**Test design:** Fault-inject intermediate, failed, and unknown constraint evaluations.  
**Comparator:** Post-hoc validation only.  
**Primary endpoint(s):** Committed violation count and rejection latency.  
**Evidence against / limitation:** Equal safety with lower overhead from post-hoc baseline.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-003 — Can Delta identity be made stable across contexts?

**Source:** OQ-003 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Context-scoped Delta identity prevents false deduplication.  
**Test design:** Generate semantically equal/different Deltas under changing parents and versions.  
**Comparator:** Name-only or payload-hash identity.  
**Primary endpoint(s):** False merge/split rate and downstream correctness.  
**Evidence against / limitation:** Scoped identity adds cost without preventing errors.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-004 — What are sound state-equivalence rules for memoization?

**Source:** OQ-004 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Future-equivalence-aware keys avoid unsound DP merges.  
**Test design:** Construct states with same visible assignment but different future feasibility.  
**Comparator:** Naive state hashing and no memoization.  
**Primary endpoint(s):** Incorrect merges, exactness, memory, runtime.  
**Evidence against / limitation:** No correctness improvement and unacceptable overhead.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-005 — How should constraint versions evolve without invalidating historical evidence?

**Source:** OQ-005 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Version-aware constraints preserve auditability during changes.  
**Test design:** Replay histories across controlled constraint updates.  
**Comparator:** Overwrite-in-place constraints.  
**Primary endpoint(s):** Replay fidelity and invalid proof reuse.  
**Evidence against / limitation:** No audit benefit versus immutable baseline.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-006 — How should CDE represent incomplete, probabilistic, or contested evidence?

**Source:** OQ-006 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Typed uncertainty states improve decision calibration.  
**Test design:** Inject missing, noisy and contradictory evidence in matching.  
**Comparator:** Binary accept/reject.  
**Primary endpoint(s):** Calibration, abstention utility, constraint violations.  
**Evidence against / limitation:** No calibration or utility improvement.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Adaptive Delta Generation and Branching


#### TH-007 — When does adaptive branching beat fixed branching?

**Source:** OQ-007 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Adaptive M-to-N branching improves budgeted solution quality on heterogeneous tasks.  
**Test design:** Pre-registered peak-search and routing families with variable landscape hardness.  
**Comparator:** Fixed-width search with tuned widths.  
**Primary endpoint(s):** Validated regret at matched evaluations and wall time.  
**Evidence against / limitation:** No advantage over tuned fixed widths.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-008 — Which branching counts are necessary for honest reporting?

**Source:** OQ-008 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Separate M/Q/N/Nc/K ledgers reveal hidden inefficiencies.  
**Test design:** Inject duplicate proposals, failed commits, and state merges.  
**Comparator:** Single branch-count statistic.  
**Primary endpoint(s):** Accounting reconciliation and diagnostic accuracy.  
**Evidence against / limitation:** No additional diagnostic utility.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-009 — How can local, regional, and long-range Deltas be allocated adaptively?

**Source:** OQ-009 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Mixed-granularity Deltas outperform one-scale search on multiscale landscapes.  
**Test design:** Compare local/regional/global portfolios on heterogeneous peak functions.  
**Comparator:** Best single-scale policy.  
**Primary endpoint(s):** Anytime regret and evaluations to threshold.  
**Evidence against / limitation:** Portfolio not better after tuning and overhead.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-010 — When is a directional field genuinely informative?

**Source:** OQ-010 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Directional evidence helps only when predictive of useful transitions.  
**Test design:** Sweep controlled signal-to-noise ratios for directional fields.  
**Comparator:** Unguided proposals and random directions.  
**Primary endpoint(s):** Validated utility versus field reliability.  
**Evidence against / limitation:** No positive dependence or persistent harm.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-011 — How should candidate diversity be measured?

**Source:** OQ-011 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Diversity-aware proposal selection reduces premature collapse.  
**Test design:** Run rugged multimodal benchmarks with equal budgets.  
**Comparator:** Top-score-only selection.  
**Primary endpoint(s):** Coverage, unique basins, regret.  
**Evidence against / limitation:** No quality gain despite added diversity cost.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-012 — Can a generator learn when *not* to generate?

**Source:** OQ-012 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Learned abstention from generating saves budget without losing quality.  
**Test design:** Inject unpromising states and scarce budgets.  
**Comparator:** Always generate and fixed stopping.  
**Primary endpoint(s):** Cost-adjusted quality, missed opportunities.  
**Evidence against / limitation:** Cost saving offset by missed solutions.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Path Memory and Loop Control


#### TH-013 — Exactly when is path-local NoSubtree sound?

**Source:** OQ-013 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** NoSubtree is sound only for immutable finite constructive decision universes.  
**Test design:** Enumerate finite decisions and adversarial dynamic candidate additions.  
**Comparator:** Unrestricted global exclusion.  
**Primary endpoint(s):** Missed valid paths and soundness violations.  
**Evidence against / limitation:** NoSubtree remains sound outside claimed assumptions.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-014 — How can Loop Back be controlled without forbidding useful revisitation?

**Source:** OQ-014 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Version-aware loop control retains useful revisitation.  
**Test design:** Construct reconfiguration and cyclic-state tasks.  
**Comparator:** Global visited-set and no loop guard.  
**Primary endpoint(s):** Valid discoveries, cycle overhead, termination.  
**Evidence against / limitation:** No gain over simple visited-set.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-015 — Which path histories are sufficient for future equivalence?

**Source:** OQ-015 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Future-equivalence needs selected history features.  
**Test design:** Compare memoization under Markov and history-dependent tasks.  
**Comparator:** State-only key and full-history key.  
**Primary endpoint(s):** Correctness, cache hit rate, runtime.  
**Evidence against / limitation:** Compressed history unsound or no efficiency gain.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-016 — Can Leftovers become a reliable research memory?

**Source:** OQ-016 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Structured Leftovers improve later problem resolution.  
**Test design:** Replay unresolved tasks with and without typed failure artifacts.  
**Comparator:** Discarded failure logs.  
**Primary endpoint(s):** Later resolution rate and analyst time.  
**Evidence against / limitation:** No benefit over plain logs.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Solver Integration and Guarantees


#### TH-017 — What is CDE's incremental value over mature solvers?

**Source:** OQ-017 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** CDE adds value only through validated coordination or reusable contracts.  
**Test design:** Matched comparison against tuned standalone solvers.  
**Comparator:** Direct CP-SAT/MIP/DP/heuristics.  
**Primary endpoint(s):** Optimality gap, compute, integration effort.  
**Evidence against / limitation:** Mature solvers dominate all tested regimes.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-018 — When can DP memoization be safely shared across Delta paths?

**Source:** OQ-018 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Verified future-equivalence allows safe shared memoization.  
**Test design:** Differential test DP keys on finite enumerated problems.  
**Comparator:** No memoization and unverified memoization.  
**Primary endpoint(s):** Exact result agreement and speed.  
**Evidence against / limitation:** Unsound merges or no net gain.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-019 — How should admissible bounds be represented and verified?

**Source:** OQ-019 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Typed admissible-bound contracts prevent false optimality claims.  
**Test design:** Inject optimistic and non-admissible bounds into B&B.  
**Comparator:** Unverified numeric bounds.  
**Primary endpoint(s):** False pruning and certificate validity.  
**Evidence against / limitation:** Same validity without typed contracts.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-020 — Can a solver portfolio improve robustness across problem families?

**Source:** OQ-020 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Solver portfolios improve robustness across heterogeneous instances.  
**Test design:** Run heterogeneous benchmarks with sealed solver-selection tuning.  
**Comparator:** Best tuned single solver.  
**Primary endpoint(s):** Performance profile and cost-adjusted regret.  
**Evidence against / limitation:** No robust advantage versus single solver.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-021 — How should optimality and infeasibility claims survive dynamic changes?

**Source:** OQ-021 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Version-pinned certificates remain trustworthy under controlled changes.  
**Test design:** Change metrics, constraints and representations mid-search.  
**Comparator:** Unversioned certificate reuse.  
**Primary endpoint(s):** Invalid claim rate and replay correctness.  
**Evidence against / limitation:** Version pinning not needed for correctness.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Solution–Metric Co-Evolution


#### TH-022 — How can metric quality be evaluated independently of score increases?

**Source:** OQ-022 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Independent metric validation resists reward hacking.  
**Test design:** Synthetic proxy misalignment with held-out ground-truth utility.  
**Comparator:** Choose metric by own score.  
**Primary endpoint(s):** Held-out utility and proxy exploitation.  
**Evidence against / limitation:** Self-scored metric selection matches or exceeds.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-023 — When should metrics be scalar, lexicographic, vector-valued, or partial orders?

**Source:** OQ-023 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Objective representation should match preference structure.  
**Test design:** Compare scalar, lexicographic and Pareto metrics under known preferences.  
**Comparator:** Weighted sum only.  
**Primary endpoint(s):** Preference violations and decision utility.  
**Evidence against / limitation:** No improvement from richer forms.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-024 — Can metric drift be detected before it corrupts search?

**Source:** OQ-024 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Metric drift monitors detect degradation earlier.  
**Test design:** Induce gradual proxy drift with delayed feedback.  
**Comparator:** No drift detection and fixed thresholds.  
**Primary endpoint(s):** Detection delay, false alarms, utility loss.  
**Evidence against / limitation:** No net utility gain after alarm cost.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-025 — How should conflicting perspectives be reconciled?

**Source:** OQ-025 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Perspective-aware metrics preserve legitimate disagreement.  
**Test design:** Simulate conflicting stakeholder priorities.  
**Comparator:** Forced scalar consensus.  
**Primary endpoint(s):** Regret by perspective and conflict transparency.  
**Evidence against / limitation:** No advantage over single metric.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-026 — Can a system discover useful measurement features rather than merely reweight them?

**Source:** OQ-026 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Feature-genesis discovers useful measures beyond reweighting.  
**Test design:** Hide relevant latent features in controlled tasks.  
**Comparator:** Fixed feature reweighting.  
**Primary endpoint(s):** Held-out decision utility and feature validity.  
**Evidence against / limitation:** No benefit after feature cost.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Distributed CDE and Structural Relay


#### TH-027 — How can distributed CDE guarantee coherent commits?

**Source:** OQ-027 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Versioned global reconciliation prevents invalid distributed commits.  
**Test design:** Inject conflicting concurrent assignments.  
**Comparator:** Independent local commits.  
**Primary endpoint(s):** Global violations and throughput.  
**Evidence against / limitation:** Same safety with simpler commit policy.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-028 — What is the right lease, epoch, and idempotency model?

**Source:** OQ-028 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Lease/epoch/fencing plus idempotency prevents stale duplicate effects.  
**Test design:** Fault inject delayed workers, retries and partitions.  
**Comparator:** At-least-once naive execution.  
**Primary endpoint(s):** Duplicate commits, stale writes, recovery cost.  
**Evidence against / limitation:** No correctness gain.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-029 — How should search overlap and shared incumbents be managed?

**Source:** OQ-029 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Overlap-aware work partitioning balances exploration and redundancy.  
**Test design:** Compare controlled overlap levels on multimodal search.  
**Comparator:** Disjoint partitions and full replication.  
**Primary endpoint(s):** Unique discoveries, coordination cost, quality.  
**Evidence against / limitation:** Overlap policy dominated by simple baselines.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-030 — What constitutes a valid Method Relay artifact?

**Source:** OQ-030 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Typed Method Relay increases reliable cross-task reuse.  
**Test design:** Transfer macros across compatible and incompatible task families.  
**Comparator:** Unstructured method notes.  
**Primary endpoint(s):** Target validation pass, benefit, negative transfer.  
**Evidence against / limitation:** No better transfer outcomes.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-031 — When does repeated execution become Temporal Relay?

**Source:** OQ-031 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Temporal Relay gains require distinct informative repetitions.  
**Test design:** Repeat experiments with correlated and independent seeds.  
**Comparator:** Count all repeats equally.  
**Primary endpoint(s):** Effective sample size and calibration.  
**Evidence against / limitation:** Typed repetition does not improve calibration.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-032 — Can negative transfer be predicted and contained?

**Source:** OQ-032 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Compatibility screening reduces negative transfer.  
**Test design:** Shift target constraints and representations systematically.  
**Comparator:** Blind method reuse.  
**Primary endpoint(s):** Harmful adoption rate and missed positive transfers.  
**Evidence against / limitation:** Screening causes more net harm than it prevents.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-033 — What evidence distinguishes sharing from Collective Learning?

**Source:** OQ-033 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Collective learning requires held-out target gains.  
**Test design:** Multi-agent source-to-target tasks with controlled sharing.  
**Comparator:** No sharing and raw-result sharing.  
**Primary endpoint(s):** Incremental held-out target utility net of cost.  
**Evidence against / limitation:** No measurable improvement over raw sharing.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Open-Ended Operators and Representations


#### TH-034 — How can representational bottlenecks be diagnosed?

**Source:** OQ-034 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Bottleneck diagnostics separate representation failure from weak search.  
**Test design:** Inject operator-limited and budget-limited tasks.  
**Comparator:** Stagnation-only heuristic.  
**Primary endpoint(s):** Classification accuracy and intervention benefit.  
**Evidence against / limitation:** Cannot distinguish bottleneck causes.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-035 — Can new operators expand reachable solutions safely?

**Source:** OQ-035 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Operator genesis expands reachable states in restricted grammars.  
**Test design:** Use finite fixtures with provably missing primitive operators.  
**Comparator:** Fixed vocabulary and tuned generator.  
**Primary endpoint(s):** Reachable target fraction and validity.  
**Evidence against / limitation:** No expanded reachability.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-036 — Which representation migrations preserve task meaning?

**Source:** OQ-036 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Migration contracts preserve declared observables.  
**Test design:** Apply lossy and lossless schema migrations with adversarial fixtures.  
**Comparator:** Schema-only transformation.  
**Primary endpoint(s):** Semantic mismatches and false equivalence.  
**Evidence against / limitation:** No fewer mismatches.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-037 — How can novelty be measured without rewarding meaningless complexity?

**Source:** OQ-037 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Semantic novelty scoring avoids redundant complexity.  
**Test design:** Generate renamed, equivalent and useful operators.  
**Comparator:** String edit-distance novelty.  
**Primary endpoint(s):** Useful novel fraction and evaluation cost.  
**Evidence against / limitation:** No advantage over simple novelty.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-038 — Can multiple representations coexist coherently?

**Source:** OQ-038 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Multi-view representations help heterogeneous solver tasks.  
**Test design:** Compare coarse/detail and graph/table views.  
**Comparator:** One canonical representation.  
**Primary endpoint(s):** Cost-adjusted utility and consistency errors.  
**Evidence against / limitation:** No benefit or consistency failures.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Scientific Discovery and Autonomous Questioning


#### TH-039 — Can CDE generate falsifiable rather than merely plausible hypotheses?

**Source:** OQ-039 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Structured hypothesis Deltas improve falsifiability.  
**Test design:** Generate hypotheses for synthetic causal mechanisms.  
**Comparator:** Unstructured LLM hypotheses.  
**Primary endpoint(s):** Pre-registered testability and discriminating predictions.  
**Evidence against / limitation:** No better falsifiability.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-040 — Can experiment selection maximize information gain under cost constraints?

**Source:** OQ-040 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Information-gain-guided experiments reduce identification cost.  
**Test design:** Known synthetic hypothesis families and interventions.  
**Comparator:** Random and heuristic experiment choice.  
**Primary endpoint(s):** Posterior identification accuracy per cost.  
**Evidence against / limitation:** No efficiency gain.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-041 — How can correlation, prediction, and causation remain distinct?

**Source:** OQ-041 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Causal claim contracts reduce unsupported causal conclusions.  
**Test design:** Confounded observational and randomized datasets.  
**Comparator:** Predictive-accuracy-only selection.  
**Primary endpoint(s):** False causal claims and calibration.  
**Evidence against / limitation:** No reduction in unsupported claims.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-042 — Can metric discovery improve scientific measurement rather than reward gaming?

**Source:** OQ-042 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Metric discovery improves measurement when validated externally.  
**Test design:** Simulated latent construct with misleading proxy.  
**Comparator:** Proxy score maximization.  
**Primary endpoint(s):** Construct validity and held-out utility.  
**Evidence against / limitation:** Metric changes only raise internal score.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-043 — Can unresolved Leftovers drive useful autonomous questions?

**Source:** OQ-043 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Leftover-guided questioning yields more actionable research.  
**Test design:** Compare questions from typed unresolved artifacts versus generic prompts.  
**Comparator:** Generic question generation.  
**Primary endpoint(s):** Answerability and downstream evidence gain.  
**Evidence against / limitation:** No improvement over generic prompts.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Collective Learning and Structural RSI


#### TH-044 — What is the smallest measurable unit of collective learning?

**Source:** OQ-044 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Validated target gains are measurable as learning units.  
**Test design:** Task transfer with controlled source artifacts and held-out targets.  
**Comparator:** Result sharing only.  
**Primary endpoint(s):** Incremental target utility with provenance.  
**Evidence against / limitation:** No separable learning effect.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-045 — Can competing agents improve evidence quality?

**Source:** OQ-045 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** Adversarial counterexample agents improve evidence integrity.  
**Test design:** Introduce deliberately flawed claims in agent teams.  
**Comparator:** Cooperative reviewers only.  
**Primary endpoint(s):** Detected invalid claims, false rejection, cost.  
**Evidence against / limitation:** No improvement after review cost.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-046 — How can generator and policy evolution be validated independently?

**Source:** OQ-046 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Independent evaluation reduces harmful generator promotions.  
**Test design:** Compare sealed evaluator and self-scored meta-updates.  
**Comparator:** Self-evaluation.  
**Primary endpoint(s):** Held-out regressions and promotion precision.  
**Evidence against / limitation:** No safety/utility gain.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-047 — Under what conditions does recursive improvement persist?

**Source:** OQ-047 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Recursive meta-updates can sometimes sustain gains across task generations.  
**Test design:** Run bounded generations on shifting task families.  
**Comparator:** Frozen generator/policy.  
**Primary endpoint(s):** Held-out trajectory net of meta-cost.  
**Evidence against / limitation:** Gains vanish or regress.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-048 — What is the appropriate boundary between agent autonomy and governance?

**Source:** OQ-048 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Governance boundaries reduce unauthorized self-modification.  
**Test design:** Inject unauthorized metric/validator change proposals.  
**Comparator:** Agent-controlled promotion.  
**Primary endpoint(s):** Unauthorized activation count and utility.  
**Evidence against / limitation:** No safety gain or unacceptable delay.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-049 — How can the system preserve plural perspectives without forced convergence?

**Source:** OQ-049 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Plural perspective portfolios improve stakeholder robustness.  
**Test design:** Conflicting stakeholder preferences with shift.  
**Comparator:** Single forced consensus metric.  
**Primary endpoint(s):** Worst-group regret and Pareto coverage.  
**Evidence against / limitation:** No robustness benefit.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


### Theoretical Boundaries and Framework Falsification


#### TH-050 — Which termination guarantees survive dynamic operator creation?

**Source:** OQ-050 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Dynamic operator genesis invalidates fixed-universe termination assumptions.  
**Test design:** Construct finite versus growing operator universes.  
**Comparator:** Apply static depth bound unchanged.  
**Primary endpoint(s):** Termination correctness and false claims.  
**Evidence against / limitation:** Static theorem holds without premises.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-051 — What is the strongest defensible completeness claim?

**Source:** OQ-051 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** Completeness claims must be scoped to bounded grammar and traversal.  
**Test design:** Finite exhaustive fixtures and open grammars.  
**Comparator:** Unqualified completeness labels.  
**Primary endpoint(s):** Claim correctness and missed reachable states.  
**Evidence against / limitation:** Unscoped claims not misleading.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-052 — Can transferability be predicted from structural similarity?

**Source:** OQ-052 · **Priority:** P2 · **Status:** `UNTESTED`  
**Hypothesis:** Structural similarity can predict conditional transfer benefit.  
**Test design:** Matched source-target structural feature pairs.  
**Comparator:** Random transfer and surface similarity.  
**Primary endpoint(s):** Predictive AUC and target utility.  
**Evidence against / limitation:** No prediction better than chance.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-053 — When does CDE add explanatory value beyond a generic orchestrator?

**Source:** OQ-053 · **Priority:** P1 · **Status:** `UNTESTED`  
**Hypothesis:** CDE adds explanatory value if contracts expose otherwise hidden failure mechanisms.  
**Test design:** Blind audit of CDE versus generic orchestrator traces.  
**Comparator:** Generic orchestration with same solvers.  
**Primary endpoint(s):** Diagnostic accuracy, audit time, cost.  
**Evidence against / limitation:** No explanatory gain.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


#### TH-054 — What would count as evidence *against* the overall CDE thesis?

**Source:** OQ-054 · **Priority:** P0 · **Status:** `UNTESTED`  
**Hypothesis:** The integrated CDE thesis survives only if at least one mechanism shows reproducible net value.  
**Test design:** Multi-domain preregistered benchmark against strong solvers and orchestrators.  
**Comparator:** Tuned non-CDE systems with equal resources.  
**Primary endpoint(s):** Reproducible cost-adjusted utility, correctness, transfer.  
**Evidence against / limitation:** No positive result across domains or systematic harms.  
**Required controls:** version-pinned contracts, matched budgets, held-out instances where relevant, complete cost/provenance logs, and a predeclared failure-handling policy.  
**Interpretation rule:** Any positive finding is limited to the tested task family and validation assumptions; non-significant results alone do not establish equivalence.


## 4. Benchmark Families and Mapping

| Family | Primary papers | Hypothesis emphasis | Strong baselines |
|---|---|---|---|
| Synthetic multiscale peak search | CASE-001, CDE-003, CDE-006 | TH-007–012, TH-017, TH-020 | CMA-ES, differential evolution, Bayesian optimization, tuned multistart/local search |
| Constrained school-bus assignment/routing | CASE-002, SPEC-004 | TH-001–005, TH-017–021, TH-034–036 | CP-SAT, MIP, routing heuristics, LNS |
| Dynamic structural matching | CASE-003, BM-ODWEI interfaces | TH-006, TH-010, TH-034–038 | Graph matching, ILP, probabilistic matching, calibrated classifiers |
| Metric discovery and preference alignment | CASE-004, CDE-007 | TH-022–026, TH-042 | Fixed metrics, preference learning, bilevel optimization |
| Distributed exploration and Method Relay | CASE-005, CDE-009–010 | TH-027–033, TH-044–045 | Parallel independent solvers, island models, distributed LNS |
| Biomimetic population search | EXT-001 | TH-009–012, TH-020 | Island evolution, particle swarm, ACO, novelty search |
| Operator and representation evolution | EXT-002 | TH-034–038, TH-046–047 | Genetic programming, grammar synthesis, expert-designed abstractions |
| Scientific hypothesis and metric discovery | EXT-003 | TH-039–043 | Random/active experimental design, causal discovery baselines |
| Collective intelligence and controlled RSI | EXT-004 | TH-044–049, TH-052–054 | Fixed policy, raw-result sharing, self-scored meta-updates |

A benchmark family is not proof of generalization. Test instances, objective functions, constraint semantics, and independent reference solutions should be documented before results are interpreted.

## 5. Preregistered Experimental Profiles

### Profile P0-A — Contract conformance and soundness

Test the state/Delta/constraint semantics (TH-001–005), NoSubtree assumptions (TH-013), DP future-equivalence (TH-018), bounds and certificates (TH-019, TH-021), and migration soundness (TH-036). Use small finite problems with independent exhaustive or formal reference oracles. The primary outcomes are **false commits, invalid pruning, unsound equivalence, incorrect certificates, and replay failure**. Zero observed errors is not proof of universal correctness; report tested scope and confidence bounds.

### Profile P0-B — Incremental value over mature solvers

Compare direct tuned solvers against the same solvers wrapped in CDE, plus CDE-specific adaptive branching, orchestration, and Relay. Hold solver access and resource accounting constant. Report **cost-adjusted solution quality, correctness, robustness, and engineering overhead**. CDE should be described as a research orchestration framework if direct solvers remain superior on the tested tasks.

### Profile P1-C — Metrics, representations, and transfer

Evaluate held-out preference utility, proxy drift, representation migrations, operator genesis, target-task Method Relay, and negative transfer. Keep metric promotion independent of the optimizing agent. Use synthetic or audited oracles where available, and disclose limits when the true utility function is unknown.

### Profile P1-D — Distributed and collective learning

Run matched single-worker, parallel-independent, raw-sharing, validated-Relay, and governance-controlled configurations. Inject stale workers, duplicate messages, network partitions, and conflicting commits. Distinguish speedup from **additional compute** and transfer from **mere information exchange**.

### Profile P2-E — Open-ended and recursive evolution

Compare frozen generators with candidate operator/representation/policy evolution over several sealed task generations. Reserve genuinely unseen structural task families for final testing. Track regressions, meta-compute cost, and harmful promotions. Do not infer unlimited growth from finite trajectories.

## 6. Cross-Cutting Negative Controls

| Control | Deliberately adverse condition | Expected interpretation |
|---|---|---|
| NC-01 | Easy problem where fixed solver is near-optimal | CDE overhead may dominate |
| NC-02 | Directional field randomized or anticorrelated | Guidance should lose value or be rejected |
| NC-03 | Equivalent Deltas with different names | Novelty must not be overstated |
| NC-04 | Hard constraint changed without version bump | Commit/certificate should be rejected |
| NC-05 | DP states with equal visible keys but different futures | Unsound merge must be detected |
| NC-06 | Metric increases its own score while true utility falls | Self-scoring must not certify quality |
| NC-07 | Representation migration discards a critical variable | Preservation claim must fail |
| NC-08 | Method transferred to incompatible target | No unvalidated adoption |
| NC-09 | Repeated evidence from one correlated source | No false independent sample count |
| NC-10 | Worker with stale lease/epoch | No stale global commit |
| NC-11 | Generator edits evaluator or held-out test set | Governance violation |
| NC-12 | Recursive policy improves training but degrades held-out | No promotion as generalized improvement |
| NC-13 | High diversity with no utility improvement | Diversity is not automatically success |
| NC-14 | More agents but no coordination benefit | No collective-learning claim |
| NC-15 | Apparent speedup from more total hardware | Report normalized cost |
| NC-16 | Open-ended operator grammar without finite coverage | No unqualified completeness claim |
| NC-17 | Failed experiment omitted from Relay registry | Reporting/conformance failure |
| NC-18 | Baseline under-tuned compared with CDE | Comparison invalid |

## 7. Ablation Matrix

Run a subset relevant to each primary hypothesis, rather than treating all ablations as mandatory for every experiment.

| Ablation | Removed or frozen component | Causal question |
|---|---|---|
| AB-01 | Adaptive branching | Benefit of dynamic width |
| AB-02 | Directional guidance | Value of weak-evidence fields |
| AB-03 | Regional/global Deltas | Value of multiscale exploration |
| AB-04 | Path memory | Effect on loops and revisitation |
| AB-05 | Future-equivalence validation | Correctness of DP reuse |
| AB-06 | Bound proof checks | Effect on false pruning |
| AB-07 | Metric independent validation | Reward-hacking susceptibility |
| AB-08 | Metric version pinning | Cross-version comparability |
| AB-09 | Operator genesis | New reachability contribution |
| AB-10 | Representation evolution | Value of changing the state language |
| AB-11 | Migration validation | Semantic drift detection |
| AB-12 | Structural Relay | Cross-task reuse benefit |
| AB-13 | Method applicability screening | Negative-transfer containment |
| AB-14 | Evidence provenance | Audit and replication quality |
| AB-15 | Leftover preservation | Future task discovery |
| AB-16 | Competing counterexample agents | Detection of unsupported claims |
| AB-17 | Independent RSI evaluator | Harmful meta-promotion rate |
| AB-18 | Rollback and monitoring | Recovery after regression |

## 8. Measurement and Accounting Contracts

### 8.1 Task-level accounting

Record `M_proposed`, `Q_attempted`, `N_realized`, `Nc_committed`, `K_unique_successors`, `D_distinct_delta_definitions`, validation calls, solver calls, constraint failures, duplicate successors, and cost. The relationship among counts depends on the explicit edge accounting contract; no universal equality between these quantities is presumed.

### 8.2 Claim statuses

Use `CANDIDATE`, `FEASIBLE`, `BEST_KNOWN`, `BOUNDED_GAP`, `OPTIMAL`, `INFEASIBLE_PROVEN`, `EXHAUSTIVE`, and `UNKNOWN` only with appropriate evidence. Heuristic failure to find a solution is not proof of infeasibility. A bounded-gap or optimality claim must include the relevant admissible bound/certificate and pinned problem version.

### 8.3 Independent validation

The component that proposes a metric, operator, representation, or recursive policy change must not have unrestricted authority to alter the held-out evaluator or authorize its own promotion. Record whether evaluator independence is organizational, procedural, data-based, or model-based; do not claim stronger independence than the design actually supplies.

### 8.4 Relay validity

Every Result, Evidence, Constraint, Method, Policy, Metric, or Leftover Relay should identify source task, versions, provenance, assumptions, target applicability, validation status, and unresolved limitations. Target use requires independent compatibility checks.

## 9. Decision Rules for Claims

- **Supported with scope:** predeclared primary endpoint meets its threshold, safety constraints pass, and independent replication or appropriately qualified evidence supports the result.
- **Not supported:** results do not meet the threshold or are too imprecise; report the confidence interval and scope.
- **Contradicted:** evidence shows meaningful harm, incorrectness, or reversal of the proposed directional effect under the stated conditions.
- **Inconclusive:** underpowered, confounded, or incomplete evidence; do not convert to support.
- **Retired/revised:** a hypothesis may be narrowed or superseded, but retain the original ID and decision history.

A negative result for one mechanism does not logically refute every part of CDE; equally, success on one case does not validate the entire framework. TH-054 is the explicit integrated falsification gate.

## 10. Minimum Reproducibility Record

```yaml
hypothesis_id: TH-001
question_id: OQ-001
study_id: null
study_status: PLANNED
preregistration_uri: null
benchmark_manifest: null
train_dev_test_split_hashes: null
baseline_versions: []
cde_component_versions: []
constraint_version: null
metric_version: null
independent_validator_ref: null
hardware_and_budget: null
seed_manifest: null
primary_endpoint: null
minimum_important_effect: null
sample_size_rationale: null
multiple_testing_policy: null
negative_controls: []
ablations: []
raw_results_uri: null
analysis_script_uri: null
confidence_interval: null
result_status: UNTESTED
known_limitations: []
```

`null` means not yet specified or measured, **not** zero. This record is a template, not a completed study.

## 11. Hypothesis-to-Research-Document Crosswalk

| Hypotheses | Primary documents |
|---|---|
| TH-001–006 | CDE-002, SPEC-001, SPEC-002 |
| TH-007–012 | CDE-003, CDE-006, CASE-001, EXT-001 |
| TH-013–016 | CDE-004, SPEC-003 |
| TH-017–021 | CDE-005, SPEC-004, CASE-002 |
| TH-022–026 | CDE-007, CASE-004, SPEC-006 |
| TH-027–033 | CDE-009, CDE-010, SPEC-005, CASE-005 |
| TH-034–038 | CDE-011, EXT-002, SPEC-006 |
| TH-039–043 | EXT-003, CASE-004, AQSI interfaces |
| TH-044–049 | CDE-010, CDE-011, EXT-004 |
| TH-050–054 | CDE-012, CDE-008, EXT-004 |

## 12. Priority and Dependency Order

**Phase 1 — P0 correctness:** TH-001–004, TH-013, TH-018–019, TH-027–028, TH-036, TH-041, TH-046, TH-048, TH-050–051, TH-054. Establish conformance and the conditions under which claims can be trusted.

**Phase 2 — Incremental optimization value:** TH-007–011, TH-017, TH-020–023. Compare to mature solvers and assess metric validity.

**Phase 3 — Distributed and transferable methods:** TH-029–033, TH-044–045. Test whether method transfer improves target tasks after full costs.

**Phase 4 — Representations and scientific discovery:** TH-024–026, TH-034–043. Evaluate operator/representation discovery and experimental intelligence.

**Phase 5 — Recursive evolution and boundaries:** TH-047, TH-049, TH-052–053 and remaining exploratory items. Avoid unqualified open-ended-growth claims.

This ordering is a suggested dependency sequence, not an assertion that later topics are less important.

## 13. Contribution and Collective Learning Protocol

Contributors may submit a new benchmark, counterexample, baseline, operator, metric, migration, Relay artifact, or study replication. Each contribution should state the linked TH/OQ IDs, assumptions, relevant versions, independent evidence, limitations, and whether it supports or contradicts the registered prediction. Preserve failures and unresolved work as typed Leftovers. Where safety, privacy, or real-world operations are implicated, use appropriately authorized, de-identified, and sandboxed experiments.

## 14. Publication and Interpretation Boundaries

This file is a **research hypothesis register**, not a results paper. It intentionally makes the framework vulnerable to falsification. The central thesis may be narrowed to a useful contractual integration layer even if a generic optimization superiority claim is not supported. Claims of completeness, optimality, causal discovery, collective learning, or recursive improvement require their own evidence; they cannot be inherited from architectural diagrams or from the number of agents involved.

**Next companion:** `VALIDATION-ROADMAP.md` should turn these hypothesis IDs into concrete experiment milestones, acceptance gates, owners, and release criteria.

---

**Version:** v1.0.0 research draft  
**Authors:** Sizhe Tan and GPT-Obot  
**License:** Apache-2.0
