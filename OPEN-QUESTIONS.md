# Open Questions — Constrained Delta Evolution (CDE)

**Project:** Constrained Delta Evolution: A General Framework for Structural Search, Optimization, Metric Evolution, and Distributed Intelligence  
**Authors:** Sizhe Tan and GPT-Obot  
**Status:** Open research agenda, v1.0.0 draft (2026-11-04)  
**License:** Apache-2.0  
**Companion documents:** [README](README.md) · [START-HERE](START-HERE.md) · [CONTENTS](CONTENTS.md) · [GLOSSARY](GLOSSARY.md) · [FUTURE-DIRECTIONS](FUTURE-DIRECTIONS.md) · [TESTABLE-HYPOTHESES](TESTABLE-HYPOTHESES.md) · [VALIDATION-ROADMAP](VALIDATION-ROADMAP.md)

> **Research posture.** The questions below are unresolved. They are neither established CDE capabilities nor evidence of performance. CDE is a proposed contract-driven framework; implementation, experiments, negative results, and independent replication remain necessary. Contributions that falsify a hypothesis or expose a boundary are as welcome as positive results.

## 1. Purpose and Scope

This register turns the 27 CDE research papers—12 foundations, six specifications, five cases, and four extensions—into specific questions that can be investigated, challenged, prioritized, and relayed. It spans classical optimization, dynamic state spaces, adaptive Delta generation, metric design, distributed exploration, representation evolution, scientific discovery, collective learning, and controlled recursive self-improvement (RSI).

The central model is a versioned exploration problem

\[
\mathcal P_v=(S_v,\Delta_v,T_v,C_v,G_v,L_v),\qquad
s_{t+1}=T_v(s_t,\delta_t),\quad \delta_t\in G_v(s_t),
\]

with hard constraints \(C_v\), explicit path/evidence context \(L_v\), and optional versioned metric, solver, and governance contracts. A candidate Delta is not a valid transition until its applicable constraints and commit rules have been checked.

**Important distinctions:** representability is not generability; generability is not budgeted reachability; reachability is not completeness; completeness is not optimality; optimization under a metric is not proof of metric quality; information sharing is not validated collective learning.

## 2. How to Use This Register

Every question has an identifier, priority, a research challenge, a possible test, a counterexample or failure condition, and pointers to the most relevant papers. The priority labels indicate a suggested *research sequence*, not a claim of intrinsic importance:

- **P0 — Foundation:** Required to make core claims interpretable and implementation results reproducible.
- **P1 — Validation:** Important to determine when proposed mechanisms outperform or underperform established baselines.
- **P2 — Exploration:** Longer-horizon, more open-ended or domain-dependent investigations.

Question states should be tracked as `OPEN`, `IN_PROGRESS`, `PARTIALLY_ANSWERED`, `SUPPORTED_WITH_SCOPE`, `REFUTED_WITH_SCOPE`, or `SUPERSEDED`. Never mark a question as universally resolved on the basis of a single benchmark.

## 3. Core Semantics, State Containers, and Constraints

### OQ-001 — What is the smallest useful CDE State Container contract? [P0]

**Challenge.** Identify the minimum fields needed for reproducible transitions, identity, validation, provenance, and optional uncertainty without imposing unnecessary complexity on simple tasks.  
**Test.** Implement the same synthetic peak-search problem using a minimal contract and an enriched contract; compare conformance coverage, runtime overhead, and failure diagnosis.  
**Failure signal.** A supposedly minimal contract cannot distinguish stale constraints, invalid states, or duplicate commits.  
**References.** CDE-002; SPEC-001; CASE-001.

### OQ-002 — Which constraints must be checked before, during, and after a Delta? [P0]

**Challenge.** Determine when precondition checking suffices, when an atomic post-state validator is essential, and how `UNKNOWN` should affect search.  
**Test.** Inject operators with valid preconditions but invalid postconditions; test atomic rejection and error handling.  
**Failure signal.** An invalid candidate is committed because a heuristic score or an unknown validator result is treated as permission.  
**References.** CDE-002; SPEC-001–002.

