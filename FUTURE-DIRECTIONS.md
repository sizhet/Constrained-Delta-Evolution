# Future Directions — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**Status:** Open research agenda, v1.0.0 draft  
**Date:** 2026-11-04  
**License:** Apache-2.0  
**Repository location:** `FUTURE-DIRECTIONS.md`

> **Research posture.** CDE is a proposed, contract-driven framework. The directions below are hypotheses and implementation opportunities, not claims of demonstrated performance, guaranteed convergence, autonomous scientific discovery, or general intelligence. Negative findings, explicit Leftovers, and independent replication are valuable outputs.

## 1. Purpose and Scope

The CDE research program begins with a simple but extensible abstraction: a **State Container** evolves through typed **Deltas**, while **Constraints** govern admissible transitions and **Generators** propose alternatives. Search can be organized as a tree, a directed acyclic graph, or a cyclic state-transition graph, depending on identity, equivalence, and Loop Back semantics. The proposed architecture extends this core toward solver portfolios, metric co-evolution, distributed exploration, Structural Relay, and controlled recursive improvement.

The next stage should prioritize falsifiable implementations and measurements. A broad conceptual framework becomes scientifically useful only when its components can be isolated, compared with strong alternatives, and shown to succeed—or fail—under stated assumptions.

This agenda is organized by maturity and dependency rather than by speculative importance:

- **Near-term:** executable contracts, small reproducible benchmarks, search correctness, and accounting.
- **Medium-term:** solver integration, metric evolution, representation discovery, and distributed Relay.
- **Long-term:** collective learning, controlled Structural RSI, open-ended capability discovery, and scientific exploration.

The suggested order is not mandatory. Independent researchers may pursue specific modules without implementing the entire CDE architecture.

## 2. The Core Research Contract

For a fixed versioned task context, write

$$
\mathcal{P}_v=(S_v,\Delta_v,T_v,C_v,G_v,L_v,M_v),
\qquad s_{t+1}=T_v(s_t,\delta_t),\quad \delta_t\in G_v(s_t).
$$

Here, $S_v$ is the state space, $\Delta_v$ the typed Delta universe, $T_v$ the transition semantics, $C_v$ the hard-constraint bundle, $G_v$ the proposal mechanism, $L_v$ the path and evidence context, and $M_v$ the metric or preference model. The subscript $v$ matters: representations, operators, constraints, and metrics may evolve, but old results cannot silently be interpreted under new semantics.

A useful implementation must distinguish **proposed** from **attempted**, **realized** from **committed**, and **unique** from **duplicate** successors. At minimum, maintain the ledger:

$$
M=\text{proposals},\quad Q=\text{attempts},\quad N=\text{realized edges},
\quad N_c=\text{committed edges},\quad K=\text{unique successors}.
$$

The counts refer to one declared scope and version. Do not silently equate them or use a global count to describe a local search branch.

### 2.1 Non-negotiable distinctions

1. **Representability** is not **generability**; generability is not budgeted reachability.
2. **Feasible** is not **optimal**; an improved score is not a certificate.
3. **Heuristic rejection** is not **proved infeasibility**.
4. **State identity** is not necessarily **future equivalence**.
5. **Higher internal metric score** is not necessarily **better metric validity**.
6. **Parallel execution** is not necessarily **collective learning**.
7. **A proposed recursive change** is not a **validated improvement**.

These distinctions should be visible in APIs, experiment manifests, and publication claims.

## 3. Research Track A — Minimal Executable CDE Core

**Priority:** Immediate. **Dependencies:** SPEC-001, SPEC-002, SPEC-003. **Primary case:** CASE-001.

Build a small, auditable reference implementation before developing ambitious distributed capabilities. The first release should support immutable or versioned State Containers, typed Delta proposals, pure transition previews, hard-constraint validation, atomic commit, path-memory records, and explicit rejection reasons.

### A1. Contract-first runtime

Implement machine-readable schemas for `StateManifest`, `DeltaProposal`, `TransitionAttempt`, `ConstraintReport`, `CommitRecord`, `PathDecision`, and `SearchRunManifest`. Unknown constraint status must never be treated as PASS. Every state and edge must carry the versions necessary to reproduce its interpretation.

### A2. Deterministic fixture suite

Create tiny problems with enumerated states and known outcomes: binary assignment, finite graph traversal, constrained integer selection, and reversible swap neighborhoods. Exhaustively test these fixtures to distinguish transition bugs from heuristic performance.