### OQ-003 — Can Delta identity be made stable across contexts? [P0]

**Challenge.** Separate an operator definition, its parameterized instance, its decision identity, its transition edge, and its resulting state.  
**Test.** Generate repeated operations under different parents, versions, and evidence; verify that deduplication does not collapse distinct transitions.  
**Failure signal.** An operator name is mistaken for a unique path decision or state identity.  
**References.** CDE-003–004; SPEC-002–003.

### OQ-004 — What are sound state-equivalence rules for memoization? [P0]

**Challenge.** A common state fingerprint need not imply identical future feasible actions, costs, or bounds.  
**Test.** Construct states with identical visible assignments but different histories or constraints; test future-equivalence and cache reuse.  
**Failure signal.** A valid solution is pruned using a cached result from a non-equivalent state.  
**References.** CDE-004–005; SPEC-003–004.

### OQ-005 — How should constraint versions evolve without invalidating historical evidence? [P1]

**Challenge.** A changed hard constraint may alter feasibility, bounds, dominance, and termination claims.  
**Test.** Modify capacity or time-window constraints mid-search and measure safe invalidation and revalidation.  
**Failure signal.** Old feasibility certificates silently survive incompatible changes.  
**References.** CDE-002, CDE-009; SPEC-001, SPEC-005.

### OQ-006 — How should CDE represent incomplete, probabilistic, or contested evidence? [P1]

**Challenge.** Avoid treating uncertain evidence as hard validity while still exploiting it for exploration.  
**Test.** Compare explicit uncertainty and abstention with forced binary judgments on matching and scientific-hypothesis tasks.  
**Failure signal.** Confidence scores are presented as proof of feasibility or causality.  
**References.** CDE-006; CASE-003; EXT-003.

## 4. Adaptive Delta Generation and M-to-N Branching

### OQ-007 — When does adaptive branching beat fixed branching? [P0]

**Challenge.** Distinguish genuine efficiency gains from differences in evaluation budget, operator diversity, or stopping policy.  
**Test.** Compare fixed \(Q\), adaptive \(Q\), and equal-cost random selection across multiple landscapes and seeds.  
**Failure signal.** The adaptive method wins only when allocated more objective evaluations.  
**References.** CDE-003; SPEC-002; CASE-001.

### OQ-008 — Which branching counts are necessary for honest reporting? [P0]

**Challenge.** Distinguish \(M\) proposals, \(Q\) attempts, \(N\) realized edges, \(N_c\) committed edges, and \(K\) unique successors.  
**Test.** Use duplicates, invalid transitions, retries, and convergent paths; reconcile each ledger.  
**Failure signal.** Duplicate successors or failed attempts are counted as new explored states.  
**References.** CDE-003; SPEC-002; CASE-005.

### OQ-009 — How can local, regional, and long-range Deltas be allocated adaptively? [P1]

**Challenge.** Determine whether multi-scale portfolios can avoid premature convergence without wasting computation.  
**Test.** Ablate each scale, compare bandit policies and fixed mixtures, and report exploration overhead.  
**Failure signal.** Long-range jumps increase novelty but decrease validated solution quality under matched budgets.  
**References.** CDE-006; CASE-001; EXT-001.

### OQ-010 — When is a directional field genuinely informative? [P1]

**Challenge.** Weak directional evidence can guide search but also induce systematic bias.  
**Test.** Inject calibrated, noisy, adversarial, and misleading directional signals.  
**Failure signal.** Directional guidance underperforms random exploration on shifted tasks or hides uncertainty.  
**References.** CDE-006; CASE-003; EXT-001.

### OQ-011 — How should candidate diversity be measured? [P1]

**Challenge.** Structural diversity, geometric distance, and future-reachability diversity need not agree.  
**Test.** Compare diversity metrics on multimodal and combinatorial benchmarks.  
**Failure signal.** A diversity measure rewards syntactic variations that lead to identical futures.  
**References.** CDE-003, CDE-006; EXT-001.

### OQ-012 — Can a generator learn when *not* to generate? [P2]

**Challenge.** Sometimes the best action is to validate, wait for evidence, stop, or switch solver rather than expand branches.  
**Test.** Compare stopping/abstention policies against always-expand baselines.  
**Failure signal.** Adaptive stopping misses feasible solutions without an acknowledged coverage tradeoff.  
**References.** CDE-008, CDE-011; SPEC-002.

## 5. Path Memory, NoSubtree, and Loop Back

### OQ-013 — Exactly when is path-local NoSubtree sound? [P0]

**Challenge.** In fixed constructive contexts, accepted and excluded decisions can be path-local. In reconfiguration contexts, legitimate revisitation may be necessary.  
**Test.** Contrast fixed finite assignment tasks with reversible routing moves and changing evidence.  
**Failure signal.** A global no-repeat rule incorrectly eliminates valid future states.  
**References.** CDE-004; SPEC-003.

### OQ-014 — How can Loop Back be controlled without forbidding useful revisitation? [P1]

**Challenge.** Repeated states may represent cycles, repair sequences, or meaningful new evidence.  
**Test.** Compare version-aware visited sets, bounded revisits, dominance rules, and tabu policies.  
**Failure signal.** Infinite loops or false pruning under a declared finite budget.  
**References.** CDE-004; SPEC-003; CASE-003.

### OQ-015 — Which path histories are sufficient for future equivalence? [P1]

**Challenge.** Storing full history is expensive, but discarding too much history may break transition validity.  
**Test.** Identify minimal sufficient history on selected finite tasks and measure cache soundness.  
**Failure signal.** History compression merges states with different admissible actions.  
**References.** CDE-004–005; SPEC-003.

### OQ-016 — Can Leftovers become a reliable research memory? [P2]

**Challenge.** Deferred, unexpressible, infeasible, and pruned proposals have different meanings.  
**Test.** Track each disposition and evaluate whether later revisions recover useful work.  
**Failure signal.** An unproved prune or failed operator is forgotten or misrepresented as a proof of impossibility.  
**References.** CDE-004, CDE-010; EXT-002.

## 6. Search, Dynamic Programming, and Branch-and-Bound

### OQ-017 — What is CDE's incremental value over mature solvers? [P0]

**Challenge.** CDE must not claim novelty for standard DP, B&B, MIP, CP-SAT, LNS, or evolutionary search.  
**Test.** Compare CDE orchestration against strong solver-native baselines with matched total cost.  
**Failure signal.** Reported gains disappear after solver tuning or orchestration overhead is included.  
**References.** CDE-005, CDE-008; SPEC-004; CASE-002.

### OQ-018 — When can DP memoization be safely shared across Delta paths? [P0]

**Challenge.** State equality may be weaker than future-equivalence under path constraints.  
**Test.** Exhaustively enumerate small fixtures to verify memoized value equality.  
**Failure signal.** An incorrect cached optimum changes the selected solution.  
**References.** CDE-005; SPEC-004.

### OQ-019 — How should admissible bounds be represented and verified? [P0]

**Challenge.** A heuristic estimate cannot be silently promoted to a lower/upper bound.  
**Test.** Compare bound certificates to exact optima on small instances; inject inadmissible bounds.  
**Failure signal.** A heuristic bound is used for proof-bearing pruning.  
**References.** CDE-005; SPEC-004.

### OQ-020 — Can a solver portfolio improve robustness across problem families? [P1]

**Challenge.** Solver selection may itself overfit benchmark features.  
**Test.** Compare single best solver, per-family tuned solver, and learned portfolio on held-out families.  
**Failure signal.** Selection cost and regressions exceed the portfolio's benefit.  
**References.** CDE-008; SPEC-004; CASE-001–005.