### A3. Minimal search policies

Implement breadth-first search, depth-first search, best-first search, and a simple beam policy over the same contracts. Record total evaluations, feasible successors, deduplication events, time, memory, and solution quality.

### A4. Exit criterion

The core is ready for broader work when independent tests can replay every committed transition, explain every rejected attempt, and reproduce ledger counts. Passing these tests does not establish competitive optimization performance.

## 4. Research Track B — Adaptive M-to-N Branching

**Priority:** Immediate to medium. **Dependencies:** CDE-003, SPEC-002. **Primary cases:** CASE-001 and CASE-002.

The central question is whether adaptive allocation of proposal and evaluation effort improves validated task outcomes relative to fixed branching and standard search heuristics.

Compare fixed branching, uncertainty-aware branching, diversity-aware branching, directional-field-guided branching, and budget-aware policies. Separate proposal cost from validation cost. Use equal total evaluation budgets and report both best-found feasible quality and the probability of finding a feasible solution.

A critical negative control is a task where simple fixed branching already covers the relevant neighborhood. In that setting, adaptive policies may add complexity without benefit.

**Research questions:** Can branching allocation be learned without leaking held-out task information? Does increasing proposal diversity improve exploration after deduplication? When does larger $M$ merely inflate duplicate candidates? Which adaptive policies remain robust when directional evidence is misleading?

## 5. Research Track C — Path Memory, NoSubtree, and Loop Back

**Priority:** Immediate to medium. **Dependencies:** CDE-004, SPEC-003.

Study path-local exclusion and acceptance semantics in fixed constructive problems, then extend to reconfiguration and dynamic-state problems where revisiting can be legitimate.

In a fixed candidate universe, the constructive YesSubtree/NoSubtree interpretation may prevent a decision from being repeated on the same path. This does **not** imply that arbitrary state-transition graphs are acyclic, nor that a dynamic operator vocabulary terminates.

A benchmark should contain deliberately distinct cases: a finite decision tree, a DAG with equivalent future subproblems, a reversible cyclic graph, and a representation-changing graph. Evaluate loop prevention, legitimate revisit preservation, cache soundness, and memory overhead.

**Open challenge:** Define the smallest practical future-equivalence certificate sufficient for sound memoization, without requiring full formal verification for every application.

## 6. Research Track D — Search, Dynamic Programming, and Branch-and-Bound

**Priority:** High. **Dependencies:** CDE-005, SPEC-004.

CDE should be evaluated as an orchestration and representation framework, not as a replacement for established exact methods. Integrate conventional search, dynamic programming, branch-and-bound, MIP, and CP-SAT through typed solver adapters.

A solver report must distinguish `FEASIBLE`, `BEST_KNOWN`, `BOUNDED_GAP`, `OPTIMAL`, `INFEASIBLE_PROVEN`, `EXHAUSTIVE`, and `UNKNOWN`. Certificates and bounds are valid only under their declared problem encoding, constraints, objective, and solver assumptions.

Test whether shared CDE contracts improve composability, reproducibility, or mixed-solver workflows. Do not claim better solution quality merely because an exact solver is wrapped in CDE.

**Suggested benchmarks:** knapsack variants, assignment, scheduling, graph search, and the school-bus routing case. Strong baselines include direct solver use without CDE orchestration.

## 7. Research Track E — Multi-Scale and Directional Delta Generation

**Priority:** Medium. **Dependencies:** CDE-006, CASE-001.

Explore local, regional, long-range, and Relay-guided Delta families. Directional fields may prioritize promising moves using gradients, weak evidence, analogies, or learned rankings; they must not be mistaken for feasibility proofs.

On synthetic landscapes, control peak separation, ridge geometry, deceptive gradients, noise, and constraint boundaries. Compare coordinate-based local search, random restarts, evolutionary strategies, Bayesian optimization, and CDE portfolios under matched evaluation budgets.

The key measurement is **validated improvement per total unit of compute**, not the number of visually diverse trajectories. Report where directional guidance is harmful and whether long-range prospecting recovers from local traps.

## 8. Research Track F — Solution–Metric Co-Evolution

**Priority:** Medium. **Dependencies:** CDE-007, SPEC-006, CASE-004.

Separate three nested processes: searching for better solutions under a pinned metric, proposing alternative metrics, and independently evaluating whether the proposed metrics support better decisions.

A metric candidate should declare its feature definitions, aggregation, perspective, uncertainty, missingness, calibration, constraints, and version. Comparisons across metric versions require re-scoring or a validated mapping; raw score increases cannot justify promotion.

### F1. Proxy and Goodhart stress tests

Construct synthetic environments where an easy-to-optimize proxy diverges from a hidden or independently specified target utility. Compare fixed proxy optimization, multiobjective optimization, preference learning, and governed metric discovery.

### F2. Realistic tradeoff case

For school-bus routing, study fleet count as a primary objective while treating maximum ride time, safety, timing, and capacity as hard constraints, and average ride burden or equity as additional declared preferences. Avoid replacing safety constraints with a weighted score.

### F3. Independent governance

The agent that proposes a metric must not independently certify that metric by optimizing it. Held-out decision utility, stakeholder perspectives, and negative controls are required.

## 9. Research Track G — Open-Ended Delta and Representation Evolution

**Priority:** Medium to long term. **Dependencies:** EXT-002, SPEC-001–006.

Investigate the distinction between finding a solution within a representation and discovering a more useful representation. Candidate meta-Deltas may add entity types, composite structures, latent variables, new operators, macro-operators, or alternative decompositions.

Every representation migration needs explicit source and target versions, preserved observables, known information loss, constraint revalidation, metric compatibility, cache invalidation, and rollback behavior.

**Testable proposition:** Some constrained tasks become reachable or cheaper when the system introduces a composite representation unavailable in the initial operator vocabulary. Test against human-designed abstractions, program synthesis, hierarchical planning, and fixed representation portfolios.

**Failure modes:** syntactic novelty without semantic value; hidden loss of essential state; stale cache reuse; complexity explosion; training-task overfitting; and unvalidated self-modification.

## 10. Research Track H — Distributed CDE Orchestration

**Priority:** Medium. **Dependencies:** CDE-009, SPEC-005, CASE-005.

Implement distributed exploration only after the single-worker semantics are stable. Define task envelopes with source snapshots, pinned constraint and metric versions, Delta scope, budgets, lease epochs, idempotency keys, validation requirements, and provenance.

Study work partitioning, overlapping search, shared incumbents, stale workers, retries, cancellation, partial results, and global reconciliation. A locally feasible partial solution may become globally infeasible after merging with another worker's result.

### H1. Efficiency experiments

Compare one worker, independent parallel restarts, island-model search, distributed large-neighborhood search, and CDE orchestration. Report speedup, total compute, communication, coordination overhead, duplicate work, and solution quality.

### H2. Fault injection

Test delayed results, duplicated commits, expired leases, mismatched metric versions, network partitions, and partial constraint evidence. Distributed termination and global optimality claims must be supported by explicit assumptions and certificates.

## 11. Research Track I — Structural Relay and Collective Learning

**Priority:** Medium to long term. **Dependencies:** CDE-010, SPEC-005, CASE-005, EXT-004.

The seven primary Relay artifact types are **Result, Evidence, Constraint, Method, Policy, Metric, and Leftover**. A Relay artifact should carry source assumptions, scope, versions, provenance, validation status, known counterexamples, and target applicability conditions.

Distinguish four levels:

1. **Sharing:** an artifact is transferred.
2. **Compatibility:** the target can interpret it.
3. **Validated transfer:** the artifact helps under independent target evaluation.
4. **Collective learning:** reuse improves subsequent performance across a declared population or task distribution, after accounting for costs.

### I1. Negative transfer

A source-successful method can fail under different target constraints. Negative transfer must be measured and retained, not hidden by selective reporting.

### I2. Temporal Relay

Repeated execution may accumulate evidence about reliability, variance, or drift. Repetition without new independent information is not automatically learning.

### I3. Governance

A shared registry should preserve immutability, supersession, access controls, and the difference between publication and deployment authorization.

## 12. Research Track J — Biomimetic and Population-Based Exploration

**Priority:** Medium. **Dependencies:** EXT-001.

Compare scout/exploit allocation, island populations, quality-diversity archives, evolutionary operator portfolios, and ecological niche preservation under CDE contracts.

The value of biological analogy is a **testable search mechanism**, not a claim that biological metaphors establish correctness. Strong baselines should include conventional evolutionary strategies, genetic algorithms, particle swarm methods where applicable, and distributed restarts.