### OQ-021 — How should optimality and infeasibility claims survive dynamic changes? [P1]

**Challenge.** New Deltas, changed metrics, and new constraints can invalidate a previously valid proof.  
**Test.** Introduce version changes after certification and verify claim revocation/reproof.  
**Failure signal.** `OPTIMAL` or `INFEASIBLE_PROVEN` remains active under incompatible semantics.  
**References.** CDE-005, CDE-012; SPEC-004, SPEC-006.

## 7. Solution–Metric Co-Evolution

### OQ-022 — How can metric quality be evaluated independently of score increases? [P0]

**Challenge.** A metric that gives higher internal scores may be worse at ranking real decisions.  
**Test.** Compare metric candidates using held-out decision utility, preference consistency, and adversarial proxy tasks.  
**Failure signal.** A metric self-certifies solely because its own optimized score increased.  
**References.** CDE-007; SPEC-006; CASE-004.

### OQ-023 — When should metrics be scalar, lexicographic, vector-valued, or partial orders? [P1]

**Challenge.** Different formulations encode different permissible tradeoffs.  
**Test.** Use school-bus fleet minimization with ride-time and fairness constraints under multiple authorized objective structures.  
**Failure signal.** An unauthorized tradeoff allows a small score gain to violate a protected priority.  
**References.** CDE-007; CASE-002, CASE-004; EXT-002.

### OQ-024 — Can metric drift be detected before it corrupts search? [P1]

**Challenge.** Distribution shifts, feedback loops, and proxy gaming can erode metric validity.  
**Test.** Inject synthetic drift and compare detection delay, false alarms, and rollback cost.  
**Failure signal.** New metrics are promoted without detecting systematic held-out deterioration.  
**References.** CDE-007; SPEC-006; CASE-004.

### OQ-025 — How should conflicting perspectives be reconciled? [P2]

**Challenge.** A common scalar may conceal irreducible differences in stakeholder values.  
**Test.** Compare Pareto, lexicographic, negotiated, and perspective-specific policies on transparent fixtures.  
**Failure signal.** One stakeholder's metric is presented as an objective universal truth.  
**References.** CDE-007; CASE-004; EXT-003.

### OQ-026 — Can a system discover useful measurement features rather than merely reweight them? [P2]

**Challenge.** Feature invention can increase explanatory power or produce spurious proxies.  
**Test.** Compare fixed-feature reweighting against feature genesis on held-out and counterfactual datasets.  
**Failure signal.** Discovered features leak labels or exploit benchmark artifacts.  
**References.** CDE-007; CASE-004; EXT-002–003.

## 8. Distributed Exploration and Structural Relay

### OQ-027 — How can distributed CDE guarantee coherent commits? [P0]

**Challenge.** Local validity does not imply global validity when workers share resources or constraints.  
**Test.** Inject conflicting vehicle assignments, stale state snapshots, and concurrent commits.  
**Failure signal.** Two individually valid worker results combine into an invalid global solution.  
**References.** CDE-009; SPEC-005; CASE-005.

### OQ-028 — What is the right lease, epoch, and idempotency model? [P0]

**Challenge.** Worker retries and delayed results can duplicate work or overwrite newer states.  
**Test.** Fault injection for timeout, worker crash, duplicate messages, and stale epochs.  
**Failure signal.** An expired worker successfully commits an outdated transition.  
**References.** CDE-009; SPEC-005; CASE-005.

### OQ-029 — How should search overlap and shared incumbents be managed? [P1]

**Challenge.** Parallel search can waste effort, but aggressive deduplication may erase useful diversity.  
**Test.** Compare independent workers, island models, shared incumbent, and coordinated partitioning.  
**Failure signal.** Coordination costs exceed savings or shared bounds are unsound.  
**References.** CDE-009; CASE-005; EXT-001.