Measure diversity at the level of distinct validated behaviors or reachable regions, not merely distinct candidate strings. Include ablations for migration, diversity pressure, counterexample agents, and shared evidence.

## 13. Research Track K — Scientific Hypothesis and Measurement Discovery

**Priority:** Long term, with small near-term prototypes. **Dependencies:** AQSI interfaces, EXT-003, CASE-004.

Represent a scientific hypothesis as a versioned object with assumptions, predictions, potential falsifiers, evidence requirements, and a declared scope. Use Deltas to propose refinements, competing explanations, experimental interventions, and new measurement models.

A scientific exploration loop may be:

`Question → Hypothesis → Discriminating Experiment → Evidence → Independent Evaluation → Metric/Representation Revision → Relay`.

Critical distinctions include prediction versus causation, model fit versus mechanism, and a plausible hypothesis versus a verified explanation. Where experiments are not feasible, the system should record an unresolved question rather than fabricate evidence.

Begin with synthetic causal systems and simulated measurement bias before attempting real-world scientific claims. Independent replication and preregistration should precede any assertion of discovery.

## 14. Research Track L — Controlled Structural RSI

**Priority:** Long term. **Dependencies:** CDE-011, SPEC-006, EXT-004.

Separate evolution of task states, Delta generators, solver-selection policies, representations, and metrics. Candidate changes must be evaluated under a stable external validation contract, with scoped authorization and rollback where possible.

A practical research progression is:

**L0:** improve solutions under fixed methods.  
**L1:** propose new generators or operators.  
**L2:** propose policy or representation changes.  
**L3:** independently validate, authorize, monitor, and reject changes.

The generator must not be able to rewrite its own promotion criteria and then declare success. Compare fixed policies, tuned policies, learned policies, and controlled meta-evolution on held-out task families. Record regressions and nonmonotonic trajectories.

**Research boundary:** Successful recursive optimization in a benchmark is not evidence of unrestricted self-improvement or AGI.

## 15. SIF Localization and Capability Addressing

CDE offers a possible extension of SIF Localization from finding a known structural address to **constructive localization**: exploring candidate states, methods, metrics, and representations until a useful address or capability is discovered.

Potential interfaces include typed problem descriptors, capability manifests, solver eligibility predicates, method-transfer contracts, and searchable failure/Leftover registries. An important question is whether this structural addressing reduces dispatch and integration costs relative to conventional workflow engines and plugin registries.

The hypothesis should be tested using measurable lookup precision, routing cost, compatibility failures, and successful task completion—not only conceptual diagrams.

## 16. Cross-Track Research Dependencies

| Stage | Deliverable | Depends on | Unlocks |
|---|---|---|---|
| 0 | Frozen terminology and schemas | SPEC-001–003 | Reproducible core |
| 1 | Minimal executable CDE | Stage 0 | Search baselines |
| 2 | Exact/heuristic solver adapters | Stages 0–1 | Optimization studies |
| 3 | Multi-scale branching benchmarks | Stages 1–2 | Directional studies |
| 4 | Versioned metric evaluation | Stages 0–2 | Metric co-evolution |
| 5 | Representation migration contracts | Stages 0–4 | Operator genesis |
| 6 | Distributed task and Relay protocols | Stages 1–5 | Collective learning |
| 7 | Independent cross-task validation | Stages 4–6 | Controlled meta-evolution |
| 8 | Longitudinal recursive studies | Stages 1–7 | Bounded RSI evidence |

This is a suggested dependency graph, not a requirement that every project implement all stages.

## 17. Benchmark Portfolio and Primary Contrasts

| Benchmark family | Central question | Essential baselines | Primary measurement |
|---|---|---|---|
| Synthetic finite states | Are transitions and deduplication sound? | BFS/DFS | Correctness and coverage |
| Multi-peak search | Does multi-scale Delta exploration help? | Restarts, evolutionary search, BO | Best feasible value per budget |
| Constrained school-bus routing | Can CDE coordinate combinatorial optimization? | MIP/CP-SAT, VRP heuristics | Fleet, constraints, cost |
| Dynamic structural matching | Can weak evidence and constraints be combined? | Graph/ILP matching | Held-out match quality |
| Metric discovery | Can proxy failures be detected? | Fixed metrics, preference methods | Independent decision utility |
| Distributed exploration | Is coordination worth its overhead? | Parallel restarts, island models | Cost-adjusted speedup |
| Method Relay | Does transfer generalize? | No transfer, naive transfer | Validated target gain |
| Representation evolution | Are new abstractions useful? | Fixed and human-designed schemas | Reachability, total cost |
| Scientific hypothesis search | Are experiments more discriminating? | Random/design-of-experiments | Information gain, replication |
| Controlled RSI | Are meta-changes independently useful? | Frozen/tuned policies | Held-out improvement and regressions |