### OQ-030 — What constitutes a valid Method Relay artifact? [P0]

**Challenge.** Reusing a method requires its preconditions, effects, evidence, scope, and failure modes.  
**Test.** Transfer a validated operator between compatible and incompatible task families.  
**Failure signal.** Source success is treated as proof of target applicability.  
**References.** CDE-010; SPEC-005; CASE-005.

### OQ-031 — When does repeated execution become Temporal Relay? [P1]

**Challenge.** Repetition can add independent evidence, or merely duplicate correlated observations.  
**Test.** Compare repeated runs with varying seeds and data to exact duplicate replay.  
**Failure signal.** Duplicate observations artificially increase confidence.  
**References.** CDE-010; CASE-005; EXT-004.

### OQ-032 — Can negative transfer be predicted and contained? [P1]

**Challenge.** A successful source-domain method may fail under target constraints, data, or representation.  
**Test.** Measure transfer success, compatibility diagnostics, and rollback on held-out domains.  
**Failure signal.** A transferred capability silently changes target semantics or worsens results.  
**References.** CDE-010; CASE-005; EXT-002, EXT-004.

### OQ-033 — What evidence distinguishes sharing from Collective Learning? [P1]

**Challenge.** Information exchange does not establish downstream capability gain.  
**Test.** Compare isolated agents, artifact-sharing agents, validated-transfer agents, and collective-policy learners under matched cost.  
**Failure signal.** More messages are reported as learning without better independent task outcomes.  
**References.** CDE-010; CASE-005; EXT-004.

## 9. Open-Ended Delta and Representation Evolution

### OQ-034 — How can representational bottlenecks be diagnosed? [P1]

**Challenge.** Search stagnation may reflect a poor solver, insufficient compute, or an unexpressible operation.  
**Test.** Construct tasks with known missing operators and matched alternative explanations.  
**Failure signal.** The system changes representation unnecessarily on already well-expressed tasks.  
**References.** CDE-011; EXT-002.

### OQ-035 — Can new operators expand reachable solutions safely? [P1]

**Challenge.** Operator genesis must distinguish semantic innovation from renaming or macro compression.  
**Test.** Compare restricted grammars with generated operators on finite reachability fixtures.  
**Failure signal.** New operators violate constraints or only duplicate existing sequences.  
**References.** CDE-011; EXT-002.

### OQ-036 — Which representation migrations preserve task meaning? [P0]

**Challenge.** A new schema may discard important distinctions or invalidate caches and proof claims.  
**Test.** Declare preserved observables and test forward migrations, partial migrations, and adversarial counterexamples.  
**Failure signal.** A lossy migration is labeled equivalent without disclosing lost information.  
**References.** SPEC-001, SPEC-006; EXT-002.

### OQ-037 — How can novelty be measured without rewarding meaningless complexity? [P2]

**Challenge.** Syntactic novelty is not functional capability.  
**Test.** Compare edit-distance novelty, behavioral novelty, new reachability, and held-out utility.  
**Failure signal.** Novelty increases while utility and reliability deteriorate.  
**References.** EXT-001–002.

### OQ-038 — Can multiple representations coexist coherently? [P2]

**Challenge.** A coarse planning view and detailed validation view may disagree.  
**Test.** Maintain synchronized multi-view states with explicit adapters and check cross-view invariants.  
**Failure signal.** A decision accepted in one view violates hard constraints in another.  
**References.** CDE-008; EXT-002.

## 10. Scientific Discovery and Autonomous Questioning

### OQ-039 — Can CDE generate falsifiable rather than merely plausible hypotheses? [P1]

**Challenge.** Fluent explanations can lack discriminating predictions.  
**Test.** Compare hypothesis generators on synthetic causal systems with known interventions.  
**Failure signal.** Candidates cannot be distinguished by any feasible experiment.  
**References.** EXT-003; AQSI-related interfaces.