Benchmarks should be published with seeds, task generation code, split definitions, baselines, and failures. Avoid comparing a system with privileged information against a baseline without that information.

## 18. Cross-Cutting Negative Controls

The following controls are especially important because CDE spans multiple optimization and intelligence claims:

- **NC-01:** A simple baseline already solves the task optimally.
- **NC-02:** Extra proposals increase $M$ but not distinct successors $K$.
- **NC-03:** Directional evidence points away from the best region.
- **NC-04:** Heuristic rejection is mislabeled as infeasibility proof.
- **NC-05:** A coarse state equivalence merges states with different futures.
- **NC-06:** A new metric improves its own score while degrading external utility.
- **NC-07:** A representation migration invalidates old caches or certificates.
- **NC-08:** Multiple agents share correlated errors and produce false consensus.
- **NC-09:** Relay succeeds on source tasks but fails on target constraints.
- **NC-10:** A meta-generator improves training benchmarks and regresses on held-out families.
- **NC-11:** Distributed execution spends more total compute than a single-worker baseline.
- **NC-12:** Repeated execution is incorrectly treated as independent evidence.
- **NC-13:** An unapproved candidate changes the evaluator or promotion criteria.
- **NC-14:** A rollback cannot undo an irreversible external action.
- **NC-15:** A novelty objective rewards meaningless operator proliferation.
- **NC-16:** An apparent gain disappears after including validation and coordination costs.

Negative outcomes should be archived as evidence with clear scope.

## 19. Cross-Cutting Ablations

A useful standard ablation matrix removes one mechanism at a time while keeping budgets and task information matched:

| Ablation | Mechanism removed | Interpretation target |
|---|---|---|
| A-01 | Adaptive branching | Allocation benefit |
| A-02 | Directional field | Guidance benefit |
| A-03 | Long-range Deltas | Escape from local traps |
| A-04 | Path memory | Loop and duplication control |
| A-05 | Sound future-equivalence caching | Memoization benefit |
| A-06 | Solver portfolio | Orchestration benefit |
| A-07 | Metric evolution | Decision-model adaptation |
| A-08 | Independent metric validation | Goodhart risk |
| A-09 | Representation evolution | Expressivity benefit |
| A-10 | Operator genesis | New move capability |
| A-11 | Structural Relay | Cross-task reuse |
| A-12 | Counterexample agents | Failure detection |
| A-13 | Diversity preservation | Population collapse |
| A-14 | Distributed coordination | Coordination overhead |
| A-15 | Leftover registry | Research continuity |
| A-16 | Controlled meta-evolution | Recursive benefit |

Some mechanisms interact; factorial studies may be needed where one-at-a-time ablations cannot identify causal contributions.

## 20. Evidence, Statistical Design, and Reporting

Every empirical study should declare its primary endpoint before inspecting final-test outcomes. At minimum, report:

1. task instances, generation method, and train/development/test separation;
2. hardware, software, solver versions, random seeds, and runtime limits;
3. state, operator, constraint, metric, generator, and policy versions;
4. $M,Q,N,N_c,K$ and duplicate rates under a declared scope;
5. total compute, wall time, memory, and coordination/validation overhead;
6. feasibility, best-known objective, bounds, and certificate status;
7. uncertainty estimates across independent instances and seeds;
8. ablations, negative controls, counterexamples, and unsuccessful runs;
9. independent validation and replication status;
10. claim boundaries and known threats to validity.

Where statistical tests are appropriate, pre-register comparisons and avoid treating many correlated observations from one run as independent replications. Where no empirical result exists, say **“not yet evaluated”** rather than inserting plausible numbers.

## 21. Conformance and Safety Priorities

The following invariants should be tested before any deployment-oriented claim:

**FD-INV-01:** Every committed transition is versioned and traceable.  
**FD-INV-02:** Hard constraints are checked under a coherent snapshot.  
**FD-INV-03:** UNKNOWN and ERROR are not silently accepted as PASS.  
**FD-INV-04:** Heuristic exclusions are not promoted into proofs.  
**FD-INV-05:** Cached results are reused only under sound equivalence assumptions.  
**FD-INV-06:** Cross-version metric comparisons require explicit re-evaluation.  
**FD-INV-07:** Distributed retries are idempotent or explicitly reconciled.  
**FD-INV-08:** Relay artifacts disclose source and target applicability.  
**FD-INV-09:** Generated operators run only under authorized capabilities and budgets.  
**FD-INV-10:** Candidate generators cannot self-authorize policy promotion.  
**FD-INV-11:** Failed transfers and unresolved Leftovers remain available for analysis.  
**FD-INV-12:** No generality, completeness, or RSI claim exceeds its supporting evidence.

In physical-world applications such as student transportation, safety and legal constraints must be independently verified by qualified operators. A research prototype is not a dispatch authorization system.

## 22. Suggested Twelve-Milestone Roadmap

These are **sequence milestones**, not promised dates.

| Milestone | Proposed artifact | Acceptance criterion |
|---|---|---|
| M01 | Machine-readable core schemas | Schema tests and examples |
| M02 | Deterministic transition engine | Replay and atomic commit tests |
| M03 | Path-memory implementation | Cyclic/acyclic fixture conformance |
| M04 | Search baseline suite | Reproducible matched-budget runs |
| M05 | Multi-scale CASE-001 prototype | Preregistered comparison and failures |
| M06 | Solver-adapter integration | Correct certificate/claim handling |
| M07 | School-bus CASE-002 prototype | Validated constraints and strong baselines |
| M08 | Metric co-evolution prototype | Independent held-out utility tests |
| M09 | Representation/operator sandbox | Migration and counterexample tests |
| M10 | Distributed task and Relay service | Fault injection and reconciliation |
| M11 | Cross-task Collective Learning study | Target validation and negative transfer |
| M12 | Controlled meta-evolution study | Independent multi-generation evaluation |

A minimal viable scientific contribution may be achieved at M05 or M06. The later milestones should not delay publication of rigorous negative or positive findings from earlier work.

## 23. Open Collaboration and Contribution Paths

CDE is intended to be **collective-learning-friendly**. Contributions can be useful without endorsing the entire framework. High-value contribution types include:

- a counterexample to a proposed invariant or equivalence relation;
- an exact solver baseline showing CDE overhead or benefit;
- a small, independently reproducible benchmark;
- a machine-readable contract and conformance test;
- a negative transfer case;
- a new Delta operator with preconditions and validation evidence;
- a representation migration adapter with explicit information-loss analysis;
- a critical review of claims, assumptions, and terminology;
- a failed experiment documented well enough to prevent repetition;
- a Leftover or open problem converted into a testable research question.

Recommended contribution record:

```yaml
contribution_id: "example-only"
type: "counterexample | method | benchmark | evidence | constraint | leftover"
source_context: "task and version references"
claim: "bounded, falsifiable statement"
assumptions: ["explicit assumptions"]
evidence: ["reproducible artifacts"]
negative_results: ["observed failures"]
validation_status: "UNVALIDATED"
license: "Apache-2.0 or separately declared compatible terms"
```

This is an illustrative schema; it does not imply an existing submission platform or validation outcome.

## 24. Publication and Repository Maintenance

The planned CDE corpus comprises **12 Foundations, 6 Specifications, 5 Cases, and 4 Extensions**, supported by repository-level navigation, figures, validation documents, and publication metadata. This file is an agenda rather than a substitute for the detailed research papers.

Keep the following root assets synchronized:

- `README.md`, `START-HERE.md`, `CONTENTS.md`, `GLOSSARY.md`, `FIGURE-INDEX.md`;
- `OPEN-QUESTIONS.md`, `TESTABLE-HYPOTHESES.md`, `VALIDATION-ROADMAP.md`;
- `CHANGELOG.md`, `CITATION.cff`, `.zenodo.json`, and `LICENSE`;
- `publication/GitHub-Release-Notes-v1.0.0.md` and related ResearchGate descriptions;
- the ten planned `figures/Fig-001...Fig-010` assets.

A composite image containing ten panels does **not** by itself establish the existence of ten separately named PNG files. Verify actual paths, labels, captions, and links before Release. Do not announce a DOI until a DOI has actually been assigned.