### OQ-040 — Can experiment selection maximize information gain under cost constraints? [P1]

**Challenge.** An experiment may be informative but infeasible, unsafe, or prohibitively expensive.  
**Test.** Compare random, heuristic, Bayesian, and CDE-selected experiments with identical budgets.  
**Failure signal.** Claimed information gain is based on assumptions unavailable to the real experimenter.  
**References.** CDE-008; EXT-003.

### OQ-041 — How can correlation, prediction, and causation remain distinct? [P0]

**Challenge.** Observational fit alone does not establish causal identification.  
**Test.** Use observationally equivalent causal graphs and interventions where available.  
**Failure signal.** A predictive model is reported as a proven causal mechanism.  
**References.** EXT-003.

### OQ-042 — Can metric discovery improve scientific measurement rather than reward gaming? [P1]

**Challenge.** Measurement definitions can change what is optimized and what appears successful.  
**Test.** Use proxy-misalignment fixtures, independent replication, and held-out decision utility.  
**Failure signal.** A new scientific metric is validated only by its own higher score.  
**References.** CASE-004; EXT-003.

### OQ-043 — Can unresolved Leftovers drive useful autonomous questions? [P2]

**Challenge.** Failed searches and unexpressible proposals may expose productive research questions.  
**Test.** Compare random questions, expert questions, and Leftover-derived Question Units on downstream evidence yield.  
**Failure signal.** More questions are generated without improving answerability or knowledge.  
**References.** CDE-010; EXT-002–003.

## 11. Collective Intelligence and Structural RSI

### OQ-044 — What is the smallest measurable unit of collective learning? [P1]

**Challenge.** Define whether a reusable method, calibrated constraint, or validated evidence artifact counts as a capability increment.  
**Test.** Track transfer artifacts and independent target-task improvement.  
**Failure signal.** Collective learning is claimed from communication volume alone.  
**References.** CDE-010–011; EXT-004.

### OQ-045 — Can competing agents improve evidence quality? [P1]

**Challenge.** Competition may reveal counterexamples but also create correlated errors or strategic gaming.  
**Test.** Compare independent challengers, cooperative reviewers, and shared-model ensembles.  
**Failure signal.** Apparent consensus reflects common training data or evaluator leakage.  
**References.** CASE-003, CASE-005; EXT-004.

### OQ-046 — How can generator and policy evolution be validated independently? [P0]

**Challenge.** A candidate meta-policy must not control the test that certifies it.  
**Test.** Use sealed task families, versioned evaluators, and explicit promotion authority.  
**Failure signal.** The meta-generator edits its evaluator or redefines success to approve itself.  
**References.** CDE-011; SPEC-006; EXT-004.

### OQ-047 — Under what conditions does recursive improvement persist? [P2]

**Challenge.** Early gains may plateau, regress, or overfit; a finite positive sequence does not establish unbounded improvement.  
**Test.** Run preregistered multi-generation experiments with independent held-out distributions and full cost accounting.  
**Failure signal.** Gains disappear on new tasks or are explained by increasing compute.  
**References.** CDE-011–012; EXT-004.

### OQ-048 — What is the appropriate boundary between agent autonomy and governance? [P0]

**Challenge.** Proposal generation, execution, validation, and promotion require different permissions.  
**Test.** Attempt unauthorized constraint edits, metric changes, evaluator modifications, and irreversible actions.  
**Failure signal.** A proposal agent can self-authorize an unvalidated deployment.  
**References.** SPEC-005–006; EXT-004.

### OQ-049 — How can the system preserve plural perspectives without forced convergence? [P2]

**Challenge.** Agents may optimize legitimate but incompatible metrics and maintain different world models.  
**Test.** Compare single-metric consensus with perspective-aware portfolios and explicit tradeoff policies.  
**Failure signal.** Minority perspectives are erased by averaging without authorization.  
**References.** CDE-007, CDE-010; EXT-004.