## 25. Priority Research Questions for the Next Cycle

**P1 — Correctness:** Can a minimal CDE implementation preserve transition, constraint, and path semantics across multiple solver types?

**P2 — Value added:** On which task families does CDE improve reproducibility, interoperability, or cost-adjusted search outcomes relative to strong direct baselines?

**P3 — Dynamic branching:** When does adaptive M-to-N exploration yield more unique useful successors rather than redundant proposals?

**P4 — Representation:** Can the system detect a representational bottleneck and propose a useful new Delta or abstraction under independent validation?

**P5 — Metrics:** Can solution–metric co-evolution resist proxy gaming better than fixed scoring or conventional preference optimization?

**P6 — Relay:** Can a validated method improve a genuinely new target task after accounting for compatibility and transfer overhead?

**P7 — Collective learning:** Is the shared system measurably better than independently optimized agents under equivalent total resources?

**P8 — RSI:** Can generator/policy changes produce sustained held-out improvements without allowing the candidate to redefine its own evaluator?

## 26. What Would Falsify the Research Program's Stronger Claims?

A rigorous research agenda should specify disconfirming evidence:

- If direct solver baselines consistently outperform CDE wrappers at equal total cost, CDE's optimization-performance claim must be narrowed to other benefits, if any.
- If proposed structural equivalences fail on small exhaustive fixtures, their use for memoization or pruning must be withdrawn until corrected.
- If adaptive branching improves internal scores but not validated outcomes, its decision policy requires revision.
- If metric co-evolution repeatedly overfits held-out decisions, the promotion mechanism is inadequate.
- If Relay transfers mostly fail or cost more than independent rediscovery, its applicable task distribution is narrower than hypothesized.
- If recursive generator changes regress outside training tasks, no sustained RSI claim is justified.
- If open-ended operator invention creates mostly redundant or unsafe operators, a fixed curated vocabulary may be preferable for the tested domain.

These outcomes would not invalidate the basic State–Delta–Constraint abstraction, but they would limit specific performance, transfer, or evolution claims.

## 27. Closing Perspective

CDE's most promising direction may be its **structural interoperability**: a way to express constrained search, adaptive proposals, solver evidence, metric changes, and transferable methods through explicit contracts. Its most ambitious direction is a controlled ecosystem that can discover and validate new operators, representations, and exploration strategies.

The distance between those directions should be crossed through executable specifications, strong baselines, counterexamples, independent validation, and carefully scoped claims—not through architectural analogy alone.

**Near-term goal:** make the core falsifiable and reproducible.  
**Medium-term goal:** measure when CDE coordination and structural evolution add value.  
**Long-term goal:** investigate whether validated Structural Relay and controlled meta-evolution support measurable Collective Learning across tasks.

The appropriate guiding principle is:

> **Explore broadly, commit only under constraints, transfer with evidence, and evolve under independent validation.**

---

### Related CDE Documents

**Foundations:** [`CDE-001`](docs/foundations/CDE-001-From-BTP-and-D-BTP-to-Constrained-Delta-Evolution.md) through [`CDE-012`](docs/foundations/CDE-012-Generalization-Boundaries-Guarantees-and-Open-Research.md).  
**Specifications:** [`SPEC-001`](docs/specifications/SPEC-001-State-Container-and-Constraint-Contract.md) through [`SPEC-006`](docs/specifications/SPEC-006-Metric-and-Recursive-Evolution-Contract.md).  
**Cases:** [`CASE-001`](docs/cases/CASE-001-Multi-Scale-Peak-Search.md) through [`CASE-005`](docs/cases/CASE-005-Distributed-CDE-and-Method-Relay.md).  
**Extensions:** [`EXT-001`](docs/extensions/EXT-001-Biomimetic-and-Population-Based-CDE.md), [`EXT-002`](docs/extensions/EXT-002-Open-Ended-Delta-and-Representation-Evolution.md), [`EXT-003`](docs/extensions/EXT-003-CDE-for-Scientific-Hypothesis-and-Metric-Discovery.md), [`EXT-004`](docs/extensions/EXT-004-CDE-Collective-Intelligence-and-Structural-RSI.md).

**Next supporting documents:** `OPEN-QUESTIONS.md`, `TESTABLE-HYPOTHESES.md`, `VALIDATION-ROADMAP.md`.