## 12. Theoretical Boundaries and Generalization

### OQ-050 — Which termination guarantees survive dynamic operator creation? [P0]

**Challenge.** Finite constructive termination arguments assume a fixed decision universe.  
**Test.** Formalize bounded and unbounded operator-genesis settings and identify applicable conditions.  
**Failure signal.** A fixed-universe theorem is invoked after new candidates are introduced.  
**References.** CDE-004, CDE-012; EXT-002.

### OQ-051 — What is the strongest defensible completeness claim? [P0]

**Challenge.** Exhaustiveness may hold for a declared finite grammar, not for all conceivable representations.  
**Test.** Enumerate a bounded fixture and compare discovered states against its defined universe.  
**Failure signal.** Partial generator coverage is presented as universal completeness.  
**References.** CDE-003, CDE-012; SPEC-002.

### OQ-052 — Can transferability be predicted from structural similarity? [P2]

**Challenge.** Similar task schemas may conceal different causal or constraint structures.  
**Test.** Compare schema distance, operator precondition overlap, and observed target transfer.  
**Failure signal.** Structural similarity predicts success only on the training distribution.  
**References.** CDE-010, CDE-012; EXT-002, EXT-004.

### OQ-053 — When does CDE add explanatory value beyond a generic orchestrator? [P1]

**Challenge.** The framework should identify measurable contributions from typed Deltas, versioned constraints, M-to-N accounting, path memory, and Relay contracts.  
**Test.** Remove each feature under controlled ablations.  
**Failure signal.** No feature provides measurable correctness, reproducibility, or utility benefits on the selected domains.  
**References.** CDE-008, CDE-012; CASE-005.

### OQ-054 — What would count as evidence *against* the overall CDE thesis? [P0]

**Challenge.** A research program should state conditions under which its proposed integration is not useful.  
**Test.** Run strong domain-native baselines, full-cost accounting, adversarial conformance tests, and independent replication.  
**Failure signal.** Added contract/orchestration complexity consistently exceeds correctness, transfer, or efficiency gains; report this openly.  
**References.** CDE-012; all Cases and Extensions.

## 13. Cross-Cutting Experimental Requirements

All proposed investigations SHOULD use the following discipline:

1. **Pin versions:** State schema, Delta/operator manifest, constraints, metric, solver, generator, policy, and evaluator.
2. **Declare the candidate universe:** Especially for reachability, completeness, and coverage claims.
3. **Separate proposal from commitment:** `PASS`, `FAIL`, `UNKNOWN`, and `ERROR` must remain distinct where applicable.
4. **Use strong baselines:** Include well-tuned classical algorithms, not merely weak random search.
5. **Match budgets:** Include proposal generation, search, validation, migration, coordination, and governance costs.
6. **Hold out tasks:** Separate generator development, model selection, promotion validation, and final tests.
7. **Preserve negative evidence:** Counterexamples, regressions, failed transfers, and unresolved Leftovers are first-class outputs.
8. **Distinguish claim levels:** Candidate, feasible, best-known, bounded-gap, optimal, infeasible-proven, exhaustive, and unknown.
9. **Report uncertainty:** Use multiple independent instances/seeds and appropriately calibrated intervals.
10. **Avoid circular validation:** A metric, policy, or generator cannot certify itself solely using an evaluator it controls.
11. **Check global constraints:** Local worker success does not guarantee global feasibility.
12. **Replicate independently:** External reproduction is especially important for claims about transfer and RSI.

## 14. Suggested Priority Sequence

| Stage | Focus | Questions | Expected artifact |
|---|---|---|---|
| S0 | Core conformance | OQ-001–004, OQ-008, OQ-013, OQ-018–019 | Executable contracts and deterministic tests |
| S1 | Initial optimizer baseline | OQ-007, OQ-009, OQ-017, OQ-020 | CASE-001 benchmark and strong baselines |
| S2 | Constrained application | OQ-002, OQ-005, OQ-021, OQ-023 | CASE-002 school-bus testbed |
| S3 | Evidence and matching | OQ-006, OQ-010, OQ-014, OQ-041 | CASE-003 validation suite |
| S4 | Metric evolution | OQ-022, OQ-024–026, OQ-042 | CASE-004 independent metric evaluation |
| S5 | Distributed Relay | OQ-027–033, OQ-044 | CASE-005 multi-worker and transfer tests |
| S6 | Representation evolution | OQ-034–038, OQ-050–051 | EXT-002 migration and operator-genesis suite |
| S7 | Scientific discovery | OQ-039–043 | EXT-003 hypothesis/experiment suite |
| S8 | Controlled RSI | OQ-045–049, OQ-052–054 | EXT-004 independent meta-evolution evaluation |

These stages are a suggested dependency order, not a claim that each research question must be solved before later work begins.

## 15. Proposed Question Unit / Contribution Template

```yaml
question_id: OQ-NEW
short_title: "A falsifiable question"
status: OPEN
priority: P1
research_scope:
  domain: "specified task family"
  representation_version: "pinned version"
  operator_manifest_version: "pinned version"
assumptions:
  - "explicit prerequisite"
competing_hypotheses:
  - "hypothesis A"
  - "hypothesis B"
proposed_test:
  baseline: "strong, specified baseline"
  intervention: "single isolated change"
  heldout_protocol: "independent task split"
  cost_accounting: "all relevant costs"
possible_refutation: "specific observable failure"
required_evidence:
  - "raw results and seeds"
  - "constraint and conformance reports"
  - "negative results and uncertainty"
related_papers:
  - "CDE-..."
relay_artifacts:
  - "Evidence"
  - "Method"
  - "Leftover"
owner: null
last_reviewed: null
```

A question may be split into narrower units when its scope is too broad for one reproducible experiment. New questions should be added without renumbering published identifiers; superseded questions should retain a traceable redirect.

## 16. Cross-Reference Map

| Research area | Core foundations | Specifications | Cases / extensions |
|---|---|---|---|
| State/constraint semantics | CDE-001–003 | SPEC-001–002 | CASE-001–003 |
| Path memory and loops | CDE-004 | SPEC-003 | CASE-001, CASE-003 |
| DP/B&B/solver integration | CDE-005, CDE-008 | SPEC-004 | CASE-001–002 |
| Directional/multi-scale search | CDE-006 | SPEC-002 | CASE-001, EXT-001 |
| Metric co-evolution | CDE-007 | SPEC-006 | CASE-004, EXT-003 |
| Distributed exploration | CDE-009 | SPEC-005 | CASE-005 |
| Structural Relay | CDE-010 | SPEC-005 | CASE-005, EXT-004 |
| Generator/policy evolution | CDE-011 | SPEC-006 | EXT-002, EXT-004 |
| Generalization boundaries | CDE-012 | All | All Cases and Extensions |

## 17. Research Integrity and Publication Notes

This register contains **54 open questions (OQ-001 through OQ-054)**. Priority and experimental designs are proposed; no empirical results are asserted. The questions are intended to remain open to revision as conformance implementations, benchmark data, and external criticism become available.

A useful result may be a verified improvement, a proof under explicit assumptions, a counterexample, a negative transfer, or a demonstration that a simpler existing method is preferable. This is consistent with the CDE principle of **explicit Leftovers and collective-learning-friendly research**.

**Next companion artifact:** `TESTABLE-HYPOTHESES.md`, which should translate selected questions into preregisterable hypotheses with measurable outcomes and falsification criteria. Follow with `VALIDATION-ROADMAP.md` for implementation milestones and independent evaluation gates.

---

**Citation context:** *Constrained Delta Evolution (CDE)*, Sizhe Tan and GPT-Obot, research draft, 2026.  
**License:** Apache-2.0.
